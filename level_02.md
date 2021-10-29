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

Edit the "scripts" property of your `package.json` so that you have a `"test": "jest"` entry, and then run with `npm test`. It will run with no tests found, and thus fail.

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
  expect(response.text).toEqual("Hello DXW!");
});
```

Run it (`npm test app.test.js`), and it should fail (as 'dxw' should be lowercase):

```js
expect(received).toEqual(expected) // deep equality

Expected: "Hello DXW!"
Received: "Hello dxw!"

  5 |   const response = await request(app).get('/');
  6 |   expect(response.status).toBe(200);
> 7 |   expect(response.text).toEqual("Hello DXW!");
    |                         ^
  8 | });
  9 |

  at Object.<anonymous> (app.test.js:7:25)
```

Nice, we can see the problem clearly, fix it and move on...

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

Add a lint script to `packages.json` to run `eslint`:

```
"lint": "eslint *.js"
```

and then `npm run lint` (because `lint` isn't one of the standard commands, you need the extra `run`).

## BDD

A more familiar "user first" way of describing the behaviour of our app is
possible with the [cypress](https://www.cypress.io) library. There's a nice
intro on this [wanago.io blog
post](https://wanago.io/2019/12/30/javascript-testing-introduction-end-to-end-testing-cypress/).

Cypress is a sophisticated test runner, which aims to replace Selenium. It builds on
several testing libraries including Mocha and Chai and hooks into Chrome as its
default driver, but can also run headless -- and lots more.

1. install the package with `npm install cypress --save-dev`
2. add a `"test:features": "cypress open"` script to `package.json`
3. run `npm run test:features` to generate some files within a new `cypress` directory

Let's describe the homepage in a way which is similar to using Capybara in Rails/RSpec:

in `cypress/integration/homepage.spec.js`:

```js
describe('The home page', () => {
  describe('visited for the first time', () => {
    it('welcomes dxw staff', () => {
      cy.visit('http://localhost:3000');

      cy.get('body').should('contain.text', 'Hello DXW!');
    });
  });
});
````

In the fancy UI which cypress provides in a Chrome window we see the rendered
HTML on the right and the test output on the left:

    Timed out retrying: expected '<body>' to contain text 'Hello DXW!', 
    but the text was 'Hello dxw!'

Nice. Notice that by default cypress is waiting for an interval to see if the
DOM changes to the expected state.

## More resources

* Explore modern ES6 syntax with the [Exercism Javascript track](https://exercism.io/my/tracks/javascript)
* Build a site in [Node and Express](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs)
* Read the Jest guides on [how to test your apps](https://jestjs.io/docs/en/getting-started.html)
* Learn about [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
