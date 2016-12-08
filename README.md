# babel-plugin-private-class-members

Uses an underscore prefix notation convention to identify private class members. Transpiles them into members that are unpredictable at a runtime.

> Note:
> This is research stage experiment.
> I am looking for feedback to evaluate usefulness/ demand.

## Example transpilations

Input:

```js
class {
  constructor () {
    this._bar = 'Hello, World!';
    this._foo();
  }

  _foo () {
    console.log(this._bar);
  }
}

```

Output:

```js
const _private_bar = '_' + Math.random() * 1e20;
const _private_foo = '_' + Math.random() * 1e20;

class {
  constructor () {
    this[_private_bar] = 'Hello, World!';
    this[_private_foo]();
  }

  [_private_foo] () {
    console.log(this[_private_bar]);
  }
}

```

## Motivation

When designing a library for public consumption, I want to ensure that certain methods and properties are not accessed by the consumer.
