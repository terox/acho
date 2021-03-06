# acho

<h1 align="center">
	<img src="http://i.imgur.com/XtTjX8G.png" alt="acho">
</h1>


[![Build Status](http://img.shields.io/travis/Kikobeats/acho/master.svg?style=flat)](https://travis-ci.org/Kikobeats/acho)
[![Dependency status](http://img.shields.io/david/Kikobeats/acho.svg?style=flat)](https://david-dm.org/Kikobeats/acho)
[![Dev Dependencies Status](http://img.shields.io/david/dev/Kikobeats/acho.svg?style=flat)](https://david-dm.org/Kikobeats/acho#info=devDependencies)
[![NPM Status](http://img.shields.io/npm/dm/acho.svg?style=flat)](https://www.npmjs.org/package/acho)
[![Gittip](http://img.shields.io/gittip/Kikobeats.svg?style=flat)](https://www.gittip.com/Kikobeats/)

> An extremely (but powerful) simple logging system for NodeJS and browser.

# Why

* Extremely basic and easy to use, customize and extend.
* Expressive API with chaineable methods.
* Mininum dependencies, just focused in one thing.


## Install

```bash
npm install acho
```

If you want to use in the browser (powered by [Browserify](http://browserify.org/)):

```bash
bower install acho --save
```

and later link in your HTML:

```html
<script src="bower_components/acho/dist/acho.js"></script>
```

## Usage

### First steps

For use it, basically you need to create a new logger instance.

```js
var Acho = require('acho');
var acho = new Acho({color: true});
```

It's time to use it!

```js
acho.info('hello world');
// => 'hello world'
```

All public methods for use the library are chaineables:

```js
acho
.info('hello world')
.error('something bad happens');
// => 'info: hello world'
// => 'error: 'something bard happens'
```

Maybe you don't want output the message, but store it and for later use it can be a good idea:

```js
acho.push('success', 'good job!');
console.log(acho.messages.success);
// => ['good job']
```

If you want to print later, just call the method `print`:

```js
acho.print()
// => 'success: good job!'
```

At this moment maybe you are thinking: Can I combine print the message with store the message? Absolutely!

```js
acho.add('info', 'this message is printed and stored');
// => 'info: 'this message is printed and stored'
console.log(acho.messages.info)
// => ['this message is printed and stored']
```

You also can redefine the print method, not exist limits!

```js
acho.print = function() {
  // You are in the acho scope, so you can use the properties
  // of the object. Check the API documentation.
  console.log();
  var _this = this;
  Object.keys(this.types).forEach(function(type) {
    // if (isSuccessOrInfoMessage(type)) console.log();
    _this.messages[type].forEach(function(message) {
      _this.printLine(type, message);
    });
  });
};
```

Do whatever you need for adapt the library of yours requisites: changes colors, add more types, sort the priorities... the internal structure of the object is public and you can edit dynamically. **You have the power**.


### Stablish the level

Stablishing the level of your logs is a good way to avoid some information that maybe you don't want to output. The error levels are:

- `error`: Display calls to `.error() messages`.
- `warning`: Display calls from `.error()`, `.warning() messages`.
- `success`: Display calls from `.error()`, `.warning(), `success()` messages`.
- `info`: Display calls from `.error()`, `.warning(), `success()`, `info()` messages`.
- `verbose`: Display calls from `.error()`, `.warning(), `success()` `info()`, `verbose()` messages`.
- `debug`: Display calls from `.error()`, `.warning(), `success()` `info()`, `verbose()` `debug()` messages`.
- `silly`: Display calls from `.error()`, `.warning(), `success()` `info()`, `verbose()` `debug()` `silly()` messages`
- `silent`: Avoid all.

The default log level is `info`. You can do it in the the constructor:

```js
var acho = new Acho({level: 'silly'})
```

or in any time:

```js
acho.level = 'debug';
```

### Customization

By default the messages structure is minimal: Just the message type followed by the content of the message.

But you can easily customize it, for example, adding an timestamp for each message.

For do it, we offer two methods, `outputType`  and `outputMessage`:

```js
acho = new Acho({
  color: true,
  level: 'silly',
  outputType: function(type) {
    return '[' + type + '] »';
  },

  outputMessage: function(message) {
    return Date() + ' :: ' + message;
  }
});
```

Now is moment to your awesome output:

```js
acho.info('I have hungry');
// => '[ info ] » Fri Mar 13 2015 18:12:48 GMT+0100 (CET) :: I have hungry'
```

You can change how to output the message in the constructor or in any time.


## API

### .constructor({Object} [options])

Create a new logger. The options that you can provide are:

- color **{Boolean}**: If you can colorize the otuput. `false` by default.
- level **{String}**: Stablish the logging level. `info` by default.
- types **{Object}**: You can provide the types and priorities.
- outputType **{Function}**: For customize the type in the output.
- outputMessage **{Function}**: For customize the message in the output.
- print **{Function}**: Provide a function for print the messages.

### .push({String} &lt;type&gt;, {String} &lt;message&gt;)

Store internally a `type` message.

### .add({String} &lt;type&gt;, {String} &lt;message&gt;)

Store internally a `type` message and also output it.

### .error({String} &lt;message&gt;)

Output a `error` message.

### .warning({String} &lt;message&gt;)

Output a `warning` message.

### .success({String} &lt;message&gt;)

Output a `success` message.

### .info({String} &lt;message&gt;)

Output a `info` message.

### .verbose({String} &lt;message&gt;)

Output a `verbose` message.

### .debug({String} &lt;message&gt;)

Output a `debug` message.

### .silly({String} &lt;message&gt;)

Output a `silly` message.

### .isPrintable({String} &lt;type&gt;)

Determines if a type of message should be outputted.

### .colorize({String} &lt;color&gt; {String} &lt;message&gt;)

Determines is a instance of `acho` is outputted with colors.

### .printLine({String} &lt;type&gt; {String} &lt;message&gt;)

Combine `.isPrintable` and `.colorize` for print a line correctly.

### .print()

Default loop for print the messages that are stored internally. By default uses `.printLine` in each message iteration.

## License

MIT © [Kiko Beats](http://www.kikobeats.com)
