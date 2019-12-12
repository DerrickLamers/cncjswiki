
# Possible Install Problems & Solutions:

  * ##### [JSON ERROR](https://github.com/cncjs/cncjs/issues/326)
    * PROBLEM: Fails at `sudo npm install -g cncjs@latest --unsafe-perm`
    * SOLUTION: Try updating npm installer itself to the latest version (6.x), and removing the cache (~/.npm/) before installing CNCjs again. 
      ```
      npm i -g npm
      $ rm -rf ~/.npm/
      ```
  * ##### ETIMEDOUT
    * PROBLEM: My 'npm install' runs frequently fail with 'ETIMEDOUT' errors, like this:
      ```
      npm ERR! code ETIMEDOUT
      npm ERR! errno ETIMEDOUT
      npm ERR! network request to https://registry.npmjs.org/@trendmicro/react-paginations/-/react-paginations-0.6.1.tgz failed, reason: connec t ETIMEDOUT 151.101.24.162:443
      npm ERR! network This is a problem related to network connectivity.
      npm ERR! network In most cases you are behind a proxy or have bad network settings.
      npm ERR! network
      npm ERR! network If you are behind a proxy, please make sure that the
      npm ERR! network 'proxy' config is set properly.  See: 'npm help config'
      
      npm ERR! A complete log of this run can be found in:
      npm ERR!     /root/.npm/_logs/2017-11-23T18_29_57_208Z-debug.log
      ```
    
    * SOLUTION: Sometimes simply re-executing the 'npm install' commands works, but the following solution has also been suggested (see https://github.com/npm/npm/blob/5b4e3f3caff1f7ed30281b0b450de056a817bbc5/CHANGELOG.md#limit-concurrent-requests for more info):
      ```
      $ npm set maxsockets 3
      ```
      Then re-execute the 'npm install' command.  Supposedly the problem is that npm normally tries to fetch a lot of things at the same time, using say 50 simultaneous network connections, and that overloads some network routers.  Setting maxsockets to 3 makes it less aggressive and thus more likely to succeed.  When I tried it, the 'npm install' succeeded, and seemed to work faster than before.  I suspect that the Raspberry Pi does not have enough memory and CPU resources to run a lot of simultaneous connections efficiently.

