# Octoprint on armbian (PCDUINO3)

Mostly the same as on raspbian but with additional requirement of manually installing the 
global virtualenv package (not automatically installed with python-virtualenv). 

__It is assumed you have set up your OS with a user named "pi" to keep as close as possible to the Raspbian install. 
If not, substitute your own username where the "pi" user is present.__

### Install:

    cd ~
    sudo apt-get update && sudo apt-get -y install python-pip python-dev python-setuptools python-virtualenv virtualenv git libyaml-dev build-essential
    git clone --depth 1 https://github.com/foosel/OctoPrint.git
    cd OctoPrint
    virtualenv venv
    ./venv/bin/pip install pip --upgrade
    ./venv/bin/python setup.py install
    mkdir ~/.octoprint

### Enable serial port access to the pi user and passwordless shutdown

    sudo usermod -a -G tty pi
    sudo usermod -a -G dialout pi
    
`sudo nano /etc/sudoers.d/octoprint-shutdown`

and add:
`pi ALL=NOPASSWD: /sbin/shutdown`

Since armbian requires a password for everything, a more comprehensive sudoers file would be

`sudo nano /etc/sudoers.d/piuser`

and add:

    pi ALL=NOPASSWD: /sbin/shutdown
    pi ALL=NOPASSWD: /usr/sbin/service octoprint
    pi ALL=NOPASSWD: /usr/bin/apt-get
    pi ALL=NOPASSWD: /bin/cp
    
(Log out and log back in to activate the change)

### Then activate the virtual environment and start octoprint:

    source venv/bin/activate
    octoprint serve

### To install the service:

Edit `~/OctoPrint/scripts/octoprint.default` and change the DAEMON path to match install path e.g. `DAEMON=/home/pi/OctoPrint/venv/bin/octoprint`

    sudo cp ~/OctoPrint/scripts/octoprint.init /etc/init.d/octoprint
    sudo chmod +x /etc/init.d/octoprint
    sudo cp ~/OctoPrint/scripts/octoprint.default /etc/default/octoprint
    sudo update-rc.d octoprint defaults

OctoPrint can now be controlled via 
`sudo service octoprint {start|stop|restart}`

### Mjpeg streamer:

_Side note: At the time of writing this, the octoprint wiki calls for the installation of libjpeg8-dev, Armbian threw an error that this package wasn't available, libjpeg62-turbo-dev was substituted in its place_

    cd ~
    sudo apt-get -y install subversion libjpeg62-turbo-dev imagemagick libav-tools cmake
    git clone https://github.com/jacksonliam/mjpg-streamer.git
    mv mjpg-streamer/mjpg-streamer-experimental/* mjpg-streamer/
    rm -r mjpg-streamer/mjpg-streamer-experimental
    cd mjpg-streamer
    export LD_LIBRARY_PATH=.
    make

Add user to the video group to access the /dev/video device
`sudo usermod -a -G video pi`

Start mjpeg streamer
`./mjpg_streamer -i "./input_uvc.so" -o "./output_http.so"`
