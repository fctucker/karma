## Overview
In order to serve you well, Karma needs to know about your project in order to test it
and this is done via a configuration file. The easiest way to generate an initial configuration file
is by using the `karma init` command. This page lists all of the available configuration options.

Note: Most of the framework adapters, reporters, preprocessors and launchers needs to be loaded as [plugins].


The Karma configuration file can be written in JavaScript or CoffeeScript and is loaded as a regular Node.js module.

Unless provided as argument, the Karma CLI will look for a configuration file at

* `./karma.conf.js`
* `./karma.conf.coffee`
* `./karma.conf.ts`
* `./.config/karma.conf.js`
* `./.config/karma.conf.coffee`
* `./.config/karma.conf.ts`

in that order.

Within the configuration file, the configuration code is put together by setting `module.exports` to point to a function
which accepts one argument: the configuration object.

```javascript
// karma.conf.js
module.exports = function(config) {
  config.set({
    basePath: '../..',
    frameworks: ['jasmine'],
    //...
  });
};
```

```coffeescript
# karma.conf.coffee
module.exports = (config) ->
  config.set
    basePath: '../..'
    frameworks: ['jasmine']
    # ...
```

```typescript
# karma.conf.ts
module.exports = (config) => {
  config.set({
    basePath: '../..',
    frameworks: ['jasmine'],
    //...
  });
}
```

## File Patterns
All of the configuration options, which specify file paths, use the [minimatch][minimatch] library to facilitate flexible
but concise file expressions so you can easily list all of the files you want to include and exclude.

You can find details about each configuration option in the section below. The following options utilize minimatch expressions:

 * `exclude`
 * `files`
 * `preprocessors`

Examples:

 * `**/*.js`: All files with a "js" extension in all subdirectories
 * `**/!(jquery).js`: Same as previous, but excludes "jquery.js"
 * `**/(foo|bar).js`: In all subdirectories, all "foo.js" or "bar.js" files

## Configuration Options
These are all of the available configuration options.

## autoWatch
**Type:** Boolean

**Default:**  `true`

**CLI:** `--auto-watch`, `--no-auto-watch`

**Description:** Enable or disable watching files and executing the tests whenever one of these files changes.


## autoWatchBatchDelay
**Type:** Number

**Default:**  `250`

**Description:** When Karma is watching the files for changes, it tries to batch
multiple changes into a single run so that the test runner doesn't try to start and restart running
tests more than it should. The configuration setting tells Karma how long to wait (in milliseconds) after any changes
have occurred before starting the test process again.


## basePath
**Type:** String

**Default:** `''`

**Description:** The root path location that will be used to resolve all relative
paths defined in `files` and `exclude`. If the `basePath` configuration is a
relative path, then it will be resolved to the `__dirname` of the configuration file.


## browserDisconnectTimeout
**Type:** Number

**Default:** `2000`

**Description:** How long does Karma wait for a browser to reconnect (in ms).

With a flaky connection, it is pretty common that the browser disconnects, but the actual test execution is still running
without any problems. Karma does not treat a disconnection as an immediate failure and will wait for `browserDisconnectTimeout` (ms).
If the browser reconnects during that time, everything is fine.


## browserConsoleLogOptions
**Type:** Object

**Default:** `{level: "debug", format: "%b %T: %m", terminal: true}`

**Description:** Configure how the browser console is logged with the following
properties, all of which are optional:

```javascript
{
  level:  string,
  format: string,
  path:   string,
  terminal: boolean
}
```


Here the `level` is the desired log-level, where level `log` always is logged. The format
is a string where `%b`, `%t`, `%T`, and `%m` are replaced with the browser string,
log type in lower-case, log type in uppercase, and log message, respectively. This format will
only effect the output file. `path` is the output-path of the output-file, and `terminal` indicates
if the log should be written in the terminal, or not.


## browserDisconnectTolerance
**Type:** Number

**Default:** `0`

**Description:** The number of disconnections tolerated.

The `disconnectTolerance` value represents the maximum number of tries a browser will attempt in the case of a disconnection.
Usually, any disconnection is considered a failure, but this option allows you to define a tolerance level when there is
a flaky network link between the Karma server and the browsers.


