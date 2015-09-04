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

    dnf install automake libjpeg ffmpeg

### Getting The Source ###

If you decide to go with a SVN-based version, use the following command to check out the source code (the latest official SVN revision is given as example):

    svn co http://www.lavrsen.dk/svn/motion/trunk/ motion-svn

For git-based forks, use the following command (Mr Dave's fork is used as example):

    git clone https://github.com/Mr-Dave/motion.git motion-mrdave

### Configure Your Build ###

There are some features (mostly related to databases) that are not used with motionEye; we disable support for those components when running the *configure* script:

    cd [your-motion-dir]
    ./configure --prefix=/usr --without-pgsql --without-sdl --without-sqlite3 --without-mysql

**note**: Replace `[your-motion-dir]` with the directory where you have downloaded the source code (e.g. `motion-svn` or `motion-mrdave`).

### Compile Motion

Just run `make` in the motion directory (add `-j N` to start a parallel make):

    make

### Install Motion

Luckily motion is a simple binary that can be copied wherever you find appropriate on your filesystem. The recommended place is `/usr/local/bin` where it will supersede any version of motion installed from official repos:

    cp motion /usr/local/bin/motion

