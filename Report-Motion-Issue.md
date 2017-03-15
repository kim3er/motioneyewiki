In order to provide necessary details for a bug report to the [Motion Project](https://github.com/Motion-Project/motion/issues), it is recommended that you run `motion` manually, separately, outside of motionEye.

First you'll need to log in to your motionEye machine and run some shell commands. Stop the motionEye server (using `sudo` if necessary):

    systemctl stop motioneye  # for systemd 
    /etc/init.d/motioneye stop  # for sysvinit

Then change the directory to where motion configuration files live:

    cd /etc/motioneye

And then just start the motion daemon by hand, redirecting the output/error to a file:

    motion -c motion.conf -d 6 -n &> ~/motion.log

Now collect the log file as well as your motion configuration files:

 * `~/motion.log`
 * `/etc/motioneye/motion.conf`
 * `/etc/motioneye/thread-*.conf`

Make sure you **remove any sensitive data** (i.e. passwords, IP addresses and whatnot) from these files before attaching them to the issue you're going to create.
