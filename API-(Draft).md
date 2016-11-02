
# motionEye API (Draft)


## Protocol And Data Format


## Authentication


## API Calls


### motionEye Server Management

#### `GET /api/main`

Returns details about the motionEye server.

 * successful response:

        {
            "name": string,
            "version": string,
            "motion_version": string
        }

#### `POST /api/main`

Changes the parameters of the motionEye server.

 * expects:

        {
            "normal_password": string
        }

 * successful response:
 
        {}


### Camera Management

#### `GET /api/cameras`

Returns the list of all registered cameras.

 * successful response:

        [
            {
                "id": number,
                "name": string
            },
            ...
        ]
    
    The order of the cameras in the response is the actual order of the camera frames in the UI.

#### `POST /api/cameras`

Adds a camera to the server.

 * expects:

        {
            "type": string,
            "device": string,
            "url": string,
            "username": string,
            "password": string,
            "camera_id": number
        }
    
    Depending on the `type`, some of the fields may be required and others may be ignored.

 * fields:

    * `type` - the camera type; possible values:

        * `"v4l2"` - V4L2 camera
        * `"netcam"` - network (IP) camera
        * `"remote"` - remote motionEye camera
        * `"simple"` - simple MJPEG camera
    
    * `device` - the device path, used only by `"v4l2"` camera type (e.g. `"/dev/video0"`)
    * `url` - the netcam or remote motionEye camera URL
    * `username` - the username of the netcam or remote motionEye camera
    * `password` - the password of the netcam or remote motionEye camera
    * `camera_id` - the id of the remote motionEye camera

 * successful response:

        {
            "id": number
        }
    
    * `id` is the id of the new camera


#### `POST /api/camera/<id>/remove`

Removes the camera with the given id from the server.

 * expects:
 
        {}

 * successful response:

        {}


### Camera Device Parameters

#### `GET /api/camera/<id>/device`

Returns the device parameters of the camera with the specified id.

 * successful response:

        {
            "enabled": boolean,
            "name": string,
            "type": string,
            "rotation": number,
            "resolution": {
                "width": number,
                "height": number
            },
            "framerate": number,
            "brightness": number,
            "constrast": number,
            "saturation": number,
            "hue": number,
            "auto_brightness": boolean
        }
    
 * fields:
 
    * `enabled` - whether the camera is enabled or not
    * `name` - the camera name (between 1 and 64 characters)
    * `type` - the camera type; possible values:

        * `"v4l2"` - V4L2 camera
        * `"netcam"` - network (IP) camera
        * `"remote"` - remote motionEye camera
        * `"simple"` - simple MJPEG camera

    * `rotation` - camera rotation (in degrees); possible values are `0`, `90`, `180` and `270`
    * `resolution`:

        * `width` - the camera resolution width (between 96 and 4096, multiple of 8)
        * `height` - the camera resolution height (between 96 and 4096, multiple of 8)
    
    * `framerate` - the capturing framerate (between 1 and 30)
    * `brightness` - the camera brightness control (between 0 and 100)
    * `contrast` - the camera contrast control (between 0 and 100)
    * `saturation` - the camera saturation control (between 0 and 100)
    * `hue` - the camera hue control (between 0 and 100)
    * `auto_brightness` - whether the brightness is adjusted automatically or not

#### `POST /api/camera/<id>/device`

