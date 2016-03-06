### Before Proceeding
* Read the general [[Installation|Installation]] page first.
* These instructions apply to Ubuntu distributions. They may work for other Ubuntu derivatives (such as *Linux Mint*) but they haven't been tested.
* All commands require *root*; use `sudo` before each command or become root using `sudo -i`.

### Instructions
1. **Skip this step for Ubuntu 15.04 or newer**. For older Ubuntu versions, you'll need to add a PPA to your apt sources, in order to install `ffmpeg`:

        add-apt-repository -y ppa:kirillshkrogalev/ffmpeg-next
        apt-get update

2. Install `motion`, `ffmpeg` and `v4l-utils`:

        apt-get install motion ffmpeg v4l-utils

    **note**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]] instead of installing it using `apt-get`.

3. Install the dependencies from the repositories:

        apt-get install python-pip python-dev curl libssl-dev libcurl4-openssl-dev libjpeg-dev

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

5. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

6. Prepare the media directory:

        mkdir -p /var/lib/motioneye

6. Add an init script, configure it to run at startup and start the `motionEye` server:

    * Ubuntu 14.10 or earlier, upstart-based:

            cp /usr/local/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
            chmod +x /etc/init.d/motioneye
            update-rc.d -f motioneye defaults
            /etc/init.d/motioneye start
 
    * Ubuntu 15.04 or later, systemd-based:

            cp /usr/local/share/motioneye/extra/motioneye.systemd-unit-local /etc/systemd/system/motioneye.service
            systemctl daemon-reload
            systemctl enable motioneye
            systemctl start motioneye

7. To upgrade to the newest version of motionEye, just issue:

            pip install motioneye --upgrade

    * Ubuntu 14.10 or earlier:
            
            service motioneye restart

    * Ubuntu 15.04 or later:

            systemctl restart motioneye
