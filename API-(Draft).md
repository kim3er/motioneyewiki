
# motionEye API (Draft, Not Implemented!)

**THIS API HASN'T YET BEEN IMPLEMENTED!!!**

## Contents

 * [Protocol And Data Format](#protocol-and-data-format)
     * [JSON, Plain Text And URL-Encoded](#json-plain-text-and-url-encoded)
     * [Content Types](#content-types)
     * [HTTP Stats Codes](#http-status-codes)
 * [Authentication](#authentication)
 * [API Calls](#api-calls)
    * [motionEye Server Management](#motioneye-server-management)
        * [`GET /api/main`](#get-apimain)
        * [`POST /api/main`](#post-apimain)
    * [Camera Management](#camera-management)
        * [`GET /api/cameras`](#get-apicameras)
        * [`POST /api/cameras`](#post-apicameras)
        * [`POST /api/camera/<id>/remove`](#post-apicameraidremove)
    * [Camera Device Parameters](#camera-device-parameters)
        * [`GET /api/camera/<id>/device`](#get-apicameraiddevice)
        * [`POST /api/camera/<id>/device`](#post-apicameraiddevice)
    * [Media Files](#media-files)
        * [`GET /api/camera/<id>/media`](#get-apicameraidmedia)
        * [`POST /api/camera/<id>/media`](#post-apicameraidmedia)
    * [Text Overlay](#text-overlay)
        * [`GET /api/camera/<id>/overlay`](#get-apicameraidoverlay)
        * [`POST /api/camera/<id>/overlay`](#post-apicameraidoverlay)
    * [Streaming](#streaming)
        * [`GET /api/camera/<id>/streaming`](#get-apicameraidstreaming)
        * [`POST /api/camera/<id>/streaming`](#post-apicameraidstreaming)
    * [Still Images](#still-images)
        * [`GET /api/camera/<id>/stills`](#get-apicameraidstills)
        * [`POST /api/camera/<id>/stills`](#post-apicameraidstills)
    * [Movies](#movies)
        * [`GET /api/camera/<id>/movies`](#get-apicameraidmovies)
        * [`POST /api/camera/<id>/movies`](#post-apicameraidmovies)
    * [Motion Detection](#motion-detection)
        * [`GET /api/camera/<id>/motion_detection`](#get-apicameraidmotion_detection)
        * [`POST /api/camera/<id>/motion_detection`](#post-apicameraidmotion_detection)
    * [Notifications](#notifications)
        * [`GET /api/camera/<id>/notifications`](#get-apicameraidnotifications)
        * [`POST /api/camera/<id>/notifications`](#post-apicameraidnotifications)
    * [Working Schedule](#working-schedule)
        * [`GET /api/camera/<id>/working_schedule`](#get-apicameraidworking_schedule)
        * [`POST /api/camera/<id>/working_schedule`](#post-apicameraidworking_schedule)
 * [Examples](#examples)

## Protocol And Data Format

`null` values should be avoided in general. In JSON requests they are however accepted but will be internally converted to `false`, `0`, or empty string (`""`), depending on the expected data type.

All string values must be encoded as UTF-8 strings.

### JSON, Plain Text And URL-Encoded

The JSON variants of the available API calls are described in detail in this document. However, for each field in each of the API calls, there's a simple plain text or URL-encoded variant.

For example, instead of issuing a [`GET /api/camera/<id>/device`](#get-apicameraiddevice) request to find out the name of a camera, one could issue the following request:

    GET /api/camera/<id>/device/name

and the response will be a simple text representing the name of the camera (e.g. `Backyard Camera`).

Changing the parameter can be achieved in a similar manner, using a POST request:

    POST /api/camera/<id>/device/name

and in the request body, the simple text representing the name of the camera is expected.

Specifying new values for one or more parameters can be done also using the URL-encoded format:

    POST /api/camera/<id>/device

and in the request body, name-value pairs for each desired parameter are expected. The following will update the name and the framerate of the camera:

    name=Frontyard%20Camera&framerate=10

In case of complex fields (such as camera device `resolution`, which has a `width` and a `height` subfields), they must be specified using the dot notation (e.g. `resolution.width`), unless the JSON format is used.

### Content Types

It is important to specify the correct content type when issuing POST requests. The following content types are accepted by motionEye:

 * `application/json`
 * `application/x-www-form-urlencoded`
 * `text/plain`
 
The response from the server will always have the correct content type (either `application/json` or `text/plain`, depending on the request path).

### HTTP Status Codes

Successful requests are always responded with a `200 OK` status.

Unauthenticated requests are responded with a `401 Unauthorized` status. Additionally, the `WWW-Authenticate` header will be set to the appropriated Digest value, with the realm set to the name of the server.

Requests to any unknown path will be responded with a `404 Not Found`. The same status will be used for requests to an unknown camera id.

If an invalid value was supplied for a parameter, or the parameter was expected but not supplied at, the response will have a `400 Bad Request` status and the body will have a JSON-encoded object with one field called `param`, indicating the name of the invalid parameter:

    {
        "param": "brightness"
    }

Other API call-specific errors and corresponding statuses are described next to the respective API call.

## Authentication

Three types of authentication are supported by the motionEye API server. Each one is described in detail in the following sections.

All requests require the admin username and the password configured for the administrator account. In case the administrator password is empty, requests can be issued without authentication data.

The server will always respond with an HTTP Digest Authentication requirement response to unauthorized requests. It's then up to the client to continue with the HTTP Digest mechanism, to include HTTP Basic Authentication data or to include the authentication signature.

### HTTP Basic Authentication

This authentication type can be used in less secure environments, where reply attacks are out of question (e.g. issuing requests directly from the running machine).

### HTTP Digest Authentication

This authentication type can be used when security is a concern, if the client (or the client library) used supports this authentication scheme.

### Signature

The signature is a query argument that must be included with every request (e.g. `?signature=<signature>`), when this authentication method is used. The signature is computed as follows:

    signature = sha1(signing_string)

where the signing string is obtained as follows:

    signing_string = <method>:<path>:<body>:<password>'

The four parameters from the formula are:

 * `method` is the HTTP method in uppercase (e.g. `POST`)
 * `path` is the requested path, including any query arguments, sorted alphabetically (e.g. `/api/camera/3/media`)
 * `body` is the entire unmodified body of the request, or an empty string in the absence of a body
 * `password` is the administrator's password

The signature must be expressed as a 40 characters lowercase hex string.


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

 * successful response:
 
        {}


### Media Files
    
#### `GET /api/camera/<id>/media`

Returns the media files configuration of the camera with the specified id.

 * successful response:

        {
            "disk": {
                "used": number,
                "total": number
            },
            "upload_stills": boolean,
            "upload_movies": boolean,
            "webhook_url": string,
            "webhook_method": string,
            "command_enabled": boolean,
            "command_exec": "string"
        }

 * fields:

    * `disk.used` - the disk usage, in bytes
    * `disk.total` - the total disk capacity, in bytes
    * `upload_stills` - whether the still images are uploaded to the configured upload service or not
    * `upload_movies` - whether the movies are uploaded to the configured upload service or not
    * `webhook_url` - the complete URL to request whenever a media file is created, including any URL-encoded query arguments (up to 512 characters); special tokens:
        * `%Y` - year
        * `%m` - month
        * `%d` - day
        * `%H` - hour
        * `%M` - minute
        * `%S` - second
        * `%f` - media file path
    * `webhook_method` - the HTTP method to use; possible values:
        * `"get"` - a simple HTTP GET
        * `"post"` - a simple HTTP POST without body
        * `"post_form"` - an HTTP POST with form data (URL-encoded) body
        * `"post_json"` - an HTTP POST with JSON body
        
        The fields transmitted in body (where applicable) are in fact the query arguments configured for `webhook_url`.
    
    * `command_enabled` - whether the configured command will be executed or not upon the creation of a media file
    * `command_exec` - a command to execute upon the creation of a media file (up to 512 characters); special tokens from `webhook_url` apply here as well

#### `POST /api/camera/<id>/media`

 * expects:

        {
            "upload_stills": boolean,
            "upload_movies": boolean,
            "webhook_url": string,
            "webhook_method": string,
            "command_enabled": boolean,
            "command_exec": "string"
        }

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/media`](#get-apicameraidmedia).

 * successful response:
 
        {}


### Text Overlay

#### `GET /api/camera/<id>/overlay`

Returns the text overlay configuration of the camera with the specified id.

 * successful response:

        {
            "left": string,
            "right": string
        }

 * fields:
 
    * `left` - the text to overlay on the bottom-left side of the image
    * `right` - the text to overlay on the bottom-left side of the image
    
        Special tokens can be used in both fields, as follows:

        * `%Y` - year
        * `%m` - month
        * `%d` - day
        * `%H` - hour
        * `%M` - minute
        * `%S` - second
        * `%T` - HH:MM:SS
        * `%q` - frame number inside a second
        * `\n` - new line

#### `POST /api/camera/<id>/overlay`

 * expects:

        {
            "left": string,
            "right": string
        }

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/overlay`](#get-apicameraidoverlay).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/streaming`](#get-apicameraidstreaming).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/stills`](#get-apicameraidstills).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/movies`](#get-apicameraidmovies).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/motion_detection`](#get-apicameraidmotion-detection).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/notifications`](#get-apicameraidnotifications).

 * successful response:
 
        {}


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

    All fields are optional and only the specified fields will be updated.
    For the meaning of each field, see [`GET /api/camera/<id>/working_schedule`](#get-apicameraidworking_schedule).

 * successful response:
 
        {}


## Examples