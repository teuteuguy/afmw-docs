---
title: "Lab 3 - Alexa"
excerpt: "Alexa turn on the aircon!"
toc: true
toc_label: Lab 3 Contents
toc_sticky: true
---

Wouldn't it be great if you could control your Air Conditioning unit with your voice? No need to search for the remote anymore!

**Alexa** solves all of this for us. So lets create our first **Alexa Skill**!

We'll be able to ask Alexa:

* Alexa turn on the aircon
* Alexa turn off the aircon

## Preliminary

This Lab requires you to have completed [Lab 2]({{ "/labs/02-thing-shadow/" | relative_url }})

## Step 1. - *Develop your first Alexa Skill*

Setting up Your Alexa Skill in the Developer Console

Go to the [Alexa Developer Console](https://developer.amazon.com/alexa/console/ask). In the top-right corner of the screen, click the "Sign In" button. (If you don't already have an account, you will be able to create a new one for free.)

![Alexa Developer Console]({{ "/assets/images/labs/lab-3/lab3-alexa-signin.png" | relative_url }})

Once you have signed in, select the **Developer Console** link and then **Alexa Skills Kit**.
From the Alexa Developer Console select the **Create Skill** button near the top-right of the list of your Alexa Skills.

![Create Skill 1]({{ "/assets/images/labs/lab-3/lab3-create-alexa-skill-1.png" | relative_url }})

Give your new skill a Name, for example, **'workshop'**. This is the name that will be shown in the Alexa Skills Store, and the name your users will refer to.

![Create Skill 2]({{ "/assets/images/labs/lab-3/lab3-create-alexa-skill-2.png" | relative_url }})

Select the Default Language. This tutorial will presume you have selected **'English (US)'**.
Select the Custom model under the **'Choose a model to add to your skill'** section. Click the **Create Skill button** at the top right.
Choose **Start from scratch** from the Choose a template section and click the **Choose** button on the top right.

### Build the Interaction Model for your skill

1. On the left hand navigation panel, select the **JSON Editor** tab under Interaction Model. In the textfield provided, replace any existing code with the code provided in the below sample. Click **Save Model**.
2. Click **"Build Model"**

![JSON Editor]({{ "/assets/images/labs/lab-3/lab3-json-editor.png" | relative_url }})

### Skill Sample

```json
{
    "interactionModel": {
        "languageModel": {
            "invocationName": "workshop skill",
            "intents": [
                {
                    "name": "AMAZON.FallbackIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                },
                {
                    "name": "AirConOnIntent",
                    "slots": [],
                    "samples": [
                        "turn on"
                    ]
                },
                {
                    "name": "AirConOffIntent",
                    "slots": [],
                    "samples": [
                        "turn off"
                    ]
                }
            ],
            "types": []
        }
    }
}
```

We will take a pause of setting up the Alexa skills here to create the lambda function.

![Wait for lambda]({{ "/assets/images/labs/lab-3/lab3-wait-for-lambda.png" | relative_url }})

## Step2 Build your Alexa Skills lambda function

In the management console, find lambda service

![Lambda 1]({{ "/assets/images/labs/lab-3/lab3-lambda-1.png" | relative_url }})

navigate to lambda function service and select **"Create”**

![Lambda 2]({{ "/assets/images/labs/lab-3/lab3-lambda-2.png" | relative_url }})

input the params for lambda function:

* function name: **"alexa_workshop”**
* Runtime: **"python 3.7”**
* Execution role: **"Create a new role with basic Lambda function”**

then **"Create"**

![Lambda 3]({{ "/assets/images/labs/lab-3/lab3-lambda-3.png" | relative_url }})

Next, configure the lambda function:

First upload the lambda function zip package, you can download the code from [HERE]({{ "/assets/lab-3/alexa_skill_lambda_v1.zip"  | relative_url }}), keep the hander as **"lambda_function.lambda_handler"**, then slick **"Save”**

![Lambda 4]({{ "/assets/images/labs/lab-3/lab3-lambda-4.png" | relative_url }})

Set your device shadow update topic as an environment variable. In Environment Variables section, create an environment variable named **shadow_update_topic** and set it's value to your thing's shadow update topic. Which should be something like: **$aws/things/[YOUR THING NAME]/shadow/update**

![Lambda env]({{ "/assets/images/labs/lab-3/lab3-lambda-env.png" | relative_url }})

Configure the lambda function service role, and the trigger events for this lambda

Configure the service role by navigate to the execution role

![Lambda 5]({{ "/assets/images/labs/lab-3/lab3-lambda-5.png" | relative_url }})

configure the role policies

![Lambda 6]({{ "/assets/images/labs/lab-3/lab3-lambda-6.png" | relative_url }})

select **"AWSIoTFullAccess”**, then **attach**,

![Lambda 7]({{ "/assets/images/labs/lab-3/lab3-lambda-7.png" | relative_url }})

Next, we set Alexa skills as the lambda function trigger,

![Lambda 8]({{ "/assets/images/labs/lab-3/lab3-lambda-8.png" | relative_url }})

select **Alexa Skills Kit**

![Lambda 9]({{ "/assets/images/labs/lab-3/lab3-lambda-9.png" | relative_url }})

Then input your skill ID

![Lambda 10]({{ "/assets/images/labs/lab-3/lab3-lambda-10.png" | relative_url }})

to check the skill ID of your Alexa skills, go back to the Alexa develop console and navigate to your skill, to check skill ID

![Lambda 11]({{ "/assets/images/labs/lab-3/lab3-lambda-11.png" | relative_url }})

After the configuration of IoT access, and the trigger events, your lambda configuration should look like this

![Lambda 12]({{ "/assets/images/labs/lab-3/lab3-lambda-12.png" | relative_url }})

Now, since the lambda function is ready, you can set your "Endpoint” of your lambda skill

![Lambda 13]({{ "/assets/images/labs/lab-3/lab3-lambda-13.png" | relative_url }})

## Step3 Configure your Alexa Skills endpoint

next we configure the Alexa skills endpoint

![skill 1]({{ "/assets/images/labs/lab-3/lab3-skill-1.png" | relative_url }})

configure your endpoint by input the ARN of your Alexa lambda function (check the next step to copy your lambda function’s ARN), and save the Endpoints

![skill 2]({{ "/assets/images/labs/lab-3/lab3-skill-2.png" | relative_url }})

You can find your ARN of the lambda function in your lambda function configure page

![skill 3]({{ "/assets/images/labs/lab-3/lab3-skill-3.png" | relative_url }})

## Step4 Test Your Alexa Skills

now we can start to test your Alexa skills and check the interaction with your device!

First, make sure your device is running and reporting sensor data like the following in Lab2

Secondly, navigate to the test console to test your skills, change the mode to Development

![Test 1]({{ "/assets/images/labs/lab-3/lab3-test-1.png" | relative_url }})

then you can input the speeches as following, this will simulate as you are talking to an Echodot or your phone

![Test 2]({{ "/assets/images/labs/lab-3/lab3-test-2.png" | relative_url }})

If the skills response **"ok! I've turned on the aircon”**, you can now check if your board reports the on / off state accordingly

![Test 3]({{ "/assets/images/labs/lab-3/lab3-test-3.png" | relative_url }})

Additionally, you can check your logs on the IoT Core console or looking at the shadow document for your thing

![Test 4]({{ "/assets/images/labs/lab-3/lab3-test-4.png" | relative_url }})

## Done

You are done with Lab 3.

**Challenge:**
* Can you modify your Alexa Skill to support changing the device temperature?
