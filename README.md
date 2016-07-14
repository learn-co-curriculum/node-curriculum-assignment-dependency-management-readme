# Node Dependency Management
You've already learned how to build a small, single file application in Node.js - congrats!

Now we'll discuss how to break up your app into smaller related chunks of code called modules to help keep things organized as your code base grows.

## Why modules?
Think of your application as though it were a business. In the early stages when your team is small, having everyone work in the same room is totally reasonable.

As your company scales and you hire employees to perform specific functions, you'll eventually need to organize your company into departments; each with its own special purpose (eg: sales, engineering, project management).

Modules provide the same organizational structure and value for your application!

## Dependencies and NPM
We'll introduce you to the concept of modules (which we'll sometimes refer to as packages) and dependencies by creating a basic app with Express - the popular Node web application framework.

Express can be installed into your application via NPM, Node's package (module) management system. We can use the NPM command line tool that ships with Node to install packages into our application from the NPM registry.

Let's use NPM to install our app's very first dependency: Express.

Open Terminal and cd into your app's root directory.

```
$ cd ~/path/to/app
```

The first thing you'll want to do is create a `package.json` file which contains meta data about your app including a list of your apps dependencies. Remember: "dependencies" are just the modules that your app needs in order to function - In other words, dependencies are the modules or packages your app *depends* on.

We can create a `package.json` file with ease by executing the following command in our project's root directory:
```
$ npm init
```

Executing this command will prompt you for a number of things, like the name and version of your application. For now, you can simply hit `return` to accept the defaults for most of the questions, with the following exception:
```
entry point: (index.js)
```
Just go ahead and enter the name of your main file then press `enter`

Next we'll install the Express package:
```
$ npm install express --save
```
NPM will search the registry for the Express package and download it to the `node_modules` folder in your projects root directory. The `--save` part just tracks Express as a new dependency in your `package.json` file.

Now we're ready to start building our simple Express app. Go ahead and copy this code into your `index.js`.

`index.js:`
```
const express = require('express');
const app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

The first thing you'll notice here is the use of `require()` to import  Express into our app.

The `require()` function is part of Nodes built in module loading system which we can use to import JavaScript from external files into our application.

Now that express is available to our main application, we can use its methods to helps us build our app. We're using the `listen()` method to listen for connections on port 3000 as well as some routing functionality to send the message `Hello World` to the client when the index `/` route is requested.

Using `require()` to import Express from another location within our app allows us to expand the capability of our main application logic in a way that's clean and manageable.

## Creating your own modules
NPM is full of packages you can use in your app to speed up development and build great things.

But what if you wanted to build your own modules? Great question!

Let's say you need to write some JavaScript code that performs calculations on shapes. Rather than writing this code in your main application, it would be a good idea to bundle all this related code into it's own module file that we can import just like we did with Express.

Let's create a module file called `circle.js`, paste in the following code and save it to our projects root directory:

`circle.js:`
```
const PI = Math.PI;

exports.area = (r) => PI * r * r;

exports.circumference = (r) => 2 * PI * r;

```
Node modules contain an `exports` object we can bind our module's functionality to. Anything bound to a module's exports object will be available to any part of our app that imports the module using `require()`.

Any variables or functions not bound to the exports object (in this case the `PI` variable), will remain private to the module.

Ok, let's go back to our `index.js` file, import the circle module we just created and use its `area()` method in our main application.

`index.js:`

```
const express = require('express');
const app = express();

const circle = require('./circle.js');

app.get('/', function (req, res) {
  res.send(`The area of a circle of radius 4 is ${circle.area(4)}`);
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

Congrats, you've created a custom module and imported it into your app!

Now that you have a better grasp on how to create and use modules in your application, we'll show you how to host your modules on npm so other Node developers can use them as well!
