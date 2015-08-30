### Requirements ###

* a machine running a recent Linux distro
* python 2.7
* tornado 3.1+
* jinja2
* PIL or pillow
* curl & pycurl
* motion (optional)
* ffmpeg (optional)
* v4l-utils (optional)

### The Motion Daemon ###

The `motion` daemon itself is optional, but needed in most cases. Install it (along with `ffmpeg` and `v4l-utils`) unless you configure a machine that will only act as a hub for other motionEye-based cameras.

There are three major versions of motion:

* The stable version (3.2.12 at the time of writing) is included with most distros, but is highly outdated and lacks features like RTSP camera support. The source code can be downloaded from [here](http://www.lavrsen.dk/foswiki/bin/view/Motion/DownloadFiles). It's important to note that this version uses an older configuration file format, incompatible with newer ones (supported by motionEye, nevertheless).

* The SVN version (r564 at the time of writing) lives in [this repository](http://www.lavrsen.dk/svn/motion/). The SVN version is newer and may perform better in some situations but seems to be less stable. The development here seems to be discontinued.

* Mr-Dave's version can be checked out [here](https://github.com/Mr-Dave/motion). This is probably the most up-to-date, featureful and stable version, itself being forked from [sackmotion repository](https://github.com/sackmotion/motion).

Read the [[Compiling Motion|Compiling-Motion]] page if you want to manually compile any of these versions.

### Install Instructions ###

motionEye releases are uploaded to [PyPI](https://pypi.python.org/pypi/motioneye/), so you can use the `pip` command to install it as well as (some of) its dependencies. Following are detailed instructions for some common distributions.

**note 1**: The given commands normally need to be run as root; type them in a root shell or use `sudo` before each command.
**note 2**: If you are configuring a motionEye system that will only act as a hub for other motionEye-based cameras (i.e. no locally connected cameras and no IP cameras), you can skip installing `motion`, `ffmpeg` and `v4l-utils`.

#### Debian ####

You'll need to add the following repo to your apt sources (required for `ffmpeg`):

    echo "deb http://www.deb-multimedia.org stable main non-free" >> /etc/apt/sources.list
    apt-get update

Install `motion`, `ffmpeg` and `v4l-utils`:

    apt-get install motion ffmpeg v4l-utils

Install the dependencies from the repositories:

    apt-get install python-dev libcurl4-openssl-dev

Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

    pip install motioneye

Prepare the configuration directory:

    mkdir -p /etc/motioneye
    cp /usr/local/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf

Add an init script, configure to run at startup and start the `motionEye` server:

*Debian 7, sysvinit-based*:

    cp /usr/local/share/motioneye/extra/motioneye.init-debian /etc/init.d/motioneye
    update-rc.d -f motioneye defaults
    /etc/init.d/motioneye start
 
*Debian 8, systemd-based*: 

    cp /usr/local/share/motioneye/extra/motioneye.systemd-unit /etc/systemd/system
    systemctl enable motioneye
    systemctl start motioneye

#### Raspbian ####

#### Fedora ####

#### Arch Linux ####

#### Other Distributions ####

#### Manual Download ####