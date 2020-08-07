---
---
# Node for Rails developers

This document is a primer for Rails devs looking at node.js. The node.js ecosystem is much more varied than Rails - there are normally many ways to get something done, and it's easy to lose a lot of time to choosing which to use. This document attempts to shortcut some of that. OK, here we go:

Install nvm via homebrew: `brew install nvm`

## Node versions

Node has a pattern of regular major version releases purely based on a calendar schedule, because semantic versioning is for losers apparently.

Even-numbered major versions are the more stable ones which will get long-term support, so in general it's best to stick to those.

Install node: `nvm install 14`

Set the node version for a project in `.nvmrc` and run `nvm use` to select it.

## NPM

`npm` is the equivalent of `bundler`. `yarn` does the same job, and we could standardise on that - it has stronger locking of packages, which might be good.

nvm can help us make sure it's all good to go: `nvm install-latest-npm`

In a new directory you can then run `npm init` to create the `package.json`, which is the equivalent of the `Gemfile`.

### Running scripts

Your default `package.json` will have a `test` script defined. You can run this (and any other command you add to `scripts`) with `npm test`.

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

app.listen(3000);

module.exports = { app };
```

Also create `index.js`:

```
const { app } = require('./app');

app.listen(3000);
```

We make these two separate files so we can test the express app without starting it up fully. Add a `start` script to `package.json` that runs `node index.js`.

Then you can run it with `npm start`, visit http://localhost:3000 and say hello.

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
