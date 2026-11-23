
# D2VLauncher

**A third-party launcher and application manager for Hyundai, Kia, and Genesis DAudio2 head units.**

D2VLauncher brings the ability to install and manage third-party applications on compatible DAudio2 infotainment systems.

This project builds on the research and work of [greenluigi1](https://github.com/greenluigi1), including his [HyundaiFirmwareDecrypter](https://github.com/greenluigi1/HyundaiFirmwareDecrypter) repository.

## ⚠️ Disclaimer

This project modifies your vehicle's infotainment firmware. While it has been tested on real vehicles, modifying firmware always carries some risk.

**There is no guarantee that this process is completely safe or that it cannot brick your head unit.**

-   Make sure you are using firmware intended for your exact vehicle and head unit.
    
-   Do not interrupt the update process once it has started.
    
-   Keep the vehicle running throughout the installation to prevent battery depletion.
    
-   Proceed at your own risk.
    

I also want to be transparent about development: portions of this project were developed with assistance from Codex and Claude, particularly some of the more involved development tasks. I have limited time available for development, but the project has been tested on real hardware.

----------

## Compatibility

D2VLauncher is intended for Hyundai, Kia, and Genesis vehicles equipped with:

-   **Head unit:** DAudio2
    
-   **Display:** 8-inch
    
-   **Navigation:** No built-in navigation
    

Other head units and configurations are not currently supported or verified.

## Features

-   Root access through an ADB backdoor over Wi-Fi.
    
-   Third-party application installation.
    
-   Application management and uninstallation.
    
-   USB-based app package installation.
    
-   Custom launcher interface.
    

----------

# Installation

Installation is split into two stages: preparing the firmware on a PC and installing it on the vehicle.

## 1. PC Preparation

### Requirements

-   A Linux machine or Windows machine running WSL.
    
-   A USB flash drive formatted as FAT32.
    
-   The firmware update package for your vehicle.
    
-   The latest D2VLauncher release.
    

Native Windows and macOS builds are planned for the future.

### Step 1: Download D2VLauncher

Download the latest ZIP archive from the [Releases](../../releases) page and extract it to a directory of your choice.

The extracted directory should contain several files, including:

-   The firmware decryption tool.
    
-   `FirmwareSettings.json`
    
-   A folder containing the launcher binary and its `appconf` file.
    

### Step 2: Download Your Vehicle's Firmware

Visit the official [Hyundai Navigation Update website](https://update.hyundai.com/) and locate the update package for your specific vehicle.

Make sure you select the correct firmware for your vehicle and DAudio2 head unit.

Extract the downloaded update package.

You will typically find two ZIP files. For example:

```
enc_d2vsystem_package_134.100.240318.zip
enc_system_package_...
```

**You want the file containing** `**d2vsystem**` **in its name, not the one containing only** `**system**`**.**

Copy this file into the same directory as the firmware decryption tool.

### Step 3: Prepare the Modified Firmware

Open a terminal in the directory containing the decryption tool.

Run the following command:

```
sudo ./HyundaiFirmwareDecrypter -a -l "YOUR_FIRMWARE_FILENAME.zip"
```

Replace `YOUR_FIRMWARE_FILENAME.zip` with the actual name of your firmware package.

#### Command arguments

`-a`

Extracts and decrypts the firmware, calculates hashes, recompresses the package, and installs an ADB backdoor for remote root access over Wi-Fi.

`-l`

Includes the D2VLauncher application manager and launcher in the modified firmware.

The `-l` argument is optional if you only want root access without the launcher and application manager.

Wait for the tool to finish processing.

Once completed, you should have a file named:

```
system_update.zip
```

Copy this file to the **root directory** of a FAT32-formatted USB flash drive.

Your USB drive should now be ready for installation.

----------

## 2. Vehicle Installation

### Step 1: Connect the USB Drive

Take the prepared USB drive to your vehicle.

Connect it to the USB data port. This is usually the left USB port, marked for data connectivity rather than charging.

Start the vehicle's engine before proceeding. This helps prevent the battery from running flat during the firmware update.

### Step 2: Enter Engineering Mode

On the infotainment system, navigate to:

```
Setup > General
```

You should see a large update button near the bottom of the screen.

To access Engineering Mode:

1.  Quickly tap the area immediately to the left of the update button approximately five times.
    
2.  Immediately tap once to the right of the update button.
    
3.  Repeat if necessary until you hear a confirmation sound.
    

This can take a few attempts.

### Step 3: Enter the Engineering Mode Code

The Engineering Mode access code varies depending on the vehicle and installed software version.

Use this [Hyundai/Kia Engineering Mode guide](https://www.oviradio.cz/hyundai-kia-radio-engineering-mode-en/) to identify the appropriate code for your system.

#### Newer software: 8-digit codes

There is a workaround for some newer software versions that require an 8-digit code.

Setting the vehicle's date to **January 1, 2023** may allow you to use the following code:

```
26031236
```

On my vehicle, the codes are:

Engineering Mode: `26031236`

Variant Coding: `23060663`

Forced Update: `3600`

These codes are specific to my vehicle/software configuration and should not be assumed to work on every system.

### Step 4: Start the Firmware Update

The update procedure depends on which software generation your vehicle is running.

Generally:

-   **4-digit Engineering Mode code:** Older software.
    
-   **8-digit Engineering Mode code:** Newer software.
    

#### Older Software

Older software versions typically have a **Radio Update** option inside Engineering Mode.

1.  Select `Radio Update`.
    
2.  You will be presented with several update options.
    
3.  Select `Update All`.
    
4.  Follow the on-screen instructions to install the modified firmware from your USB drive.
    

#### Newer Software

Newer software versions may no longer expose the Radio Update option in Engineering Mode.

Instead, you can trigger the update using a button combination.

1.  Turn on the vehicle's radio/media system.
    
2.  Enter the following sequence using the volume knob and tune button:
    

```
Vol. 7 > Tune > Vol. 5 > Tune > Vol. 3 > Tune
```

3.  A popup should appear displaying **Forced Update** and the update filename found on your USB drive.
    
4.  Accept the update and allow the system to complete the installation.
    

**Do not disconnect the USB drive or interrupt power during the update.**

----------

# 3. Post Installation

If everything went smoothly and your head unit has rebooted back into its normal home screen, congratulations!

Your DAudio2 system should now have D2VLauncher installed.

### Launching D2VLauncher

The launcher can currently be accessed by opening the **Voice Memos** application on the head unit.

This launch method is temporary and may change in future releases. Not every vehicle configuration includes the Voice Memos application.

### Installing Applications

The launcher does not come with third-party applications preinstalled.

A test app package is included in the repository's [Releases](../../releases) section to help get you started.

D2VLauncher supports installing app package packages, allowing the community to begin developing and experimenting with applications for these head units.

This is where I hope the community can really help move the project forward!

----------

# 4. Bugs, Issues & Contributions

This project is still evolving, and there will inevitably be things that need improving.

If you encounter bugs, unexpected behavior, or compatibility issues, please report them through the repository's [Issues](../../issues) page.

When reporting an issue, please include:

-   Vehicle make, model, and year.
    
-   Head unit software version.
    
-   Whether you are running older or newer software.
    
-   A description of the issue.
    
-   Any relevant logs or screenshots.
    

Contributions, testing, and community-developed applications are all welcome!

----------

**Thanks to everyone who has contributed research, testing, and development to make projects like this possible.**

Special thanks to [greenluigi1](https://github.com/greenluigi1) for the original research and firmware decryption work that helped make D2VLauncher possible.# D2VLauncher

**A third-party launcher and application manager for Hyundai, Kia, and Genesis DAudio2 head units.**

D2VLauncher brings the ability to install and manage third-party applications on compatible DAudio2 infotainment systems.

This project builds on the research and work of [greenluigi1](https://github.com/greenluigi1), including his [HyundaiFirmwareDecrypter](https://github.com/greenluigi1/HyundaiFirmwareDecrypter) repository.


Special thanks to [greenluigi1](https://github.com/greenluigi1) for the original research and firmware decryption work that helped make D2VLauncher possible.
