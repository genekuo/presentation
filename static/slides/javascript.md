# `JavaScript`

---

## `Functional Programming`
* Functional programming is the art of composing functions to advance the state of a program in a pure manner
* Higer-order functions, composition, pure functions
* Functions as data
  - declare a constant value
  - identity function
  - create new objects
  - declare and reference (later) variables in different scope: closure
  - Arbitrary business logic

---

## `FP Rules`
* always input and return values
* Immutability
* side-effect free
* The same input always produces the same output

* So, FP is the art of combining functions that play by these rules to advance the state of a program to its final outcome

---

## `Function composition`

---

## `Curried functions`
<section>
	<pre><code data-trim data-noescape>
const curry =
  fn =>
    (...args1) =>
      args1.length === fn.length
        ? fn(...args1)
        : (...args2) => {
          const args = [...args1, ...args2]
          return args.length >= fn.length ? fn(...args) : curry(fn)(...args)
        }
  </code></pre>
</section>

---

## `prop and props`
<section>
	<pre><code data-trim data-noescape>
const isFunction =
  a =>
    a &&
    typeof a === 'function' &&
    Object.prototype.toString.call(a) === '[object Function]'

const prop =
  curry(
    (name, obj) =>
      obj[name] && isFunction(obj[name]) ?
        obj[name].call(obj) : obj[name]
  )
const props =
  curry(
    (name, obj) =>
      names.map(n => prop(n, obj))
  )
  </code></pre>
</section>

---

## `The order of arguments`
<section>
	<pre><code data-trim data-noescape>
const tx = {
  sender: 'a@b.com',
  recipient: 'c@d.com',
  funds: 10.8
}

const fundsOf = prop('funds')
fundsOf(tx)
  </code></pre>
</section>

---

## `Curried and composition`
<section>
	<pre><code data-trim data-noescape>
const HasHash =
  () => ({
    calculateHash() {
      return
        compose(
          computeCipher,
          assemble
        )(this)
    }
  })
  const computeCipher =
    (data, i = 0, hash = 0) =>
      i > data.length
        ? hash ** 2
        : computeCipher(
            data,
            i + 1,
            ((hash << 5)) - hash + data.charCodeAt(i)
          )
  const assemble =
    ({sender, recipient, funds}) =>
      [sender, recipient, funds].join('')
  </code></pre>
</section>

---

## `Generalize assemble`
<section>
	<pre><code data-trim data-noescape>
const assemble =
  (...pieces) =>
    pieces.map(JSON.stringify).join('')
const keys = ['sender', 'recipient', 'funds']
const transaction = {
  sender: 'a@b.com',
  recipient: 'c@d.com',
  funds: 10.8
}

assemble(keys.map(k => transaction[k]))
  </code></pre>
</section>

---

## `Applying curry`
<section>
	<pre><code data-trim data-noescape>
const computeCipher =
  curry(
    (options, data) =>
      crypto
        .createHash(options.algorithm)
        .update(data)
        .digest(options.encoding)
  )
compose(
  computeCipher({
    algorithm: 'SHA256',
    encoding: 'hex'
    }),
  assemble)
  </code></pre>
</section>

---

## `HasHash`
<section>
	<pre><code data-trim data-noescape>
const HasHash = (
  keys,
  options = {
    algorithm: 'SHA256',
    encoding: 'hex'
  }
) => ({
    calculateHash() {
      return compose(
        computeCipher(options),
        assemble.
        props(keys)
        )(this)
    }
  })
  </code></pre>
</section>

---

## `HasTransaction`
<section>
	<pre><code data-trim data-noescape>
const hasTransaction = Object.assign(
    {
      sender: 'a@b.com',
      recipient: 'c@d.com',
      funds: 10.8
    },
    HasHash(['sneder', 'recipient', 'funds'])
  )

hasTransaction.calculateHash()
  </code></pre>
</section>

---

## `Pass a copy of data`
<section>
	<pre><code data-trim data-noescape>
calculateHash() {
  const objToHash = Object.fromEntries(
    new Map(keys.map(k => [k, prop(k, this)]))
    )
  return compose(
    computeCipher(options),
    assemble,
    props(keys)
    )(objToHash)
}

foo({...data})
foo([...data])
  </code></pre>
</section>

---

## `Immutable object`
* No global identity
* Closed to modification
* Closed to extension
* Define its own equality

---

## `Money curried`
<section>
	<pre><code data-trim data-noescape>
const USD = Money('$')
USD(3.0)
USD(3.0).amount
USD(7.0).currency
[USD(3.0), USD(7.0)].map(prop('amount')).reduce(add, 0)
USD(3.0).plus(USD(7.0)).equals(USD(10))
  </code></pre>
</section>

---

## `Money`
<section>
	<pre><code data-trim data-noescape>
