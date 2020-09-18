# PostCSS Tape [<img src="http://postcss.github.io/postcss/logo.svg" alt="PostCSS" width="90" height="90" align="right">][PostCSS]

[<img alt="NPM Version" src="https://img.shields.io/npm/v/postcss-tape.svg" height="20">][npm-url]
[<img alt="Build Status" src="https://img.shields.io/travis/csstools/postcss-tape/master.svg" height="20">][cli-url]
[<img alt="Support Chat" src="https://img.shields.io/badge/support-chat-blue.svg" height="20">][git-url]

[PostCSS Tape] lets you quickly test [PostCSS] plugins.

1. Install this dependency to your project:
   ```sh
   npm install postcss-tape --save-dev
   ```

2. Add the `postcss-tape` task to your `package.json`:
   ```json
   {
      "scripts": {
        "test": "postcss-tape"
      }
   }
   ```

3. Add tests to your `.tape.js` file:
   ```js
   export default {
     'basic': {
       message: 'supports basic usage'
     }
   };
   ```

That’s it! Empty tests will be auto-generated.

```sh
npm test
```

## Options

Options may be passed through `package.json` using `postcssConfig`:

```json
{
  "postcssConfig": {
    "plugin": "path/to/plugin.js",
    "config": "path/to/.tape.js",
    "fixtures": "path/to/cssdir"
  }
}
```

Options may be passed through arguments:

```sh
postcss-tape --plugin path/to/plugin.js --config path/to/.tape.js
```

## Test Options

### message

The message provided to the console to identify the test being run.

```js
{
  'some-test': {
    message: 'supports some test usage'
  }
}
```

### options

The options passed into the PostCSS plugin for the test.

```js
{
  'some-test': {
    options: {
      someOption: true,
      anotherOption: 5,
      etc: 'etc'
    }
  }
}
```

### processOptions

The process options passed into PostCSS for the test. Read the
[PostCSS documentation](http://api.postcss.org/global.html#processOptions) for
more details.

```js
{
  'some-test': {
    processOptions: {
      map: {
        inline: true,
        sourcesContent: true
      }
    }
  }
}
```

### warnings

Either the number of warnings expected to be generated by the test, or an
object used to match warnings given by the test.

```js
{
  'some-test': {
    warnings: 3
  }
}
```

```js
{
  'some-test': {
    warnings: {
      text: /should warn/
    }
  }
}
```

### error

An object used to match an error thrown by the test.

```js
{
  'some-test': {
    error: {
      message: 'You should not have done that'
    }
  }
}
```

In that example, the error expects a field of `message` to be the string
`You should not have done that`. So that an error can be inspecific, regular
expressions may also be used, so that something like
`message: /^You should not have done/` would also match
`You should not have done this`.

### source

The location of the source CSS file, relative to the `fixtures` directory. This
file is passed through the PostCSS plugin.

```js
{
  'some-test': {
    source: 'alterate-source.css'
  }
}
```

In that example, a `some-test` field would automatically populate the
`source` as `some-test.css` unless it was overridden. In order that multiple
tests may use the same source file, a `some-test:modifier` field would still
populate the `source` as `some-test.css`.

### expect

The location of the expected CSS file, relative to the `fixtures` directory.
This file is represents the expected CSS result of `source` after being passed
through the PostCSS plugin.

```js
{
  'some-test': {
    expect: 'alterate-expect.css'
  }
}
```

In that example, a `some-test` field would automatically populate the
`expect` as `some-test.expect.css` unless it was overridden. In order that
multiple tests may use the same source file, a `some-test:modifier` field would
still populate the `source` as `some-test.css`, but alter the `expect` to be
`some-test.modifier.expect.css`.

### result

The location of the resulting CSS file, relative to the `fixtures` directory.
This file is the CSS result of `source` after being passed through the PostCSS
plugin.

```js
{
  'some-test': {
    result: 'alterate-result.css'
  }
}
```

In that example, a `some-test` field would automatically populate the
`result` as `some-test.result.css` unless it was overridden. In order that
multiple tests may use the same source file, a `some-test:modifier` field would
still populate the `source` as `some-test.css`, but alter the `result` to be
`some-test.modifier.result.css`.

### before

A function to be run before the particular test. This is helpful for generating
variables, content, or files that will be used by the plugin.

```js
{
  'some-test': {
    before: () => {
      /* do something before the plugin runs */
    }
  }
}
```

### after

A function to be run after the particular test. This is helpful for cleaning up
variables, content, or files that were used by the plugin.

```js
{
  'some-test': {
    after: () => {
      /* do something after the plugin runs */
    }
  }
}
```

### plugin

A plugin or array of plugins that will specifying alternative plugin

```js
{
  'some-test': {
    plugin: () => require('postcss')(
      require('some-plugin-that-runs-before'),
      require('.'),
      require('some-plugin-that-runs-after')
    )
  }
}
```

## CLI Options

Options can be passed into the `postcss-tape` function or defined in
`package.json` using the `postcssConfig` property.

### plugin

The path to the plugin being tested.

```bash
postcss-tape --plugin path/to/plugin.js
```

```json
{
  "postcssConfig": {
    "plugin": "path/to/plugin.js"
  }
}
```

By default, `plugin` is the resolved file from the current working directory.

### config

The path to the configuration file used to run tests.

```bash
postcss-tape --config path/to/config.js
```

```json
{
  "postcssConfig": {
    "config": "path/to/config.js"
  }
}
```

By default, [PostCSS Tape] will try to use the file defined by `config`, or
the `config` directory’s `postcss-tape.config.js` or `.tape.js` file. By
default, `config` is the current working directory.

### fixtures

The path to the fixtures used by tests.

```bash
postcss-tape --fixtures path/to/tests
```

```json
{
  "postcssConfig": {
    "fixtures": "path/to/tests"
  }
}
```

[npm-url]: https://www.npmjs.com/package/postcss-tape
[cli-url]: https://travis-ci.org/csstools/postcss-tape
[git-url]: https://gitter.im/postcss/postcss

[PostCSS]: https://github.com/postcss/postcss
[PostCSS Tape]: https://github.com/csstools/postcss-tape