## browserNoActivityTimeout
**Type:** Number

**Default:** `10000`

**Description:** How long will Karma wait for a message from a browser before disconnecting from it (in ms).

If, during test execution, Karma does not receive any message from a browser within `browserNoActivityTimeout`(ms), it will disconnect from the browser.


## browsers
**Type:** Array

**Default:**  `[]`

**CLI:** `--browsers Chrome,Firefox`

**Possible Values:**

  * `Chrome` (launcher comes installed with Karma)
  * `ChromeCanary` (launcher comes installed with Karma)
  * `PhantomJS` (launcher comes installed with Karma)
  * `Firefox` (launcher requires karma-firefox-launcher plugin)
  * `Opera` (launcher requires karma-opera-launcher plugin)
  * `IE` (launcher requires karma-ie-launcher plugin)
  * `Safari` (launcher requires karma-safari-launcher plugin)

**Description:** A list of browsers to launch and capture. When Karma starts up, it will also start up each browser
which is placed within this setting. Once Karma is shut down, it will shut down these
browsers as well. You can capture any browser manually by opening the browser and visiting the URL where
the Karma web server is listening (by default it is `http://localhost:9876/`).

See [config/browsers] for more information. Additional launchers can be defined through [plugins].


## captureTimeout
**Type:** Number

**Default:** `60000`

**Description:** Timeout for capturing a browser (in ms).

The `captureTimeout` value represents the maximum boot-up time allowed for a browser to start and connect to Karma.
If any browser does not get captured within the timeout, Karma will kill it and try to launch
it again and, after three attempts to capture it, Karma will give up.

## client.args
**Type:** Array

**Default:** `undefined`

**CLI:** All arguments after `--` (only when using `karma run`)

**Description:** When `karma run` is passed additional arguments on the command-line, they
are passed through to the test adapter as `karma.config.args` (an array of strings).
The `client.args` option allows you to set this value for actions other than `run`.

How this value is used is up to your test adapter - you should check your adapter's documentation to see how (and if) it uses this value.


## client.useIframe
**Type:** Boolean

**Default:** `true`

**Description:** Run the tests inside an iFrame or a new window

If true, Karma runs the tests inside an iFrame. If false, Karma runs the tests in a new window. Some tests may not run in an
iFrame and may need a new window to run.


## client.captureConsole
**Type:** Boolean

**Default:** `true`

**Description:** Capture all console output and pipe it to the terminal.


## client.clearContext
**Type:** Boolean

**Default:** `true`

**Description:** Clear the context window

If true, Karma clears the context window upon the completion of running the tests. If false, Karma does not clear the context window
upon the completion of running the tests. Setting this to false is useful when embedding a Jasmine Spec Runner Template.

## colors
**Type:** Boolean

**Default:**   `true`

**CLI:** `--colors`, `--no-colors`

**Description:** Enable or disable colors in the output (reporters and logs).


## concurrency
**Type:** Number

**Default:** `Infinity`

**Description:** How many browsers Karma launches in parallel.

Especially on services like SauceLabs and Browserstack, it makes sense only to launch a limited amount of browsers at once, and only start more when those have finished. Using this configuration, you can specify how many browsers should be running at once at any given point in time.


## customContextFile
**Type:** string

**Default:** `null`

**Description:** If `null` (default), uses karma's own `context.html` file.


## customDebugFile
**Type:** string

**Default:** `null`

**Description:** If `null` (default), uses karma's own `debug.html` file.


## customHeaders
**Type:** Array

**Default:** `undefined`

**Description** Custom HTTP headers that will be set upon serving files by Karma's web server.
Custom headers are useful, especially with upcoming browser features like Service Workers.

The `customHeaders` option allows you to set HTTP headers for files that match a regular expression.
`customHeaders` is an array of `Objects` with properties as follows:

* match: Regular expression string to match files
* name: HTTP header name
* value: HTTP header value

**Example:**
```javascript
customHeaders: [{
  match: '.*foo.html',
  name: 'Service-Worker-Allowed',
  value: '/'
}]
```


