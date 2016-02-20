# Box: ms_1

This box sets up a minecraft server on Ubuntu 14.04 and screen.


## Usage

* ssh to server
* Attach to screen: `screen -r mcscreen`
* Work with minecraft server console
* When done detach from screen: `Ctrl-A, followed by D`
* Exit ssh session: `exit`


## Setup

Follow these [instructions](http://39digits.com/create-a-minecraft-server-on-digitalocean/).


## How to provision a box

* Set up an Ubuntu 14.04 64bit bare box:
    * DigitalOcean
        * 1GB droplet
        * Ubuntu 14.04
* Record the following:
    * root password
    * IP address
* Provision the box using sunzi
    * Switch into sunzi dir
          cd minecraft_server/boxes/ms_1/sunzi
    * Review the settings in sunzi.yml and adjust as needed
    * Execute sunzi script
        * If you use root
              sunzi deploy root@52.5.94.110
        * If you don’t use root
              sunzi deploy ubuntu@52.5.94.110 --sudo
      Enter root password if/when prompted. May not be necessary if you use public key.
    * Follow further instructions printed at the end of sunzi run


## Screen

This setup uses screen for the minecraft server process so that Mikko can attach to the process and issue minecraft commands on the server console, and can detach when his computer goes to sleep without killing the minecraft server process.

~~~ bash
#!/bin/bash

# Some reminder text for the user...
echo "Welcome to the simple Minecraft screen script."
echo "----------------------------------------------"
echo " If you run this and no screen instance exists,"
echo " one will be created and you will be transferred"
echo " into that screen instance."
echo " "
echo " To leave and return to the terminal prompt (i.e. detach)"
echo " press:  Ctrl-A, followed by D"
echo "----------------------------------------------"
echo " To shutdown your instance of Minecraft Server,"
echo " switch to the Minecraft screen instance"
echo " by typing: screen -r mcscreen"
echo " from anywhere and type stop"
echo "----------------------------------------------"

# Run a check for an already running minecraft screen session
RUNNING=$(pgrep -f "mcscreen" | wc -l)

# DEBUG. UNCOMMENT TO SEE VALUE
# echo $RUNNING

# If there is no detached screen running, create one and switch
# Else default to switching to the Minecraft screen instance.
if [ $RUNNING -eq 0 ]
then
    echo "Minecraft screen not yet running - starting one now..."
    echo " "
    read -p "Press any key to initiate..." -n 1

    screen -S mcscreen java -Xms768M -Xmx900M -jar minecraft_server.jar nogui
else
    echo "An instance of Minecraft screen is already running."
    echo " "
    echo "To detach from inside screen:  Press Ctrl-A followed by D"
    read -p "Press any key to switch now. " -n 1

    screen -r mcscreen
fi
~~~
