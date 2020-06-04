# `Functional Programming`
Gene Kuo

---

## `Parameters and Arguments`
* Arguments are the values you pass in, and parameters are the named variables inside the function that receive those passed-in values.
* Default parameters
* Arity

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y,z) {    
	// ..
}
foo.length;             // 3
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y = 2) {    
  // ..
}
function bar(x,...args) {    
  // ..
}
function baz( {a,b} ) {    
  // ..
}
foo.length;             // 1
bar.length;             // 1
baz.length;             // 1
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y,z) {    
  console.log( arguments.length );
}
foo( 3, 4 );    // 2
</code></pre>
</section>

---

## `Spread and gather`
* Access the arguments in a positional array-like way

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y,z,...args) {    
  console.log( x, y, z, args );
}
foo();                  // undefined undefined undefined []
foo( 1, 2, 3 );         // 1 2 3 []
foo( 1, 2, 3, 4 );      // 1 2 3 [ 4 ]
foo( 1, 2, 3, 4, 5 );   // 1 2 3 [ 4, 5 ]
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(...args) {    
	console.log( args[3] );
}
var arr = [ 2 ];
foo( 1, ...arr, 3, ...[4,5] );      // 4
</code></pre>
</section>

---

## `Parameter destructuring`
* Declare a pattern for the structure (object, array, etc.) that you expect, and indicate  decomposition (assignment) of its individual parts
* JavaScript doesn’t have named arguments, but parameter object destructuring can be used.

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo( [x,y,...args] = [] ) {    
  // ..
}
foo( [1,2,3] );
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
	function foo( {x,y} = {} ) {    
	  console.log( x, y );
	}
	foo( {    
		y: 3
	} ); 
</code></pre>
</section>

---

## `Returning values`
* In JavaScript, functions always return a value
* Return multiple values

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo() {}
function bar() {    
  return;
}
function baz() {    
  return undefined;
}
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo() {    
  var retValue1 = 11;    
  var retValue2 = 31;    
  return [ retValue1, retValue2 ];
}

var [ x, y ] = foo();
console.log( x + y );           // 42
</code></pre>
</section>

---

## `Explicit return and implicit return`
* Implicit return: A function output some or all of its values by changing variables outside itself
* Side-effects

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
var y;
function f(x) {    
  y = (2 * Math.pow( x, 2 )) + 3;
}
f( 2 );
y;   // 11
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function f(x) {    
  return (2 * Math.pow( x, 2 )) + 3;
}    

var y = f( 2 );
y;                      // 11
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function sum(list) {    
  var total = 0;    
  for (let i = 0; i < list.length; i++) {        
    if (!list[i]) list[i] = 0;        
    total = total + list[i];    
  }    
  return total;
}
var nums = [ 1, 3, 9, 27, , 84 ];
sum( nums );            // 124
</code></pre>
</section>

---

## `Higher-order function`
* A function that receives or returns other function values

---

## `Example`
<section>
	<pre><code data-trim data-noescape>
function forEach(list, fn) {
	for (let v of list) {
		fn(v)
	}
}
forEach([1,2,3,4,5], function each(val) {
	console.log(val)
});

function foo() {
	return function inner(msg) {
		return msg.toUpperCase()
	}
}
var f = foo();
f('Hello');
  </code></pre>
</section>

---

## `Closure`
* When an inner function makes reference to a variable from the outer function
* In this case, the function remembers and accesses variables from outside of its own scope, even when that function is executed in a different scope
* Can remember function values via closure

---

## `Example`
<section>
	<pre><code data-trim data-noescape>
function person(name) {
	return function identify() {
		console.log(`I am ${name}`)
	}
}
var john = person('John');
john();		// I am John

function runningCounter(start) {
	var val = start
	return function current(increment = 1) {
		val = val + increment
		return val
	}
}
var score = runningCounter(0);
score();		// 1
score();		// 2
score(13);  // 15
  </code></pre>
</section>

---

## `Remember function values`
<section>
	<pre><code data-trim data-noescape>
function formatter(formatFn) {
	return function inner(str) {
		return formatFn(str)
	}
}
var lower = formatter(
	function formatting(v) {
		return v.toLowerCase()
	}
)
var upperFirst = formatter(
	function formatting(v) {
		return v[0].toUpperCase() + v.substr(1).toLowerCase()
	}
)
lower('HELLO')
upperFirst('hello')
  </code></pre>
</section>

---