## detached
**Type:** Boolean

**Default:** `false`

**CLI:** `--detached`

**Description:** When true, this will start the karma server in another process, writing no output to the console.
The server can be stopped using the `karma stop` command.


## exclude
**Type:** Array

**Default:** `[]`

**Description:** List of files/patterns to exclude from loaded files.

## failOnEmptyTestSuite
**Type:** Boolean

**Default:** `true`

**CLI:** `--fail-on-empty-test-suite`, `--no-fail-on-empty-test-suite`

**Description:** Enable or disable failure on running empty test-suites. If disabled the program
will return exit-code `0` and display a warning.

## files
**Type:** Array

**Default:** `[]`

**Description:** List of files/patterns to load in the browser.

See [config/files] for more information.

## forceJSONP
**Type:** Boolean

**Default:** `false`

**Description:** Force socket.io to use JSONP polling instead of XHR polling.

## frameworks
**Type:** Array

**Default:** `[]`

**Description:** List of test frameworks you want to use. Typically, you will set this to `['jasmine']`, `['mocha']` or `['qunit']`...

Please note just about all frameworks in Karma require an additional plugin/framework library to be installed (via NPM).

Additional information can be found in [plugins].


## hostname
**Type:** String

**Default:** `'localhost'`

**Description:** Hostname to be used when capturing browsers.


## httpsServerOptions
**Type:** Object

**Default:** `{}`

**Description:** Options object to be used by Node's `https` class.

