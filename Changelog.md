##### 0.28.3:
 * lightswitch detection is now disabled by default
 * minimum motion frames option is now set to 10 frames by default
 * fixed no preview bug when using digest authentication for the video streaming
 * fixed various bugs related to dealing with ungrouped media files (those created directly in the root directory of the camera)

##### 0.28.2:
 * the working schedule mechanism won't be enabled unless at least one day is enabled
 * fixed memory leaks with working schedule enabled
 * fixed email notifications not being sent
 * added new settings: `motion_control_localhost`, `motion_control_port`, `list_media_timeout` and `timelapse_timeout` (previously hardcoded)

##### 0.28.1:
* light switch detection threshold slider has been moved to *Motion Detection* section, where it belongs
* the settings panel is no longer automatically opened after login
* fixed a bug where motion detection events were not relayed correctly to the UI (the frame wouldn't turn to red)
* added `motion_control_localhost` and `motion_control_port` settings to configure the motion HTTP control interface

#### 0.28:
* added continuous movie recording mode
* the destination address is now used as "from" when sending email notifications
* light switch detection setting is now a slider (was a simple on/off button before)
* fixed various URL-related issues when running behind a reverse proxy

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