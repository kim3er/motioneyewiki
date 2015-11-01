For the sake of security, you may wish to run motionEye as an unprivileged (non-root) user. This article assumes that you have already [installed](https://github.com/ccrisan/motioneye/wiki/Installation) motionEye. When following the instructions below, replace `youruser` with the username of your unprivileged account.

### Ownership & Permissions

Your unprivileged user must have the required permissions to the various directories motionEye uses. The simplest way to achieve this is to create a folder that is specific to motionEye in your user's home directory. As `youruser`, run:

    mkdir /home/youruser/motioneye

### Paths & Configuration

All files used and created by motionEye must live in `/home/youruser/motioneye`. Therefore you need to use a `motioneye.conf` with adjusted path settings. Copy your system-wide `motioneye.conf` to your user's directory:

    cp /etc/motioneye/motioneye.conf /home/youruser/motioneye/motioneye.conf

Edit the `/home/youruser/motioneye/motioneye.conf` file by adjusting the following settings as indicated:

    * `conf_path /home/youruser/motioneye`
    * `run_path /home/youruser/motioneye`
    * `log_path /home/youruser/motioneye`
    * `media_path /home/youruser/motioneye`

Of course you can use separate subdirectories for some or all of the above; just make sure to create them first.

### motionEye Server Startup

The motionEye server must be instructed to use the new configuration file and to start as another user. Here's how you can do that, depending on the way the server is started:

### On sysvinit-based Systems

### On systemd-based Systems

Just edit the systemd unit file `/etc/systemd/system/motioneye.service` by adding a `User` directive to the `[Service]` section and changing the path to the configuration file:

    ...
    [Service]
    ExecStart=/usr/bin/meyectl startserver -c /home/youruser/motioneye/motioneye.conf
    User=youruser
    ...

Don't forget to reload the units and restart the service in order for the changes to take effect:

    systemctl daemon-reload
    systemctl restart motioneye

### Manually Started motionEye Server

Assuming `sudo` is available on your system, the command should look something like this:

    sudo -u youruser meyectl startserver -b -c /home/youruser/motioneye/motioneye.conf
