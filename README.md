# sinon-cheat-sheet

## Stubs

#### yields([arg1, arg2, ...])

Let's assume you have a function that accepts a callback as a parameter. If you would like to stub out this function but have the callback still called, you can use the `yields` function. Without using `yields` you would have to stub the entire function call. See an exmaple of using a traditional function stub and using `yields`:

```javascript
// Given
let car = {
  start: (driver, cb) => {
    let duration = 0.45;
    cb(driver + ' has started the car in ' + duration + ' milliseconds');
  }
};
```

```javascript
// Traditional stub
sinon.stub(car, 'start', function(driver, cb) {
  cb(0.01);
};

assert.equals(car.start('me', (result) => return result), 0.01);
```

```javascript
// Using yields
let carStub = sinon.stub(car, 'start');
carStub.yields(0.01); // callback will always be called with 0.01 value

assert.equals(car.start('me', (result) => return result), 0.01);
```
