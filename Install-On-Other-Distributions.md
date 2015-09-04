**note**: If your distribution is based on one of the "officially supported" distros listed in the [[Installation|Installation#install-instructions]] article, you should probably follow those instructions instead.

1. Unless you configure your machine for a remote cameras-only hub, make sure you have `motion`, `ffmpeg` and `v4l-utils` installed on your system. This step depends on the distro you're using.

2. Make sure you have Installed a [`python 2.7`](https://www.python.org/) interpreter, [`pip`](https://pip.pypa.io/en/stable/installing.html), [`libcurl`](http://curl.haxx.se/libcurl/) and [`libjpeg`](http://libjpeg.sourceforge.net/).

3. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

4. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

    **note**: The sample file path may differ. If the one above is wrong, you may try `/usr/local/share/motioneye/extra/motioneye.conf.sample`.

5. Add an init script, configure it to run at startup and start the `motionEye` server:

    * sysvinit-based:

            cp /usr/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
            chmod +x /etc/init.d/motioneye
            update-rc.d -f motioneye defaults
            /etc/init.d/motioneye start

        **note**: If at step (4) you used the path starting with `/usr/local`, you'll probably need to adjust this one as well.
 
    * systemd-based:

            cp /usr/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system/motioneye.service
            systemctl daemon-reload
            systemctl enable motioneye
            systemctl start motioneye

        **note**: If at step (4) you used the path starting with `/usr/local`, you'll probably need to adjust this one as well; also, use the `motioneye.systemd-unit-local` instead.

    * `/etc/rc.local` or other simple startup script:

        Just make sure to run the following command:

            meyectl startserver -b -c /etc/motioneye/motioneye.conf