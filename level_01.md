---
---
# Level 01

## NVM

Install nvm (the Node Version Manager, like `rbenv`) via homebrew: `brew install nvm`.

## Node versions

Node has a pattern of regular major version releases purely based on a calendar schedule, because semantic versioning is for losers apparently.

Even-numbered major versions are the more stable ones which will get long-term support, so in general it's best to stick to those.

Install node: `nvm install 14`

Set the node version for a project in `.nvmrc` and run `nvm use` to select it.

## NPM

`npm` is the equivalent of `bundler`. `yarn` does the same job, but uses a different lockfile syntax.

nvm can help us make sure it's all good to go: `nvm install-latest-npm`

In a new directory you can then run `npm init` to create the `package.json`, which is the equivalent of the `Gemfile`.

### Running scripts

Your default `package.json` will have a `test` script defined. You can run this (and any other command you add to `scripts`) with `npm run test` (or `npm test` for short in this case, as one of `npm`'s built in commands).

## More resources

* JavaScript [first steps](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps)
* Do Codeacademy's ["Introduction to Node.js" course](https://www.codecademy.com/learn/learn-node-js/modules/introduction-to-node-js)
