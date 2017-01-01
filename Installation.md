## Installation

### [Raspberry Pi Setup Guide](https://github.com/cheton/cnc/wiki/Raspberry-Pi-Setup-Guide)
We have a dedicated setup guide for setting up Node.js, NVM, CNC.js, Autostart with pm2, all tested on the Raspberry Pi. Just use the link above in the title to get to the separate article.

--------------------

### Node.js Installation

Node.js v4 or higher is recommended. You can install [Node Version Manager](https://github.com/creationix/nvm) to manage multiple Node.js versions. If you have `git` installed, just clone the `nvm` repo, and check out the latest version:
```
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
cd ..
. ~/.nvm/nvm.sh
nvm install 4
```

Add these lines to your `~/.bash_profile`, `~/.bashrc`, or `~/.profile` file to have it automatically sourced upon login: 
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

If you're using Node.js v4 or earlier versions, it's recommended that you use npm v3 to install packages. To upgrade, run:
```
npm install npm@latest -g
```

### Getting Started

Make sure you're using Node.js v4 (or higher) and npm v3:
```
$ nvm use 4
Now using node v4.5.0 (npm v3.10.6)
```

Install `cncjs` without `sudo`, or the `serialport` module may not install correctly on some platforms like Raspberry Pi.
```
npm install -g cncjs
```

It's recommended that you run [Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) on the RPi2 or RPi3. For Raspbian Wheezy, be sure to [install gcc/g++ 4.8](https://somewideopenspace.wordpress.com/2014/02/28/gcc-4-8-on-raspberry-pi-wheezy/) before npm install.

Check out [Git Installation](https://github.com/cheton/cnc.js#git-installation) and [Docker Image Installation (x64 only)](https://github.com/cheton/cnc.js#docker-image-installation-x64-only) for other installation methods.

### Upgrade
Run `npm install -g cncjs@latest` to install the latest version. To determine the version, use cnc -V.

Maybe you have to uninstall your cncjs version first in case of errors after upgrade, see
[Installing a Specific Version](https://github.com/cheton/cnc/wiki/Installation#installing-a-specific-version-of-cncjs-release)

### Usage
Run `cnc` or `~/.npm/bin/cnc` to start the server, and visit `http://yourhostname:8000/` to view the web interface:
```
$ cnc
2016-12-01T15:00:41.499Z - info: Started the server at http://192.168.1.100:8000
```

Run `cnc` with -h for detailed usage:
```
$ cnc -h

  Usage: cnc [options]
  
  Options:

    -h, --help                          output usage information
    -V, --version                       output the version number
    -p, --port                          set listen port (default: 8000)
    -l, --host                          set listen address or hostname (default: 0.0.0.0)
    -b, --backlog                       set listen backlog (default: 511)
    -c, --config <filename>             set config file (default: ~/.cncrc)
    -v, --verbose                       increase the verbosity level
    -m, --mount [<url>:]<path>          set the mount point for serving static files (default: /static:static)
    -w, --watch-directory <path>        watch a directory for changes
    --access-token-lifetime <lifetime>  access token lifetime in seconds or a time span string (default: 30d)
    --allow-remote-access               allow remote access to the server

  Examples:

    $ cnc -vv
    $ cnc --mount /pendant:/home/pi/tinyweb
    $ cnc --watch-directory /home/pi/watch
    $ cnc --access-token-lifetime 60d  # e.g. 3600, 30m, 12h, 30d
    $ cnc --allow-remote-access
```

Instead of passing command line options for --watch-directory, --access-token-lifetime, and --allow-remote-access, you can create a ~/.cncrc file that contains the following configuration in JSON format:
```json
{
    "watchDirectory": "/path/to/dir",
    "accessTokenLifetime": "30d",
    "allowRemoteAccess": false
}
```

Check out an example configuration file [here](https://github.com/cheton/cnc/blob/master/examples/.cncrc).

If you need view detailed logs for troubleshooting, you can run the server in debug mode.
```
$ cnc -vv
```

## Git Installation
If you prefer to use Git instead of `npm install`, You can create a local clone of the repository on your computer and sync from GitHub. Type the following commands to install and run `cnc`:
```
git clone https://github.com/cheton/cnc.git
cd cnc
git checkout master
npm install
npm run prepublish
./bin/cnc
```

To update your local copy with latest changes, use:
```
git checkout master
git pull origin master
npm install
npm run prepublish
./bin/cnc
```

This is the fastest method to bring your local copy up-to-date.

## Docker Image Installation (x64 only)
Alternatively, you can install and run a Docker image within a Docker container. The first installation may take a long time to complete, but further updates will be much faster.

To install and set up cnc, take the following steps:

<b>Step 1:</b> Enter the following command to retrieve the latest version of the image:
```
docker pull cheton/cnc:latest
```

<b>Step 2:</b> Use the `docker run` command to create the Docker container and run the server, like so:
```
docker run --privileged -p 8000:8000 --rm --name cnc cheton/cnc:latest
```
By default a container is not allowed to access any devices, but a "privileged" container is given access to all devices on the host.

<b>Step 3:</b> If everything works fine, you should be able to view the web interface at `http://yourhostname:8000/`.

### Docker Images
https://hub.docker.com/r/cheton/cnc/tags/

### Tips

If you run into issues and need to restart the Docker container, use the following commands to first stop the Docker application, and then start it up again:
```
docker stop cnc
docker start cnc 
```

To view a list of all containers that are currently running in your Docker environment, use:
```
docker ps
```

To view all the images you have pulled into your Docker environment, use:
```
docker images
```

To delete containers in your Docker environment, use:
```
docker rm CONTAINER_ID
```

To delete images in your Docker environment, use:
```
docker rmi IMAGE_ID
```

To view the container in your terminal, use:
```
docker attach cnc
```

------- 

### Installing a Specific Version of [CNC.js Release](https://github.com/cheton/cnc/releases)
```
# Removed Current Version of CNC.js
npm uninstall -g cncjs

# Install Specific Version of CNC.js
npm install -g cncjs@1.6.3  --unsafe-perm
```