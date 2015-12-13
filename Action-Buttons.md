### What Are Action Buttons?

Starting with version 0.30, motionEye can be configured to overlay buttons on top of a camera frame. These buttons will then execute custom commands when clicked.

### Available Actions

The following actions are defined:

* `lock`
* `unlock`
* `light_on`
* `light_off`
* `alarm_on`
* `alarm_off`

While the available actions are limited to the above set, the commands executed can be practically anything and have to be defined by the user.

### Enabling Actions

motionEye will look inside its configuration folder for executable files named `[action]_[cameraid]`, where `action` is one of the available actions (listed above) and `cameraid` is the id of the camera on top of which the action button will be displayed.

For example, on a setup using the default configuration, the presence of the *executable* file `/etc/motioneye/unlock_1` tells motionEye to show an *unlock* button on top of camera number one. The file will be executed upon pressing the button. Buttons will have distinctive icons that correspond to the name of the action.

### A Few Remarks

* When adding or removing these action files, you need to reload the application in your browser for the changes to take effect.
* Make sure the file is executable and has no extension.
* Action files can be symlinks, as long as they point to an executable file.
* No arguments are passed when executing the file; if you need to pass arguments to your commands, create a shell script as a wrapper around your command.
* The command should not block (at least not for a long period of time); action buttons are disabled during the execution of their commands.
* The output (*stdout* and *stderr*) will show up in motionEye's log file; if the command exits successfully (with code `0`) the output will be logged as *debug*; otherwise it will show up as an *error*.