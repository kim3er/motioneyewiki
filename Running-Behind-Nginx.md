### Why Nginx?

[Nginx](http://nginx.org/) is configurable, fast and easy to install. Running behind the Nginx webserver (using Nginx as a *reverse proxy*) can be useful when:
 * you want to use HTTPS with motionEye
 * you want to serve multiple websites (including motionEye) on the same webserver (accessible at the same IP address)
 * you want to take advantage of various Nginx modules (such as rate limiting or HTTP authentication)

### Sample Virtual Host

Here's the content of a sample virtual host file that normally goes to `/etc/nginx/sites-avaiable/motioneye.example.com`:

    server {
        listen 80;
        server_name motioneye.example.com;
        location /cams/ {
            proxy_pass http://192.168.1.7:8765/;
            proxy_read_timeout 120s;
            access_log off;
        }
    }

Your motionEye UI will be available at `http://motioneye.example.com/cams`.
It's important to note the trailing slashes at `location /cams/` and at `http://192.168.1.7:8765/`. They make sure paths are correctly passed around when forwarding the HTTP requests to motionEye.

### motionEye Base Path

Your `motioneye.conf` file should also reflect the `/cams` base path at which it is accessed (static files, such as JavaScript, images and CSS files need to be requested at that path as well). Uncomment and set the `base_path` option in your `motioneye.conf`:

    ...
    base_path /cams
    ...

### MJPEG And Nginx

motionEye doesn't normally rely on MJPEG streaming to present the live video to the user; it rather uses separate HTTP requests to repeatedly fetch the most recent JPEG for each camera frame.

However, if you explicitly add a *Simple MJPEG Camera* to your motionEye, it will require direct access to the given MJPEG stream that's usually accessible in your local network. There are two ways to make it accessible to the outside world:
 1. Configure port forwarding on your router so that the MJPEG port (often 8080) is forwarded to the IP address of your local MJPEG streaming camera; then make sure to (re)add your *Simple MJPEG Camera* with your public IP address instead of the local, private one.
 2. Add a new location directive to your Nginx configuration:

        location /mjpeg {
            proxy_pass http://your_netcam:8080/;
            proxy_read_timeout 86400;
        }
    Then make sure to (re)add your *Simple MJPEG Camera* with your public IP address followed by the `/mjpeg` path.
