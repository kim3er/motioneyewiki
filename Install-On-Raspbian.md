### Before Proceeding
* Read the general [[Installation|Installation]] page first.
* These instructions apply only to an up-to-date Raspbian, the official Raspberry PI distro.
* All commands require *root*; use `sudo` before each command or become root using `sudo -i`.
* If you want to use the CSI camera module for the Raspberry PI, you need to add `bcm2835-v4l2` to `/etc/modules` and reboot.

### Instructions

1. As you probably know, `ffmpeg` is missing from the official Debian repos. Moreover, the variant offered  by *deb-multimedia.org* no longer works with Raspbian after recent updates. You can either compile it yourself (not recommended) or download this [prebuilt package](precompiled/ffmpeg_3.1.1-1_armhf.deb) and install it:

        wget https://github.com/ccrisan/motioneye/wiki/precompiled/ffmpeg_3.1.1-1_armhf.deb
        dpkg -i ffmpeg_3.1.1-1_armhf.deb

    **note**: If you have previously added the *deb-multimedia* repo to your system and installed their version of ffmpeg, you'll need to remove the repo from your apt sources and run the following commands to reinstall the official version of some *libav* libraries:

        apt-get remove libavcodec-extra-56 libavformat56 libavresample2 libavutil54
        apt-get install libavutil54 libavformat56 libswscale3

2. Install `motion`:

        wget https://github.com/ccrisan/motioneye/wiki/precompiled/motion-mrdave-raspbian -O /usr/local/bin/motion
        chmod +x /usr/local/bin/motion

    **note 1**: The motion version provided by the official repos is too old and not recommended.

    **note 2**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]].

3. Install the dependencies from the repositories:

        apt-get install python-pip python-dev curl libssl-dev libcurl4-openssl-dev libjpeg-dev

    **note**: `v4l-utils` appears to be preinstalled on Raspbian systems; if it isn't, please install it

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
