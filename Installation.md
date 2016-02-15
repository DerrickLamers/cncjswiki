## NPM Installation

First, ensure you have `prefix=~/.npm` in  `~/.npmrc`. For example:
```bash
$ echo "prefix=~/.npm" >> ~/.npmrc
$ cat ~/.npmrc
prefix=~/.npm
```

Then, install `cncjs` without `sudo`, or the `serialport` module may not install correctly on some platforms like Raspberry Pi.
```bash
$ npm install -g cncjs
```

If you want to run it on Raspberry Pi with Raspbian Wheezy, be sure to [install gcc/g++ 4.8](https://somewideopenspace.wordpress.com/2014/02/28/gcc-4-8-on-raspberry-pi-wheezy/) before npm install.

Check out [Git Installation](https://github.com/cheton/cnc.js#git-installation) and [Docker Image Installation (x64 only)](https://github.com/cheton/cnc.js#docker-image-installation-x64-only) for other installation methods.

## Upgrade
Run `npm update -g cncjs` to upgrade to a newer version.

## Usage
Run `cnc` or `~/.npm/bin/cnc` to start the server, and visit `http://yourhostname:8000/` to view the web interface:
```bash
$ cnc
```

Run `cnc` with -h for detailed usage:
```
$ cnc -h

  Usage: cnc [options]
  
  Options:

    -h, --help               output usage information
    -V, --version            output the version number
    -p, --port               set listen port (default: 8000)
    -l, --host               set listen address or hostname (default: 0.0.0.0)
    -b, --backlog            set listen backlog (default: 511)
    -c, --config <filename>  set config file (default: ~/.cncrc)
    -d, --debug              run in debug mode
```

## Git Installation
If you prefer to use Git instead of `npm install`, You can create a local clone of the repository on your computer and sync from GitHub. Type the following commands to install and run `cnc.js`:
```bash
$ git clone https://github.com/cheton/cnc.js.git
$ cd cnc.js
$ npm install --production
$ ./bin/cnc
```

To update your local copy with latest changes, use:
```bash
$ git pull
$ npm install --production
$ ./bin/cnc
```

This is the fastest method to bring your local copy up-to-date.

## Docker Image Installation (x64 only)
Alternatively, you can install and run a Docker image within a Docker container. The first installation may take a long time to complete, but further updates will be much faster.

To install and set up cnc.js, take the following steps:

<b>Step 1:</b> Enter the following command to retrieve the latest version of the image:
```bash
$ docker pull cheton/cnc:latest
```

<b>Step 2:</b> Use the `docker run` command to create the Docker container and run the server, like so:
```bash
$ docker run --privileged -p 8000:8000 --rm --name cnc cheton/cnc:latest
```
By default a container is not allowed to access any devices, but a "privileged" container is given access to all devices on the host.

<b>Step 3:</b> If everything works fine, you should be able to view the web interface at `http://yourhostname:8000/`.

### Docker Images
https://hub.docker.com/r/cheton/cnc/tags/

### Tips

If you run into issues and need to restart the Docker container, use the following commands to first stop the Docker application, and then start it up again:
```bash
$ docker stop cnc
$ docker start cnc 
```

To view a list of all containers that are currently running in your Docker environment, use:
```bash
$ docker ps
```

To view all the images you have pulled into your Docker environment, use:
```bash
$ docker images
```

To delete containers in your Docker environment, use:
```bash
$ docker rm CONTAINER_ID
```

To delete images in your Docker environment, use:
```bash
$ docker rmi IMAGE_ID
```

To view the container in your terminal, use:
```bash
$ docker attach cnc
```