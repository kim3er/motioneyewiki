### Why Compile Motion? ###

The stable version (3.2.12 at the time of writing), which is included with most distros, is highly outdated and lacks features like RTSP camera support.

Recently, thanks to [Mr Dave's](https://github.com/Mr-DaveDev/) efforts, the motion project has been revived and now lives at [https://motion-project.github.io/](https://motion-project.github.io/).

For some time now, Mr Dave's fork has been the best choice of motion variant and has recently been deemed as version 3.4 of the project. The repositories are as follows:

 * official repo: https://github.com/Motion-Project/motion
 * Mr Dave's development repo: https://github.com/Mr-DaveDev/motion

The following instructions should normally work with any variant of motion source code you may find online.

### Requirements ###

* a Linux machine with [`gcc`](https://gcc.gnu.org/), [`automake`](http://www.gnu.org/software/automake/) and [`autoconf`](http://www.gnu.org/software/autoconf/)
* [`libjpeg`](http://libjpeg.sourceforge.net/)
* a relatively recent version of [`ffmpeg`](https://www.ffmpeg.org/)
* [`subversion`](https://subversion.apache.org/) and/or [`git`](https://git-scm.com/)

**On a Debian-based distro**, the following command installs all the requirements (as root):

    apt-get install build-essential autoconf libjpeg-dev libavformat-dev libavcodec-dev libswscale-dev ffmpeg subversion git

**note**: Plain Debian systems require the *deb-multimedia* repo for `ffmpeg`. See [Install On Debian](https://github.com/ccrisan/motioneye/wiki/Install-On-Debian), the first step, for more details.

**On Arch**, one would run (as root):

    pacman -S base-devel libjpeg ffmpeg subversion git

**On Fedora**, make sure to add the *rpmfusion* repo to your system (see [Install On Fedora](https://github.com/ccrisan/motioneye/wiki/Install-On-Fedora), the first step, for details). Then run following command (as root):

    dnf install automake autoconf libjpeg ffmpeg

### Getting The Source ###

If you decide to go with a SVN-based version, use the following command to check out the source code (the latest official SVN revision is given as example):

    svn co http://www.lavrsen.dk/svn/motion/trunk/ motion-svn

For git-based forks, use the following command (Mr Dave's fork is used as example):

    git clone https://github.com/Mr-Dave/motion.git motion-mrdave

### Configure Your Build ###

There are some features (mostly related to databases) that are not used with motionEye; we disable support for those components when running the *configure* script:

    cd [your-motion-dir]
    autoreconf  # only required if the `configure` script is missing
    ./configure --prefix=/usr --without-pgsql --without-sdl --without-sqlite3 --without-mysql

**note 1**: Replace `[your-motion-dir]` with the directory where you have downloaded the source code (e.g. `motion-svn` or `motion-mrdave`).

**note 2**: Your system may have ffmpeg libs and headers installed in non-standard locations. You will need to specify them when running the configure script with `--with-ffmpeg=` (and `--with-ffmpeg-headers=`, for older versions).

### Compile Motion

Just run `make` in the motion directory (add `-j N` to start a parallel make):

    make

### Install Motion

Luckily motion is a simple binary that can be copied wherever you find appropriate on your filesystem. The recommended place is `/usr/local/bin` where it will supersede any version of motion installed from official repos:

    cp motion /usr/local/bin/motion
