#### Bower. 
- It is a package manager like npm, but it is optimized for frontend packages like Bootstrap and jQuery.
The main benefits of using Bower/a different package manager for the front-end are:

1. **Separation of Power:**
- Front-end developers have one file for their dependencies, and the back-end team has a different file for theirs. This can prevent version conflicts and disorganization.

2. **Optimization:**
- Some packages depend on specific versions of other packages. Sometimes two packages require different versions of the same dependency. npm installs each package's dependencies separately. If each of our backend modules has its own version of a package, they are independent and there is no conflict. Each package can have its own node_modulesfolder, and this is convenient but it can lead to complex paths and lots of files. Bower makes sure that each package is installed only once, even if it is used by multiple packages. This makes sense since we can't use jQuery-1.12.0 at the same time as jQuery-2.2.0 in the browser.

#### Installing:
**$ npm install bower -g** installing it globally

#### Initializing:
**$ bower init**
- using Bower the same way that we begin a project with npm. Bower also uses a manifest file in the top level of the project directory, but this time it must be named bower.json. 
- The terminal will prompt us for some information about the project. Feel free to add a name and description for the project, but we can just press Enter to just use all the default values.

Now, we should have a bower.json file that looks like this:
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
