**note**: These instructions apply to Debian and Debian-based distributions. For [[Ubuntu|Install-On-Ubuntu]] and [[Raspbian|Install-On-Raspbian]] however, you should check out their specific instructions.

1. You'll need to add the following repo to your apt sources, replacing [name] with `wheezy` (Debian 7) or `jessie` (Debian 8). This is required for `ffmpeg`:

        echo "deb http://www.deb-multimedia.org [name] main non-free" >> /etc/apt/sources.list
        apt-get update

2. Install `motion`, `ffmpeg` and `v4l-utils`:

        apt-get install motion ffmpeg v4l-utils

    **note**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]] instead of installing it using `apt-get`.

3. Install the dependencies from the repositories:

        apt-get install python-pip python-dev libssl-dev libcurl4-openssl-dev

    **note**: Python 2.7 is required. If your system still runs Python 2.6 or older, please upgrade.

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

5. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

6. Add an init script, configure it to run at startup and start the `motionEye` server:

    * Debian 7, sysvinit-based:

            cp /usr/local/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
            update-rc.d -f motioneye defaults
            /etc/init.d/motioneye start
 
    * Debian 8, systemd-based:

            cp /usr/local/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system/motioneye.service
            systemctl daemon-reload
            systemctl enable motioneye
            systemctl start motioneye