---
---
# Level 02

## Express

Express is a common webapp framework for node; it's very minimal; think Sinatra, rather than Rails. Let's install it:

`npm install express`

Note that npm has added the dependency to `package.json` automatically. Pretty helpful.

### Hello world

Create `app.js` and add the following code:

```
var express = require('express');
var app = express();

app.get('/', function(req, res){
   res.send("Hello dxw!");
});

module.exports = { app };
```

Also create `index.js`:

```
const { app } = require('./app');

app.listen(3000);
```

We make these two separate files so we can test the express app without starting it up fully. Add a `start` script to `package.json` that runs `node index.js`.

Then you can run it with `npm start`, visit http://localhost:3000 and say hello.

## Auto-restarting development server

You're used to Rails looking out for you so that changes to source code are
visible when you refresh the browser. You don't get this 'out of the box' with
node. But, if you use the `nodemon` package you'll get this behaviour:

1. install with `npm install nodemon --save-dev`

2. change the `"start"` script in `package.json` to use nodemon rather than node to run `index.js`

Now start the server with `npm start` and change the body of the homepage to "hello
from dxw!", and observe that nodemon has detected the change to the filesystem
and restarted express for you.

## Testing

Let's prove it works. There are many options for testing in node, but in general the easiest and most full featured is [Jest](https://jestjs.io/).

`npm install jest --save-dev`

Note this adds jest as a dev dependency, not production.

Change your `test` script to simply `jest`, and then run with `npm test`. It will run with no tests found, and thus fail.

### Adding a test

We need to use something else to run our express app in the tests. [Supertest](https://github.com/visionmedia/supertest) does the job:

`npm install supertest --save-dev`

Then create `app.test.js` - jest will pick up this pattern automatically. In general JS keeps the test files next to the source files they are testing. This is a bit odd when you're used to the Rails way, but it may actually be better. You'll get used to it.

Let's add a test:

```
const { app } = require('./app');
const request = require('supertest');

test('hello!', async () => {
  const response = await request(app).get('/')
  expect(response.status).toBe(200);
  expect(response.text).toEqual("Hello dxw!");
});
```

Run it, and it should pass!

## Code style

There are many ways to do things in node and JS, so to keep things consistent it's a good idea to use a linter. `eslint` is the tool you want here,
and dxw already has a convenient package with all our recommendations in it. Install it and all its dependencies like so:

`npm install --save-dev eslint prettier eslint-plugin-prettier eslint-plugin-jest typescript @typescript-eslint/eslint-plugin @wearedxw/eslint-config`

Create `.eslintrc.json`:

```
{
  "env": {
    "node": true,
    "jest": true
  },
  "extends": ["@wearedxw/eslint-config"]
}
```

Add a lint script to run `eslint`, and then `npm run lint` (because `lint` isn't one of the standard commands, you need the extra `run`).

## More resources

* Explore modern ES6 syntax with the [Exercism Javascript track](https://exercism.io/my/tracks/javascript)
* Build a site in [Node and Express](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs)
* Read the Jest guides on [how to test your apps](https://jestjs.io/docs/en/getting-started.html)
* Learn about [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
