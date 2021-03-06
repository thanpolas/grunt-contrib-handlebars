# grunt-contrib-handlebars [![Build Status](https://secure.travis-ci.org/gruntjs/grunt-contrib-handlebars.png?branch=master)](http://travis-ci.org/gruntjs/grunt-contrib-handlebars)

> Precompile Handlebars templates to JST file.


## Getting Started
If you haven't used [grunt][] before, be sure to check out the [Getting Started][] guide, as it explains how to create a [gruntfile][Getting Started] as well as install and use grunt plugins. Once you're familiar with that process, install this plugin with this command:

```shell
npm install grunt-contrib-handlebars --save-dev
```

[grunt]: http://gruntjs.com/
[Getting Started]: http://gruntjs.com/getting-started


## Handlebars task
_Run this task with the `grunt handlebars` command._

_This task is a [multi task][] so any targets, files and options should be specified according to the [multi task][] documentation._
[multi task]: https://github.com/gruntjs/grunt/wiki/Configuring-tasks


_Version `0.4.x` of this plugin is compatible with Grunt `0.4.x`. Version `0.3.x` of this plugin is compatible with Grunt `0.3.x`._

### Options

#### separator
Type: `String`
Default: linefeed + linefeed

Concatenated files will be joined on this string.

#### namespace
Type: `String` `false`
Default: 'JST'

The namespace in which the precompiled templates will be assigned.  *Use dot notation (e.g. App.Templates) for nested namespaces or false for no namespace wrapping.*  When false with `amd` option set `true`, templates will be returned directly from the AMD wrapper.

Example:
```js
options: {
  namespace: 'MyApp.Templates'
}
```

#### wrapped
Type: `Boolean`
Default: `false`

Determine if preprocessed template functions will be wrapped in Handlebars.template function.

#### node
Type: `Boolean`
Default: `false`

Enable the compiled file to be required on node.js by preppending and appending proper declarations. You can use the file safely on the front-end.

For this option to work you need to define the `namespace` option.

#### amd
Type: `Boolean`
default: `false`

Wraps the output file with an AMD define function and returns the compiled template namespace unless namespace has been explicitly set to false in which case the template function will be returned directly.

```js
define(function() {
    //...//
    returns this['[template namespace]'];
});
```

#### processContent
Type: `function`

This option accepts a function which takes one argument (the template file content) and returns a string which will be used as the source for the precompiled template object.  The example below removes leading and trailing spaces to shorten templates.

```js
options: {
  processContent: function(content) {
    content = content.replace(/^[\x20\t]+/mg, '').replace(/[\x20\t]+$/mg, '');
    content = content.replace(/^[\r\n]+/, '').replace(/[\r\n]*$/, '\n');
    return content;
  }
}
```

#### processName
Type: `function`

This option accepts a function which takes one argument (the template filepath) and returns a string which will be used as the key for the precompiled template object.  The example below stores all templates on the default JST namespace in capital letters.

```js
options: {
  processName: function(filename) {
    return filename.toUpperCase();
  }
}
```

#### processPartialName
Type: ```function```

This option accepts a function which takes one argument (the partial filepath) and returns a string which will be used as the key for the precompiled partial object when it is registered in Handlebars.partials. The example below stores all partials using only the actual filename instead of the full path.

```js
options: {
  processPartialName: function(filePath) { // input:  templates/_header.hbs
    var pieces = filePath.split("/");
    return pieces[pieces.length - 1]; // output: _header.hbs
  }
}
````

Note: If processPartialName is not provided as an option the default assumes that partials will be stored by stripping trailing underscore characters and filename extensions. For example, the path *templates/_header.hbs* will become *header* and can be referenced in other templates as *{{> header}}*.

#### partialRegex
Type: `Regexp`
Default: /^_/

This option accepts a regex that defines the prefix character that is used to identify Handlebars partial files.

``` javascript
// assumes partial files would be prefixed with "par_" ie: "par_header.hbs"
options: {
  partialRegex: /^par_/
}
```

### Usage Examples

```js
handlebars: {
  compile: {
    options: {
      namespace: "JST"
    },
    files: {
      "path/to/result.js": "path/to/source.hbs",
      "path/to/another.js": ["path/to/sources/*.hbs", "path/to/more/*.hbs"]
    }
  }
}
```


## Release History

 * 2013-01-31   v0.5.4rc7   When `namespace` is false and `amd` is true, return handlebars templates directly from AMD wrapper.
 * 2013-01-31   v0.5.3rc7   Add `node` option to produce dual node.js / front-end compiled file.
 * 2013-01-28   v0.5.2rc7   Define handlebars as a dependency for AMD option.
 * 2013-01-27   v0.5.1rc7   Add AMD compilation option. Add processContent option. Do not generate templates into a namespaces when namespace option is false.
 * 2013-01-22   v0.5.0rc7   Updating grunt/gruntplugin dependencies to rc7. Changing in-development grunt/gruntplugin dependency versions from tilde version ranges to specific versions. Default wrapped option to true.
 * 2013-01-08   v0.4.0rc5   Updating to work with grunt v0.4.0rc5. Switching to this.files api.
 * 2012-11-20   v0.3.3   Reset for each target
 * 2012-10-11   v0.3.2   Rename grunt-contrib-lib dep to grunt-lib-contrib.
 * 2012-10-02   v0.3.1   Bugfix default processPartialName.
 * 2012-09-22   v0.3.0   Options no longer accepted from global config key.
 * 2012-09-15   v0.2.3   Support for nested namespaces.
 * 2012-09-11   v0.2.2   Escape single quotes in filenames.
 * 2012-09-09   v0.2.0   Refactored from grunt-contrib into individual repo.

---

Task submitted by [Tim Branyen](http://tbranyen.com)

*This file was generated on Fri Feb 08 2013 10:35:22.*
