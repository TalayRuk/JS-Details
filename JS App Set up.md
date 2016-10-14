## Keep in mind *Asynchronicity* refers to the way that JavaScript code can be executed out of order.
## Node Package Manager (npm)

### Project folder
### Starting Project
1. Make the Project folder
2. Make js folder 
  - make method.js file
  - make method-interface.js file
3. Make SCSS folder
  - make styles.scss 
  - to add color at the top $grey: #hex color;
4. Make gulpfile.js 
5. Make .gitignore
6. Make index.html
7. Add links for index.html
8. Make .env file
9. Make Readme.md
```
    <link rel="stylesheet" href="build/css/vendor.css">
    <link rel="stylesheet" href="build/css/styles.css">
    <script src="build/js/vendor.min.js"></script>
    <script type="text/javascript" src="build/js/app.js"></script>
```    

# When Clone the project from github on a diff computer, May need to run :
```
- npm install
- bower install
- maybe -npm install browser-sync --save-dev? ... but 
- maybe npm install gulp-sass gulp-sourcemaps --save-dev?
then run:
- gulp build 
- if there's error 
- run gulp jshint
- make new .env

```

### Add .gitignore file 
```
(after add bower gulpfile pkg)

node_modules/
.DS_Store
bower_components/
build/
tmp/ 
.env     
      (file that can only be access locally)
```

###  build .js file add
```
(Add API:)
var apiKey = require('./../.env').apiKey;

function Calculator(constructorParameter) {
  // constructor
}

Calculator.prototype.pingPong = function(methodParameter) {
  // method code
}

exports.calculatorModule = Calculator;


Then add this to js/project-interface.js

var Calculator = require('./../js/myMap.js').calculatorModule;

$(document).ready ...

```

**Once make all these folder need to git init and git add . & commit & push to github**


# This is the finished gulpfile.js from all the installs
```
var gulp = require('gulp');
var browserify = require('browserify');
var source = require('vinyl-source-stream');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var utilities = require('gulp-util');
var buildProduction = utilities.env.production;
var del = require('del');
var jshint = require('gulp-jshint');
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
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');


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

// (after run uglify)

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
  gulp.start('bower');
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
  gulp.start('bower');
  gulp.start('cssBuild');
});

gulp.task('jshint', function() {
  return gulp.src(['js/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
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
  gulp.watch(["scss/*.scss"], ['cssBuild']);
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

gulp.task('cssBuild', function() {
  return gulp.src(['scss/*.scss'])
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('./build/css'))
    .pipe(browserSync.stream());
});

```

# This is all install need to be done after copy the gulpfile.js
```
npm init
npm install gulp --save-dev
npm install browserify --save-dev
npm install gulp --save-dev
npm install vinyl-source-stream --save-dev
npm install gulp-concat --save-dev 
gulp jsBrowserify
npm install gulp-uglify --save-dev
npm install gulp-util --save-dev 
npm install del --save-dev
npm install jshint --save-dev
npm install gulp-jshint --save-dev
gulp build 

-install Bower

npm install bower -g
bower init
bower install jquery --save
bower install
bower install bootstrap --save
bower install moment --save
npm install bower-files --save-dev

-install BrowserSync

npm install browser-sync --save-dev

-install SASS .. once ruby & gem is install

npm install gulp-sass gulp-sourcemaps --save-dev

Run:      gulp cssBuild
          gulp build
and then: gulp serve

To see any error Run: gulp jshint 
When finish Run: gulp build production  

```

### USE $ gulp jshint to find error like ; ..

### gulp serve : to run the server 
ctrl c to close the server *Every time we run the html, we want to run the server*

### When edit the file like js file need to run gulp build then gulp serve to open edited function html

## how to use API KEY
 $.get('http://api.openweathermap.org/data/2.5/weather?q=' + city + '&appid=' + apiKey, function(response)
 FROM example: http://api.openweathermap.org/data/2.5/weather?q=London,uk&appid=44db6a862fba0b067b1930da0d769e98
 e'll need to sign up for a free account at the Open Weather Map website to get the API key
#### Create an API keys folder 
```
.env
------------
exports.apiKey = "your-Api-Goes-Here";
```
then add var apiKey = require('./../.env').apiKey; in interface.js
 **Need to include instruction for key in a local file w/filename & location in README**




### initailizing npm in detail
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
var browserSync = require('browser-sync').create();

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
 to look like this: *in BOTH build, function() and build, ['clean']
  ......
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
npm install browser-sync --save-dev

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

Now we can run **gulp serve** from the top level of our project directory to launch our server and run the app.


### Installing Sass **for the 1st time
First, you must have Ruby installed. For Windows users, there is a handy Ruby installer which also gives you some command line tools. Mac users can install Ruby using homebrew.

First install homebrew:
```
Do this in admin
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
Then, install Ruby.

$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
Finally, both Mac and Windows users can install Sass from the command line using:

do this in desktop

$ gem install sass

if gem install sass ... has error about certificate 
fix it by:
Replace the ssl gem source with non-ssl as a temp solution:

gem sources -r https://rubygems.org/  -remove
gem sources -a http://rubygems.org/  - add back in 

then 
gem update 

```
### USING SASS
```
npm install gulp-sass gulp-sourcemaps --save-dev

In gulpfile.js:
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('cssBuild', function() {
  return gulp.src(['scss/*.scss'])
    .pipe(sourcemaps.init()) 
    .pipe(sass())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('./build/css'))
    .pipe(browserSync.stream());
});

** add a watcher for our SCSS files to our serve task, so that they are built automatically whenever they are changed.

gulp.watch(["scss/*.scss"], ['cssBuild']);

*** add to gulp.task("build". function() -below gulp.start('bower'); ***
gulp.start('cssBuild'); 

```
#### The last two methods save our compiled SCSS with its source maps in a destination folder called scss. 
- **cssBuild create a CSS file at the path *css/styles.css* for the html and return using scss folder in the project. 

## with this you can call out color name with hex color once at the top of the styles.scss page and then can change the hex color just at that top section when you want to change the color. While color name in various area still stay the same. 

## Run gulp cssBuild and then gulp serve, we will see our new styles on our weather page.
- or Run Serve only once gulp cssBuild is already run.
- with gulp.watch  ... once the gulp cssBuild & serve run once .. If there's only changes in styles.css we don't have to run gulp serve again. The browser will change w/o requiring us to reload

## Once project is finished also add:
```

run: gulp build or gulp build --production (so it'll compress the code's files)
```
## See complete files & folders in Weather repo
