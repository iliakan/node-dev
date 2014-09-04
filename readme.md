
# Deprecated

As of now, this module is effectively deprecated. 
I recommend https://github.com/remy/nodemon.

# Linux only

The module relies on inotify, only available for Linux.

For other OS I'd recommend supervisor or nodemon.

# What does it do?

It autoreloads Node.JS in case of any file changes. 


    $ npm install dev -g

    $ node-dev app.js

    Starting: app.js
    > server is listening on http://127.0.0.1:8080

`node-dev` will rerun `app.js` whenever one of the watched files is
changed.

The module is based on inotify. So, unlike most other modules of this kind, *it starts watching new files automatically*. 

A number of additional options make the module really flexible and extendible.

## Install

`npm install dev -g`

Global installation is preferred to have `node-dev` utility in path.

### Simple usage

If you've installed it globally, then there is a `node-dev` on path, so chdir to your app and run it:

    $ node-dev app.js

    Starting: app.js
    > server is listening on http://127.0.0.1:8080

Then go to your IDE and edit files. `node-dev` keeps your app up-to-date. The only need to switch to terminal is when there are errors.

But even if there are errors, you can switch back to IDE, correct them and `node-dev` will autorestart the server again for you.

By default, files under `./public`, files with extensions `.db, .dirtydb`, files and directories starting with dot `.` are not watched.

### Advanced usage

The `node-dev` utility is a tiny file which basically contains two lines:

    var manager = require("dev")(options);
    manager.start();

You can copy and modify it, or create your own, more featured autorestarter on it's base.
The `options` object may have following properties:

#### Running

- `run`: the js file to run, e.g `./app.js`, it is the only required option.

#### Watch/Ignore

- `watchDir`: the folder to watch recursively, default: `.`
- `ignoredPaths` [ paths ]: array of ignored paths, which are not watched, members can be:
    * `string`, matched exactly against path, like `./public`,
    * `RegExp`, e.g an extension check: `/\.gif$/`
    * `function(path)`, which takes the path and returns `true` if it should be ignored

#### Logging
- `debug`: enables additional logging output about watches and changes, default: `false`
- `logger`: custom logger object, must have `error(...)` and `debug(...)` methods, delegates to `console.log` by default. Can use any other logger.

#### Info
- `onRunOutput`: callback `function(output)`, called for `stdout` data from the running process
- `onRunError`: callback `function(output)`, called for `stderr` data from the running process

You can use these to send error notifications and integrate with your development environment if you wish.

### Troubleshooting

This module doesn't compile/run on non-Linux OS. See the head of this file for the details.

There are limits on the number of watched files in inotify. For example, Debian has 8192 by default. In most cases, that should be enough. If it's not, and you really really need to watch so many files, then you can adjust the limit.

To change the limit:

    $ echo 16384 > /proc/sys/fs/inotify/max_user_watches

Or:

    $ sudo sysctl fs.inotify.max_user_watches=16364

To make the change permanent, edit the file `/etc/sysctl.conf` and add this line to the end of the file:

    fs.inotify.max_user_watches=16384



