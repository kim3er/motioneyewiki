### motioneye.conf

motionEye uses a configuration file (usually called `motioneye.conf`) that allows customizing your setup (including paths, logging, various timeouts and other options that cannot be modified through the UI). The default location for this file is `/etc/motioneye/motioneye.conf` and can be changed using the `-c` command line switch.

### Available Options

##### `base_path`

Allows altering the base URL path at which motionEye lives. Change this if you run motionEye behind a reverse proxy and you want to make motionEye accessible at a specific base path (e.g. /cams). Defaults to `/`.

##### `conf_path`

Defines the folder for the various configuration files used by motionEye. These include `motion.conf`, `thread-*.conf` (both used by the *motion* daemon). This folder must be writable by the user with which motionEye runs. Defaults to `/etc/motioneye`.

##### `run_path`

Defines the folder where pid files are created. This folder must be writable by the user with which motionEye runs. Defaults to the first of the following which exists: `/run`, `/var/run`, `/tmp`, `/var/tmp`.

##### `log_path`

Defines the folder where log files are created. This folder must be writable by the user with which motionEye runs. Defaults to the first of the following which exists: `/log`, `/var/log`, `/tmp`, `/var/tmp`, `run_path`.

##### `media_path`

Defines the folder where the media files are created, by default. This folder must be writable by the user with which motionEye runs. Defaults to `/var/lib/motioneye`.

##### `log_level`

Changes the log level of motionEye as well as of the motion daemon started by motionEye. Accepted values are `quiet`, `error`, `warning`, `info` and `debug`. Defaults to `info`.

##### `listen`

Defines the IP address on which the motionEye server will listen. Use `0.0.0.0` for all interfaces or `127.0.0.1` for localhost. Defaults to all interfaces.

##### `port`

Defines the TCP port on which the motionEye server will listen. Defaults to `8765`.

##### `motion_binary`

Instructs motionEye to use a specific *motion* daemon. The path will be automatically detected by default, using the `PATH` environment variable.

##### `motion_control_localhost`

Set this to `false` to restrict the motion daemon's HTTP control server to listen on all interfaces. Defaults to `true`.

##### `motion_control_port`

Defines the TCP port for the motion daemon's HTTP control server. Defaults to `7999`.

##### `motion_check_interval`

Configures the interval in seconds at which motionEye checks if motion is running. Defaults to `10`.

##### `motion_restart_on_errors`

Configures whether the motion daemon should be restarted when an error occurs while communicating with it. Defaults to `false`.

##### `mount_check_interval`

Configures the interval in seconds at which motionEye checks the mounted filesystems (notably SMB/network share mounts). Defaults to `300`.

##### `cleanup_interval`

Configures the interval in seconds at which the janitor function is called to remove old media files. Defaults to `43200` (twice a day).

##### `remote_request_timeout`

Configures the timeout in seconds to wait for response from a remote motionEye server. Defaults to `10`.

##### `mjpg_client_timeout`

Configures the timeout in seconds to wait for a MJPEG frame from the motion daemon. Defaults to `10`.

##### `mjpg_client_idle_timeout`

Configures the timeout in seconds after which an idle internal MJPEG client is closed and removed (set to `0` to keep clients connected indefinitely). Defaults to `10`.

##### `smb_shares`

Set this to `true` to enable management of SMB (network) shares (requires motionEye to run as root). Defaults to `false`.

##### `smb_mount_root`

Sets the directory where the SMB mount points will be created. Defaults to `/media`.

##### `wpa_supplicant_conf`

Defines the path to the `wpa_supplicant` configuration file used by the system. Set this to enable wifi management from the UI. This is disabled by default.

##### `local_time_file`

Defines the path to the `localtime` configuration file used by the system. Set this to enable time zone management from the UI. This is disabled by default.

##### `enable_reboot`

Enables shutdown and rebooting after changing system settings (such as wifi settings or time zone). This option requires motionEye to run as root. Defaults to `false`.

##### `smtp_timeout`

Configures the timeout in seconds to use when talking to the SMTP server. Defaults to `60`.

##### `list_media_timeout`

Configures the timeout in seconds to wait for media files list. Defaults to `120`.

##### `list_media_timeout_email`

Configures the timeout in seconds to wait for media files list, when sending emails. Defaults to `10`.

##### `zip_timeout`

Configures the timeout in seconds to wait for zip file creation. Defaults to `300`.

##### `timelapse_timeout`

Configures the timeout in seconds to wait for timelapse file creation. Defaults to `300`.

##### `add_remove_cameras`

Set this to `false` to disable adding and removing of cameras from the UI. Defaults to `true`.

##### `validate_certs`

Controls whether HTTPS/SSL certificates are validated by various clients used by motionEye, or not. Defaults to `true`.