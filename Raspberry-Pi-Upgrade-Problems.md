Upgrading a Raspberry Pi to a new OS version can sometimes cause problems due to the Pi's limited SD card storage size.  Here are some solutions.

### Slowness / File System Filled up
After upgrading from Raspbian wheezy to Raspbian jessie, the system ran very slowly.  Just logging in took a long time, command execution was sluggish, and after a few minutes the file system filled up and things stopped working.  It turns out that the upgrade changed the size of the swap file to a size that was much too large for the SD card.  The fix was to edit the file /etc/dphys-swapfile (e.g. with 'sudo nano /etc/dphys-swapfile'), changing its last line from
```
#CONF-MAXSWAP=2048
```
to
```
CONF-MAXSWAP=100
```
That changed the swap file size from unlimited (because the line was originally commented out) to 100 MB.
After that change and a reboot, the system stabilized and ran at its normal speed.

### Wasted Disk Space
After upgrading and fixing the Slowness problem above, the root filesystem was still fairly full - 'df .' reported 77% full.  The fix was:
```
$ sudo apt-get clean
$ df .
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/root        6581636 3593648   2630612  58% /
```
That removed some files in /var/cache/apt that were used during the upgrade but are no longer needed.