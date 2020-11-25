---
---
# Level 01

## Nodenv

Install [`nodenv`](https://github.com/nodenv/nodenv/blob/master/README.md) (`rbenv` for Node) via homebrew: `brew install nodenv`. Note the caveats have you add to your `.{z,ba}shrc` - you'll need this for it to work.

## Node versions

Node has a pattern of regular major version releases purely based on a calendar schedule, because semantic versioning is for losers apparently.

Even-numbered major versions are the more stable ones which will get long-term support, so in general it's best to stick to those.

Install Node: `nodenv install 14.14.0`

Set the Node version for a project in `.node-version` and `nodenv` will use it automatically when you're in that directory or a sub-directory (like `rbenv`).

## NPM

`npm` is the equivalent of `bundler`. `yarn` does the same job, and we could standardise on that - it has stronger locking of packages, which might be good.

`nodenv` will have installed the matching version of `npm` for you, but if you want to update it, you can always run `npm install --global npm@latest` to update it.

In a new directory you can then run `npm init` to create the `package.json`, which is the equivalent of the `Gemfile`.

### Running scripts

Your default `package.json` will have a `test` script defined. You can run this (and any other command you add to `scripts`) with `npm test`.

## More resources

* JavaScript [first steps](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps)
* Do Codeacademy's ["Introduction to Node.js" course](https://www.codecademy.com/learn/learn-node-js/modules/introduction-to-node-js)