## `Anonymous function`
<section>
	<pre><code data-trim data-noescape>
function foo(fn) {    
	 console.log( fn.name );
}
var x = function(){};
foo( x );               // x
foo( function(){} );    //
</code></pre>
</section>

---

## `Recursion self-reference`
<section>
	<pre><code data-trim data-noescape>
function findPropIn(propName,obj) {    
  if (obj == undefined || typeof obj != "object") return;    
  if (propName in obj) {        
    return obj[propName];    
  }  else {        
    for (let prop of Object.keys( obj )) {            
      let ret = findPropIn( propName, obj[prop] );            
      if (ret !== undefined) {                
        return ret;            
      }        
    }    
  }
}
</code></pre>
</section>

---

## `Immediately Invoked Function Expression (IIFE)`
<section>
	<pre><code data-trim data-noescape>
(function IIFE(){    
  // an IIFE!
})();
</code></pre>
</section>

---

## `Function manipulation`
<section>
	<pre><code data-trim data-noescape>
function unary(fn) {
  return function onlyOneArg(arg) {
    return fn(arg)
  }
}

var unary =
  fn =>
    arg =>
      fn(arg)

['1','2','3'].map(unary(parseInt))
  </code></pre>
</section>

---

## `Identity function`
* Coercion, default function as argument

---

## `Identity function`

<section>
	<pre><code data-trim data-noescape>
function identity(v) {
  return v
}

var identity =
  v =>
    v

'     This is a functional programming text...'
  .split(/\s|\b/).filter(identity)
  </code></pre>
</section>

---

## `Constant function`
* Represent value as a function as to send to other function expecting functions

---

## `Constant function`
<section>
	<pre><code data-trim data-noescape>
function constant(v) {
  return function value() {
    return v
  }
}

var constant =
  v =>
    () =>
      v
p1.then( foo ).then( () => p2 ).then( bar );
p1.then( foo ).then( constant( p2 ) ).then( bar );
</code></pre>
</section>

---

## `Adapt a function's signature`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y){
  console.log(x + y)
}
function bar(fn) {
  fn([3, 9])
}
bar(foo)	// ??
  </code></pre>
</section>

---

## `Spread arguments`
<section>
	<pre><code data-trim data-noescape>
function spreadArgs(fn) {
  return function spreadFn(argsArr) {
    return fn(...argsArr)
  }
}

var spreadArgs=
  fn =>
    argsArr =>
      fn(...argsArr)

bar(spreadArgs(foo))	// 12
  </code></pre>
</section>

---

## `Gather arguments`
<section>
	<pre><code data-trim data-noescape>
function gatherArgs(fn) {
  return function gatheredFn(...argsArr) {
    return fn(argsArr)
  }
}

var gatherArgs =
  fn =>
    (...argsArr) =>
      fn(argsArr)

function combine([v1, v2]) {
  return v1 + v2
}
[1,2,3,4,5].reduce(gatherArgs(combine))		// 15
  </code></pre>
</section>

---

