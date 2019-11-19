---
title: "Lab 1 - AWS IoT Button"
excerpt: "Create your own AWS IoT Button, using the M5StickC!"
toc: true
toc_label: Lab 1 Contents
toc_sticky: true
---

A couple years ago, AWS announced the AWS IoT Button you could order from Amazon.com. Why not create our own?

For this lab, our M5StickC will act as an AWS IoT Button, allowing you to trigger downstream AWS Services.

## Configuration

In order to configure the code and your M5StickC for Lab1, you must change the following lines of code:

In file: {{ site.data.globals['code'].lab-config-file }}

```bash
Replace:
#define M5CONFIG_LAB0_DEEP_SLEEP_BUTTON_WAKEUP

With:
#define M5CONFIG_LAB1_AWS_IOT_BUTTON
```

## Rebuild Code

```bash
cd {{ site.data.globals['code'].make-dir }}
make all -j4
```

## Flash Code

Please follow the same procedure from [Flash the firmware files]({{ "/labs/00-setup-the-code/#flash-the-firmware-files" | relative_url }}).

## Result

Log into your AWS IoT Management console, use the web MQTT client and subscribe to the following topic:

```bash
m5stickc/+
```

Click your M5StickC button: You should see the following JSON document flow on the broker

```json
{
	"serialNumber": "[YOUR DEVICE SERIAL NUMBER]",
	"clickType": "SINGLE"
}
```

Hold your M5StickC button: You should see the following JSON document flow on the broker

```json
{
	"serialNumber": "[YOUR DEVICE SERIAL NUMBER]",
	"clickType": "HOLD"
}
```

## Done

You are done with Lab 1.

## Knowledge check
* Q1: **m5stickc/+**, what does the **"+"** do? <button id="q1" class="challenge btn btn--info btn--small">Answer</button>
{% capture q1-answer %}
The <strong>"+"</strong> in the topic name, acts as a wildcard for that given section of the topic. There are 2 topic wildcards that can be used:

| Wildcard | Description |
| :------: | ----------- |
| **#**	| Must be the last character in the topic to which you are subscribing. Works as a wildcard by matching the current tree and all subtrees. For example, a subscription to **Sensor/#** receives messages published to **Sensor/**, **Sensor/temp**, **Sensor/temp/room1**, but not the messages published to **Sensor**. |
| **+** | Matches exactly one item in the topic hierarchy. For example, a subscription to **Sensor/+/room1** receives messages published to **Sensor/temp/room1**, **Sensor/moisture/room1**, and so on. |
{% endcapture %}
<div id="q1-answer" class="notice--info hide">
  {{ q1-answer | markdownify }}
</div>

* Q2: What can you replace **"+"** with? <button id="q2" class="challenge btn btn--info btn--small">Answer</button>
{% capture q2-answer %}
In our IoT Button use-case, we saw that the button publishes to a topic that has **m5stickc/[YOUR DEVICE SERIAL NUMBER]**. In the MQTT client, if you subscribe to that specific topic (ie. **m5stickc/[YOUR DEVICE SERIAL NUMBER]**), you will see the data coming only from your device.
{% endcapture %}
<div id="q2-answer" class="notice--info hide">
  {{ q2-answer | markdownify }}
</div>

## Challenge
* Can you create an AWS IoT Rule that gets triggered on a button press and sends your cell phone an SMS?
* Can you make that rule ONLY send you an SMS if you HOLD the button?
* Can you modify the AWS IoT Policy you created in prior steps to limit communications to only the required topics?
