### Requirements ###

* a machine running a recent Linux distro
* python 2.7
* tornado 3.1+
* jinja2
* PIL or pillow
* curl, libcurl & pycurl
* motion (optional)
* ffmpeg (optional)
* v4l-utils (optional)

### The Motion Daemon ###

The `motion` daemon itself is optional, but needed in most cases. Install it (along with `ffmpeg` and `v4l-utils`) unless you configure a machine that will only act as a hub for other motionEye-based cameras.

There are three major variants of motion:

* The stable version (3.2.12 at the time of writing) is included with most distros, but is highly outdated and lacks features like RTSP camera support. The source code can be downloaded from [here](http://www.lavrsen.dk/foswiki/bin/view/Motion/DownloadFiles). It's important to note that this version uses an older configuration file format, incompatible with newer ones (supported by motionEye, nevertheless).

* The 3.4 version (yet to be released, at the time of writing), the recommended one, can be found here: https://github.com/Motion-Project/motion.

* Mr-Dave's development version can be checked out [here](https://github.com/Mr-DaveDev/motion).

Read the [[Compiling Motion|Compiling-Motion]] page if you want to manually compile any of these versions.

### Install Instructions ###

motionEye releases are uploaded to [PyPI](https://pypi.python.org/pypi/motioneye/), so you can use the `pip` command to install it as well as (some of) its dependencies. Following are detailed instructions for some common distributions.

**note 1**: The given commands normally need to be run as root; type them in a root shell or use `sudo` before each command.

**note 2**: If you are configuring a motionEye system that will only act as a hub for other motionEye-based cameras (i.e. no locally connected cameras and no IP cameras), you can skip installing `motion`, `ffmpeg` and `v4l-utils`.

Choose one of the following specific install instructions. When you're done, you may want to come back here and read on to find out how to access the frontend or how to update your motionEye.

* [[Install On Debian|Install-On-Debian]]
* [[Install On Ubuntu|Install-On-Ubuntu]]
* [[Install On Raspbian|Install-On-Raspbian]]
* [[Install On Fedora|Install-On-Fedora]]
* [[Install On Arch|Install-On-Arch]]
* [[Install On Other Distributions|Install-On-Other-Distributions]]
* [[Manual Download And Installation|Manual-Download-And-Installation]]

### Accessing The Frontend ###

After having successfully followed the installation instructions, the motionEye server should be running on your system and listening on port 8765. Fire up your favorite web browser and visit the following URL (replacing `[your_ip]` with... well, your system's IP address):

    http://[your_ip]:8765/

Use `admin` with empty password when prompted for credentials. For further details on how to configure motionEye, see [[Configuration|Configuration]].

### Upgrading ###

Upgrading should be as easy as running the following command (as root):

    pip install --upgrade motioneye

To upgrade to a specific version (say 0.27.1), use:

    pip install --upgrade motioneye==0.27.1

If you have manually downloaded and installed motionEye, the `pip` command above won't work and you'll need to repeat the manual installation procedure, preserving the configuration directory.