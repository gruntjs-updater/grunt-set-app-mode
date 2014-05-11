# grunt-set-app-mode

[![Built with Grunt](https://cdn.gruntjs.com/builtwith.png)](http://gruntjs.com/)

[![NPM](https://nodei.co/npm/grunt-set-app-mode.png?downloads=true&stars=true)](https://nodei.co/npm/grunt-set-app-mode/)

> Deploys run mode-specific files, e.g. src/config.staging.js to build/config.js

It is a problem if code for one run mode (dev) runs in another (prod). At the very least, the databases and user credentials can be different. One way of avoiding this problem is using run mode-specific configuration files which specify the relevant details.

This Grunt plugin simplifies deploying these files and also ensures that stray source files (config.dev.js, config.prod.js etc) also aren't present in the built project. The alternative is a mess of copy and clean tasks and a more confusing Gruntfile. See `example-gruntfile/Gruntfile.orig.js` and `example-gruntfile/Gruntfile.set-app-mode.js` for an example of how the `grunt-set-app-mode` plugin can simplify a Gruntfile and make its intent clearer.


## Getting Started

This plugin requires Grunt `~0.4.4`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-set-app-mode --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-set-app-mode');
```


## The "set_app_mode" task

### Overview
In your project's Gruntfile, add a section named `set_app_mode` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
    set_app_mode: {
      set: {
        expected_modes: [ "dev", "staging", "prod" ]
        files: [
          {
            src: "src/server/config.{{MODE}}.js",
            dest: "build/server"
          },
          {
            src: "src/scripts/upstart/simple-error-server.{{MODE}}.override",
            dest: "build/scripts/upstart"
          }
        ]
      }
    }
});
```

Note that you must use the "File Array" format ([details here](http://gruntjs.com/configuring-tasks#files-array-format)) to specify the location of the run mode-specific files and their destination directory. The processed Grunt task files property is not used in the code because Grunt cannot easily glob across files with "{{MODE}}".

### Options

#### options.mode
Type: `String`
Default value: None, this is normally not specified in the Gruntfile but via the command line

#### options.expected_modes
Type: `Array of String`
Default value: `[ "dev", "staging", "prod" ]'`

### Usage Examples

See the unit tests for a range of examples. In normal usage, define `--mode=DESIRED_MODE` in the command line used to run grunt. E.g.: `grunt test build deploy --mode=prod`.

#### Using the default options

In this example, the default modes are used to ensure the correct config file for the build target is included in the final build. Any files matching the pattern `config.{{MODE}}.js` for one of the expected (default) modes will be removed from the destination folder.

```js
grunt.initConfig({
  set_app_mode: {
    files: [
      {
        src: "src/config.{{MODE}}.js",
        dest: "build"
      }
    ]
  },
});
```


## Contributing


In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).


## Release History
- _0.1.0_ - Initial release

## License

Copyright (c) 2014 Christo Fogelberg

Licensed under the MIT License
