# code-part
[![npm version](https://badge.fury.io/js/code-part.svg)](http://badge.fury.io/js/code-part) [![Build Status](https://secure.travis-ci.org/bline/code-part.png?branch=master)](http://travis-ci.org/bline/code-part) [![Coverage Status](https://coveralls.io/repos/bline/code-part/badge.png?branch=master)](https://coveralls.io/r/bline/code-part?branch=master) [![Dependency Status](https://david-dm.org/bline/code-part.svg)](https://david-dm.org/bline/code-part) [![devDependency Status](https://david-dm.org/bline/code-part/dev-status.svg)](https://david-dm.org/bline/code-part#info=devDependencies)

Parts comments and code into a data structure with htmlParser2 for html and
line based comment parsing for everything else. code-part tracks starting line
numbers for each code section so it's possible to add line numbers if you plan
to use a syntax highlighter like
[google-code-prettify](https://code.google.com/p/google-code-prettify/) to
display your code.

code-part's code for line based parsing was modified from
[docco](http://jashkenas.github.io/docco/). This code base does not include
docco. The only current dependencies are
[htmlParser2](https://github.com/fb55/htmlparser2) and
[lodash](https://lodash.com/).

Comment support for everything except html only extracts comments
at the beginning of the line and only the single line version of
the languages comment. For example `/* not extracted */` does not
get extracted but `// extracted` does.

To see a list of the languages supported by the line based parsing
have a look at [resources/languages.json](./resources/languages.json).

## Usage

```javascript

  var part = require('code-part');

  // Path is used to decide which parser to use
  // (html or lineBased currently) and decides comment
  // parsing in lineBased.
  var sections = part(path, code, config);

  // If code is null, the specified path is assumed
  // to be a path on the file system and is read in
  // with `readFileSync`.
```

## Configuration

```javascript

  var section = part(path, code, {
    // Options used when parsing html.
    // By default the parser will skip comments that start with
    // `<!--[`. Set to `true` to include these as comments.
    noSkipDirectives: false, // default

    // Used instead of path's extension when determining
    // the parser (html or lineBased). Also used in the
    // lineBased parser.
    // when looking up comment markers and deciding if it
    // is literate (litcoffee).
    extension: '.css'
  });
```

## Output

* input:

```javascript
// comment 1
var code = 1;
// comment 2
if (code) code += 1
```

* output:

```javascript
[ { docsText: 'comment 1\n',
    docsLine: 1,
    codeText: 'var code = 1;\n',
    codeLine: 2 },
  { docsText: 'comment 2\n',
    docsLine: 3,
    codeText: 'if (code) code += 1\n',
    codeLine: 4 } ]
```

* input:

```html
<html>
  <!-- title part -->
  <head><title> title </title></head>
<body>
  <!-- main body -->
  <h1>hello world</h1>
<!-- the end -->
</body>
</html>
```

output:

```javascript
[ { docsText: '',
    docsLine: 1,
    codeText: '<html>\n  ',
    codeLine: 1 },
  { docsText: 'title part',
    docsLine: 2,
    codeText: '\n  <head><title> title </title></head>\n<body>\n  ',
    codeLine: 3 },
  { docsText: 'main body',
    docsLine: 5,
    codeText: '\n  <h1>hello world</h1>\n',
    codeLine: 6 },
  { docsText: 'the end',
    docsLine: 7,
    codeText: '\n</body>\n</html>\n',
    codeLine: 8 } ]
```
## BUGS

It is possible to use jade, but multi-line comments are not supported.

Supported:

```jade

html
  // comment 1
  head
    // comment 2
    title A Title
```

***Not Supported:***

```
html
  //
    This is a
    multi-line comment
    and is not supported
  head
    title Another Title
```

## TODO

* Multi-line comment parsing like [jade](http://jade-lang.com/) comments.
* 

## LICENSE

Copyright (C) 2014 Scott Beck, all rights reserved

Licensed under the MIT license


