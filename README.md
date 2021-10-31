# Start Linux machine on LAN/WAN
Start your pc, laptop, server remotely on your network or on internet Wake On LAN (WOLAN) / Wake On WAN (WOWAN)

# On the machine you want to wake up

## BIOS SETUP

First you need to enable and disable some parameters in the bios each BIOS is built different so take your time

### To acces BIOS

If fastboot option on the BIOS was disable you might saw the button to press to enter BIOS
Curently it's F2 or F10 or Fn with n a number defind be the cunstructor so check out the data sheet of your pc or just try every Fn button

### DISABLE DEEP-SLEEP

If your bios have a deep-sleep parameter disable it.

### ENABLE Wake On LAN

Look for Wake on LAN and enable it.
Maybe you have a laptop and want to wake on LAN when power cable is unplugged so enable WOLAN

## OS SETUP



# On other PC
In SuperUser or with sudo.
* *Ubuntu*
    apt-get install wakeonlan

* *Fedora*
    dnf install wakeonlan
