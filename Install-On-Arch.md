**note**: These instructions apply to Arch Linux. They may work for other Arch-based distributions but the procedure hasn't been tested.

1. Install `motion`, `ffmpeg` and `v4l-utils`:

        pacman -S motion ffmpeg v4l-utils

    **note**: For other versions of `motion` check out [[Compiling Motion|Compiling-Motion]] instead of installing it using `pacman`.

2. Install the dependencies from the repositories:

        pacman -S python2-pip base-devel

3. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip2 install motioneye

4. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

5. Prepare the media directory:

        mkdir -p /var/lib/motioneye

6. Add an init script, configure it to run at startup and start the `motionEye` server:

        cp /usr/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system/motioneye.service
        systemctl daemon-reload
        systemctl enable motioneye
        systemctl start motioneye