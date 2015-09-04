### Why Compile Motion? ###

The stable version (3.2.12 at the time of writing), which is included with most distros, is highly outdated and lacks features like RTSP camera support. There are various newer versions and forks, some of them more stable and featureful than others. Two notable variants are:

* the [SVN](http://www.lavrsen.dk/svn/motion/) version
* [Mr Dave's fork](https://github.com/Mr-Dave/motion) (recommended)

The following instructions should normally work with any variant of motion you may find online.

### Requirements ###

* a Linux machine with [`gcc`](https://gcc.gnu.org/) and [`automake`](http://www.gnu.org/software/automake/)
* [`libjpeg`](http://libjpeg.sourceforge.net/)
* a relatively recent version of [`ffmpeg`](https://www.ffmpeg.org/)
* [`subversion`](https://subversion.apache.org/) and/or [`git`](https://git-scm.com/)

**On a Debian-based distro**, the following command installs all the requirements (as root):

    aptitude install build-essential libjpeg-dev libavformat-dev libavcodec-dev libswscale-dev ffmpeg subversion git

    **note**: Plain Debian systems require the *deb-multimedia* repo for `ffmpeg`. See [Install On Debian](https://github.com/ccrisan/motioneye/wiki/Install-On-Debian), the first step, for more details.

**On Arch**, one would run (as root):

    pacman -S base-devel libjpeg ffmpeg subversion git

**On Fedora**, make sure to add the *rpmfusion* repo to your system (see [Install On Fedora](https://github.com/ccrisan/motioneye/wiki/Install-On-Fedora), the first step, for details). Then run following command (as root):

    dnf install automake jibjpeg ffmpeg

### Getting The Source ###

If you decide to go with a SVN-based version, use the following command to check out the source code (the latest official SVN revision is given as example):

    svn co http://www.lavrsen.dk/svn/motion/trunk/ motion-svn

For git-based forks, use the following command (Mr Dave's fork is used as example):

    git clone https://github.com/Mr-Dave/motion.git motion-mrdave

