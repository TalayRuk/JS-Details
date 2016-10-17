# Installing TypeScript
```
npm install typescript -g

tsc -v to get the version number currently installed.

-Atom Package
We will also want to install the Atom TypeScript packages so that we can use autocomplete and TypeScript specific linting as we work.

apm install atom-typescript


Note: After installing this package you will have to quit Atom entirely and restart it before you see anything change.
_____________
mkdir Project
cd project
mkdir app 
touch app/hello.ts
_____________
app/hello.ts
var greeting: string = "Hi TypeScript!";
console.log(greeting);

compile $ tsc app/hello.ts

index.html
_____________
<!DOCTYPE html>
<html>
  <head>
    <script type="text/javascript" src="app/hello.js"></script>
    <title>Hello TypeScript!</title>
  </head>
  <body>
  </body>
</html>

```

## *Optional typing* feature. We are using the : to designate that the variable's datat type.

### This is how to write the number type in TypeScript
```
app/sum.ts
___________
var number: number;
var otherNumber: number;

number = parseInt(prompt('please enter a number.'));
otherNumber = parseInt(prompt('enter another number.'));
var sum = number + otherNumber;
alert("The sum of your two numbers is: " + sum);

---------
or use ":" to declare the function
app/sum.ts
__________
var findSum = function(first: number, second: number) {
  var sum = first + second;
  alert("The sum of your two numbers is: " + sum);
}

var number = prompt('please enter a number.');
var otherNumber = prompt('enter another number.');
findSum(number, otherNumber);

```

### Another way to declare our intentions and organize our code is using classes. 
### You can think of a **class** as a blueprint for an object, which itself is the most basic and most customizable building block of JavaScript. 
- **Classes** allow us to declare methods - actions that every instance of that class will be able to take. 
- **Methods** are functions that are defined inside a class. 
- **Functions** are blocks of code that can be called and run outside of a class - for example in the JavaScript console.

## Declaring Classes in TypeScript
- TypeScript gives us a new syntax to declare classes. This still translates into vanilla JavaScript when we compile.
- Example of a class from the TypeScript documentation page. Let's try putting it into a file called greetings.ts in our app folder:

```
app/greetings.ts
_______________________
class Greeter {
  greeting: string;

  constructor (message: string) {
    this.greeting = message;
  }

  greet() {
    return "Hello, " + this.greeting;
  }
}

```
We have a class called Greeter, which will be the blueprint for objects whose job it is to say hello. Greeter objects have a property called greeting, which stores a string. 
We use the keyword constructor to say that a Greeter object must be passed a string on initialization for the message parameter, which is to be stored in the greeting property.

Finally, we declare a greet method, which may be called on any Greeter object. It returns a phrase composed of the word "Hello", followed by whatever message is in the Greeter object's greeting property.

Let's try it out! We'll add some code to our app/greetings.ts file to make some new instances of Greeter objects.

app/greetings.ts
------------------------------
var greeters: Greeter[] = [];
greeters.push(new Greeter("world"));
greeters.push(new Greeter("how are you?"));
greeters.push(new Greeter("my baby, hello my honey, hello my ragtime gal!"));
console.log(greeters);
for(var greeter of greeters){
  alert(greeter.greet());
}

// Looping in TypeScript
   for(var thing of things) {
     console.log(thing);
  };//

```
- We start with a variable called greeters and we declare it to be an array. And using TypeScript, we are able to say that it isn't just any array 
- it will be an array used to hold our Greeter objects. But, at the end of the day it's still a normal JavaScript array, so we can call its push method each time that we create a new instance of Greeter with a different message passed into its constructor.
- We still use the new keyword to create new instances of an object. Finally, we print the array of objects in the console, loop through the array, and call each object's greet method in an alert box.

Let's compile this and add a <script> tag to our index.html file to load it. We should see all our greetings printed to the console, and they should each be displayed in an alert box.

Looping puts the index number of each element of the array into the variable we create. This is often used for looping through an object, where the property names would be stored in thing instead of index numbers. 
