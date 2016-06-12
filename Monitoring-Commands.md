### What Are Monitoring Commands?

Starting with version 0.32, motionEye can be configured to overlay monitoring information on top of a camera frame. The information is gathered by running a command and capturing its output. These commands can be configured on a per-camera basis and are usually shell scripts provided by the user.

Monitoring commands can be used to watch system parameters such as the system load, the CPU temperature or the network transfer rate.

### Enabling A Monitoring Command

motionEye will look inside its [[configuration folder|Configuration-File#conf_path]] for executable files named `monitor_[cameraid]`, where `cameraid` is the id of the camera on top of which the monitoring info will be displayed.

For example, on a setup using the default configuration, the presence of the *executable* file `/etc/motioneye/monitor_1` will determine motionEye to run the script every 1 second (by default), to read its *standard output* and to display it on top of the first camera frame.

### Execution (Refresh) Rate

By default, all monitoring scripts are called every 1 second. If a script wishes to explicitly reschedule its next execution, it should simply write the delay, in seconds, to its *standard error*.

### A Few Remarks

* When adding or removing these scripts, you need to reload the application in your browser for the changes to take effect.
* Make sure the file is executable and has no extension.
* Monitoring command files can be symlinks, as long as they point to an executable file.
* No arguments are passed when executing the file; if you need to pass arguments to your commands, create a shell script as a wrapper around your command.
* The command must return immediately (i.e. should not block).

### Example

The following monitoring script can be used to display the temperature of a system every 10 seconds:

    #!/bin/bash
    
    temp=$(cat /sys/devices/virtual/thermal/thermal_zone0/temp)
    temp=$(($temp / 1000))
    echo "${temp}&deg;"
    echo 10 1>&2
