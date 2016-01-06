chillout
========

Reduce JavaScript CPU usage by asynchronous iteration.

Provides asynchronous iteration functions that runs with low CPU usage.
Each iteration adds delay if the processing is heavy.
Iterate without delay if processing is fast.
It can execute JavaScript without "Warning: Unresponsive Script" alert in browser. 

## Benchmarks

Benchmarks the ForStatement and `chillout.repeat`.


### ForStatement:

![CPU usage without chillout](https://raw.github.com/wiki/polygonplanet/chillout/images/cpu-usage-without-chillout.png)

* Processing time: 51049ms.
* CPU total average: **31.10%**

### chillout.repeat

![CPU usage with chillout](https://raw.github.com/wiki/polygonplanet/chillout/images/cpu-usage-with-chillout.png)

* Processing time: 59769ms.
* CPU total average: **22.76%**

#### Source

```javascript
function heavyProcess() {
  for (var i = 0; i < 10000; i++) {
    for (var j = 0; j < 10000; j++) {
      var r = Math.random();
    }
  }
}
```

ForStatement:

```javascript
var time = Date.now();
for (var i = 0; i < 500; i++) {
  heavyProcess();
}
var processingTime = Date.now() - time;
console.log(processingTime);
```

chillout.repeat:

```javascript
var time = Date.now();
chillout.repeat(500, function(i) {
  heavyProcess();
}).then(function() {
  var processingTime = Date.now() - time;
  console.log(processingTime);
});
```

----

## iteration functions

### each

Executes a provided function once per array or object element.  
The iteration will break if the callback function returns `false`.

* chillout.**each** ( obj, callback [, context ] )  
  @param {_Array|Object_} _obj_ Target array or object.  
  @param {_Function_} *callback* Function to execute for each element, taking three arguments:  
  - value: The current element being processed in the array/object.
  - key: The key of the current element being processed in the array/object.
  - obj: The array/object that `each` is being applied to.

  @param {_Object_} [_context_] Value to use as `this` when executing callback.  
  @return {_Promise_} Return new Promise.

Example of array iteration:
```javascript
var sum = 0;
chillout.each([1, 2, 3], function(value, i) {
  sum += value;
}).then(function() {
  console.log(sum); // 6
});
```

Example of object iteration:
```javascript
var result = '';
chillout.each({ a: 1, b: 2, c: 3 }, function(value, key) {
  result += key + value;
}).then(function() {
  console.log(result); // 'a1b2c3'
});
```

### repeat

Executes a provided function the specified number times.  
The iteration will break if the callback function returns `false`.

* chillout.**repeat** ( count, callback [, context ] )  
  @param {_number|Object_} _count_ The number of times or object for execute the function.  
  Following parameters are available if specify object:
  - start: The number of start.
  - step: The number of step.
  - end: The number of end.

  @param {_Function_} _callback_ Function to execute for each times, taking an argument:
  - i: The current number.

  @param {_Object_} [_context_] Value to use as `this` when executing callback.
  @return {_Promise_} Return new Promise.

Example of specify number:

```javascript
chillout.repeat(5, function(i) {
  console.log(i);
}).then(function() {
  console.log('end');
});
// 0
// 1
// 2
// 3
// 4
// end
```

Example of specify object:

```javascript
chillout.repeat({ start: 10, step: 2, end: 20 }, function(i) {
  console.log(i);
}).then(function() {
  console.log('end');
});
// 10
// 12
// 14
// 16
// 18
// end
```

### forever

Executes a provided function forever.  
The iteration will break if the callback function returns `false`.

* chillout.**forever** ( callback [, context ] )  
  @param {_Function_} _callback_ The function that is executed for each iteration.  
  @param {_Object_} [_context_] Value to use as `this` when executing callback.  
  @return {Promise} Return new Promise.

```javascript
var i = 0;
chillout.forever(function() {
  console.log(i);
  i++;
  if (i === 5) {
    return false; // stop iteration
  }
}).then(function() {
  console.log('end');
});
// 0
// 1
// 2
// 3
// 4
// end
```

## License

MIT