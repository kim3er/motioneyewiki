##### 0.32.1:
 * more than 10 cameras can now be added to a motionEye server
 * fixed a bug where media files cleanup wouldn't work when having multiple cameras
 * RTSP network cameras: added support for basic authentication
 * Dropbox API v2 is now used (v1 has been deprecated)
 * Google Drive: added support for relative path (subfolders for media files)
 * Google Drive: media files are no longer uploaded to folders that have been moved to trash
 * added a test button for email notifications
 * added a test button for network shares
 * HEAD method is now supported by all URIs

#### 0.32:
 * added support for [[monitoring commands|Monitoring Commands]]
 * HTTP 403 status is now used to signal any authentication problem

##### 0.31.5:
 * fixed a bug where the `relayevent.sh` script would compute a wrong signature when using special characters in admin password

##### 0.31.4:
 * fixed a bug with numeric passwords used for remote motionEye cameras
 * when removing a single movie file from the media browser, the associated .thumb file is removed as well

##### 0.31.3:
 * event passing between motion and motionEye is now faster thanks to a `relayevent.sh` shell script replacing old python-based method

##### 0.31.2:
 * fixed a bug where File Storage section would show up in settings menu for Simple MJPEG Cameras
 * hidden files and dirs are now properly ignored when listing media files

##### 0.31.1:
 * the root media folder is no longer removed when removing a media files group
 * right click on a picture frame allows now saving the picture locally (thanks to @nmrugg)

#### 0.31:
 * hidden folders are ignored when listing media files
 * media file listing performance improvements
 * added support for more network share security methods
 * added support for open wifi networks
 * SMTP passwords can now contain percent characters
 * the From field can now be customized when configuring email notifications

#### 0.30:
 * camera frames layout has been completely redesigned
 * current streaming/capture frame rate is now shown in each camera frame
 * added support for [[action buttons|Action Buttons]]
 * added frame rate dimmer control
 * added resolution dimmer control
 * added `meyectl shell` command (for debugging purposes)
 * fixed mixed up cameras when sending email notifications
 * semicolons are now accepted in SMTP account passwords

##### 0.29.1:
 * fixed camera live preview when enabling streaming authentication
 * motion log level follows now motionEye's log level setting as close as possible

#### 0.29:
 * fixed media browser layout on Firefox 42+
 * fixed webhook POST
 * fixed boolean extra motion options
 * added support for transmitting data as form or json when calling a webhook
 * added support for calling webhooks and running commands with each media file creation
 * added support for media file uploading to Google Drive and Dropbox

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