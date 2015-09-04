Although not recommended, manually installing motionEye can give you more control over the directory where motionEye is installed and the way the server is started.

1. Unless you configure your machine for a remote cameras-only hub, make sure you have `motion`, `ffmpeg` and `v4l-utils` installed on your system. This step depends on the distro you're using.

2. Make sure you have Installed a [`python 2.7`](https://www.python.org/) interpreter, [`pip`](https://pip.pypa.io/en/stable/installing.html), [`libcurl`](http://curl.haxx.se/libcurl/) and [`libjpeg`](http://libjpeg.sourceforge.net/).

3. Download `motioneye` from [PyPI](https://pypi.python.org/pypi/motioneye#downloads) or directly from [releases](https://github.com/ccrisan/motioneye/releases) on github.

4. Install `motioneye`, which will automatically pull Python dependencies (`tornado`, `jinja2`, `pillow` and `pycurl`):

    * using `pip`:

            pip install motioneye-x.y.tar.gz

    * manually running `setup.py`:

            tar zxvf motioneye-x.y.tar.gz
            cd motioneye-x.y
            python setup.py install --prefix=/usr

5. Prepare the configuration directory (you can choose to use whatever configuration directory you want):

        mkdir -p /path/to/motioneye
        cp /usr/share/motioneye/extra/motioneye.conf.sample /path/to/motioneye/motioneye.conf

6. You will probably want to edit the configuration file and change various paths:

        nano /path/to/motioneye/motioneye.conf

    **note**: if you comment out all the paths in `motioneye.conf`, the `/path/to/motioneye/` folder will be used for *conf*, *run*, *log* and *media* paths.

7. Run the `motionEye` server:

        meyectl startserver -c /path/to/motioneye/motioneye.conf

    (hit Ctrl+C to terminate)

    **note**: You can use the `-b` argument to `meyectl` to start the server in background, and then `meyectl stopserver` to stop it.
