PostCSS Separator
============

Split up your Data-URI (or anything else) into a separate CSS file.

Written with [PostCSS](https://github.com/postcss).


## Installation

`$ npm install postcss-separator`

## Usage

Read `source.css`, process its content, and output processed CSS to `styles.css` and `data.css`.

``` js

var fs = require('fs');
var separator = require('postcss-separator');

var original = fs.readFileSync('source.css', 'utf8');

var icons = separator.separate(original, {
	dataFile: true
});
var cleanUp = separator.separate(original, {
	dataFile: false
});

fs.writeFile('data.css', icons.css);
fs.writeFile('styles.css', cleanUp.css);

```

If `source.css` has:

```css
a.top:hover, a.top:focus {
	background-image: url("data:image");
}

a.top {
	background-image: url("data:image");
}

caption, th, td {
	text-align: left;
	font-weight: normal;
	vertical-align: middle;
}

q, blockquote {
	quotes: none;
}

q:before, q:after, blockquote:before, blockquote:after {
	content: "";
}

a img {
	border: none;
}
```

You will get the following output:

```css
a.top:hover, a.top:focus {
	background-image: url("data:image");
}
a.top {
	background-image: url("data:image");
}
```

## Options

#### keepOrigin (only available in Grunt)
Type: `boolean`
Default value: false

Keep the origin file untouched when defined as `true`. When defined as `false` the origin css file will be cleaned up.

#### dataFile
Type: `boolean`
Default value: true

The generated css content of your file matches your pattern when defined as `true`. When defined as `false` the matching pattern will be removed from your css file. 

#### pattern.matchValue
Type: `RegExp`
Default value: /data:/

A string value that is used to set the value your are searching for in your css.

#### pattern.matchProp
Type: `RegExp`
Default value: false

A string value that is used to set the property your are searching for in your css.

#### pattern.matchRule
Type: `RegExp`
Default value: false

A string value that is used to set the rule your are searching for in your css.

#### pattern.matchMedia
Type: `RegExp`
Default value: false

A value that is used to set the media query your are searching for in your css.

#### pattern.matchParent
Type: `Boolean`
Default value: true

A boolean value that is used to include/exclude the rules parent node (eg. in @media blocks).


## Api

### separate(css, [options])

Remove or separate into another file any data in your `css`.

The second argument is optional. The `options` is same as the second argument of
PostCSS's `process()` method. 

### postcss()

Returns PostCSS processor.

You can use this property for combining with other PostCSS processors.

```javascript
var autoprefixer = require('autoprefixer');
var separator = require('postcss-separator');
var postcss = require('postcss');

var css = fs.readFileSync('test.css', 'utf8');
postcss().use(
  autoprefixer.postcss
).use(
  separator.postcss
).process(css, options);
```

## Grunt Plugin

There is also a grunt module available: see [grunt-postcss-separator](https://github.com/Sebastian-Fitzner/grunt-postcss-separator)

## License
Copyright (c) 2015 Sebastian Fitzner. Licensed under the MIT license.

## ToDos

- Add tests
- Add further plugin compatibility for postcss