Object description can be found in the [NodeJS.org API docs](https://nodejs.org/api/tls.html#tls_tls_createserver_options_secureconnectionlistener)

**Example:**
```javascript
httpsServerOptions: {
  key: fs.readFileSync('server.key', 'utf8'),
  cert: fs.readFileSync('server.crt', 'utf8')
},
```

## logLevel
**Type:** Constant

**Default:** `config.LOG_INFO`

**CLI:** `--log-level debug`

**Possible values:**

  * `config.LOG_DISABLE`
  * `config.LOG_ERROR`
  * `config.LOG_WARN`
  * `config.LOG_INFO`
  * `config.LOG_DEBUG`

**Description:** Level of logging.


## loggers
**Type:** Array

**Default:** `[{type: 'console'}]`

**Description:** A list of log appenders to be used. See the documentation for [log4js] for more information.


## middleware
**Type:** Array

**Default:** `[]`

**Description:** List of names of additional middleware you want the karma server to use. Middleware will be used in the order listed.

You must have installed the middleware via a plugin/framework (either inline or via NPM). Additional information can be found in [plugins].

The plugin must provide an express/connect middleware function (details about this can be found in [the Express docs](http://expressjs.com/guide/using-middleware.html). An example of custom inline middleware is shown below.

**Example:**
```javascript
var CustomMiddlewareFactory = function (config) {
  return function (request, response, /* next */) {
    response.writeHead(200)
    return response.end("content!")
  }
}
```

```javascript
middleware: ['custom']
plugins: [
  {'middleware:custom': ['factory', CustomMiddlewareFactory]}
  ...
]
```


## mime
**Type:** Object

**Default:** `{}`

**Description:** Redefine default mapping from file extensions to MIME-type

Set property name to required MIME, provide Array of extensions (without dots) as it's value

**Example:**
```javascript
mime: {
   'text/x-typescript': ['ts','tsx']
   'text/plain' : ['mytxt']
}
```


## beforeMiddleware
**Type:** Array

**Default:** `[]`

**Description:** This is the same as middleware except that these middleware will be run before karma's own middleware.


## plugins
**Type:** Array

**Default:** `['karma-*']`

**Description:** List of plugins to load. A plugin can be a string (in which case it will be required by Karma) or an inlined plugin - Object.
By default, Karma loads all sibling NPM modules which have a name starting with `karma-*`.

Note: Just about all plugins in Karma require an additional library to be installed (via NPM).

See [plugins] for more information.


## port
**Type:** Number

**Default:** `9876`

**CLI:** `--port 9876`

**Description:** The port where the web server will be listening.


## preprocessors
**Type:** Object

**Default:** `{'**/*.coffee': 'coffee'}`

**Description:** A map of preprocessors to use.

Preprocessors can be loaded through [plugins].

Note: Just about all preprocessors in Karma (other than CoffeeScript and some other defaults)
require an additional library to be installed (via NPM).

Be aware that preprocessors may be transforming the files and file types that are available at run time. For instance,
if you are using the "coverage" preprocessor on your source files, if you then attempt to interactively debug
your tests, you'll discover that your expected source code is completely changed from what you expected.  Because
of that, you'll want to engineer this so that your automated builds use the coverage entry in the "reporters" list,
but your interactive debugging does not.

Click <a href="preprocessors.html">here</a> for more information.


## protocol
**Type:** String

**Default:** `'http:'`

**Possible Values:**

* `http:`
* `https:`

**Description:** Protocol used for running the Karma webserver.

Determines the use of the Node `http` or `https` class.

Note: Using `'https:'` requires you to specify `httpsServerOptions`.


## proxies
**Type:** Object

**Default:** `{}`

**Description:** A map of path-proxy pairs.

The proxy can be specified directly by the target url or path, or with an object to configure more options. The available options are:

- `target` The target url or path (mandatory)
- `changeOrigin` Whether or not the proxy should override the Host header using the host from the target (default `false`)

**Example:**
```javascript
proxies: {
  '/static': 'http://gstatic.com',
  '/web': 'http://localhost:9000',
  '/img/': '/base/test/images/',
  '/proxyfied': {
    'target': 'http://myserver.localhost',
    'changeOrigin': true
  }
},
```


## proxyValidateSSL
**Type:** Boolean

**Default:** `true`

**Description:** Whether or not Karma or any browsers should raise an error when an invalid SSL certificate is found.


## reportSlowerThan
**Type:** Number

**Default:** `0`

**Description:** Karma will report all the tests that are slower than given time limit (in ms).
This is disabled by default (since the default value is 0).


## reporters
**Type:** Array

**Default:** `['progress']`

**CLI:** `--reporters progress,growl`

**Possible Values:**

  * `dots`
  * `progress`

**Description:** A list of reporters to use.

Additional reporters, such as `growl`, `junit`, `teamcity` or `coverage` can be loaded through [plugins].

Note: Just about all additional reporters in Karma (other than progress) require an additional library to be installed (via NPM).


## restartOnFileChange
**Type:** Boolean

**Default:** `false`

**Description:** When Karma is watching the files for changes, it will delay a new run until
the current run is finished. Enabling this setting will cancel the current run and start a new run
immediately when a change is detected.


## retryLimit
**Type:** Number

**Default:** 2

**Description:** When a browser crashes, karma will try to relaunch. This defines how many times karma should relaunch
a browser before giving up.

## singleRun
**Type:** Boolean

**Default:** `false`

**CLI:** `--single-run`, `--no-single-run`

**Description:** Continuous Integration mode.

If `true`, Karma will start and capture all configured browsers, run tests and then exit with an exit code of `0` or `1` depending
on whether all tests passed or any tests failed.



## transports
**Type:** Array

**Default:** `['polling', 'websocket']`

**Description:** An array of allowed transport methods between the browser and testing server. This configuration setting
is handed off to [socket.io](http://socket.io/) (which manages the communication
between browsers and the testing server).


## urlRoot
**Type:** String

**Default:** `'/'`

**Description:** The base url, where Karma runs.

All of Karma's urls get prefixed with the `urlRoot`. This is helpful when using proxies, as
sometimes you might want to proxy a url that is already taken by Karma.


## jsVersion
**Type:** Number

**Default:** `0`

**Description:** The JavaScript version to use in the Firefox browser.

If `> 0`, Karma will add a JavaScript version tag to the included JavaScript files.

Note: This will only be applied to the Firefox browser. It is currently the only browser that supports the version tag.


[plugins]: plugins.html
[config/files]: files.html
[config/browsers]: browsers.html
[config/preprocessors]: preprocessors.html
[log4js]: https://github.com/nomiddlename/log4js-node
[minimatch]: https://github.com/isaacs/minimatch
