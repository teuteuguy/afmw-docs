---
title: "Lab 0 - Setup the Code"
excerpt: "Create an IoT Thing, download, setup the code, compile and flash your M5StickC."
toc: true
toc_label: Lab 0 Contents
toc_sticky: true
---

In this lab we will start by creating our first AWS IoT Thing, create its unique Certificate, attach an IoT Policy to it

## Create {{ site.data.globals['iot'].thing-name }}

First we need to create our AWS IoT Thing. For the purpose of this Lab, we will call our Thing: **{{ site.data.globals['iot'].thing-name }}** but you are free to name it whatever you want.

Creating a Thing requires 3 main steps:

1. Create a thing for the device in IoT Core
2. Create a certificate and attach it to the thing
3. Create a Policy and attach it to the certificate

### Create Thing

Navigate to **IoT Core** to enter the AWS IoT service

![IoT Core 0]({{ "/assets/images/labs/lab-0/iotcore-0.png" | relative_url }})

Follow the below 3 steps to register your thing:

1. Make sure to select the **us-east-1** region
2. Navigate to **Manager → Things**
3. Click **Create** to start the creation and registration of your device

![IoT Core 1]({{ "/assets/images/labs/lab-0/iotcore-1.png" | relative_url }})

If this is the very first time you create a device, the following page will appear, please click **Register a thing** to get started

![IoT Core 2]({{ "/assets/images/labs/lab-0/iotcore-2.png" | relative_url }})

In Creating AWS IoT Things page, choose **Create a single thing**

![IoT Core 3]({{ "/assets/images/labs/lab-0/iotcore-3.png" | relative_url }})

Input a Thing name of your choice and click **Next**

![IoT Core 4]({{ "/assets/images/labs/lab-0/iotcore-4.png" | relative_url }})

**Note 1:** In the above screenshot, **device_workshop** is our thing name. You can choose any thing name you want. For the remainder of the labs we will refer to the Thing as **{{ site.data.globals['iot'].thing-name }}**
{: .notice--warning}


### Create a certificate and attach it to the thing

Choose the recommended **One-click certificate creation**

![IoT Core 5]({{ "/assets/images/labs/lab-0/iotcore-5.png" | relative_url }})

If this is the first time you create a thing in AWS IoT, you may not already have IoT policies defined. You can click **Register Thing** and we'll attach a policy later.

**Note 2:** Make sure you activate your certificate, or else your certificate will be created, but your device will not be able to use it to connect.
{: .notice--warning}

**Note 3:** Make sure to download the certificate and private key!
{: .notice--warning}

![IoT Core 6]({{ "/assets/images/labs/lab-0/iotcore-6.png" | relative_url }})


#### Check the Results

We created a Thing named {{ site.data.globals['iot'].thing-name }}

![IoT Core 7]({{ "/assets/images/labs/lab-0/iotcore-7.png" | relative_url }})

We created a certificate that is attached to {{ site.data.globals['iot'].thing-name }}

![IoT Core 8]({{ "/assets/images/labs/lab-0/iotcore-8.png" | relative_url }})

We downloaded {{ site.data.globals['iot'].thing-name }}'s certificate with its private and public keys

![IoT Core 9]({{ "/assets/images/labs/lab-0/iotcore-9.png" | relative_url }})

**Note 4:** Do not lose this zip file, it contains your thing's private key file which cannot be retrieved again.
{: .notice--warning}

**ProTip:** If you lose the files, no worries, you can always create a new certificate for the thing and attach it. Just make sure to revoke and delete the lost one.
{: .notice--info}


### Create a Policy and attach it to the certificate

We now need to create an IoT Policy for the certificate we've created in the previous step.

![IoT Core 10]({{ "/assets/images/labs/lab-0/iotcore-10.png" | relative_url }})

Input the policy name of your choice and enter the **advanced mode** to paste the following policy document

![IoT Core 11]({{ "/assets/images/labs/lab-0/iotcore-11.png" | relative_url }})

Copy/Paste the following policy document into the input box

```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Subscribe",
        "iot:Connect",
        "iot:Receive"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```
![IoT Core 12]({{ "/assets/images/labs/lab-0/iotcore-12.png" | relative_url }})

**ProTip:** The above policy is extremely open. Any entity holding this policy will be able to Connect, Publish, Subscribe and Receive data on any AWS IoT Core topic. It is recommended to restrict device policies to the bare minimum that they need.
{: .notice--warning}


After creation, in the IoT Core console, navigate to **“Secure → Policies**“. You should see your IoT Policy

![IoT Core 13]({{ "/assets/images/labs/lab-0/iotcore-13.png" | relative_url }})

Now, navigate to **“Secure → Certificates**“ to start attaching the IoT Policy to the certificate

![IoT Core 14]({{ "/assets/images/labs/lab-0/iotcore-14.png" | relative_url }})

In the certificate page click **“Actions → Attach policy”**

