# babel-plugin-private-class-members

Uses an underscore prefix notation convention to identify private class members. Transpiles them into members that are unpredictable at a runtime.

* [Example transpilations](#example-transpilations)
* [Motivation](#motivation)

> Note:
> This is research stage experiment.
> I am looking for feedback to evaluate usefulness/ demand.

## Example transpilations

Input:

```js
class Foo {
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
const _private_state = new WeakMap();

const _set = (object, name, value) => {
  if (!_private_state.has(object)) {
    _private_state.set(object, new Map());
  }

  _private_state.get(object).set(name, value);
};

const _get = (object, name) => {
  return _private_state.get(object).get(name);
};

const _private_foo = (context) => {
  console.log(_get(context, 'bar'));
};

class Foo {
  constructor () {
    _set(this, 'bar', 'Hello, World!');
    _private_foo(this);
  }
}

```

## Motivation

When designing a library for public consumption, I want to ensure that certain methods and properties are not accessed by the consumer.
