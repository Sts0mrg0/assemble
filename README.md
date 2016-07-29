<p align="center">

<a href="https://github.com/assemble/assemble">
<img height="250" width="250" src="https://raw.githubusercontent.com/assemble/assemble/master/docs/logo.png">
</a>
</p>

# assemble

[![NPM version](https://img.shields.io/npm/v/assemble.svg?style=flat)](https://www.npmjs.com/package/assemble) [![NPM downloads](https://img.shields.io/npm/dm/assemble.svg?style=flat)](https://npmjs.org/package/assemble) [![Build Status](https://img.shields.io/travis/assemble/assemble.svg?style=flat)](https://travis-ci.org/assemble/assemble) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/assemble/assemble)

> Get the rocks out of your socks! Assemble makes you fast at creating web projects. Assemble is used by thousands of projects for rapid prototyping, creating themes, scaffolds, boilerplates, e-books, UI components, API documentation, blogs, building websites / static site generator, alternative to jekyll for gh-pages and more! Assemble can also be used with gulp and grunt.

Welcome to Assemble! We're happy to announce the release of Assemble v0.16.2! If you're new to assemble, the [About](#about) section and [FAQ](#faq) might be a good place to start.

## Developer toolkit

Assemble can be used standalone, or alongside the following libraries as part of a build workflow:

* [generate](https://github.com/generate/generate): create projects
* [assemble](https://github.com/assemble/assemble): build projects (<= you are here)
* [verb](https://github.com/verbose/verb): document projects
* [update](https://github.com/update/update): maintain projects

_(All of these applications are built on [base](https://github.com/node-base/base).)_

## What is Assemble?

Assemble makes it easy to create, customize, generate and maintain complete web projects. Here are some highlights:

* **Render templates** using any template engine, including [handlebars](http://www.handlebarsjs.com/), [lodash](https://lodash.com/), [swig](https://github.com/paularmstrong/swig) and [pug](http://jade-lang.com), and anything supported by [consolidate](https://github.com/visionmedia/consolidate.js).
* **Engine detection**: If you register multiple engines, Assemble will automatically detect the correct one to use on each template at render time.
* **gulp plugins**: Full support for [gulp](http://gulpjs.com) plugins. Read our [gulp FAQ](docs/src/subjects/gulp-faq.md) for more info about gulp support.
* **Collections**: First class collection support, with paging, pagination, lists (arrays), groups and sorting. Collections even support plugins and collection-specific routes.
* **Markdown**: Transform content from markdown or any other plain text format to HTML using plugins, middleware helpers or engines.
* **Easy to extend**: Extremely pluggable and easy to extend with helpers, plugins, routes, middleware or engines.
* **Data**: Use any data source for rendering templates, making it easy to begin a project using mock data and switch "live" data source later on.

### Why assemble?

**Why should I use assemble, instead of X?**

* Powerful flow control using [gulp](http://gulpjs.com)-style [tasks](docs/tasks.md)
* Use [middleware](docs/middleware.md) at any point in the [render cycle](docs/render-cycle.md)
* Granular [control over context](docs/why-assemble#context)
* [Plugin ecosystem](docs/why-assemble#plugin-ecosystem): Use any [gulp](http://gulpjs.com) plugin, or plugins from any [base](https://github.com/node-base/base) application
* Facilitates the use of _modular, encapsulated components_ in your markup, like pages, partials and layouts, resulting in consistent design across your projects.
* Built using [base](https://github.com/node-base/base). By learning how to use assemble, you'll also learn how to work with [verb](https://github.com/verbose/verb), [generate](https://github.com/generate/generate), [update](https://github.com/update/update) and other [base](https://github.com/node-base/base) apps.
* Functional, expressive API
* Intuitive CLI, with commands that map elegantly to API methods and conventions

### What can I do with assemble?

Use assemble to create:

* [blogs](examples/blog)
* [build system](examples/build-tool)
* documentation
* e-books
* front-end UI
* [generate boilerplates](examples/boilerplates)
* [generate targets](examples/targets)
* landing pages
* [project generator](examples/generator)
* [project scaffolds](examples/scaffold)
* rapid prototyping
* static site generator
* styleguides
* themes

### Additional features

* templates are [vinyl](http://github.com/gulpjs/vinyl) files
* rich plugin support, use any [gulp](http://gulpjs.com) or [base](https://github.com/node-base/base) plugin!
* render templates with any [template engine](#engine), including [nunjucks][engine-nunjucks], [handlebars](https://github.com/jonschlinkert/engine-handlebars), [lodash][engine-lodash] and any consolidate engine!
* [helpers](#helpers): support for sync and async
* [templates collections](#collections) support
* partials and includes
* layouts
* pages
* custom template "types"
* pagination
* [permalinks][assemble-permalinks]
* [middleware](#middleware) can be used to tranform files at any stage in the render cycle
* pagination
* Much more!

## Documentation

- [Getting started](#getting-started)
  * [Installing assemble](#installing-assemble)
  * [Example usage](#example-usage)
    + [Rendering templates](#rendering-templates)
    + [Running tasks](#running-tasks)
- [CLI](#cli)
  * [Tasks](#tasks)
  * [Options](#options)
  * [Object expansion](#object-expansion)
  * [cwd](#cwd)
  * [file](#file)
- [API](#api)
- [Templates API](#templates-api)
- [Render cycle](#render-cycle)
- [File System API](#file-system-api)
  * [.src](#src)
  * [.dest](#dest)
  * [.copy](#copy)
  * [.symlink](#symlink)
- [Task API](#task-api)
  * [.task](#task)
  * [.build](#build)
  * [.watch](#watch)
- [Plugins](#plugins)
  * [Discovering plugins](#discovering-plugins)
  * [Authoring plugins](#authoring-plugins)
- [Learning](#learning)
  * [Help](#help)
  * [More information](#more-information)
  * [FAQ](#faq)
- [About](#about)
  * [Community](#community)
  * [Related projects](#related-projects)
  * [Similar projects](#similar-projects)
  * [Release history](#release-history)
  * [Contributing](#contributing)
  * [Authors](#authors)
  * [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

## Getting started

### Installing assemble

To use assemble's CLI, you will first need to install it globally using [npm](https://www.npmjs.com):

```sh
$ npm --global install assemble
```

This adds the `assemble` command to your system path, allowing it to be run from any directory.

### Example usage

#### Rendering templates

Render a template with with [handlebars](http://www.handlebarsjs.com/):

```js
var assemble = require('assemble');
var app = assemble();
var locals = {title: 'Home!'};

// add a "page" 
app.page('home.hbs', {content: 'This is '});

// render it!
app.render('home.hbs', locals, function(err, view) {
  if (err) throw err;
  console.log(view.content);
  //=> 'This is Home!'
});
```

#### Running tasks

Create an `assemblefile.js` and add tasks to run:

```js
var assemble = require('assemble');
var htmlmin = require('gulp-htmlmin');
var app = assemble();

app.page('a.hbs', {content: '...'});
app.page('b.hbs', {content: '...'});
app.page('c.hbs', {content: '...'});

app.task('default', function() {
   return app.toStream('pages')
    .pipe(app.renderFile())
    .pipe(htmlmin())
    .pipe(app.dest('site'));
});

// expose your instance of assemble
module.exports = app;
```

## CLI

Run assemble from the command line.

```sh
$ assemble <tasks> [options]
```

Note that for most command line options, _order or definition makes no difference_, so tasks can be defined before or after options.

### Tasks

Optionally specify one or more tasks to run. Multiple tasks are separated by a space.

**Example**

To run tasks `foo` and `bar`, you would enter the following in the command line:

```sh
$ assemble foo bar
```

### Options

Non-task commands and options are prefixed with `--` and are specified using any of the following formats:

* single value, like `--foo`, or
* key-value pair, like `--foo=bar`. Also, key-value pairs may be separated by either `=` or a single whitespace, so `--foo=bar` and `--foo=bar` should both work.

Additionally, as mentioned above, tasks may be defined before or after options, so both of the following are equivalent:

```sh
$ assemble --cwd foo bar
# or 
$ assemble foo bar --cwd
```

**Example**

To emit views as they're loaded and log them to `stderr`, run assemble with the following command:

```sh
$ assemble --emit view
# or
$ assemble --emit=view
```

### Object expansion

Object-paths may be specified using dot-notation for **either the key or value** in a command line argument.

Additionally, assemble uses [expand-object](https://github.com/jonschlinkert/expand-object) (and some custom parsing) to make it easier to pass non-trivial options and commands via command line. So all of the following formats are possible.

**Examples**

Boolean values:

```sh
$ assemble --foo 
# { foo: true }
```

Key-value pairs:

```sh
$ assemble --foo=bar
# { foo: 'bar' }
```

Nested booleans:

```sh
$ assemble --option=foo 
# {options: { foo: true }}
```

Nested key-value pairs:

```sh
$ assemble --option=foo:bar
# {options: { foo: 'bar' }}
```

Deeply nested key-value pairs:

```sh
$ assemble --option=foo.bar.baz:qux
# {options: foo: { bar: { baz: 'qux' }}}}
```

Or on the left-side of the `=`:

```sh
$ assemble --option.foo.bar.baz=qux
# {options: foo: { bar: { baz: 'qux' }}}}
```

### cwd

Change the `cwd` for the `assemblefile.js` to run, optionally specifying any tasks to run:

```sh
$ assemble <tasks> --cwd [directory]
```

**Example**

To run the `scaffolds` example in the `examples/` directory, you would enter:

```sh
$ assemble --cwd examples/scaffolds
```

If successful, in the command line, you should see something like this:

<img width="527" alt="screen shot 2016-01-09 at 1 35 52 pm" src="https://cloud.githubusercontent.com/assets/383994/12217685/0a14294e-b6d6-11e5-9e06-dc4738f0e53a.png">

### file

The name of which file to load when running, defaulting to `assemblefile.js`:

```sh
$ assemble --file assemblefile.dev.js
```

## API

### [Assemble](index.js#L24)

Create an `assemble` app. This is the main function exported by the assemble module.

**Params**

* `options` **{Object}**: Optionally pass default options to use.

**Example**

```js
var assemble = require('assemble');
var app = assemble();
```

## Templates API

Assemble has an extensive API for working with templates and template collections. In fact, the entire API from the [templates](https://github.com/jonschlinkert/templates) library is available on Assemble.

While we work on getting the assemble docs updated with these methods you can visit [the templates library](https://github.com/jonschlinkert/templates) to learn more about the full range of features and options.

## Render cycle

TODO

***

## File System API

Assemble offers the following low-level methods for working with the file system:

* [src](#src)
* [symlink](#symlink)
* [dest](#dest)
* [copy](#copy)

Assemble has first-class support for [vinyl-fs](http://github.com/wearefractal/vinyl-fs), so any [gulp](http://gulpjs.com) plugin can be used in your assemble pipeline.

### .src

Create a [vinyl](http://github.com/gulpjs/vinyl) stream. Takes glob patterns or filepaths to the source files to read.

**Params**

* `glob` **{String|Array}**: Glob patterns or file paths to source files.
* `options` **{Object}**: Options or locals to merge into the context and/or pass to `src` plugins

**Example**

```js
app.src('src/*.hbs');

// define `src` options
app.src('src/*.hbs', { layout: 'default' });
```

### .dest

Specify a destination for processed files.

**Params**

* `dest` **{String|Function}**: File path or rename function.
* `options` **{Object}**: Options and locals to pass to `dest` plugins

**Example**

```js
app.dest('dist/');
```

### .copy

Copy files with the given glob `patterns` to the specified `dest`.

**Params**

* `patterns` **{String|Array}**: Glob patterns of files to copy.
* `dest` **{String|Function}**: Desination directory.
* `returns` **{Stream}**: Stream, to continue processing if necessary.

**Example**

```js
app.task('assets', function() {
  // return, to let assemble know when the task has completed
  return app.copy('assets/**', 'dist/');
});
```

### .symlink

Same as `.src` but takes glob patterns or filepaths for the symlinks to read.

**Params**

* `glob` **{String|Array}**: Glob patterns or file paths

**Example**

```js
app.symlink('src/*.hbs');
```

***

## Task API

Assemble has the following methods for running tasks and controlling workflows:

* [task](#task)
* [build](#build)
* [watch](#watch)

### .task

Define a task to be run when the task is called.

**Params**

* `name` **{String}**: Task name
* `fn` **{Function}**: function that is called when the task is run.

**Example**

```js
app.task('default', function() {
  app.src('templates/*.hbs')
    .pipe(app.dest('site/'));
});
```

### .build

Run one or more tasks.

**Params**

* `tasks` **{Array|String}**: Task name or array of task names.
* `cb` **{Function}**: callback function that exposes `err`

**Example**

```js
app.build(['foo', 'bar'], function(err) {
  if (err) throw err;
  console.log('done!');
});
```

### .watch

Watch files, run one or more tasks when a watched file changes.

**Params**

* `glob` **{String|Array}**: Filepaths or glob patterns.
* `tasks` **{Array}**: Task(s) to watch.

**Example**

```js
app.task('watch', function() {
  app.watch('docs/*.md', ['docs']);
});
```

## Plugins

### Discovering plugins

Plugins from any applications built on [base](https://github.com/node-base/base) should work with Assemble and can be used in your `assemblefile.js`:

* [base][base-plugin]: find base plugins on npm using the `baseplugin` keyword
* [assemble][assemble-plugin]: find assemble plugins on npm using the `assembleplugin` keyword
* [generate][generate-plugin]: find generate plugins on npm using the `generateplugin` keyword
* [templates][templates-plugin]: find templates plugins on npm using the `templatesplugin` keyword
* [update][update-plugin]: find update plugins on npm using the `updateplugin` keyword
* [verb][verb-plugin]: find verb plugins on npm using the `verbplugin` keyword

### Authoring plugins

Visit the [plugin documentation](docs/plugins.md) guide to learn how to use, author and publish plugins.

## Learning

### Help

**Get in touch!**

Have questions, suggestions, or want to discuss assemble? Join the conversation on [gitter](http://gitter.im/assemble/assemble) or give us a shout on [twitter](https://twitter.com/assemblejs). The assemble team and community are always happy to help!

### More information

* [Documentation](docs)
* [API documentation](docs/api)
* [Generaters maintained by the core team](https://github.com/generate)

### FAQ

**Website is outdated and being refactored!**

Assemble's website, assemble.io, only has information related to [gulp-assemble](http://assemble.io). We're working hard to update the site with information about the latest release.

In the meantime, you might find the [WIP docs](docs/src/content) useful. The [unit tests](test/) are also great examples!

**Is the assemble website up-to-date?**

No, as mentioned above, it's completely out-of-date. If you're using [grunt-assemble](http://assemble.io), some of the documentation at assemble.io might still be useful. If you're using assemble v0.6.0 and higher, the documentation is probably wrong in almost every way.

We're actively (daily) working on a refactor and it's a very high priority.

**What's the difference between [assemble-core](https://github.com/assemble/assemble-core) and assemble?**

Assemble adds a CLI, a few built-in view collections: `pages`, `layouts`, and `partials`, middleware for parsing front-matter, and a few other basic defaults that we've found many users expect. If you'd prefer different defaults, [assemble-core](https://github.com/assemble/assemble-core) is a great starting point.

If you want something that handles templates, rendering, engines, helpers, collections, etc. but you don't need to run tasks or work with the file system, then consider using [templates](https://github.com/jonschlinkert/templates) instead of assemble-core.

**I use gulp, why is it recommended to use assemble directly, instead of running assemble with gulp?**

You can run gulp plugins with assemble, but it won't always work the other way around. This is because, as a build system, assemble does things that gulp doesn't do, like handle middleware.

For example, assemble's `.src` and `.dest` methods have built-in `.onStream`, `.preWrite`, and `.postWrite` middleware handlers. If you still wish to use gulp and your build cycle includes middleware that requires these handlers, you can use the [assemble-handle](https://github.com/assemble/assemble-handle) plugin with gulp to ensure that the handlers are still called as needed.

This is a long way of saying, you can find ways to make gulp work, but you would just be adding an extra dependency to your project to do things that assemble already does.

**What is the relationship between gulp and assemble?**

Please read our [gulp FAQ](docs/src/subjects/gulp-faq.md) for more information.

## About

### Community

Are you using assemble in your project? Have you published an assemble project and want to share your project with the world?

Here are some suggestions!

* If you get like Assemble and want to tweet about it, please feel free to mention `@assemble` or use the `#assemble` hashtag
* Tell us about [your assemble project](https://github.com/assemble/assemble/issues/300)
* Show your love by starring [Assemble](https://github.com/assemble/assemble) and `assemble`
* Get implementation help on [StackOverflow](http://stackoverflow.com/questions/tagged/assemble) (please use the `assemble` tag in questions)
* **Gitter** Discuss Assemble with us on [Gitter](https://gitter.im/assemble/assemble)
* If you publish an assemble plugin, thank you! To make your project as discoverable as possible, please add the keyword `assembleplugin` to package.json.

**Contributing**

Please read our [contributing guide](.github/contributing.md) if you'd like to learn more about contributing to this project.

### Related projects

You might also be interested in these projects from [@doowb](https://github.com/doowb) and [@jonschlinkert](https://github.com/jonschlinkert):

* [boilerplate](https://www.npmjs.com/package/boilerplate): Tools and conventions for authoring and using declarative configurations for project "boilerplates" that can be… [more](https://github.com/jonschlinkert/boilerplate) | [homepage](https://github.com/jonschlinkert/boilerplate "Tools and conventions for authoring and using declarative configurations for project "boilerplates" that can be consumed by any build system or project scaffolding tool.")
* [generate](https://www.npmjs.com/package/generate): Command line tool and developer framework for scaffolding out new GitHub projects. Generate offers the… [more](https://github.com/generate/generate) | [homepage](https://github.com/generate/generate "Command line tool and developer framework for scaffolding out new GitHub projects. Generate offers the robustness and configurability of Yeoman, the expressiveness and simplicity of Slush, and more powerful flow control and composability than either.")
* [scaffold](https://www.npmjs.com/package/scaffold): Conventions and API for creating declarative configuration objects for project scaffolds - similar in format… [more](https://github.com/jonschlinkert/scaffold) | [homepage](https://github.com/jonschlinkert/scaffold "Conventions and API for creating declarative configuration objects for project scaffolds - similar in format to a grunt task, but more portable, generic and can be used by any build system or generator - even gulp.")
* [update](https://www.npmjs.com/package/update): Be scalable! Update is a new, open source developer framework and CLI for automating updates… [more](https://github.com/update/update) | [homepage](https://github.com/update/update "Be scalable! Update is a new, open source developer framework and CLI for automating updates of any kind in code projects.")
* [verb](https://www.npmjs.com/package/verb): Documentation generator for GitHub projects. Verb is extremely powerful, easy to use, and is used… [more](https://github.com/verbose/verb) | [homepage](https://github.com/verbose/verb "Documentation generator for GitHub projects. Verb is extremely powerful, easy to use, and is used on hundreds of projects of all sizes to generate everything from API docs to readmes.")

### Similar projects

If assemble doesn't do what you need, there are some other great open source projects you might be interested in, created by our friends on GitHub (in alphabetical order):

**Static site generators**

* [docpad](https://github.com/docpad)
* [metalsmith](https://github.com/metalsmith)
* [punch](https://github.com/laktek/punch)
* [wintersmith](https://github.com/jnordberg/wintersmith)

**Blog frameworks**

* [hexojs](https://github.com/hexojs)
* [ghost](https://github.com/TryGhost/Ghost)

### Release history

#### key

Changelog entries are classified using the following labels _(from [keep-a-changelog](https://github.com/olivierlacan/keep-a-changelog)_):

* `added`: for new features
* `changed`: for changes in existing functionality
* `deprecated`: for once-stable features removed in upcoming releases
* `removed`: for deprecated features removed in this release
* `fixed`: for any bug fixes

### v0.16.1

* bump dependencies. In particular, there was a bug in [parser-front-matter](https://github.com/jonschlinkert/parser-front-matter) where leading whitespace was removed after extracting front-matter, which caused the first line of indentation to be removed. This has been fixed.

### v0.16.0

* **Added**: `.log()` method, which also exposes additional methods, like `.log.info()`, `.log.success()`, etc.
* docs were moved to `support/docs`, so that markdown docs can be built to the `docs` directory
* docs were updated, new docs added
* Moves some private prototype methods to static methods, to allow them to be used without creating an instance
* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.25.0

### v0.15.0

* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.24.0 to get the latest versions of [templates](https://github.com/jonschlinkert/templates) and [base-data](https://github.com/node-base/base-data) which removes the `renameKey` option from the `.data` method. Use the `namespace` option instead.

### v0.14.0

Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.22.0 to take advantage of fixes and improvements to lookup methods: `.find` and `getView`. No API changes were made. Please [let us know](../../issues) if regressions occur.

* fixes `List` bug that was caused collection helpers to explode
* Improvements to lookup functions: `app.getView()` and `app.find()`
* Bumps [base](https://github.com/node-base/base) to take advantages of code optimizations.

### v0.13.0

* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.21.0. Support for the `queue` property was removed on collections. See [assemble-core](https://github.com/assemble/assemble-core) for additional details.
* Fixes bug where glob parent was not being used for `file.base`, causing dest directory to be relative to cwd instead of glob parent in some cases.
* Some changes were made to context handling that effected one unit test out of ~1,000. although it's unlikely you'll be effected by the change, it warrants a minor bump
* Externalizes common [templates](https://github.com/jonschlinkert/templates) tests to base-test-runner, so that assemble plugins and other [base](https://github.com/node-base/base) applications can use the tests
* Includes a fix from [assemble-loader](https://github.com/assemble/assemble-loader), where a bug caused `renameKey` to not always be used when defined on collection loader options.
* Includes fixes from templates for resolving layouts

### v0.12.0

* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.18.0, which includes a bump in [templates](https://github.com/jonschlinkert/templates). See the changelog on the templates library for more details.

### v0.11.0

* `debug` methods and related code have been removed
* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.17.0

### v0.10.0

* Adds support for using es6 generators with tasks
* Bumps [assemble-core](https://github.com/assemble/assemble-core) to v0.15.0

### v0.9.0

* Bumps several dependencies. No API changes, this is mostly an optimization release. Be sure to completely remove `node_modules` and reinstall all dependencies to avoid errors such as `isRegistered is not a function`

### v0.8.0

* Updates [composer](https://github.com/doowb/composer) to v0.11.0, which removes the `.watch` method in favor of using the [base-watch](https://github.com/node-base/base-watch) plugin.
* Changes in [templates](https://github.com/jonschlinkert/templates). Please see v0.11.0 in [templates history](https://github.com/jonschlinkert/templates#history) for more details.

### v0.7.0

* Stability improvements and optimizations of the API introduced in v0.6.0.

### v0.6.0

* Major refactor. Assemble was completely re-written from the ground-up as a standalone node.js library and is no longer a grunt plugin. Grunt plugin support has been moved to [grunt-assemble](http://assemble.io). Please see that repo for additional details.

### Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

Please read the [contributing guide](.github/contributing.md) for avice on opening issues, pull requests, and coding standards.

If Assemble doesn't do what you need, [please let us know](https://github.com/undefined/issues/new)

### Authors

**Jon Schlinkert**

* [github/jonschlinkert](https://github.com/jonschlinkert)
* [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

**Brian Woodward**

* [github/doowb](https://github.com/doowb)
* [twitter/doowb](http://twitter.com/doowb)

### License

Copyright © 2016, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT license](https://github.com/assemble/assemble/blob/master/LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.1.28, on July 29, 2016._