const JS_LITE = 'jsl'
const US_DOLLAR = '$'
const Money =
  curry((currency, amount) =>
    compose(
      Object.seal,
      Object.freeze
    )({
        amount,
        currency,
        constructor: Money,
        equals: other =>
          Object.is(currency, other.currency) &&
          Object.is(amount, other.amount),
        round: (precision = 2) =>
          Money(currency, precisionRound(amount, precision))
        minus: m => Money(currency, amount - m.amount),
        plus: m => Money(currency, amount + m.amount),
        times: m => Money(currency, amount * m.amount),
        compareTo: other => amount - other.amount
        asNegative: () => Money(currency, amount * -1),
        toString: () => `${currency} ${amount}`
      })
  )
  </code></pre>
</section>

---

## `Static helpers`
<section>
	<pre><code data-trim data-noescape>
Money.zero =
  (currency = JS_LITE) =>
    Money(currency, 0)
Money.sum =
  (m1, mm2) =>
    ma.plus(m2)
Money.subtract =
  (m1, m2) =>
    m1.minus(m2)
Money.multiply =
  (m1, m2) =>
    m1.times(m2)
  </code></pre>
</section>

---

## `Point free`
* a style or technique in which function definitions do not explicitly identify the arguments (or the “points”) they receive

<section>
	<pre><code data-trim data-noescape>
const countWordsInFile = compose(
  count,
  split,
  decode('utf8'),
  read
)
const countWords = compose2(count, split)
const countWordsInFile = compose2(
  countWords,
  compose2(decode('utf8'), read)
)
const countBlocksInFile = compose(
  count,
  JSON.parse,
  decode('utf8'),
  read
)
  </code></pre>
</section>

---

## `Functional transformation`
* Make the data explicit function arguments instead of `this`
* Transform loops and nested conditionals into unidirectional data transformation: map and filter
* Improve addition using variable reassignment: reduce

---

## `Calculate balance`
<section>
	<pre><code data-trim data-noescape>
const balanceOf =
  curry((addr, tx)) =>
    Money.sum(
      tx.recipient === addr ? tx.funds : Money.zero(),
      tx.sender === addr ? tx.funds.asNegative() : Money.zero()
    )
  )
computeBalance(ledger, address) {
  return Array.from(ledger)
    .filter(not(prop('isGenesis')))
    .map(prop('data'))
    .flat()
    .map(balanceOf(address))
    .reduce(Money.sum, Money.zero())
    .round()
}
  </code></pre>
</section>

---

## `Extract instance methods into functions: Array.prototype.map(f)`
<section>
	<pre><code data-trim data-noescape>
const map = curry((f, arr) => arr.map(f))
map(balanceOf(address))

const computeBalance =
  address =>
    compose(
      Money.round,
      reduce(Money.sum, Money.zero()),
      map(balanceOf(address)),
      flatMap(prop('data')),
      filter(
        compose(
          not,
          prop('isGenesis')
        )
      ),
      Array.from
    )
  </code></pre>
</section>

---

## `Function binding`

---

## `Pipelining`

---

## `Higher-kinded composition`
* wrapping the context over which a function executes. This context centrally handles things like validation or error handling, while keeping our code fluent, compact, and declarative.
* An Algebraic Data Type (ADT) is a composite type that encapsulates a single concern like validation, error handling, null checking, sequence of elements, and others.
* ADT allows building complete programs from simple, individual patterns
* ADT universal API: Functor (map) and Monad (flatMap)
* Repetitive validation logic, context-aware, no side effects

---

## `Validate-aware functions`
<section>
	<pre><code data-trim data-noescape>
compose(validate, f3, validate, f2, validate, f1, validate)

const validate =
  fn =>
    data =>
      data ? fn(data) : throw new Error(`Received invalid data ${data}`)

compose(...[f3,f2,f1].map(validate))
  </code></pre>
</section>

---

## `Closed context (container)`
* An object that encapsulates data and abstracts the application of an (side)effect to this data as part of exercising business logic.
* minimal APIs
  - A static function (type lifting function) to construct new containers with a value: c.of, c.unit
  - A function to transform this data: C.prototype.map
  - A function to extract the end result from the container (unfolding or reducing)
* Id and []

---

## `Container: Array`
<section>
	<pre><code data-trim data-noescape>
const unique =
  letters =>
    Array.from(new Set([...letters]))
const join =
  arr =>
    arr.join('')
const toUpper =
  str =>
    str.toUpperCase()

const letters = ['aabbcc'] // Or Array.of('aabbcc'), put the value insdie an array
  .map(unique)
  .map(join)
  .map(toUpper)
  .pop()  // extract a value from an array
  </code></pre>
</section>

---

## `Identity context`
* an identity context wraps a single value and doesn’t do any additional processing over what you provide in your mapping functions—it has no effect on the data.

<section>
	<pre><code data-trim data-noescape>
const identity =
  a =>
    a
  </code></pre>
</section>

---

