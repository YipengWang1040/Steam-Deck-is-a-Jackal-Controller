# Steam-Deck-is-a-Jackal-Controller

## Introduction
[Steam Deck](https://en.wikipedia.org/wiki/Steam_Deck) is a handheld console released by [Valve](https://en.wikipedia.org/wiki/Valve_Corporation). 
Unlike other devices with similar form factor, Steam Deck features a quad-core X86 CPU, 16 GB RAM, and upgradable NVMe storage (m2. 2230), 
giving it similar performance as an [Intel NUC 11](https://www.intel.com/content/www/us/en/products/sku/205608/intel-nuc-11-pro-mini-pc-nuc11tnkv7/specifications.html).

It also features a touch screen, 2 touchpads, 8 customizable buttons, as well as a PS-style controller interface, and 5+ hours of battery life, which makes it almost the best controller for Jackal and similar UAVs. 


## Configure Steam Deck

### Install Ubuntu and Windows
#### Install Ubuntu
The architecture of Steam Deck is similar to a normal laptop. Thus Ubuntu can be installed normally using a bootable USB device with no issue.
Since Steam Deck is a relatively new device, thus make sure to use a newer release of Ubuntu (20.04.5 above, or 22.04.1 above).
Also, please **make sure** to wipe out the Steam OS entirely while installing the new OS.

Currently, Steam Deck has no audio driver under Ubuntu 20. There is a [workaround](https://gitlab.com/open-sd/acp5x-ucm-files) but requires kernel version 6.1+, which is not recommended for our purpose.

#### Instal Windows (optional)
After installing Ubuntu, you may also install Windows just like a normal dual-boot computer. There are [proper drivers](https://store.steampowered.com/news/app/1675200/view/3131696199122435099) for Windows. 

#### Install Steam OS Back (not recommended)
Currently, Steam OS will only begin its installation if the internal SSD has no other OS installed, which requires you to wipe out the internal SSD entirely. 
It is possible to install Steam OS first then other OS, but the unstoppable upgrade of Steam OS has a chance to corrupt all other OS, sometimes even itself.

Thus, it's not recommended to have Steam OS installed while you are using this device as a computer. 
If you really need Steam OS, having it on another SSD and swapping it back may save you some time.

#### Boot Into Specific OS
Hold the `volume -` button and power button until you hear the beep sound, then release both buttons. The system will boot into a OS select window.

### Configure Buttons, Touchpads, and Joysticks
[SC-Controller](https://github.com/kozec/sc-controller) is an open-source tool that allows you to configure all keys on Steam Deck with a friendly GUI.
It also implements a Steam Deck-style virtual keyboard, which allows you to type using two touchpads. 
Each button has 3 actions: click, press, and double-click, while each action can be bound to any key combination or even macro.

A customized configuration for Jackal can be found [here](Jackal.sccprofile). You can simply import it to SC-Controller.

### Control Jackal

#### Connect to Jackal
It's recommended to configure the built-in WIFI card of Jackal to a WIFI hotspot, then plug the antenna on both sides to strengthen the signal. 
Steam Deck can then connect to the hotspot to connect to Jackal. 

It is also possible to use another hotspot to connect Jackal and Steam Deck, 
but you must correctly route them so that Steam Deck can access Jackal's remote ROS core or ssh into Jackal

#### Send Control Command
Jackal uses the package `jackal_control` by default, which mainly launches 3 other packages:

- `joy_node` to read controller input from `/dev/input/*` and publish it to `/joy`
- `teleop_twist_joy` to convert `joy` to velocity command based on configuration and publish it to `/cmd_vel`
- `controller_manager` to control Jackal based on `/cmd_vel`

If a computer can connect to the ROS core on Jackal, it can control Jackal by either having `joy_node` running locally and publishing messages to `/joy`, 
or having both `joy_node` and `teleop_twist_joy` running locally and publishing messages to `/cmd_vel`. Since the configuration of each controller may be different, 
the latter option is recommended.

A modified version of `teleop_twist_joy` for Steam Deck can be found [here](/sd_ws/teleop_twist_joy_adjust). It achieves smooth motion and avoids too high response rate on Steam Deck.
