# api documentation for  [find (v0.2.7)](https://github.com/yuanchuan/find#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-find.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-find) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-find.svg)](https://travis-ci.org/npmdoc/node-npmdoc-find)
#### Find files or directories by name

[![NPM](https://nodei.co/npm/find.png?downloads=true)](https://www.npmjs.com/package/find)

[![apidoc](https://npmdoc.github.io/node-npmdoc-find/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-find_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-find/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-find/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-find/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "yuanchuan",
        "email": "yuanchuan23@gmail.com",
        "url": "http://yuanchuan.name"
    },
    "bugs": {
        "url": "https://github.com/yuanchuan/find/issues"
    },
    "dependencies": {
        "traverse-chain": "~0.1.0"
    },
    "description": "Find files or directories by name",
    "devDependencies": {
        "mocha": "^2.2.4",
        "tmp": "^0.0.25"
    },
    "directories": {
        "test": "test"
    },
    "dist": {
        "shasum": "7afbd00f8f08c5b622f97cda6f714173d547bb3f",
        "tarball": "https://registry.npmjs.org/find/-/find-0.2.7.tgz"
    },
    "gitHead": "2f3e0b73d1ac01127e7d5ba2ebda240f68847cd5",
    "homepage": "https://github.com/yuanchuan/find#readme",
    "keywords": [
        "find",
        "findfile",
        "search",
        "files"
    ],
    "license": "MIT",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "yuanchuan",
            "email": "yuanchuan23@gmail.com"
        }
    ],
    "name": "find",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/yuanchuan/find.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "url": "https://github.com/yuanchuan/find",
    "version": "0.2.7"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module find](#apidoc.module.find)
1.  [function <span class="apidocSignatureSpan">find.</span>dir (pat, root, fn)](#apidoc.element.find.dir)
1.  [function <span class="apidocSignatureSpan">find.</span>dirSync (pat, root)](#apidoc.element.find.dirSync)
1.  [function <span class="apidocSignatureSpan">find.</span>eachdir (pat, root, action)](#apidoc.element.find.eachdir)
1.  [function <span class="apidocSignatureSpan">find.</span>eachfile (pat, root, action)](#apidoc.element.find.eachfile)
1.  [function <span class="apidocSignatureSpan">find.</span>file (pat, root, fn)](#apidoc.element.find.file)
1.  [function <span class="apidocSignatureSpan">find.</span>fileSync (pat, root)](#apidoc.element.find.fileSync)



# <a name="apidoc.module.find"></a>[module find](#apidoc.module.find)

#### <a name="apidoc.element.find.dir"></a>[function <span class="apidocSignatureSpan">find.</span>dir (pat, root, fn)](#apidoc.element.find.dir)
- description and source-code
```javascript
dir = function (pat, root, fn) {
  var buffer = [];
  if (arguments.length == 2) {
    fn = root;
    root = pat;
    pat = '';
  }
  process.nextTick(function() {
    traverseAsync(
      root
    , type
    , function(n) { buffer.push(n);}
    , function() {
        if (is.func(fn) && pat) {
          fn(buffer.filter(function(n) {
            return compare(pat, n);
          }));
        } else {
          fn(buffer);
        }
      }
    );
  });
  return {
    error: function(handler) {
      if (is.func(handler)) {
        find.__errorHandler = handler;
      }
    }
  }
}
```
- example usage
```shell
...

'''javascript
find.file(__dirname, function(files) {
  //
})
'''

#### .dir([pattern,] root, callback)
'''javascript
find.dir(__dirname, function(dirs) {
  //
})
'''
...
```

#### <a name="apidoc.element.find.dirSync"></a>[function <span class="apidocSignatureSpan">find.</span>dirSync (pat, root)](#apidoc.element.find.dirSync)
- description and source-code
```javascript
dirSync = function (pat, root) {
  var buffer = [];
  if (arguments.length == 1) {
    root = pat;
    pat = '';
  }
  traverseSync(root, type, function(n) {
    buffer.push(n);
  });
  return pat && buffer.filter(function(n) {
    return compare(pat, n);
  }) || buffer;
}
```
- example usage
```shell
...
'''

#### .fileSync([pattern,] root)
'''javascript
var files = find.fileSync(__dirname);
'''

#### .dirSync([pattern,] root)
'''javascript
var dirs = find.dirSync(__dirname);
'''

#### .error([callback])

Handling errors in asynchronous interfaces
...
```

#### <a name="apidoc.element.find.eachdir"></a>[function <span class="apidocSignatureSpan">find.</span>eachdir (pat, root, action)](#apidoc.element.find.eachdir)
- description and source-code
```javascript
eachdir = function (pat, root, action) {
  var callback = function() {}
  if (arguments.length == 2) {
    action = root;
    root = pat;
    pat = '';
  }
  process.nextTick(function() {
    traverseAsync(
        root
      , type
      , function(n) {
          if (!is.func(action)) return;
          if (!pat || compare(pat, n)) {
            action(n);
          }
        }
      , callback
    );
  });
  return {
    end: function(fn) {
      if (is.func(fn)) {
        callback = fn;
      }
      return this;
    },
    error: function(handler) {
      if (is.func(handler)) {
        find.__errorHandler = handler;
      }
      return this;
    }
  };
}
```
- example usage
```shell
...

'''javascript
find.eachfile(__dirname, function(file) {
  //
})
'''

#### .eachdir([pattern,] root, action)

'''javascript
find.eachdir(__dirname, function(dir) {
  //
})
'''
...
```

#### <a name="apidoc.element.find.eachfile"></a>[function <span class="apidocSignatureSpan">find.</span>eachfile (pat, root, action)](#apidoc.element.find.eachfile)
- description and source-code
```javascript
eachfile = function (pat, root, action) {
  var callback = function() {}
  if (arguments.length == 2) {
    action = root;
    root = pat;
    pat = '';
  }
  process.nextTick(function() {
    traverseAsync(
        root
      , type
      , function(n) {
          if (!is.func(action)) return;
          if (!pat || compare(pat, n)) {
            action(n);
          }
        }
      , callback
    );
  });
  return {
    end: function(fn) {
      if (is.func(fn)) {
        callback = fn;
      }
      return this;
    },
    error: function(handler) {
      if (is.func(handler)) {
        find.__errorHandler = handler;
      }
      return this;
    }
  };
}
```
- example usage
```shell
...
'''javascript
find.dir(__dirname, function(dirs) {
  //
})
'''


#### .eachfile([pattern,] root, action)

'''javascript
find.eachfile(__dirname, function(file) {
  //
})
'''
...
```

#### <a name="apidoc.element.find.file"></a>[function <span class="apidocSignatureSpan">find.</span>file (pat, root, fn)](#apidoc.element.find.file)
- description and source-code
```javascript
file = function (pat, root, fn) {
  var buffer = [];
  if (arguments.length == 2) {
    fn = root;
    root = pat;
    pat = '';
  }
  process.nextTick(function() {
    traverseAsync(
      root
    , type
    , function(n) { buffer.push(n);}
    , function() {
        if (is.func(fn) && pat) {
          fn(buffer.filter(function(n) {
            return compare(pat, n);
          }));
        } else {
          fn(buffer);
        }
      }
    );
  });
  return {
    error: function(handler) {
      if (is.func(handler)) {
        find.__errorHandler = handler;
      }
    }
  }
}
```
- example usage
```shell
...
## Examples

Find all files in current directory.

'''javascript
var find = require('find');

find.file(__dirname, function(files) {
  console.log(files.length);
})
'''

Filter by regular expression.

'''javascript
...
```

#### <a name="apidoc.element.find.fileSync"></a>[function <span class="apidocSignatureSpan">find.</span>fileSync (pat, root)](#apidoc.element.find.fileSync)
- description and source-code
```javascript
fileSync = function (pat, root) {
  var buffer = [];
  if (arguments.length == 1) {
    root = pat;
    pat = '';
  }
  traverseSync(root, type, function(n) {
    buffer.push(n);
  });
  return pat && buffer.filter(function(n) {
    return compare(pat, n);
  }) || buffer;
}
```
- example usage
```shell
...

'''javascript
find.eachdir(__dirname, function(dir) {
  //
})
'''

#### .fileSync([pattern,] root)
'''javascript
var files = find.fileSync(__dirname);
'''

#### .dirSync([pattern,] root)
'''javascript
var dirs = find.dirSync(__dirname);
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
