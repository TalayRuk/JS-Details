#### Bower. 
- It is a package manager like npm, but it is optimized for frontend packages like Bootstrap and jQuery.
The main benefits of using Bower/a different package manager for the front-end are:

1. **Separation of Power:**
    - Front-end developers have one file for their dependencies, and the back-end team has a different file for theirs. This can prevent version conflicts and disorganization.

2. **Optimization:**
    - Some packages depend on specific versions of other packages. Sometimes two packages require different versions of the same dependency. npm installs each package's dependencies separately. If each of our backend modules has its own version of a package, they are independent and there is no conflict. Each package can have its own node_modulesfolder, and this is convenient but it can lead to complex paths and lots of files. Bower makes sure that each package is installed only once, even if it is used by multiple packages. This makes sense since we can't use jQuery-1.12.0 at the same time as jQuery-2.2.0 in the browser.

#### Bower Installing:
```
**$ npm install bower -g** installing it globally
```
#### Bower Initializing:
```
**$ bower init**
```
- using Bower the same way that we begin a project with npm. Bower also uses a manifest file in the top level of the project directory, but this time it must be named bower.json. 
- The terminal will prompt us for some information about the project. Feel free to add a name and description for the project, but we can just press Enter to just use all the default values.

Now, we should automatically have a bower.json file that looks like this:
```
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
```

#### Installing Front-End Dependencies via Bower jQuery
- We can install jQuery with this simple command. We are using the --save flag instead of our usual --save-dev flag because we do want to use jQuery in the final production build.
```
$ bower install jquery --save
```

#### Install Bower dependencies:
```
$ bower install
```

#### Add bower_components folder to .gitignor file.
Now we can see that Bower has created a directory to hold our dependencies called **bower_components.** We should add this folder to our **.gitignore file,** just as we did with our node_modules folder holding npm dependencies. 

#### Add jQuery into our html app 
- from *this folder by simply changing the path in our script tag*, instead of using the old tag:

```
<script src="bower_components/jquery/dist/jquery.min.js"></script> : for html file
```

#### Add BootStrap
```
$ bower install bootstrap --save

- *here is the bootstrap link for .html file:
   <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">
   <script src="bower_components/bootstrap/dist/js/bootstrap.min.js"></script>*
```
#### Add Monment.js
```
**$ bower install moment --save**

 tag look like in html file: <script src="bower_components/moment/min/moment.min.js"></script>
 in js/time-interface.js .. this is how to use the moment function:
 $(document).ready(function(){
  $('#time').text(moment());
});
  in html file: <h3>Well look at the time! </h3>
                <h3 id="time"></h3>
```
- This is a JavaScript dependency work with dates and times in various formats .. install it with bower 

#### Add Gulp package called bower-files"
```
$ npm install bower-files --save-dev
```
- This help streamline script and link tags to HTML file and being responsible for adding a new one everytime we add another Bower dependency.

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
 gulp.start('bower'); below gulp.start('jsBrowserify');
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
- We have created a task we can now run with gulp bower any time we add a bower dependency.

- This task has no callback function, but it has 2 dependency tasks: bowerJS and bowerCSS. Incidentally, it is important to note that the order of the tasks in the dependency array is ignored by gulp. So we can't use this method if we need one task to be completed before the next one in the array begins. Our bower task above will run both the bowerJS and bowerCSS tasks concurrently. This is nice because it is faster than running them one after the other! But if we need to run task1 before task2, then we must specify task1 as a dependency of task2, not just list them in order as part of a third master task.

-To make sure that this bower task runs automatically when we build. Since we will always want to include our vendor files whether or not we are making a production build, we will just call the bower task using gulp.start at the end of our build task. Which means it will be running in parallel to our other tasks, but sinc our *bower* task only deals with 3rd party packages installed w/Bower, we can safely leave these tasks independent of our other development tasks. 