## `Partial application`
* A reduction in a function's arity

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function ajax(url,data,callback) {     // .. }
</code></pre>
</section>

---

## `Partial applied function`
<section>
	<pre><code data-trim data-noescape>
function partial(fn, ...presetArgs) {
	return function partialApplied(...laterArgs) {
		return fn(...presetArgs, ...laterArgs)
	}
}

var partial =
	(fn, ...presetArgs) =>
		(...laterArgs) =>
			fn(...presetArgs, ...laterArgs)
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
var getPerson = partial( ajax, "http://some.api/person" );
var getOrder = partial( ajax, "http://some.api/order" );
var getCurrentUser = partial( getPerson, { user: CURRENT_USER_ID } );

function add(x,y) {    
  return x + y;
}
[1,2,3,4,5].map( partial( add, 3 ) ); // [4,5,6,7,8]
</code></pre>
</section>

---

## `Reverse arguments`
<section>
	<pre><code data-trim data-noescape>
function reverseArgs(fn) {    
  return function argsReversed(...args){        
    return fn( ...args.reverse() );    
  };
}
var reverseArgs =    
  fn =>        
    (...args) =>            
      fn( ...args.reverse() );
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
var cache = {};
var cacheResult = reverseArgs(    
  partial( reverseArgs( ajax ), function
    onResult(obj){        
      cache[obj.id] = obj;    
    } )
);

cacheResult( "http://some.api/person", { user: CURRENT_USER_ID } );
</code></pre>
</section>

---

## `Partial right`
<section>
	<pre><code data-trim data-noescape>
function partialRight(fn,...presetArgs) {    
  return function partiallyApplied(...laterArgs){        
    return fn( ...laterArgs, ...presetArgs );    
  };
}
var partialRight =    
  (fn,...presetArgs) =>        
    (...laterArgs) =>            
      fn( ...laterArgs, ...presetArgs );
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function foo(x,y,z,...rest) {    
  console.log( x, y, z, rest );
}
var f = partialRight( foo, "z:last" );
f( 1, 2 );          // 1 2 "z:last" []
f( 1 );             // 1 "z:last" undefined []
f( 1, 2, 3 );       // 1 2 3 ["z:last"]
f( 1, 2, 3, 4 );    // 1 2 3 [4,"z:last"]
</code></pre>
</section>

---

## `Currying`
<section>
	<pre><code data-trim data-noescape>
curriedAjax( "http://some.api/person" )    
  ( { user: CURRENT_USER_ID } )        
    ( function foundUser(user){ ... } );
</code></pre>
</section>

---

## `curry function`
<section>
	<pre><code data-trim data-noescape>
function curry(fn,arity = fn.length) {    
  return (function nextCurried(prevArgs){        
    return function curried(nextArg){
      var args = [ ...prevArgs, nextArg ];            
      if (args.length >= arity) {                
        return fn( ...args );            
      } else {                
        return nextCurried( args );            
      }        
    };    
  })( [] );
}
</code></pre>
</section>

---

## `curry function`
<section>
	<pre><code data-trim data-noescape>
var curry =    
  (fn,arity = fn.length,nextCurried) =>        
    (nextCurried = prevArgs =>
      nextArg => {                
        var args = [ ...prevArgs, nextArg ];                
        if (args.length >= arity) {                    
          return fn( ...args );                
        } else {                    
          return nextCurried( args );                
        }            
      }        
  )( [] );
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function sum(...nums) {    
  var total = 0;    
  for (let num of nums) {        
    total += num;    
  }    
  return total;
}
sum( 1, 2, 3, 4, 5 );     // 15
var curriedSum = curry( sum, 5 );
curriedSum( 1 )( 2 )( 3 )( 4 )( 5 );
</code></pre>
</section>

---

## `Loose curry`
<section>
	<pre><code data-trim data-noescape>
function looseCurry(fn,arity = fn.length) {    
  return (function nextCurried(prevArgs){
    return function curried(...nextArgs){            
      var args = [ ...prevArgs, ...nextArgs ];            
      if (args.length >= arity) {                
        return fn( ...args );            
      } else {                
        return nextCurried( args );            
      }        
    };    
  })( [] );
}
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
var curriedSum = looseCurry( sum, 5 );
curriedSum( 1 )( 2, 3 )( 4, 5 ); 
</code></pre>
</section>

---

## `Uncurry`
<section>
	<pre><code data-trim data-noescape>
function uncurry(fn) {    
  return function uncurried(...args){  
    var ret = fn;        
    for (let arg of args) {            
      ret = ret( arg );        
    }        
    return ret;    
  };
}
</code></pre>
</section>

---

## `Uncurry`
<section>
	<pre><code data-trim data-noescape>
var uncurry =    
  fn =>        
    (...args) => {            
      var ret = fn;            
      for (let arg of args) {
        ret = ret( arg );            
      }            
      return ret;        
    };
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function sum(...nums) {    
  var sum = 0;    
  for (let num of nums) {        
    sum += num;    
  }    
  return sum;
}
var curriedSum = curry( sum, 5 );
var uncurriedSum = uncurry( curriedSum );
curriedSum( 1 )( 2 )( 3 )( 4 )( 5 );        // 15
uncurriedSum( 1, 2, 3, 4, 5 );    
uncurriedSum( 1, 2, 3 )( 4 )( 5 );  
</code></pre>
</section>

---

## `Function composition`
<section>
	<pre><code data-trim data-noescape>
function compose(...fns) {
  return function composed(result) {
    var list = [...fns]
    while (list.length > 0) {
      result = list.pop()(result)
    }
    return result
  }
}
var compose =
  (...fns) =>
    result => {
      var list = [...fns]
      while (list.length > 0) {
        result = list.pop()(result)
      }
      return result
    }
  </code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function skipShortWords(words) {    
  var filteredWords = [];    
  for (let word of words) {        
    if (word.length > 4) {            
      filteredWords.push( word );        
    }    
  }    
  return filteredWords;
}
function words(str) {
  return String(str)
    .toLowerCase()
    .split(/\s|\b/)
    .filter(function alpha(v){
        return /^[\w]+$/.test(v)
    })
}
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
function unique(list) {
  var uniqueList = []
  for (let v of list) {
    if (uniqueList.indexOf(v) === -1) {
      uniqueList.push(v)
    }
  }
  return uniqueList
}
var text = "To compose two functions together, pass the \
  output of the first function call as the input of the \
  second function call.";
var biggerWords = compose( skipShortWords, unique, words );
var wordsUsed = biggerWords(text);
wordsUsed		// ["compose", "functions", "together", "output", "first", "function", "input", "second"]
  </code></pre>
</section>

---

## `Partial application with compose`
<section>
	<pre><code data-trim data-noescape>
var filterWords = partialRight( compose, unique, words );
var biggerWords = filterWords( skipShortWords );
biggerWords( text );
</code></pre>
</section>

---

## `Currying with compose`
<section>
	<pre><code data-trim data-noescape>
var biggerWords = curry(reverseArgs(compose), 3)(words)(unique)(skipShortWords)
biggerWords(text)
</code></pre>
</section>

---

## `Reduction of functions`
<section>
	<pre><code data-trim data-noescape>
function compose(...fns) {    
  return function composed(result){        
    return [...fns].reverse().reduce( function
      reducer(result,fn){            
        return fn( result );        
      }, result );    
  };
}
var compose = (...fns) =>    
  result =>        
    [...fns].reverse().reduce(            
      (result,fn) =>                
        fn( result )            
      , result        
    );
</code></pre>
</section>

---

## `Outer composed function with multiple args`
<section>
	<pre><code data-trim data-noescape>
function compose(...fns) {    
  return fns.reverse().reduce(
    function reducer(fn1,fn2){
      return function composed(...args){            
        return fn2( fn1( ...args ) );        
      };    
    } );
}
var compose =    
  (...fns) =>        
    fns.reverse().reduce( (fn1,fn2) =>            
      (...args) =>                
        fn2( fn1( ...args ) )        
    );
</code></pre>
</section>

---

## `Compose with recursion`
<section>
	<pre><code data-trim data-noescape>
function compose(...fns) {    
  // pull off the last two arguments    
  var [ fn1, fn2, ...rest ] = fns.reverse();
  var composedFn = function composed(...args){        
    return fn2( fn1( ...args ) );    
  };    
  if (rest.length == 0) return composedFn;    
  return compose( ...rest.reverse(), composedFn );
}
var compose =    
  (...fns) => {        
    // pull off the last two arguments
    var [ fn1, fn2, ...rest ] = fns.reverse();        
    var composedFn =            
      (...args) =>                
        fn2( fn1( ...args ) );        
    if (rest.length == 0) return composedFn;        
    return compose( ...rest.reverse(), composedFn );    
  };
</code></pre>
</section>

---

## `Pipe`
<section>
	<pre><code data-trim data-noescape>
function pipe(...fns) {    
  return function piped(result){        
    var list = [...fns];        
    while (list.length > 0) {            
      // take the first function from the list            
      // and execute it            
      result = list.shift()( result );
    }        
    return result;    
  };
}
var pipe = reverseArgs( compose );
</code></pre>
</section>

---

## `Examples`
<section>
	<pre><code data-trim data-noescape>
var biggerWords = pipe( words, unique, skipShortWords );
biggerWords(text)
</code></pre>
</section>

---

## `Recursion`
* Direct recursion, binary recursion, mutual recursion
* Implicitly track state via `return` of call stacks
* Declarative vs. imperative

---

<section>
	<pre><code data-trim data-noescape>
function sum(num1,...nums){
  if (nums.length == 0) return num1
  return num1 + sum(...nums)
}
  </code></pre>
</section>

---

## `maxEven`
<section>
	<pre><code data-trim data-noescape>
function maxEven(num1,...restNums){
    var maxRest = restNums.length > 0 ?
          maxEven(...restNums) :
          undefined
    return (num1 % 2 != 0 || num1 < maxRest) ?
          maxRest:
          num1
}
  </code></pre>
</section>
* Other way: list operations: with a filter(..) to include only evens and then a reduce(..) to find the max

---

## `Binary tree`
<section>
	<pre><code data-trim data-noescape>
depth(node):
  1 + max(depth(node.left), depth(node.right))

function depth(node) {
  if (node) {
    let depthLeft = depth(node.left)
    let depthRight = depth(node.right)
    return 1 + Math.max(depthLeft, depthRight)
  }
  return 0
}
</code></pre>
</section>

---

## `Tail calls`
* Stack frames
* isOdd/isEven
* Tail Call Optimization (TCO)
* Proper tail calls (PTC)

---

## `Refactor to tail calls`
<section>
	<pre><code data-trim data-noescape>
"use strict"
var sum = (function IIFE(){
  return function sum(...nums) {   // exposed func     
    return sumRec( /*initialResult=*/0, ...nums )   
  }    
  function sumRec(result,num1,...nums) { // hidden func
    result = result + num1
    if (nums.length == 0) return result       
    return sumRec( result, ...nums )   
  }
})()
  </code></pre>
</section>

---

<section>
	<pre><code data-trim data-noescape>
"use strict"
function sum(num1,num2,...nums) {    
  num1 = num1 + num2   
  if (nums.length == 0) return num1    
  return sum( num1, ...nums )
}
  </code></pre>
</section>

---

## `Methods of Array prototype`
* With imperative code, each intermediate result in a set of calculations is stored in variable(s) through assignment
* `forEach()`, `some()` , `every()`

---

## `map()`
<section>
	<pre><code data-trim data-noescape>
function map(mapperFn,arr) {
  var list = []
  for (let [idx,v] of arr.entries()) {
    list.push(
        mapperFn(v, idx, arr)
      )
  }
  return list
}

function unary(fn) {
  return function onlyOneArg(arg) {
    return fn(arg)
  }
}
  </code></pre>
</section>

---

## `Transform functions`
* Independent transformation
* Eager operations vs, lazy operations
* Mapper without side effects

---

<section>
	<pre><code data-trim data-noescape>
var one = () => 1
var two = () => 2
var three = () => 3
[one,two,three].map( fn => fn() );

var increment = v => ++v
var decrement = v => --v
var square = v => v * v
var double = v => 2 * v
[increment, decrement, square]
  .map(fn => compose(fn, double))
  .map(fn => fn(3))
  </code></pre>
</section>

---

## `Functors`
* A functor is a value (array) that has a utility (map()) for using an operator function (mapper) on that value, which preserves composition (returns new array)

---

## `string functor`
<section>
	<pre><code data-trim data-noescape>
function uppercaseLetter(c) {
	var code = c.charCodeAt(0)
	if (code >= 97 && code <= 122) {
		code = code - 32
	}
	return String.fromCharCode(code)
}

function stringMap(mapperFn,str) {
	return [...str].map(mapperFn).join('')
}
stringMap(uppercaseLetter, 'Hello World!')
  </code></pre>
</section>

---

## `filter()`
* Predicate function

---

## `Implementation`
<section>
	<pre><code data-trim data-noescape>
function filter(predicateFn,arr) {
	var list = []
	for (let [idx,v] of arr.entries()) {
		if(predicateFn(v, idx, arr)) {
			list.push(v)
		}
	}
	return list
}
var isOdd = v => v % 2 == 1

[1,2,3,4,5].filter(isOdd)
  </code></pre>
</section>

---

## `Point free`
<section>
	<pre><code data-trim data-noescape>
function not(predicate) {
	return function negated(...args) {
		return !predicate(...args)
	}
}

var isOdd = v => v % 2 == 1
var isEven = not(isOdd)
[1,2,3,4,5].filter(not(isEven))
  </code></pre>
</section>

---

## `FilterIn and filterOut`
<section>
	<pre><code data-trim data-noescape>
var filterIn = filter
function filterOut(predicateFn,arr) {
	return filterIn(not(predicateFn), arr)
}

filterIn(isOdd, [1,2,3,4,5])
filterOut(isOdd, [1,2,3,4,5])
  </code></pre>
</section>

---

## `reduce()`
* A combination/reduction takes two values and makes them into one value.
* Numbers combined through arithmetic, strings through concatenation, and functions through composition
* At least one value in the reduction (either in the array or specified as initialValue)
* Reducers receive the current reduction result and the next value to reduce it with

---

## `Implementation`
<section>
	<pre><code data-trim data-noescape>
function reduce(reducerFn,initialValue,arr) {
	var acc, startIdx
	if (arguments.length == 3) {
		acc = initialValue
		startIdx = 0
	} else if (arr.length > 0) {
		acc = arr[0]
		startIdx = 1
	} else {
		throw new Error('Must provide at least one value.')
	}
	for (let idx = startIdx; idx < arr.length; idx++) {
		acc = reducerFn(acc, arr[idx], idx, arr)
	}
	return acc
}

[5,10,15].reduce((product,v) => product * v, 3)
  </code></pre>
</section>

---

## `compose()`
<section>
	<pre><code data-trim data-noescape>
function compose(...fns) {
	return function composed(result) {
		return
			[...fns].reverse().reduce(function
				reducer(result,fn) {
					return fn(result)
					}, result)
	}
}
  </code></pre>
</section>

---

## `pipe()`
<section>
	<pre><code data-trim data-noescape>
function pipe(...fns) {
	return function piped(result){        
		var list = [...fns];        
		while (list.length > 0) {
			result = list.shift()(result);        
		}        
		return result;    
	};
}
var pipeReducer = (composedFn,fn) => pipe(composedFn, fn)

var fn =
	[3,17,6,4]
		.map(v => n => v * n)
		.reduce(pipeReducer)
fn(9)
  </code></pre>
</section>

---

## `reduceRight()`
<section>
	<pre><code data-trim data-noescape>
var hyphenate = (str,char) => `${str}-${char}`
['a','b','c'].reduce(hyphenate)
['a','b','c'].reduceRight(hyphenate)

function compose(...fns) {
	return function composed(result) {
		return fns.reduceRight(function reducer(result,fn) {
			return fn(result)
			}, result)
	}
}
  </code></pre>
</section>

---

## `Map as reduce`
<section>
	<pre><code data-trim data-noescape>
var double = v => v * 2
[1,2,3,4,5].map(double)
[1,2,3,4,5].reduce(
		(list,v) => (
				list.push(double(v)),
				list
			), []
	)
  </code></pre>
</section>

---

## `Filter as reduce`
<section>
	<pre><code data-trim data-noescape>
var isOdd = v => v % 2 == 1
[1,2,3,4,5].filter(isOdd)
[1,2,3,4,5].reduce(
		(list,v) => (
				isOdd(v) ? list.push(v):
				undefined,
				list
			), []
	)
  </code></pre>
</section>

---

## `Unique`
<section>
	<pre><code data-trim data-noescape>
var unique =
	arr =>
		arr.filter(
				(v,idx) =>
					arr.indexOf(v) == idx
			)

var unique =
	arr =>
		arr.reduce(
				(list,v) =>
					list.indexOf(v) == -1 ?
						(list.push(v), list) : list
						, []
			)
  </code></pre>
</section>

---

## `Flatten`
<section>
	<pre><code data-trim data-noescape>
var flatten =
	arr =>
		arr.reduce(
				(list,v) =>
					list.concat(Array.isArray(v) ?
						flatten(v) : v)
						, []
			)

flatten([[0,1],2,3,[4,[5,6,7],[8,[9,[10,[11,12],13]]]]])
  </code></pre>
</section>

---

## `Map and Flatten`

---

## `Zip`
<section>
	<pre><code data-trim data-noescape>
function zip(arr1,arr2) {
	var zipped = []
	arr1 = [...arr1]
	arr2 = [...arr2]
	while (arr1.length > 0 && arr2.length > 0) {
		zipped.push([arr1.shift(), arr2.shift()])
	}
	return zipped
}

zip([1,3,5,7,9], [2,4,6,8,10])
  </code></pre>
</section>

---

## `Merge`
<section>
	<pre><code data-trim data-noescape>
function mergeLists(arr1,arr2) {
	var merged = []
	arr1 = [...arr1]
	arr2 = [...arr2]
	while (arr1.length > 0 || arr2.length > 0) {
		if (arr1.length > 0) {
			merged.push(arr1.shift())
		}
		if (arr2.length > 0) {
			merged.push(arr2.shift())
		}
	}
	return merged
}

mergeLists([1,3,5,7,9], [2,4,6,8,10])
  </code></pre>
</section>

---

## `Unifying strategies`

---

## `Transducing`
* Transforming with reduction

<section>
	<pre><code data-trim data-noescape>
function isLongEnough(str) {
  return str.length >= 5
}
function isShortEnough(str) {
  return str.length <= 10
}

var words = ['You', 'have', 'written', 'something', 'very', 'interesting']
words
  .filter(isLongEnough)
  .filter(isShortEnough)
  </code></pre>
</section>
