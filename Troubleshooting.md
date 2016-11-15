
* [Raspberry PI Camera Module Not Detected](#raspberrypi-camera-module-not-detected)
* [Camera Frame Keeps Showing No Camera Symbol](#camera-frame-keeps-showing-no-camera-symbol)


***

### Raspberry PI Camera Module Not Detected

Make sure that:
 * you have connected the camera to the right port; the Raspberry PI has two identical ports for the camera module and for the touchscreen)
 * the cable/ribbon/connector is properly connected and not damaged
 * you have loaded the `bcm2835-v4l2` kernel module, by adding it to `/etc/modules` and rebooting afterwards
 * you have enabled the camera using `raspi-config`
 * you have allocated at least 128 MB of RAM to the GPU
 * you are using a genuine camera module, not a "compatible" one
 * the command `raspistill -o cam.jpg` captures an image from the camera, without complaining

The special file `/dev/video0` should be present when all of the above are met.

### Camera Frame Keeps Showing No Camera Symbol

Make sure you have set the *Frame Rate Dimmer* preference to a value greater than 0.