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

#### Debian/Ubuntu ####

#### Fedora ####

#### Arch Linux ####

#### Other Distributions ####

#### Manual Download ####
