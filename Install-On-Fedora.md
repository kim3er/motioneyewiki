### Before Proceeding
* Read the general [[Installation|Installation]] page first.
* These instructions apply to relatively recent Fedora distributions. Depending on the release, you'll need to replace `dnf` with the older `yum` command.
* All commands require *root*; use `sudo` before each command or become root using `sudo -i`.

### Instructions

1. Make sure you have [rpmfusion](http://rpmfusion.org/) added to your system (required for both `ffmpeg` and `motion`):

        dnf install --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

2. Install `motion`, `ffmpeg` and `v4l-utils`:

        dnf install motion ffmpeg v4l-utils

3. Install the dependencies from the repositories:

        apt-get install python-pip python-devel gcc libcurl-devel

    **note**: Python 2.7 is required. If your system still runs Python 2.6 or older, please upgrade.

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

        pip install motioneye

5. Prepare the configuration directory:

        mkdir -p /etc/motioneye
        cp /usr/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

6. Prepare the media directory:

        mkdir -p /var/lib/motioneye

7. Add an init script, configure it to run at startup and start the `motionEye` server:

            cp /usr/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system/motioneye.service
            systemctl daemon-reload
            systemctl enable motioneye
            systemctl start motioneye

8. To upgrade to the newest version of motionEye, just issue:

        pip install motioneye --upgrade
        systemctl restart motioneye