![IoT Core 15]({{ "/assets/images/labs/lab-0/iotcore-15.png" | relative_url }})

Select your IoT Policy and attach it 

![IoT Core 16]({{ "/assets/images/labs/lab-0/iotcore-16.png" | relative_url }})

Now, when selecting the **policies** section of your certificate, your attached policy should be present there

![IoT Core 17]({{ "/assets/images/labs/lab-0/iotcore-17.png" | relative_url }})

## Get the lab code

Now that we have created our **Thing** ({{ site.data.globals['iot'].thing-name }}), its **Certificate** and **IoT Policy**, lets get the code.

### Clone the repo

In your Cloud9 terminal environment, clone the lab repo:

```bash
cd ~/environment
git clone https://github.com/teuteuguy/amazon-freertos-m5stickc-workshop.git --recurse-submodules workshop
```

**Note 5:** If you chose to install the labs on your own computer, run the above commands within a terminal. Make sure you replace **~/environment** with the folder of your choice
{: .notice--info}


### Create the aws\_clientcredential\_keys.h file

For lab purposes, you can use the following tool to convert your Certificate/Private key into the **aws\_clientcredential\_keys.h** that we will include in our project source code.

Navigate to [https://yona75.github.io/credformatter/](https://yona75.github.io/credformatter/), upload your Certificate and Private key that you downloaded in the [previous step]({{ "/labs/00-setup-the-code/#check-the-result" | relative_url }}) and generate an **aws\_clientcredential\_keys.h** file.

### Copy aws\_clientcredential\_keys.h to the project

Copy the file to **~/environment/workshop/amazon-freertos/demos/include/** directory by dragging it there

![Copy aws_clientcredential_keys.h]({{ "/assets/images/labs/lab-0/copy-credentials-file.png" | relative_url }})

### Edit aws_clientcredential.h

Open the **aws\_clientcredential.h** file by double-clicking on it. And change/set the following values:

```bash
...
#define clientcredentialMQTT_BROKER_ENDPOINT "[YOUR AWS IOT ENDPOINT]"
...
#define clientcredentialIOT_THING_NAME       "{{ site.data.globals['iot'].thing-name }}"
...
#define clientcredentialWIFI_SSID            "[WILL BE PROVIDED]"
...
#define clientcredentialWIFI_PASSWORD        "[WILL BE PROVIDED]"
...
#define clientcredentialWIFI_SECURITY        eWiFiSecurityWPA2
...
```

{% capture notice-text %}

* You can find your AWS IoT Endpoint by navigating to the IoT Core console and clicking on **Settings**
* The clientcredentialWIFI\_SECURITY #define is **without** double quotes
* Replace the IoT Thing Name with the one you chose.

{% endcapture %}

<div class="notice--info">
  <strong>Note 6:</strong>
  {{ notice-text | markdownify }}
</div>


### Setup the code

```bash
mv ~/environment/workshop/m5stickc ~/environment/workshop/amazon-freertos/vendors/espressif/boards
```

### Configure the code

Navigate to the project source code:

```bash
cd {{ site.data.globals['code'].make-dir }}
```

Run **make menuconfig** and simply exit. This will setup the project defaults for the code as well as the M5StickC.

```bash
make menuconfig
```

Click *Save* and then *Exit*

![make menuconfig]({{ "/assets/images/labs/lab-0/make-menuconfig.png" | relative_url }})

### Compile the code

```bash
make all -j4
```

If compilation is successful, you should be greeted with the following message:

```bash
esptool.py v2.6
To flash all build output, run 'make flash' or:
python /home/ec2-user/environment/workshop/amazon-freertos/vendors/espressif/esp-idf/components/esptool_py/esptool/esptool.py --chip esp32 --port /dev/cu.usbserial-29568143B4 --baud 1500000 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 /home/ec2-user/environment/workshop/amazon-freertos/vendors/espressif/boards/m5stickc/aws_demos/build/bootloader/bootloader.bin 0x20000 /home/ec2-user/environment/workshop/amazon-freertos/vendors/espressif/boards/m5stickc/aws_demos/build/aws_demo.bin 0x8000 /home/ec2-user/environment/workshop/amazon-freertos/vendors/espressif/boards/m5stickc/aws_demos/build/partition-table.bin
```

**Note 7:** If you get errors at this stage, the most common mistake is that your compilation environment has not been setup properly, go back to the setup guide. Check your PATH variable with **printenv PATH** and make sure the ESP IDF has been properly set.
{: .notice--warning}


### Download the binary files

**Note 8:** If you are compiling on your laptop, you can skip this step and go to [flash directly]({{ "/labs/00-setup-the-code/#flash-the-firmware-files" | relative_url }})
{: .notice--primary}

Once compilation is done, download these 3 files to your local computer:

* **build/aws_demo.bin**
* **build/partition-table.bin**
* **build/bootloader/bootloader.bin**

![download bin files from Cloud9]({{ "/assets/images/labs/lab-0/download.png" | relative_url }})

**Note 9:** the "build" folder is located in **{{ site.data.globals['code'].build-dir }}**
{: .notice--info}

## Flash the firmware files

For the following part of the lab, we will be connecting to the device, and as such, all commands need to be run on the computer, not the Cloud9 environment.

1. Connect your M5StickC to your laptop using the provided USB cable
2. Identify which port the device is connected to

   On Windows it will be **COMX**, on Mac OS typically it enumerated as **/dev/cu.usbserial-xxx** and on Linux most likely **/dev/ttyUSB0**

**Note 10:** If you do not see your device, you may need to install a driver. You can dowload the [FTDI VCP Drivers](https://www.ftdichip.com/Drivers/VCP.htm) here. Install it. unplug your device and reconnect. You may also need to reboot your machine, after the driver installation.
{: .notice--danger}

<!-- You may need to install the [CP210x USB to UART Bridge VCP Drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers), if you cannot find the device once connected. -->

**Note 11:** If you are compiling on your laptop, you can skip the below setup and simply execute **make flash**. You should also run **make menuconfig** again, and set your serial port accordingly as well as set it's boad rate to 1500000
{: .notice--primary}

### Mac/Linux

**Note 12:** There is currently a known problem when trying to run on Catalina. If you are running on Catalina, please use provided paperclip to connect pins **G0** to **GND** of the M5StickC when trying to flash. This should solve the problem on current design.
{: .notice--danger}

*  Install esptool.py:

```bash
sudo pip install esptool pyserial
```

* if you compiled the code within the Cloud9 environment, execute the following command from the directory you placed the 3 downloaded files:

```bash
cd [THE FOLDER WHERE YOU DOWNLOADED THE 3 FILES IN PREVIOUS STEP]
esptool.py --chip esp32 --port [YOUR /dev/usbserial-xxx] --baud 1500000 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 bootloader.bin 0x20000 aws_demo.bin 0x8000 partition-table.bin
```

### Windows

* Download the [esptool](https://dl.espressif.com/dl/esptool-2.3.1-windows.zip) binary from Espressif

* Drop it in the subfolder that is already in your PATH or add the subfolder you placed esptool to your PATH variable

* Open a Commnd Prompt and execute the following command (from the directory you placed the 3 downloaded files):

```bash
esptool --chip esp32 --port [YOUR COMX] --baud 1500000 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 bootloader.bin 0x20000 aws_demo.bin 0x8000 partition-table.bin
```

## Monitor the flashing process

```bash
bash-3.2$ esptool.py --chip esp32 --port /dev/cu.usbserial-29568143B4 --baud 1500000 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 bootloader.bin 0x20000 aws_demo.bin 0x8000 partition-table.bin
esptool.py v2.5.1
Serial port /dev/cu.usbserial-29568143B4
Connecting........__
Chip is ESP32D0WDQ5 (revision 1)
Features: WiFi, BT, Dual Core
MAC: 24:0a:c4:23:de:7c
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 921600
Changed.
Configuring flash size...
Auto-detected Flash size: 4MB
Flash params set to 0x0220
Compressed 21936 bytes to 13046...
Wrote 21936 bytes (13046 compressed) at 0x00001000 in 0.2 seconds (effective 1145.0 kbit/s)...
Hash of data verified.
Compressed 628432 bytes to 398564...
Wrote 628432 bytes (398564 compressed) at 0x00020000 in 5.9 seconds (effective 854.5 kbit/s)...
Hash of data verified.
Compressed 3072 bytes to 119...
Wrote 3072 bytes (119 compressed) at 0x00008000 in 0.0 seconds (effective 3255.9 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

## Monitor code execution through the serial console

### Windows

1. Install PuTTY

You can download putty from [http://www.putty.org/](http://www.putty.org/) or [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Run Installation wizard

![Install PuTTY]({{ "/assets/images/labs/lab-0/putty-installer.png" | relative_url }})

3. Check all options

![PuTTY Wizard]({{ "/assets/images/labs/lab-0/putty-setup.png" | relative_url }})

4. Setup the port and speed (Port **COM3** and **115200** in our case)

![PuTTY setup port]({{ "/assets/images/labs/lab-0/putty-port-open.png" | relative_url }})

5. Open console access to ESP32

![PuTTY Console]({{ "/assets/images/labs/lab-0/putty-console-output.png" | relative_url }})


### Mac/Linux

Use **screen** command to see the ESP32 console:

```bash
screen /dev/cu.usbserial-29568143B4 115200 -L
```

**Note 13:** In order to exit screen press **Ctrl + A** and then **K**
{: .notice--info}

## Done

You are done with Lab 0. If we recap, you've:

* Created a Thing, its Certificate and IoT Policy
* Downloaded and setup the Amazon FreeRTOS demo code for the labs
* Compiled and Flashed the code on your M5StickC
