# ls-files
Recursively list all files in a directory and its subdirectories. It does not list the directories themselves.

Because it uses fs.readdir, which calls [readdir](http://linux.die.net/man/3/readdir) under the hood
on OS X and Linux, the order of files inside directories is [not guaranteed](http://stackoverflow.com/questions/8977441/does-readdir-guarantee-an-order).

## Installation

    npm install ls-files

## Usage

```javascript
// some/path
// -----------0.txt
// -----------abc/
// -----------abc/1.txt
// -----------abc/ddd/
// -----------abc/ddd/2.txt
var list = require("ls-files");

list("some/path", function (err, files) {
  // `files` is an array of file paths
  console.log(files);
  // ['some/path/0.txt', 'some/path/abc/1.txt', 'some/path/abc/ddd/2.txt']
});
```

It can also take a list of files to ignore.

```javascript
var list = require("ls-files");

// ignore files named "foo.cs" or files that end in ".html".
recursive("some/path", ["foo.cs", "*.html"], function (err, files) {
  console.log(files);
});
```

You can also pass functions which are called to determine whether or not to
ignore a file:

```javascript
var list = require("ls-files");

function ignoreFunc(file, stats) {
  // `file` is the path to the file, and `stats` is an `fs.Stats`
  // object returned from `fs.lstat()`.
  return stats.isDirectory() && path.basename(file) == "test";
}

// Ignore files named "foo.cs" and descendants of directories named test
list("some/path", ["foo.cs", ignoreFunc], function (err, files) {
  console.log(files);
});
```

## Promises
You can omit the callback and return a promise instead.

```javascript
list("some/path").then(
  function(files) {
    console.log("files are", files);
  },
  function(error) {
    console.error("something exploded", error);
  }
);
```
