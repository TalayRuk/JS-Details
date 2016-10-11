## Node Package Manager (npm)

### Project folder
### Starting Project
1. Make the Project folder
2. Make js folder 
  - make method.js file
  - make method-interface.js file
3. Make CSS folder
  - make styles.css 
4. Make gulpfile.js 
5. Make index.html



###  build .js file add
```
function Calculator(constructorParameter) {
  // constructor
}

Calculator.prototype.pingPong = function(methodParameter) {
  // method code
}

exports.calculatorModule = Calculator;

Then add this to js/project-interface.js
(Add API:)
var apiKey = "YOUR-API-KEY-GOES-HERE";
var Calculator = require('./../js/pingpong.js').calculatorModule;

$(document).ready ...
```
### initailizing npm 
```
$ npm init

- this will create manifest file = package.json will look like this:
{
  "name": "ping-pong",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "directories": {
    "test": "test"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

$ npm install gulp --save-dev
-(this automatically add below to the .json file
"devDependencies": {
    "gulp": "^3.9.1"
  } 

$ npm install browserify --save-dev
-(this will auto install node_modules folder)

$ npm install gulp --save-dev

$ npm install gulp -g (do this once .. don't need to do @school computer)

$ npm install vinyl-source-stream --save-dev

$ npm install gulp-concat --save-dev 
-(then add gulp.task concatInterface)
-(add gulp.task (jsBrowserify) then run

$ gulp jsBrowserify

$ npm install gulp-uglify --save-dev
-then add var uglify & gulp.task ("minifyScripts...) to gulpfile.js

$ npm install gulp-util --save-dev 
- for gulp-util then add var utilities to gulpfile.js

$ npm install del --save-dev
-hen add var delete to gulp file

$ npm install jshint --save-dev
$ npm install gulp-jshint --save-dev
-then add var jshint to gulp file

everytime the project files is modified need to run: (just works similar to dnx kestrel)
Before RUN THiS COMMAND .. MAKE SURE JS FOLDER & NODE are closed!!!!!!
$ gulp build --production   :(for user interface or when launch, this also put codes in app.js in bulid
  folder to be all in one line --> "ugly" code.)
or 
$ gulp build (for developer to use - make code readable) 
(**this is when the build file is auto made and tmp file also)


//..after gulp.task set up 
run
$ gulp name(whatever the name is )
```

## This is What package.json should look like when finished above:
```
{
  "name": "weather",
  "version": "1.0.0",
  "description": "",
  "main": "gulpfile.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "vichitra pool",
  "license": "ISC",
  "devDependencies": {
    "browserify": "^13.1.0",
    "del": "^2.2.2",
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.0",
    "gulp-jshint": "^2.0.1",
    "gulp-uglify": "^2.0.0",
    "gulp-util": "^3.0.7",
    "jshint": "^2.9.3",
    "vinyl-source-stream": "^1.1.0"
  }
}
```

### to check for error .. run $ gulp jshint 
### Create gulpfile.js

```
(/gulp.task(name, function(){

});// this is how gulp. task set up

start here:)
var gulp = require('gulp');
var browserify = require('browserify');
var source = require('vinyl-source-stream');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var utilities = require('gulp-util');
var buildProduction = utilities.env.production;
var del = require('del');
var jshint = require('gulp-jshint');


gulp.task('concatInterface', function() {
  return gulp.src(['./js/*-interface.js'])
    .pipe(concat('allConcat.js'))
    .pipe(gulp.dest('./tmp'));
});

gulp.task('jsBrowserify', ['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js'] })
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'));
});

//(after run uglify)

gulp.task("minifyScripts", ["jsBrowserify"], function(){
  return gulp.src("./build/js/app.js")
    .pipe(uglify())
    .pipe(gulp.dest("./build/js"));
});

gulp.task("build", ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});

gulp.task("clean", function(){
  return del(['build', 'tmp']);
});

gulp.task("build", function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});

gulp.task('jshint', function() {
  return gulp.src(['js/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

```

### Add Bower
```
$ npm install bower -g installing it globally

$ bower init

this will show:
{
    "name": "pingpong",
    "description": "",
    "main": "index.js",
    "license": "ISC",
    "moduleType": [],
    "homepage": "",
    "ignore": [
    "*/.",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ]
}

$ bower install jquery --save

$ bower install

$ bower install bootstrap --save

$ bower install moment --save

$ npm install bower-files --save-dev
```

#### Using links & Scripts below for HTML file 
```
    <link rel="stylesheet" href="build/css/vendor.css">
    <script src="build/js/vendor.min.js"></script>
    <script type="text/javascript" src="build/js/app.js"></script>
  ***if we using our own css file the link for example <link rel="stylesheet"href="/css/script.css"> also need to be added***
```

#### Using Bower Pkges in the Gulpfile

```
In gulpfile.js:
var lib = require('bower-files')({
  "overrides":{
    "bootstrap" : {
      "main": [
        "less/bootstrap.less",
        "dist/css/bootstrap.css",
        "dist/js/bootstrap.js"
      ]
    }
  }
});

gulp.task('bowerJS', function () {
  return gulp.src(lib.ext('js').files)
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./build/js'));
});

gulp.task('bowerCSS', function () {
  return gulp.src(lib.ext('css').files)
    .pipe(concat('vendor.css'))
    .pipe(gulp.dest('./build/css'));
});

gulp.task('bower', ['bowerJS', 'bowerCSS']);

***Then add only
 gulp.start('bower'); below {gulp.start('jsBrowserify'); }
 to look like this:
 gulp.task('build', ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
  gulp.start('bower');
});
 
```
### Install BrowserSync
```
$ npm install browser-sync --save-dev

then in gulpfile.js

gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });
  gulp.watch(['js/*.js'], ['jsBuild']);
  gulp.watch(['bower.json'], ['bowerBuild']);
  gulp.watch(['*.html'], ['htmlBuild']);
});

gulp.task('jsBuild', ['jsBrowserify', 'jshint'], function(){
  browserSync.reload();
});

gulp.task('bowerBuild', ['bower'], function(){
  browserSync.reload();
});

gulp.task('htmlBuild', function() {
  browserSync.reload();
});

```

Now we can run **gulp serv** from the top level of our project directory to launch our server and run the app.

### Add .gitignore file to the folder
```
node_modules
bower_components
.DS_Store
```
