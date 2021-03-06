# sinon-cheat-sheet

## Stubs

#### [Creation]

#### stub()

This is an anonymous stub, where you are not overriding an existing function but are providing a sinon function whose behavior can be controlled and monitored.

```javascript
let explosiveStart = sinon.stub(); // anonymous stub
explosiveStart.returns('BOOM!');

let car = {
  start: explosiveStart
};

assert.equal(car.start(), 'BOOM!');
```


#### [Methods]

#### withArgs(arg1[, arg2, ...])

This provides the ability to conditionally stub a function, where based on what arguments are passed into a function determines how it responds. `withArgs` is part of the conditional functions (e.g., `onCall` and `onSecondCall`) and is used in combination with result functions (e.g., `returns` and `throws`).

```javascript
let start = sinon.stub();
start.withArgs('ford').returns('awesome');
start.withArgs('chevy').throws();

assert.equal(start('ford'), 'awesome');
assert.throws(start('chevy'), Error);
```

#### yields([arg1, arg2, ...])

Let's assume you have a function that accepts a single callback as a parameter, where a callback is something of type `function`. If you would like to stub out this function but have the callback still called, you can use the `yields` function. Without using `yields` you would have to stub the entire function call. See an exmaple of using a traditional function stub and using `yields`:

```javascript
// Given
let car = {
  start: (driver, cb) => {}
};
```

```javascript
// Traditional stub
sinon.stub(car, 'start', function(driver, cb) {
  cb(0.01);
};
```

```javascript
// Using yields
let startStub = sinon.stub(car, 'start');
startStub.yields(0.01); // callback will always be called with 0.01 value

car.start('me', (result) => {
  assert.equal(result, 0.01);
});
```

Also note, that if function takes more than one callback, `yields` will call the first one. For example:

```javascript
let car = {
  start: (firstCb, secondCb) => {};
};

let startStub = sinon.stub(car, 'start');
startStub.yields('firstly');

car.start(
  (result) => {
    assert.equal(result, 'firstly'); // callback yielded since it was the first

  }, 
  (result) => {
    assert.equal(result, undefined); // default stub for second callback
  }
);
```

Also note, that if no callback is provided to the stubbed function, and you apply `yields` on the stub, then at runtime you will receive the following error:

```javascript
TypeError: start expected to yield, but no callback was passed.
```

#### yieldsOn(context, [arg1, arg2, ...])

Like `yields` with the additional ability to provide the `this` context within the callback.
