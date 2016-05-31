---
author: Arun Raj
date: 2016-05-27 12:39:30+00:00
layout: post
slug: getting-started-hl-emulator
title: 'Getting Started : HL Emulator & Web Portal'
---


I use the emulator most of the time to debug as deploying to device takes longer and seems less productive. One neat fact is that you can even use your microphone connected to your pc for voice commands in the HL emulator.If your system meets the requirements, please ensure that the "Hyper-V" feature has been enabled on your system through Control Panel -> Programs -> Programs and Features -> Turn Windows Features on or off -> ensure that "Hyper-V" is selected for the Emulator installation to be successful.

For those who don't have the device as of now the emulator is available for download [here.](http://go.microsoft.com/fwlink/?LinkID=724053)

 Note that to deploy your app to HL emulator you have to select x86 configuration and till this date the emulator will not appear in the drop-down for your Visual Studio project if the project's TargetPlatformVersion is greater than 10240. To work around this, change your project's TargetPlatformVersion to 10240. This will be fixed in a future release of the HoloLens emulator. [Refer MSFT Known issues](https://developer.microsoft.com/en-us/windows/holographic/known_issues#Emulator) to know if this still exists.

**HL Web portal**

Every hololens emulator/device has a developer web portal associated with it. This shows the configs