Changes the device parameters of the camera with the specified id.

 * expects:

        {
            "enabled": boolean,
            "name": string,
            "type": string,
            "rotation": number,
            "resolution": {
                "width": number,
                "height": number
            },
            "framerate": number,
            "brightness": number,
            "constrast": number,
            "saturation": number,
            "hue": number,
            "auto_brightness": boolean
        }

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/device`](#get-apicameraiddevice).


### Media Files
    
#### `GET /api/camera/<id>/media`

 * successful response:

        {
            "disk": {
                "used": number,
                "total": number
            },

            "upload_stills": boolean,
            "upload_movies": boolean,
            "webhook_url": string,
            "webhook_method": ["get", "post", "post_form", "post_json"], 
            "command_enabled": boolean,
            "command_exec": "string"
        }

#### `POST /api/camera/<id>/media`

 * expects:

        {
            "upload_stills": boolean,
            "upload_movies": boolean,
            "webhook_url": string,
            "webhook_method": ["get", "post", "post_form", "post_json"], 
            "command_enabled": boolean,
            "command_exec": "string"
        }


### Text Overlay

#### `GET /api/camera/<id>/overlay`

 * successful response:

        {
            "left": string,
            "right": string
        }

#### `POST /api/camera/<id>/overlay`

 * expects:

        {
            "left": string,
            "right": string
        }


### Streaming

#### `GET /api/camera/<id>/streaming`

 * successful response:

        {
            "snapshot_url": string,
            "streaming_url": string,
            "embed_url": string,
            "enabled": boolean,
            "auth_mode": ["disabled", "basic", "digest"],
            "framerate": number, /* 0 - 30 */
            "quality": number, /* 0 - 100 */
            "resolution_downscale": number, /* 0 - 100 */
            "motion_optimization": boolean
        }

#### `POST /api/camera/<id>/streaming`

 * expects:

        {
            "enabled": boolean,
            "auth_mode": ["disabled", "basic", "digest"],
            "framerate": number, /* 0 - 30 */
            "quality": number, /* 0 - 100 */
            "resolution_downscale": number, /* 0 - 100 */
            "motion_optimization": boolean
        }


### Still Images

#### `GET /api/camera/<id>/stills`

 * successful response:
 
        {
            "mode": ["disabled", "motion-triggered", "interval-snapshots", "all-frames"],
            "filename": string,
            "quality": number, /* 0 - 100 */,
            "interval": number, /* 1 - 86400 */
            "preserve": number, /* 0 - 3650 */
        }

#### `POST /api/camera/<id>/stills`

 * expects:
 
        {
            "mode": ["disabled", "motion-triggered", "interval-snapshots", "all-frames"],
            "filename": string,
            "quality": number, /* 0 - 100 */,
            "interval": number, /* 1 - 86400 */
            "preserve": number, /* 0 - 3650 */
        }

        
### Movies

#### `GET /api/camera/<id>/movies`

 * successful response:
 
        {
            "mode": ["disabled", "motion-triggered", "continuous"],
            "filename": string,
            "quality": number, /* 0 - 100 */
            "format": ["mpeg4", "msmpeg4", "swf", "flv", "mov", "ogg", "mp4", "hevc", "mkv"],
            "max_length": number, /* 0 - 86400 */
            "preserve": number, /* 0 - 3650 */
        }

#### `POST /api/camera/<id>/movies`

 * expects:
 
        {
            "mode": ["disabled", "motion-triggered", "continuous"],
            "filename": string,
            "quality": number, /* 0 - 100 */
            "format": ["mpeg4", "msmpeg4", "swf", "flv", "mov", "ogg", "mp4", "hevc", "mkv"],
            "max_length": number, /* 0 - 86400 */
            "preserve": number, /* 0 - 3650 */
        }


### Motion Detection
    
#### `GET /api/camera/<id>/motion_detection`

 * successful response:
 
        {
            "enabled": boolean,
            "frame_changes": boolean,
            "threshold": number, /* 0 - 20 */
            "noise_level": number, /* 0 - 25 */
            "light_switch_detect": boolean,
            "despeckle_filter": boolean,
            "event_gap": number, /* 1 - 86400 */
            "pre_capture": number, /* 0 - 1000 */
            "post_capture": number, /* 0 - 1000 */
            "min_frames": number, /* 0 - 1000 */
            "mask_mode": ["disabled", "smart", "editable"],
            "mask_sluggishness": number, /* 1 - 10 */
            "mask_lines": number[][], /* 0/1 */
        }

#### `POST /api/camera/<id>/motion_detection`

 * expects:
 
        {
            "enabled": boolean,
            "frame_changes": boolean,
            "threshold": number, /* 0 - 20 */
            "noise_level": number, /* 0 - 25 */
            "light_switch_detect": boolean,
            "despeckle_filter": boolean,
            "event_gap": number, /* 1 - 86400 */
            "pre_capture": number, /* 0 - 1000 */
            "post_capture": number, /* 0 - 1000 */
            "min_frames": number, /* 0 - 1000 */
            "mask_mode": ["disabled", "smart", "editable"],
            "mask_sluggishness": number, /* 1 - 10 */
            "mask_lines": number[][], /* 0/1 */
        }


### Notifications

#### `GET /api/camera/<id>/notifications`

 * successful response:
 
        {
            "email_enabled": boolean,
            "email_from": string,
            "email_to": string[],
            "email_smtp_server": string,
            "email_smtp_port": number, /* 1 - 65535 */
            "email_smtp_account": string,
            "email_smtp_tls": boolean,
            "email_picture_timespan": number, /* 0 - 60 */
            "webhook_enabled": boolean,
            "webhook_url": string,
            "webhook_method": ["get", "post", "post_form", "post_json"], 
            "command_enabled": boolean,
            "command_exec": string
        }

#### `POST /api/camera/<id>/notifications`

 * expects:
 
        {
            "email_enabled": boolean,
            "email_from": string,
            "email_to": string[],
            "email_smtp_server": string,
            "email_smtp_port": number, /* 1 - 65535 */
            "email_smtp_account": string,
            "email_smtp_tls": boolean,
            "email_picture_timespan": number, /* 0 - 60 */
            "webhook_enabled": boolean,
            "webhook_url": string,
            "webhook_method": ["get", "post", "post_form", "post_json"], 
            "command_enabled": boolean,
            "command_exec": string
        }


### Working Schedule
        
#### `GET /api/camera/<id>/working_schedule`

 * successful response:
 
        {
            "mode": ["disabled", "during", "outside"],
            "monday": [
                {
                    "from": number, /* 0 - 23 */
                    "to": number /* 0 - 59 */
                },
                ...
            ],
            ...
        }

#### `POST /api/camera/<id>/working_schedule`

 * expects:
 
        {
            "mode": ["disabled", "during", "outside"],
            "monday": [
                {
                    "from": number, /* 0 - 23 */
                    "to": number /* 0 - 59 */
                },
                ...
            ],
            ...
        }
