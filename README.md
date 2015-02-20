# grunt-browserify-alias
Map paths to JS files and directories for use as aliases so that modules can be require'd without messy relative file paths.

## Why use aliases?

You want to require modules from specific directories, without needing to use `../../../../some-module` to resolve relative paths.

For example, have a look at the following javascript directory structure:

```javascript
root project directory
│
├──•Gruntfile.js
│
└───src
     │
     └───js
          │
          └───app
               │
               ├──• app.js
               │
               ├───controller
               │    │
               │    └──sunshine
               │       │
               │       └──• sunshine-controller.js 
               │
               └───model
                    │
                    └──sunshine
                       │
                       └──• sunshine-model.js 


```

Now, say we want to require *sunshine-model* from within the *sunshine-controller* module. The standard old crappy way to do this is:

```javascript
require("../../../model/sunshine/sunshine-model.js");
```

Now imagine that you refactor your code and modules get moved around. Suddenly, managing all those relative paths becomes even more of a pain in the butt.

Using *grunt-browserify-alias* at compile-time will allow you to `require` modules from the aliased directories you specify.

New hotness:

```javascript
require("model/sunshine/sunshine-model.js");
```

## Usage

### Installation

```
npm install --save-dev grunt-browserify-alias
```

### Implementation

Simply specify the files and directories you want to map by using a globbing pattern. Here is a very basic Gruntfile as an example:

```
function Gruntfile (grunt)
{
    "use strict";

    var alias = require("grunt-browserify-alias");

    grunt.initConfig({

        browserify: {
            options : {
                alias: alias.map(grunt, {

                    // alias all js files in the 'app' directory
                    cwd: "src/js/app",
                    src: ["**/*.js"],
                    dest: ""
                })
            },

            src: "src/js/app/app.js",
            dest: "dist/js/app.js",
        },
    });


    grunt.loadNpmTasks("grunt-browserify");

    grunt.registerTask("default", ["browserify"]);
}

module.exports = Gruntfile;
```


It is also possible to specify multiple targets using an array of objects:

```javascript
function Gruntfile (grunt)
{
    "use strict";

    var alias = require("grunt-browserify-alias");

    grunt.initConfig({

        browserify: {
            options : {
                alias: alias.map(grunt, [
                    {
                        // alias app files
                        cwd: "src/js/app",
                        src: ["**/*.js"],
                        dest: ""
                    },
                    {
                        // alias lib files
                        cwd: "src/js/libs",
                        src: ["**/*.js"],
                        dest: ""
                    }
                ])
            },

            src: "src/js/app/app.js",
            dest: "dist/js/app.js",
        },
    });


    grunt.loadNpmTasks("grunt-browserify");

    grunt.registerTask("default", ["browserify"]);
}

module.exports = Gruntfile;
```