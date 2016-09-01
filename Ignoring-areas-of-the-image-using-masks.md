If you need to ignore certain parts of the image, for example if a busy street is in view, you can configure MotionEye to ignore that area. 

To configure MotionEye to use a mask, you need to:

* in your favourite image editor create an image of the exact same pixel dimensions as your camera (be careful — if you have several cameras, they may likely produce images with different px dimensions). This image should be black-and-white (or greyscale). White means 100% sensitive, black means %100 ignore, and grey proportionally between those. In the example of the busy street, you would draw a black area over the street area, and the rest of the image would be white
* next, convert that image to .pgm format (there are several ways to do this; one option is https://convertio.co/pgm-converter/)
* place this pgm file at a path that MotionEye can reach it, and enter the following into "Extra Motion Options":

    mask_file /path/to/mymaskfile.pgm