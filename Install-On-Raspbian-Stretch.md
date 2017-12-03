### Before Proceeding
* Read the general [[Installation|Installation]] page first.
* These instructions apply only to an up-to-date Raspbian Stretch. If you're running Jessie, read the [[Jessie|Install-On-Raspbian]] guide.
* All commands require *root*; use `sudo` before each command or become root using `sudo -i`.
* If you want to use the CSI camera module for the Raspberry PI, you need to add `bcm2835-v4l2` to `/etc/modules` and reboot.

### Instructions

1. Ensure you have the latest version of Stretch. Some libraries weren't included/up-to-date in the intital release.

        apt-get update
        apt-get dist-upgrade

2. Install the dependencies from the repositories:

        apt-get install libssl-dev libcurl4-openssl-dev libmariadbclient18 libpq5 mysql-common ffmpeg libjpeg-dev python-pip

3. Install `motion`:

        wget https://github.com/Motion-Project/motion/releases/download/release-4.0.1/pi_stretch_motion_4.0.1-1_armhf.deb
        dpkg -i pi_stretch_motion_4.0.1-1_armhf.deb

    **note**: All official precompiled binaries of motion can be found [here](https://github.com/Motion-Project/motion/releases/).

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

5. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

6. Prepare the media directory:

        mkdir -p /var/lib/motioneye

7. Add an init script, configure it to run at startup and start the `motionEye` server:

        cp /usr/local/share/motioneye/extra/motioneye.systemd-unit-local /etc/systemd/system/motioneye.service
        systemctl daemon-reload
        systemctl enable motioneye
        systemctl start motioneye

8. To upgrade to the newest version of motionEye, just issue:

        pip install motioneye --upgrade
        systemctl restart motioneye
