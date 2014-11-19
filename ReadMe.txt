Create a UFOcoin Full Node


This instruction is to create a UFOcoin full node on a raspberry Pi.
You will need the following things before we start
* Raspberry Pi Model B, or B+
* Minimum 8Gb class 10SD card, but I recommend a 16Gb Note: You may even get away with a 4GB card but I recommend against it
* Monitor, keyboard and mouse to plug into the Pi for Initial setup (Not needed after setup is complete
* You need to read up on your router and find out how to make it statically assign an IP address to the Raspberry Pi and how to set up port forwarding on it for both inbound and outbound traffic on port 9336
* You need to be able to SSH from your main computer to the PI to check it. If you have never done this before you will need to download and install Putty from here http://www.putty.org/
* To use Putty just start it and enter the IP address your router has assigned the Pi and make sure port 22 is entered into the port field
* To work out what IP address your router has assigned the Pi you can either open up lxterminal when the pi first booted up and type in ifconfig and it will give you the Pi’s IP address or you can get this by logging into your router




I will not go into the detail on how to do a standard install of Raspbian off Noobs as the Raspberry Pi website gives great instruction on how to do this.
if you do not know where to download Noobs from you get it here.
http://www.raspberrypi.org/downloads/
This link is to the howto to install Noobs onto the SD card
http://www.raspberrypi.org/help/noobs-setup/


The few extra things you should do over a standard install is
* Change the password from the default password for extra security. 
* I also recommend doing minor overclocking to 800Mhz.
* Don’t forget to enable SSH during setup
* I recommend changing the memory split from 64 to only 32Meg for the video to give more memory to the CPU
* Don’t forget to select your timezone
* Consider changing the hostname if you already have more than one Pi on your network

Set up Rasbian as per the instructions from the Raspberry Pi website and SSH into the Pi.
Note this script will download the source from github and compile it on the Pi, it will take about 4 hours to compile

To Download everything off github onto your Pi use the following command

sudo git clone https://github.com/tmuir12/UFO_RaspBerry_Pi_Full_Node

Now enter the directory UFO_RaspBerry_Pi_Full_Node

cd UFO_RaspBerry_Pi_Full_Node/

We now need to make the script executable by typing

sudo chmod 755 ufo_node.sh

Now we need to start the install script by typing.
./ufo_node.sh


This will 
* Update the Pi to the latest version
* Install needed applications to run ufod, 
* Set up a firewall, that will allow ssh in and feathercoind to talk to other clients
* Create a conf file with input from you to select a password for RDC
* create an /etc/init.d entry to start ufod as using ‘pi’ at boot

You are now on the home stretch.

You now need to log into your router and statically assign the IP address to the Pi and enable port forwarding to this IP address both inbound and outbound on port 9336.
You will need to read your router's manual to find out how to do this.

Once you have set up your router its time to test it.
Type sudo reboot
Once the Pi has booted back up SSH back into it
Give it 2 or 3 minutes for the UFOcoin daemon to start up and then to check it go


ufod getinfo

and you should be presented something like this

{
    "version" : 80801,
    "protocolversion" : 60005,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "blocks" : 284218,
    "timeoffset" : 0,
    "connections" : 0,
    "proxy" : "",
    "difficulty" : 1.28399295,
    "testnet" : false,
    "keypoololdest" : 1415746250,
    "keypoolsize" : 101,
    "paytxfee" : 0.00000000,
    "mininput" : 0.00001000,
    "errors" : ""
}

If you get an error when trying the above command wait another couple of minutes and try again as it does take a few minutes for feathercoind daemon to fully start up.
Whilst it is catching up it will use about 90% of the CPU but once it has caught up it will only use 2% or 3%
It will take around 20 hours to catch up if you are downloading the whole blockchain as the Pi is slower at doing this than your PC

You can also look at installing apache mysql and PHP and install a web front end to let you see what is happening by your web browser if you want, but don't forget to open up port 80 on the Pis firewall if you do.


Troubleshooting
If after rebooting ufod doesn't appear to be running try the command

sudo /etc/init.d/ufocoin start

You will be prompted for user pi password deatils before it will start and if it doesn't start will give you an error message
explaining why.


