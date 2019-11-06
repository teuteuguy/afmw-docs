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
cd {{ site.data.globals['code'].build-dir }}
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

## Done

You are done with Lab 1.

**Quizz:**
* Can you explain what subscribing to **m5stickc/+** does?
* What could you replace the **"+"** from the subscription with?

**Challenge:**
* Can you create an AWS IoT Rule that gets triggered on an AWS IoT Button press that sends your cell phone an SMS?
