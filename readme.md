# What does it do?

It autoreloads Node.JS if any file changes.

Unlike most other utilites of this kind, it is based on inotify. So it hooks on new files automatically.

    $ npm install dev -g

    $ node-dev app.js

    Starting: app.js
    > server is listening on http://127.0.0.1:8080</pre>

`node-dev` will rerun app.js whenever one of the watched files is
changed.

No more switching to the terminal to rerun your code. Just change a file and
your code will be rerun.

This is especially nice for webservers, as you can skip the terminal and
alt-tab to the browser to see your updated code happily running.

A number of additional options make the module really flexible and extendible.

## Install

`npm install dev -g`

Global installation to have node-dev in path.

### Advanced usage

The node-dev is a tiny file which basically contains:

    var manager = require("dev")(options);
    manager.start();


The options are:

#### Running

- `run`: the js file to run, e.g `./app.js`, the only required option.

#### Watch/Ignore

- `watchDir`: the folder to watch, default: `.`
- `ignoredPaths` [ paths ]: array of ignored paths, which are not watched, members can be:
    * `string`, matched exactly against path, like `./public`,
    * `RegExp`, like extension check: `\.gif$`
    * `function(path)`, which takes the path and returns `true` if it should be ignored

#### Logging
- `debug`: adds additional output about watches and changes, default: `false`
- `logger`: custom logger object, must have `error(...)` and `debug(...)` methods, delegates to `console.log` by default. Can use any other logger.

#### Info
- `onRunOutput`: callback `function(output)`, called for `stdout` data from the running process
- `onRunError`: callback `function(output)`, called for `stderr` data from the running process

You can use these to send error notifications and integrate with your development environment if you wish.

### TODO

Tell me what to do?