## `Id`
<section>
	<pre><code data-trim data-noescape>
class Id extends Array {
  constructor(value) {
    super(1)
    this.fill(value)
  }
  static get [Symbol.species]() {
    return this
  }
}

Id.of('aabbcc')
  .map(unique)
  .map(join)
  .map(toUpper)
  .pop()
  </code></pre>
</section>

---

## `Contextual composition: map, flatMap`
* Composing functions together over the underlying array context.

<section>
	<pre><code data-trim data-noescape>
const uniqueUpperCaseOf =
  compose(
    toUpper,
    join,
    unique
  )
uniqueUpperCaseOf('aabbcc')
  </code></pre>
</section>

---

## `Data types`
* Seven data types: boolean, number, string, undefined, null, Symbol, Object
* Boxing
* Coercion

---

## `flat, flatMap`
* flatMap allows you to apply functions from a value to another structure
* Contextual composition

<section>
	<pre><code data-trim data-noescape>
['aa',['bb'],['cc']].flat().map(toUpper)
[[[[['down here!']]]]].flat(Infinity)

const unique =
  letters =>
    Array.from(new Set([...letters]))
['aa', 'bb', 'cc'].map(unique).flat()
['aa', 'bb', 'cc'].flatMap(unique)
  </code></pre>
</section>

---

## `map vs. compose`
<section>
	<pre><code data-trim data-noescape>
Function.prototype.map = function(f) {
  return compose(
    f,
    this
    )
}

compose(toUpper, join, unique)('aabbcc')
unique.map(join).map(toUpper)('aabbcc')
  </code></pre>
</section>

---

## `Higher-kinded composition`
* flatMap allows composite types to compose functions returning other composites, a higher-kinded (nested) form of composition

---

## `Higher-kinded composition`
<section>
	<pre><code data-trim data-noescape>
const flat =
  arr =>
    arr.flat()
Function.prototype.flatMap = function(F) {
  return compose(
    flat,
    this.map(F)  
  )
}

toUpper.flatMap(unique)('aa')
  </code></pre>
</section>

---

## `Structure preserving`
* Symbol.species: species preserving
* map and flatMap

---

## `Functor`
* A functor is something (e.g. an object) that can be mapped over or that implements the map interface properly
* Identity: mapping the identity function over a container yields an equivalent container
* Composition: mapping the composition of two functions f after g is equivalent to mapping g first and then f

---

## `Example`
<section>
	<pre><code data-trim data-noescape>
['aa','bb','cc'].map(identity)
['aa','bb','cc'].map(
  compose(
    toUpper,
    join,
    unique
    )
)
['aa','bb','cc']
  .map(unique)
  .map(join)
  .map(toUpper)
  </code></pre>
</section>

---

## `Id`
<section>
	<pre><code data-trim data-noescape>
class Id {
  #val
  constructor(value) {
    this.#val = value
  }
  static of(value) {
    return new Id(value)
  }
  get() {
    return this.#val
  }
  equals(other) {
    return this.#val === other.value
  }
}
  </code></pre>
</section>

---

## `Functor mixin`
* functors let you map a simple function to transform the wrapped value and put it back into a new container of the same type

---

## `Functor mixin`
<section>
	<pre><code data-trim data-noescape>
const Functor =
  () =>
    ({
      map(f = identity) {
        return this.constructor.of(f(this.get()))
      }
    })
Object.assign(Id.prototype, Functor())

Id.of('aabbcc')
  .map(unique)
  .map(join)
  .map(toUpper)
  .get()
  </code></pre>
</section>

---

## `Monads`
* An object becomes a monad by implementing the functor specification, and in addition the flatMap interface
* can compose functions that work with the same or other containers or contexts

---

## `Left identity`
* M.of(a).flatMap(f) and f(a), equivalent

<section>
	<pre><code data-trim data-noescape>
const f = x => [x**2]
Array.of(2).flatMap(f)
f(2)
  </code></pre>
</section>

---

## `Right identity`
* m.flatMap(M.of)) and m, equivalent

<section>
	<pre><code data-trim data-noescape>
Array.of(2).flatMap(x => Array.of(x))
Array.of(2)
  </code></pre>
</section>

---

## `Associativity`
* m.flatMap(f).flatMap(g) ==== m.flatMap(a => f(a).flatMap(g))

<section>
	<pre><code data-trim data-noescape>
const f = x => [x ** 2]
const g = x => [x * 3]
Array.of(2).flatMap(f).flatMap(g)
Array.of(2).flatMap(a => f(a).flatMap(g))
  </code></pre>
</section>

---

## `Monad mixin`
<section>
	<pre><code data-trim data-noescape>
const Monad =
  () => ({
    flatMap(f) {
      return this.map(f).get()
    },
    chain(f) {
      return this.flatMap(f)
    },
    bind(f) {
      return this.flatMap(f)
    }
  })
  </code></pre>
</section>
