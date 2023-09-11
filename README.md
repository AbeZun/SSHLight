# SSHLight
`SSHLight` is an auto script to turn on an LED Light when you SSH into a Linux computer and an Executible file to turn it off remotely.

This works using the uhubctl utility which can turn port power on and off from specific USB Hubs.

MORE INFO ON uhubctl
===================

For a full list of compatible USB Hubs and all things uhubctl check: https://github.com/mvp/uhubctl


Install Uhubctl On Linux
=========

First, you need to install library libusb-1.0 (version 1.0.12 or later, 1.0.16 or later is recommended):

* Ubuntu: `sudo apt-get install libusb-1.0-0-dev`

> :warning: On Linux, use `sudo` or configure USB permissions as described below!

To list all supported hubs:

    uhubctl 


To Test that `uhuctl` is talking to your usb hub use the following codes:

> :warning: Remember to substitute 20–5.2 with your device ID and and 3 with your port number. If all goes well, you should see the light turn off. To turn it back on again, replace off with on.

You can turn off the power on a USB port(s) like this:

    uhubctl -l 20-5.2 -a off -p 3

You can turn on the power on a USB port(s) like this:

    uhubctl -l 20-5.2 -a on -p 3


FIX USB PERMISSIONS ISSUE
=========
 > :warning: Otherwise you will need to run code with sudo everytime

a. In Terminal: Navigate to /etc/udev/rules.d

b. RUN: 

    sudo nano 52-usb.rules

--THIS WILL CREATE A FILE CALLED "52.usb.rules"

c. Paste The Following

    SUBSYSTEM=="usb", DRIVER=="usb", MODE="0666" 
    # Linux 6.0 or later (its ok to have this block present for older Linux kernels):SUBSYSTEM=="usb", DRIVER=="usb",\
    RUN="/bin/sh -c \"chmod -f 666 $sys$devpath/*-port*/disable || true\""

d. For the udev rule changes to take effect, RUN:

    sudo udevadm trigger --attr-match=subsystem=usb


OPTIONAL STEP
=========
> :warning: In order to not have to type in password everytime to exicute sudo via script, edit your "sudoers" file by doing the following:

STEP 1) RUN:

    sudo visudo

STEP 2) at the end of the file add the following line:
> :warning: #NOTE: REPLACE (username) with your computer username

    username ALL=(ALL) NOPASSWD:ALL

STEP 3) save file and exit editor 


Add Bash Script and save it to `/etc/profile.d/`
=====
> :warning: The will turn on Light at SSH Login.
1. Write SCRIPT:

    #!/etc/profile.d
	#TURN ON LIGHT WHEN SSH SESSION STARTS
	uhubctl -l 3-3 -a on -p 3

2. Restart Profile By Running:

    source /etc/profile.d

3. DONE! Now start a new SSH session to test.


TURN OFF LIGHT REMOTELY
=====

---- 1 ON MAC ----
1a. Open a terminal window and type:

    nano turnlightsoff.sh

2a. Here type the commaned to turn your light off
> :warning: (REMEMBER to replace "1-1.1" with your own usb hub id)

    uhubctl -l 1-1.1 -a off -p 3

3a. In a terminal window type:

     nano LIGHTSOFF.command

4a. Here type the command that will execute when clicked on and will run the bash script from your local client computer to your remote server:

    ssh username@address < /PATH/TO/THE/BASH/SCRIPT/turnlightsoff.sh

5a. --OPTIONAL-- change the icon and add it to your desktop for easy of access


---- 2 ON PC ----
1b. Open your favorite Text Editor (I Use VSCODE) and create a file called "turnlightsoff.sh"

2b. Here type the commaned to turn your light off:
> :warning: (REMEMBER to replace "1-1.1" with your own usb hub id)

    uhubctl -l 1-1.1 -a off -p 3

3b. In your text editor create another file called "LIGHTSOFF.bat".
> :warning: Here will write the code that will launch cmd, and run the bash script from your local client computer to your remote server.

    start cmd.exe /c "ssh username@address < "C:\PATH\TO\THE\BASH\SCRIPT\lightsoff.sh""

4b. --OPTIONAL-- create a shortcut of this bat file to your desktop and change its icon (can only be done on shortcuts) for ease of use.