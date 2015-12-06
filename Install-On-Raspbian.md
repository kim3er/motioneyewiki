### Before Proceeding
* Read the general [[Installation|Installation]] page first.
* These instructions apply only to Raspbian, the official Raspberry PI distro.
* All commands require *root*; use `sudo` before each command or become root using `sudo -i`.
* If you want to use the CSI camera module for the Raspberry PI, you need to add `bcm2835-v4l2` to `/etc/modules` and reboot.

### Instructions

1. As you probably know, `ffmpeg` is missing from the official Debian repos. Moreover, the variant offered  by *deb-multimedia.org* no longer works with Raspbian after recent updates. You can either compile it yourself (not recommended) or download this [prebuilt package](precompiled/ffmpeg_2.8.3.git325b593-1_armhf.deb) and install it:

        wget https://github.com/ccrisan/motioneye/wiki/precompiled/ffmpeg_2.8.3.git325b593-1_armhf.deb
        dpkg -i ffmpeg_2.8.3.git325b593-1_armhf.deb

2. Install `motion`:

        apt-get install motion ffmpeg

    **note 1**: `v4l-utils` appears to be preinstalled on Raspbian systems.

    **note 2**: A prebuilt version of Mr Dave's motion for Raspbian can be downloaded from [[here|precompiled/motion-mrdave-raspbian]].

    **note 3**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]] instead of installing it using `apt-get`. Also make sure to configure the build using `--with-ffmpeg=/usr/lib/arm-linux-gnueabihf --with-ffmpeg-headers=/usr`.

3. Install the dependencies from the repositories:

        apt-get install python-pip python-dev libssl-dev libcurl4-openssl-dev libjpeg-dev

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

5. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

6. Prepare the media directory:

        mkdir -p /var/lib/motioneye

7. Add an init script, configure it to run at startup and start the `motionEye` server:

        cp /usr/local/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
        chmod +x /etc/init.d/motioneye
        update-rc.d -f motioneye defaults
        /etc/init.d/motioneye start
 