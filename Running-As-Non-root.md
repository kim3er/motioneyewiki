For the sake of security, you may wish to run motionEye as an unprivileged (non-root) user. Here's how you can do that, depending on the way the server is started:

### On sysvinit-based Systems

### On systemd-based Systems

Just edit the systemd unit file `/etc/systemd/system/motioneye.service` by adding a `User` directive to the `[Service]` section:

    ...
    [Service]
    ExecStart=/usr/bin/meyectl startserver -c /etc/motioneye/motioneye.conf
    Restart=on-abort
    User=youruser
    ...

Don't forget to reload the units and restart the service in order for the changes to take effect:

    systemctl daemon-reload
    systemctl restart motioneye

### Manually Started motionEye Server
