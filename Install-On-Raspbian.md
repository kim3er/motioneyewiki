**note**: These instructions apply only to Raspbian, the official Raspberry PI distro.

1. Install `motion`, `ffmpeg`:

        apt-get install motion ffmpeg v4l-utils

    **note 1**: `v4l-utils` appears to be preinstalled on Raspbian systems.
    **note 2**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]] instead of installing it using `apt-get`.

2. Install the dependencies from the repositories:

        apt-get install python-pip python-dev libssl-dev libcurl4-openssl-dev

3. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

4. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

5. Add an init script, configure it to run at startup and start the `motionEye` server:

        cp /usr/local/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
        chmod +x /etc/init.d/motioneye
        update-rc.d -f motioneye defaults
        /etc/init.d/motioneye start
 
