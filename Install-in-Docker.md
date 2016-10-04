motionEye comes with a `Dockerfile` and a sample `docker-compose.yml`, both in the `/extra` directory of the project (https://github.com/ccrisan/motioneye/tree/master/extra).

If you like contribute or testing `motioneye` project, you can use the docker container.

### Instructions
1. clone your fork or official `motioneye` project

        git clone https://github.com/ccrisan/motioneye.git

2. Build your motionEye Docker image from the `Dockerfile`. 

        # enter project folder
        cd motioneye
        
        # If you would like build docker image from official project
        docker build -f extra/Dockerfile -t motioneye .

*Note:* If /etc/motioneye/motioneye.conf not exist, it's copied from /usr/share/motioneye/extra/motioneye.conf.sample (Not overwrite the volume)

3. Have a cup of coffee while the image builds :)

4. Either start a container using `docker run` or use the provided sample `docker-compose.yml` together with `docker-compose`.

#### With docker run:

        docker run -it --name=motioneye \
            --device=/dev/video0 \
            -p 8081:8081 \
            -p 8765:8765 \
            -e TIMEZONE="America/New_York" \
            -v /mnt/motioneye/media:/media \
            -v /mnt/motioneye/config:/etc/motioneye \
            -v $PWD:/usr/local/src/motioneye \
            --restart=always \
            motioneye

#### With docker-compose.yml:

Edit `docker-compose.yml` and modify the timezone to your own (A list is available at http://php.net/manual/en/timezones.php).

Also edit the two mount points to a directory in your system. Save the file, and then run:

        docker-compose -f docker-compose.yml -p motioneye up -d

#### Test your developpement:

If you would contribute or test your development, update the code, and rebuild the docker container, see the step 2