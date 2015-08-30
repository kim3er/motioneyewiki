1. You'll need to add the following repo to your apt sources (required for `ffmpeg`):

        echo "deb http://www.deb-multimedia.org stable main non-free" >> /etc/apt/sources.list
        apt-get update

2. Install `motion`, `ffmpeg` and `v4l-utils`:

        apt-get install motion ffmpeg v4l-utils

3. Install the dependencies from the repositories:

        apt-get install python-dev libcurl4-openssl-dev

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

            cp /usr/local/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system
            systemctl enable motioneye
            systemctl start motioneye

#### Raspbian ####

#### Fedora ####

#### Arch Linux ####

#### Other Distributions ####

#### Manual Download ####