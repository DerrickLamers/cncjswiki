# Example Output from Installation on Raspberry Pi

## First command and its output

```
pi@pi2:~ $ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

## Installing the NodeSource Node.js 8.x LTS Carbon repo...


## Populating apt-get cache...

+ apt-get update
Get:1 http://archive.raspberrypi.org/debian stretch InRelease [25.4 kB]
Get:2 http://raspbian.raspberrypi.org/raspbian stretch InRelease [15.0 kB]
Get:3 https://deb.nodesource.com/node_10.x stretch InRelease [4,585 B]
Get:4 http://raspbian.raspberrypi.org/raspbian stretch/main armhf Packages [11.7 MB]
Get:5 https://deb.nodesource.com/node_10.x stretch/main armhf Packages [766 B]
Get:6 http://archive.raspberrypi.org/debian stretch/main armhf Packages [220 kB]
Fetched 11.9 MB in 27s (429 kB/s)
Reading package lists... Done

## Confirming "stretch" is supported...

+ curl -sLf -o /dev/null 'https://deb.nodesource.com/node_8.x/dists/stretch/Release'

## Adding the NodeSource signing key to your keyring...

+ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
OK

## Creating apt sources list file for the NodeSource Node.js 8.x LTS Carbon repo...

+ echo 'deb https://deb.nodesource.com/node_8.x stretch main' > /etc/apt/sources.list.d/nodesource.list
+ echo 'deb-src https://deb.nodesource.com/node_8.x stretch main' >> /etc/apt/sources.list.d/nodesource.list

## Running `apt-get update` for you...

+ apt-get update
Hit:1 http://archive.raspberrypi.org/debian stretch InRelease
Hit:2 http://raspbian.raspberrypi.org/raspbian stretch InRelease
Get:3 https://deb.nodesource.com/node_8.x stretch InRelease [4,620 B]
Get:4 https://deb.nodesource.com/node_8.x stretch/main Sources [762 B]
Get:5 https://deb.nodesource.com/node_8.x stretch/main armhf Packages [1,011 B]
Fetched 6,393 B in 3s (2,012 B/s)
Reading package lists... Done

## Run `sudo apt-get install -y nodejs` to install Node.js 8.x LTS Carbon and npm
## You may also need development tools to build native addons:
     sudo apt-get install gcc g++ make
## To install the Yarn package manager, run:
     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
     sudo apt-get update && sudo apt-get install yarn
```

## Second command and its output
```
pi@pi2:~ $ sudo apt install -y nodejs build-essential
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  build-essential nodejs
0 upgraded, 2 newly installed, 0 to remove and 102 not upgraded.
Need to get 12.4 MB of archives.
After this operation, 61.0 MB of additional disk space will be used.
Get:2 https://deb.nodesource.com/node_8.x stretch/main armhf nodejs armhf 8.16.2-1nodesource1 [12.4 MB]
Get:1 http://mirrors.ocf.berkeley.edu/raspbian/raspbian stretch/main armhf build-essential armhf 12.3 [7,342 B]
Fetched 12.4 MB in 5s (2,411 kB/s)
Selecting previously unselected package build-essential.
(Reading database ... 86685 files and directories currently installed.)
Preparing to unpack .../build-essential_12.3_armhf.deb ...
Unpacking build-essential (12.3) ...
Selecting previously unselected package nodejs.
Preparing to unpack .../nodejs_8.16.2-1nodesource1_armhf.deb ...
Detected old npm client, removing...
Unpacking nodejs (8.16.2-1nodesource1) ...
Setting up nodejs (8.16.2-1nodesource1) ...
Setting up build-essential (12.3) ...
Processing triggers for man-db (2.7.6.1-2) ...
```

## Third command and its output

```
pi@pi2:~ $ sudo npm install -g npm@latest
/usr/bin/npm -> /usr/lib/node_modules/npm/bin/npm-cli.js
/usr/bin/npx -> /usr/lib/node_modules/npm/bin/npx-cli.js
+ npm@6.13.4
added 65 packages from 19 contributors, removed 22 packages and updated 72 packages in 260.305s


   ╭────────────────────────────────────────────────────────────────╮
   │                                                                │
   │      New minor version of npm available! 6.11.3 → 6.13.4       │
   │   Changelog: https://github.com/npm/cli/releases/tag/v6.13.4   │
   │               Run npm install -g npm to update!                │
   │                                                                │
   ╰────────────────────────────────────────────────────────────────╯

```

## Fourth command and its output

```
pi@pi2:~ $ sudo npm install -g cncjs@latest --unsafe-perm
npm WARN deprecated bcrypt-nodejs@0.0.3: bcrypt-nodejs is no longer actively maintained. Please use bcrypt or bcryptjs. See https://github.com/kelektiv/node.bcrypt.js/wiki/bcrypt-vs-brypt.js to learn more about these two options
npm WARN deprecated superagent@3.8.3: Please note that v5.0.1+ of superagent removes User-Agent header by default, therefore you may need to add it yourself (e.g. GitHub blocks requests without a User-Agent header).  This notice will go away with v5.0.2+ once it is released.
npm WARN deprecated core-js@1.2.7: core-js@<2.6.8 is no longer maintained. Please, upgrade to core-js@3 or at least to actual version of core-js@2.
npm WARN deprecated core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
/usr/bin/cnc -> /usr/lib/node_modules/cncjs/bin/cnc
/usr/bin/cncjs -> /usr/lib/node_modules/cncjs/bin/cnc
/usr/bin/cncjs-server -> /usr/lib/node_modules/cncjs/bin/cnc

> core-js@2.6.11 postinstall /usr/lib/node_modules/cncjs/node_modules/@babel/runtime-corejs2/node_modules/core-js
> node -e "try{require('./postinstall')}catch(e){}"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
> https://opencollective.com/core-js
> https://www.patreon.com/zloirock

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)


> core-js@2.6.11 postinstall /usr/lib/node_modules/cncjs/node_modules/attr-accept/node_modules/core-js
> node -e "try{require('./postinstall')}catch(e){}"


> core-js@2.6.11 postinstall /usr/lib/node_modules/cncjs/node_modules/babel-polyfill/node_modules/core-js
> node -e "try{require('./postinstall')}catch(e){}"


> core-js@2.6.11 postinstall /usr/lib/node_modules/cncjs/node_modules/babel-runtime/node_modules/core-js
> node -e "try{require('./postinstall')}catch(e){}"


> core-js@2.6.11 postinstall /usr/lib/node_modules/cncjs/node_modules/opencollective/node_modules/core-js
> node -e "try{require('./postinstall')}catch(e){}"


> cncjs@1.9.20 postinstall /usr/lib/node_modules/cncjs
> opencollective postinstall


                           Thanks for installing cncjs
                 Please consider donating to our open collective
                        to help us maintain this package.

                            Number of contributors: 20
                               Number of backers: 8
                              Annual budget: US$ 481
                             Current balance: US$ 488

             Donate: https://opencollective.com/cheton/donate

npm WARN react-datepicker@1.5.0 requires a peer of react@^16.0.0 but none is installed. You must install peer dependencies yourself.
npm WARN react-datepicker@1.5.0 requires a peer of react-dom@^16.0.0 but none is installed. You must install peer dependencies yourself.

+ cncjs@1.9.20
added 3 packages from 10 contributors, removed 8 packages and updated 32 packages in 531.787s
```
