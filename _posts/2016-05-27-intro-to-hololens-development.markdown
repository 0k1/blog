---
author: Arun Raj
date: 2016-05-27 12:39:30+00:00
layout: post
slug: intro-to-development-in-hololens
title: 'Intro to HL development'
---

Terry Myerson – EVP, Windows and Devices Group wasn't kidding when he [said](https://news.microsoft.com/speeches/satya-nadella-and-terry-myerson-build-2016/) that it really is such a great time to be a Windows developer. Now with the release of Unity for Hololens you can start developing for Microsoft Hololens with out purchasing any development tools. All you need is a Windows 10 machine with the specs they've mentioned, Visual Studio 2015 Update 2 (you can get the community edition free) , Unity HoloLens Technical Preview which is ready to download at from Unity3D.com

**System requirements**

The Windows 10 SDK works best on the Windows 10 operating system. This SDK is also supported on Windows 8.1, Windows 8, Windows 7, Windows Server 2012, Windows Server 2008 R2. Note that not all tools are supported on older operating systems. Visual Studio 2015 also has hardware requirements.

The HoloLens emulator is based on Hyper-V and uses RemoteFx for hardware accelerated graphics. To use the emulator, make sure your PC meets these hardware requirements:

* 64-bit Windows 10 Pro, Enterprise, or Education (The Home edition does not support Hyper-V. Installing the HoloLens Emulator will fail on the Home edition)
* 64-bit CPU
* CPU with 4 cores (or multiple CPU's with a total of 4 cores)
* 8 GB of RAM or more
* In the BIOS, the following features must be supported and enabled:
    Hardware-assisted virtualization,
    Second Level Address Translation (SLAT),
    Hardware-based Data Execution Prevention (DEP)
* GPU (The emulator might work with an unsupported GPU, but will be significantly slower)
     DirectX 11.0 or later,
     WDDM 1.2 driver or later

**Setting Up the Dev environment**

Please go through [MSFT Developer portal for hololens](https://developer.microsoft.com/en-us/windows/holographic/install_the_tools) and install the tools mentioned to set up the dev environment. 