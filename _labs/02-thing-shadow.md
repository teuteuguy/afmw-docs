---
title: "Lab 2 - Interract with a Thing"
excerpt: "Use Thing Shadows to interract with your device!"
toc: true
toc_label: Lab 2 Contents
toc_sticky: true
---

It's the summer, and its hot outside. Lets play around with remote controlled Air Conditioning units.

For this lab, our M5StickC will act as our Air Conditioning unit.

You can turn the AirCon **ON** or **OFF** remotely as well as set a target temperature.

* If the AirCon is OFF, the temperature will rise to a stable 40 deg celcius.
* If the AirCon is ON, the temperature will decrease to reach your chosen temperature

![M5StickC Lab2]({{ "/assets/images/labs/lab-2/lab2.png" | relative_url }})

## Configuration

In order to configure the code and your M5StickC for Lab1, you must change the following lines of code:

In file: {{ site.data.globals['code'].lab-config-file }}

```bash
Replace:
#define M5CONFIG_LAB1_AWS_IOT_BUTTON

With:
#define M5CONFIG_LAB2_SHADOW
```

## Rebuild Code

```bash
cd {{ site.data.globals['code'].make-dir }}
make all -j4
```

## Flash Code

Please follow the same procedure from [Flash the firmware files]({{ "/labs/00-setup-the-code/#flash-the-firmware-files" | relative_url }}).

## Result

Log into your AWS IoT Management console. Select your Thing ({{ site.data.globals['iot'].thing-name }}), and select **Shadow**.

1. If the device has successfully run the demo code, it should have **reported** it's first shadow document:

```json
{
	"reported": {
		"powerOn": 0,
		"temperature": 40
	}
}
```

2. Edit this shadow document, on the page with the following:

```json
{
	"desired": {
		"powerOn": 1,
		"temperature": 22
	}
}
```

**Note:** For the purpose of the demo, **powerOn** parameter is used to toggle turning **ON** or **OFF** the simulated Air Conditioning unit, and **desired.temperature** is used to set the desired temperature value.
{: .notice--info}

Modifying the shadow desired document, should instantly result in a compiled shadow document looking something like:

```json
{
	"desired": {
		"powerOn": 1,
		"temperature": 22
	},
	"reported": {
		"powerOn": 0,
		"temperature": 40
	},
	"delta": {
		"powerOn": 1,
		"temperature": 22
	}
}
```

Now that the Air Con is turned **ON**, watch how first of all, the device will **report** turning itself **ON** and how it's temperature will decrease to meet your set value.

**Note:** Be carefull when setting the **desired.temperature** value. The lab code only supports **positive integer values**. Do not use a floating point value.
{: .notice--warning}

## Done

You are done with Lab 2.


## Challenge
* Can you modify the code to control the device's LED via the thing shadow?
* Can you plug the M5StickC into your actual Air Conditioning and control it's on/off and temperature states? (if so, send us a picture!)
