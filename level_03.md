---
---

# Level 03

## A note on asynchronous code

You might have noticed something in the small test we wrote in the introduction, namely the call to `await`. The Node.js engine is asynchronous, so when a call is made that takes some time to return (such as an API or Database call), rather than wasting time waiting for a response, the engine just processes the code that follows. This can result in issues when you try to use a variable that has not yet been initialized. For example, if you had:

```
test('hello!', () => {
  response = request(app).get('/')
  expect(response.status).toBe(200);
  expect(response.text).toEqual("Hello dxw!");
});
```

`response` would return `undefined`, as the Node engine has not got round to initialising that variable. Traditionally, we've used callbacks to get around this, which would probably look something like this:

```
test('hello!', () => {
  request(app).get('/', (response) => {
    expect(response.status).toBe(200);
    expect(response.text).toEqual("Hello dxw!");
  })
});
```

However, this could often lead to extremely messy code. Consider if you had another asynchronous function inside this test:

```
test('hello!', () => {
  request(app).get('/', (response) => {
    expect(response.status).toBe(200);
    expect(response.text).toEqual("Hello dxw!");
    doSomethingAsync((result) => {
      console.log(result)
    })
  })
});
```

And potentially another within that function:

```
test('hello!', () => {
  request(app).get('/', (response) => {
    expect(response.status).toBe(200);
    expect(response.text).toEqual("Hello dxw!");
    doSomethingAsync(response, (result) => {
      doAnotherAsyncThing(result, (r) => {
        console.log(r);
      })
    })
  })
});
```

You can see how things could get _very_ messy. This is known as "Callback Hell". ES2015 introduced the concept of _Promises_, which allows functions to return a proxy for a variable which eventually will be returned, and then eventually return that variable.

This allows us to chain another function to the end of our async function, and only do something once that function has returned a value. This is how it would look in our test from before:

```
test('hello!', () => {
  request(app).get('/').then((response) => {
    expect(response.status).toBe(200);
    expect(response.text).toEqual("Hello dxw!");
  })
});
```

And this is how our theoretical "Callback hell" example from earlier would look:

```
test('hello!', () => {
  request(app).get('/').then((response) => {
    expect(response.status).toBe(200);
    expect(response.text).toEqual("Hello dxw!");
    return doSomethingAsync(response);
  }).then((result) => {
    return doAnotherAsyncThing(result);
  }).then((r) => {
    console.log(r);
  });
});
```

Still a bit messy, but easier to parse than our earlier example.

Things got even easier with ES2017, and the async/await syntax. This allows us to define a function as asynchronous with the `async` keyword, and then use `await` to tell the calling code to wait until the function completes. Our "callback hell" example now looks much more like traditional synchronous code:

```
test('hello!', async () => {
  let response = await request(app).get('/');
  expect(response.status).toBe(200);
  expect(response.text).toEqual("Hello dxw!");

  let result = await doSomethingAsync(response);
  let r = await doAnotherAsyncThing(result);

  console.log(r);
});
```

Much easier to read, and far less boilerplate than previously.

For more on Asynchronous programming in Node, see [The Node.JS guide](https://nodejs.dev/learn/modern-asynchronous-javascript-with-async-and-await).

## More resources

- Learn about [asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
- [TypeScript for JavaScript programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
- Mock remote APIs with [Nock](https://github.com/nock/nock)
- Learn about [snapshot testing with Jest](https://jestjs.io/docs/en/snapshot-testing)
