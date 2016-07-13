
Quote about standing on the shoulders of giants

# Node Dependency Management

So far, all our code has been written in a single file. But for big projects, writing everything in one giant file becomes hard to manage. In this lesson, we'll cover how to spread code out over several files.

## Objectives
After this lesson, you'll be able to...

1. Spread code out over several files instead of writing everything in one file.
2. Explain different export strategies.
3. Recognize what parts of a file can be used in other files.


## Life With Only One File

Imagine that we only had one file to put all of our code in. As our program got bigger, the file would grow in size. Eventually it could be 1,000 or 10,000 or even 100,000 lines of code! It could work. But it would make it hard to navigate our code.

Pretend that you live in a very big house, but that it only has one big room. Everything just piles up in that one room. Maybe the beds are on one side, and clothes and shoes are next to them. Maybe there's a sink and a refrigerator in the middle of this big room. Just like with our one big file, we could try to keep things organized by grouping them together.

```js

// Kitchen functions
// -----------------

function sink() {
  return '💦';
}

function fridge() {
  return '🍕';
}

// Bedroom functions
// -----------------

function closet() {
  return 👗;
}

// More functions...

console.log('I went to the sink and found', sink());
console.log('I went to the fridge and found', fridge());
console.log('I went to the closet and found', closet());

```

But wouldn't it be better in our house if we had walls and separate rooms? The same is also true for our code.

## Multiple Files to the Rescue

Let's us play the role of architect and create different files (rooms) for our code. To run the code below, create two different files `app.js` and `kitchen.js`.

```js
// File app.js

// Error! This code doesn't work.
console.log('I went to the sink and found', sink());
```

```js
// File kitchen.js

function sink() {
    return '💦';
}
```

Try putting the above code into two different files, `app.js` and `kitchen.js`, and then run the app by typing `node app.js` in the command line.

You should see an error. The problem is our `app.js` code doesn't know about the code in `kitchen.js`!

Luckily for us, Node makes it easy to solve this. Let's change our files to.

```js
// File app.js

var sink = require('./kitchen');

console.log('I went to the sink and found', sink());
```

```js
// File kitchen.js

function sink() {
    return '💦';
}

module.exports = sink;
```

This works! We can now share code from one file, `kitchen.js` to another file, `app.js`.

## Exporting more than one thing

Now let's say there are multiple functions in the kitchen that we want to call from our app. We could have a kitchen that looks like this:

```js
// File kitchen.js

function sink() {
    return '💦';
}

function fridge() {
  return '🍕';
}

function stove() {
  return '🍳';
}

module.exports = sink;
```

The problem is we are only exporting one function, the `sink` function.

A great strategy to solve this is to export an `object`, instead of a `function`. Let's take a quick look at this to see how it works:

```js
// File kitchen.js

function sink() {
    return '💦';
}

function fridge() {
  return '🍕';
}

function stove() {
  return '🍳';
}

module.exports = {
  sink: sink,
  fridge: fridge,
  stove: stove,
}
```

Now when someone requires our `kitchen.js` file, they will get an object that has all three functions. Let's change our `app.js` file to take advantage of this change.

```js
// File app.js

var kitchen = require('./kitchen');

console.log('I went to the sink and found', kitchen.sink());
console.log('I went to the fridge and found', kitchen.fridge());
console.log('I went to the stove and found', kitchen.stove());
```

Now we should be able to call all three of the functions in our `kitchen.js` file from our `app.js` file.

Another benefit of this is we know exactly which sink we are using when we call `kitchen.sink()`. If we add a third file, `bathroom.js` it could have it's own sink function. This could look like:

```js
// File bathroom.js

function sink() {
  return 'Hi from the bathroom sink';
}

module.exports = {
  sink: sink,
}
```

And then we could call both `sink` functions in `app.js` and we'll never be unsure about which one we are calling.

```js
// File app.js

var kitchen = require('./kitchen');
var bathroom = require('./bathroom');

console.log('I went to the kitchen sink and found', kitchen.sink());
console.log('I went to the bathroom sink and found', bathroom.sink());
```

## Exporting More Than Just Functions

So far we've only been exporting functions. What else can we export? The answer is anything! As long as we put it in a variable, we can export it. Take a look below for an example:

```js
// File closet.js

var shirt = '👕';
var pants = '👖';
var shoes = {
  white: '👟',
  brown: '👞',
}

function workClothes() {
  return shirt + ' ' + pants + ' ' + shoes.brown;
}

module.exports = {
  shirt: shirt,
  pants: pants,
  shoes: shoes,
  workClothes: workClothes,
}
```

```js
// File app.js

var closet = require('./closet');

console.log('Today I am going to wear', closet.workClothes(), 'to work');
console.log('That means I am not going to wear', closet.shoes.white);
```

## What about code that isn't exported?

We saw that we can pick which variables we want to export by including it in our export object (the object that we set `module.exports` equal to). What about variables that aren't in that export object?

The short answer is that they will only be able to be used in the file they're in. In the example of `closet.js` above, if we didn't export `shoes` and our file looked like this:

```js
// File closet.js

var shirt = '👕';
var pants = '👖';
var shoes = {
  white: '👟',
  brown: '👞',
}

function workClothes() {
  return shirt + pants + shoes.brown;
}

module.exports = {
  shirt: shirt,
  pants: pants,
  workClothes: workClothes,
}
```

then `shoes` can only be accessed inside of `closet.js` like it is in the `workClothes` function.

## Alternative Way To Export

At the top of every Node file, you can imagine
```js
var exports = module.exports = {};
```

What this means for us is instead of setting `module.exports` equal to an object like we've been doing, we can instead assign properties. That means

```js
module.exports = {
  shirt: shirt,
  pants: pants,
  workClothes: workClothes,
}
```

```js
module.exports.shirt = shirt;
module.exports.pants = pants;
module.exports.workClothes = workClothes;
```

```js
exports.shirt = shirt;
exports.pants = pants;
exports.workClothes = workClothes;
```

Are all the same!

Be careful of `exports` though. Make sure to only set properties on it, like `exports.shirt = shirt;`. If you ever write `exports = { shirt: shirt };`, it won't work.

Congratulations! You are now a file splitting architect! Play around with exporting more functions, strings (with or without emojis), objects, and more.

<p class='util--hide'>View <a href='https://learn.co/lessons/node-curriculum-assignment-dependency-management-readme'>Node Curriculum Assignment Dependency Management Readme</a> on Learn.co and start learning to code for free.</p>
