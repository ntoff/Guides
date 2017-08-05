OctoPi
=

The easiest way to get started using OctoPrint is by downloading the precompiled image from [http://octoprint.org/download/](http://octoprint.org/download/), designed for use an a Raspberry Pi. 

# Install

The image is written to an SD card in the same way you would write any other Raspberry Pi image to an SD card (See [here](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) for details. The OctoPi image comes with everything you need, preinstalled and preconfigured. 

*Please note, the first boot will take longer than usual while the partition is expanded to fill the entire SD card and may automatically reboot. Please do not interrupt this process.*

# Setup

**WiFi**

To set up wireless connectivity, you will need to edit `octopi-network.txt` found in the root of the SD card when accessed as a mass storage device, or in /boot when logged in to the Pi via SSH (you will obviously need to plug the Raspberry Pi into an ethernet port on your router to set up wifi this way).

To configure for wifi, uncomment (remove the # marks) from the lines that apply to your setup in this section of `octopi-network.txt`. 

    ## WPA/WPA2 secured
    #iface wlan0-octopi inet manual
    #    wpa-ssid "put SSID here"
    #    wpa-psk "put password here"

    ## WEP secured
    #iface wlan0-octopi inet manual
    #    wireless-essid "put SSID here"
    #    wireless-key "put password here"

    ## Open/unsecured
    #iface wlan0-octopi inet manual
    #    wireless-essid "put SSID here"
    #    wireless-mode managed

For example, a WPA2 wifi connection might look something like this:

    ## WPA/WPA2 secured
    iface wlan0-octopi inet manual
        wpa-ssid "myssid"
        wpa-psk "mywpakeyhere"

Take note that you must also uncomment the `#iface wlan0-octopi inet manual` line, as well as the ssid and key configuration lines.

**IP address / host name**

If your computer supports [bonjour](https://en.wikipedia.org/wiki/Bonjour_(software)) / [zeroconf](https://en.wikipedia.org/wiki/Zero-configuration_networking) then you can find your OctoPi device at `octopi.local`, if your computer does not support these functions, you will need the IP address that was assigned to it by your router. See your router's help pages for information on how to access address assignment information.

**SSH Password**

*For an SSH Client, Windows users can use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), OSX/\*nix users may find they already have an SSH client installed. See your OS help files / man pages for details.*

Once your OctoPi device is connected to the network and you have it's IP address (or are able to access it via `octopi.local`) you must change the default SSH password. Log in to the OctoPi device using the default username `pi` and the password `raspberry` (both are case sensitive), and use the `passwd` command to enter a new password. Make it something secure, especially if others may have access to your device as this password gives anyone full access to your device.
