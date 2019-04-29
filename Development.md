# Development Tips

You can compile cncjs in development mode to enable some node.js debugging tools, but there are some tricks you can use to do effective debugging with a production compile. If you live and breathe javascript and node.js development, you probably know much better ways to debug, but for the rest of us "casual javascripters", the following tips may be helpful.

## Debugging the browser app
In the Chrome browser, you can enable Developer Tools to set breakpoints in the cncjs application code, view the console log, etc.  Developer Tools is found via the "Customize and Control" button in the upper right corner, under More Tools > Developer tools.  The shortcut under Windows is Ctrl-Shift-i.

## Debugging the cncjs server
Most of the many .js and .jsx files that comprise cncjs are bundled into one combined file in dist/cncjs/server/index.js .  Instead of changing an individual source file and recompiling, you can edit that index.js directly to make simple test changes like adding "log.info()" printouts or adding/removing a few lines of code. Then just kill and restart the cncjs process to pick up the changes.  This is especially useful when you are running the server on a Raspberry Pi, where rebuilding takes forever.  You should make a backup copy of the unmodified index.js just in case, since that file is not under source control (or you could check it in to git).
The combined code in index.js doesn't look exactly like the source but it is close enough that you can usually figure it out.  CNCjs is written using very modern Ecmascript constructs, then "transpiled" (using Babel) into older Javascript so it will run on more systems.  The combined index.js is in the older Javascript form.  Fortunately, the comments are preserved, which is helpful for finding the right place in the giant file.
