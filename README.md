# Start Debian based Linux machine on LAN/WAN
Start your pc, laptop, server remotely on your network or on internet Wake On LAN (WOLAN) / Wake On WAN (WOWAN).

# LAN part

## On the machine you want to wake up

### BIOS SETUP

First you need to setting your BIOS.

Each BIOS is built different so take your time.

#### To acces BIOS

If fastboot option on the BIOS was disable you might saw the button to press to enter BIOS

Currently it's F2 or F10 or FX with X a number defined be the constructor so check out the data sheet of your pc or just try every Fn button

#### DISABLE DEEP-SLEEP

If your bios have a deep-sleep parameter disable it.

#### ENABLE WOLAN

Look for Wake on LAN and enable it.
Maybe you have a laptop and want to WOLAN when power cable is unplugged so enable WOLAN 

### OS SETUP

Type all the command in SuperUser or with sudo at the beginning

#### Get the MAC address also call ethernet address

MAC or ethernet address is an address of a network card (layer 2 of the OSI model).

To get this address I personaly use

    ifconfig
You can get this command by adding net-tools

    apt-get install net-tools

From there you can see your networks cards 

*lo* for loopback we don't need the information for this card.

The other card should be call *ethX* with X a number or *enpXsY* with X and Y two numbers.

But you search only an active card so search *RUNNING*, example :

    eth0: flags=xxxx<UP,BROADCAST,**RUNNING**,MULTICAST>
this card should have a MAC address ( ether ... xx:xx:xx:xx:xx:xx) write down this address.

#### Get your network card ready for WOLAN

Install ethtool.

    apt-get install ethtool
Activate WOLAN of your network interface, here enp4s0.

    ethtool -s enp4s0 wol g
#### Prepare the WOLAN service.

A service also call deamon is a background program, 
the WOLAN service will listen the port 9 in UDP to WOLAN if the pc get the request

First create wol.service file like this :

    touch /etc/systemd/system/wol.service
Then edit this file with nano for exemple :

    nano /etc/systemd/system/wol.service
Write down :

    [Unit]
    Description=Configure Wake On Lan

    [Service]
    Type=oneshot
    ExecStart=/sbin/ethtool -s enp4s0 wol g

    [Install]
    WantedBy=basic.target
To save with nano Ctrl o.

To exit with nano Ctrl x.

reload the sercive

    systemctl daemon-reload
activate it

    systemctl enable wol.service
Then reboot

    reboot

## On other PC
Type all the command in SuperUser or with sudo at the beginning

Install wakeonlan.

    apt-get install wakeonlan
To WOLAN the pc target, shutdown the pc target by typing on his terminal :

    shutdown -h now
On your other pc type and replace xx:xx:xx:xx:xx:xx by the MAC address of your target pc:
 
    wakeonlan xx:xx:xx:xx:xx:xx
# WAN part
Connect to your router, get the external IPv4 address (exemple of IPv4 address 84.127.245.90) write down this address.

Search your target machine, once you have find it thanks to MAC address write out his name.

Then search NAT/PAT settings, create new rule :
* app/service : new (call it WOWAN for exemple)
* internal port : 9
* external port : 9
* protocole : UDP
* device : (your target divice)

To WOWAN the pc target, shutdown the pc target by typing on his terminal :

    shutdown -h now
On your other pc you need to be on an other distant network, 
type and replace yyy.yyy.yyy.yyy by the IPv4 address of your 
router and xx:xx:xx:xx:xx:xx by the MAC address of your target pc:
 
    wakeonlan -i yyy.yyy.yyy.yyy xx:xx:xx:xx:xx:xx
