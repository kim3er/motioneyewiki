##### 0.27.2:
* fixed relayevent/sendmail/webhook error when launched with `-b`
* conf, run, log and media paths are now guessed from the `-c` argument unless otherwise instructed in the config file
* the default media path is now `/var/lib/motioneye`

##### 0.27.1:
* fixed an old motion compatibility problem

#### 0.27:
* motionEye has been moved to Github
* motionEye has been ported to `setuptools` and uploaded to [PyPI](https://pypi.python.org/pypi/motioneye/).
* there is now a single main script called `meyectl` that will control the server