# Elm Build Scripts
Honestly these are just a couple of over-engineered shell scripts that I cooked
up for building elm projects, since `elm make` is kind of lacking a proper
build workflow.

## Important Notes
These scripts (the build script at least) will only work if your project
contains a `html/` directory, which in turn contains a `css/` directory, where
you keep your `*.html` and `*.css` files. See the structure below. 
```
.
├── html
│   ├── css
│   │   └── styles.min.css
│   └── index.html
```

Another important thing to note is that your `index.html` must also be
structured similarly to the one in the `public/` directory of this repository.
Particularly the below line.
```
...

<script src="bundle.js"></script>

...
```
As this will be modified by the script depending on if you are minifying or
compressing your output `bundle.js` (e.g. `src="bundle.js"` will be replaced
with `src="bundle.min.js"` if you are minifying your output bundle).

### Modifying the Build Script
One final note; this can all be modified by editing the constants at the top of
the `build` script. I'm sure you can figure out what they do.
```
...

build_dir=./dist
public_dir=./public
css_dir=./public/css

target=bundle.js
target_min=bundle.min.js

...
```

## Dependencies
The primary dependencies that you need to install in order to run these scripts
are: `elm` (for compilation) and `uglify-js` (for minification).

The `build` script will automatically detect missing dependencies, so don't
worry about too much going wrong.

### Installing Elm
[Here](https://guide.elm-lang.org/install/elm.html) are the instructions for
installing elm (honestly surprised you're looking at this if you don't already
have it installed).

### Installing UglifyJS
UglifyJS must be installed globally (as a command-line app).
[These](https://www.npmjs.com/package/uglify-js) are the official installation
instructions. 
```
$ npm install uglify-js --global
```

## Installation
So far I just like putting these scripts inside a directory called `scripts`
and running them from the root directory of my project, like so. 
```
.
├── elm.json
├── public
│   ├── css
│   │   └── styles.min.css
│   └── index.html
├── README.md
├── scripts
│   ├── build
│   └── clean
└── src
    └── Main.elm
```

## Usage
The script itself is fairly informative, `-h` or `--help` to print some usage
information. It will yell at you when you do something wrong, so it's somewhat
user-friendly.
```
$ ./scripts/build --help
```
```
Usage: ./scripts/build MAIN [-o|--optimize] [-c|--compress] [-m|--minify]

 Available options:
 	MAIN		The main file for the elm application.
 	-o,--optimize	Optimizations to make code smaller and faster.
 	-m,--minify	Minify the output javascript bundle.
 	-c,--compress	Compress the output javascript bundle.
```

### Production Builds
E.g. compiling a production build (no minification, optimization or
compression).
```
$ ./scripts/build src/Main.elm
```
```
Compiling bundle.js ...

Success!
	 src/Main.elm ───> ./dist/bundle.js
```

### Release Builds
E.g. compiling a release build (with minification, optimization and
compression).
```
$ ./scripts/build src/Main.elm --optimize --minify --compress
```
```
Compiling bundle.js ...
Minifying bundle.js ...
Compressing bundle.min.js ...

Success!
	 src/Main.elm ───> ./dist/bundle.min.js.gz
```

