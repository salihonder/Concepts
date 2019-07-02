# Memoization
Memoization is the programmatic practice of making long recursive/iterative functions run much faster. By caching the values that the function returns after its initial execution.
```js
function memo(func){
  var cache = {};
    return function(){
      var key = JSON.stringify(arguments);
      if (cache[key]){
        console.log(`cache used for ${arguments[0]}`);
        return cache[key];
      }
      else{
        val = func.apply(null, arguments);
        cache[key] = val;
        return val; 
      }
  }
}
```
> closures do not inherit an outer function’s arguments object.

Example using fibanocci
```js
var fib = memo(function(n) {
   if (n < 2){
     return 1;
   }else{
    //We'll console.log a loader every time we have to recurse
     console.log("loading...");
     return fib(n-2) + fib(n-1);
   }
});

fib(3); 
fib(3); 
fib(2); 
fib(4);

/*
For same input we directly get cached result. no loading..
If we ask for fib(4) we see one loading since fib(3) and fib(2) are already cached.
======================
OUTPUT 
======================
loading...
loading...
cache used for 1
cache used for 3
cache used for 2
loading...
cache used for 2
cache used for 3
*/
```

## Resources
- [Understanding JavaScript Memoization In 3 Minutes](https://codeburst.io/understanding-memoization-in-3-minutes-2e58daf33a19)