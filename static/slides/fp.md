# `Functional Programming`

---

## `Explicit return and implicit return`

---

## `Higher-order function`
* A function that received or returns other function values

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
})

function foo() {
	return function inner(msg) {
		return msg.toUpperCase()
	}
}
var f = foo
f('Hello')
  </code></pre>
</section>

---

## `Closure`
* When an inner function makes reference to a variable from the outer function
* In this case, the function remembers and accesses variables from outside of its own scope, even when that function is executed in a different scope

---

`Example`
<section>
	<pre><code data-trim data-noescape>
function person(name) {
	return function identify() {
		console.log(`I am ${name}`)
	}
}
var john = person('John')
john()

function runningCounter(start) {
	var val = start
	return function current(increment = 1) {
		val = val + increment
		return val
	}
}
var score = runningCounter(0)
score()
score()
score(13)
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

<section>
	<pre><code data-trim data-noescape>
function constant(v) {
  return fucntion value() {
    return v
  }
}

var constant =
  v =>
    () =>
      v
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
bar(foo)
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

bar(spreadArgs(foo))
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
[1,2,3,4,5].reduce(gatherArgs(combine))
  </code></pre>
</section>

---

## `Partial application`
* A reduction in a function's arity

---

## `Partial appled function`
<section>
	<pre><code data-trim data-noescape>
	function partial(fn, presetArgs) {
		return function partialApplied(...laterArgs) {
			return fn(...presetArgs, ...laterArgs)
		}
	}

	var partial =
		(fn, presetArgs) =>
			(...laterArgs) =>
				fn(...presetArgs, ...laterArgs)
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

## `Functions`
<section>
	<pre><code data-trim data-noescape>
function words(str) {
  return String(str)
    .toLowerCase()
    .split(/\s|\b/)
    .filter(function alpha(v){
        return /^[\w]+$/.test(v)
    })
}

function unique(list) {
  var uniqueList = []
  for (let v of list) {
    if (uniqueList.indexOf(v) === -1) {
      uniqueList.push(v)
    }
  }
  return uniqueList
}

var wordsUsed = compose(unique, words)
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
