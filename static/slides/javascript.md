# `JavaScript`

---

## `Higher-kinded composition`

---

## `Validation`
* Repetitive validation logic
* Context lost
* Side effects

---

## `Algebraic Data Structure`
* build complete programs from simple, individual patterns
* Functor and Monad

---

## `Context`
<section>
	<pre><code data-trim data-noescape>
const validate =
  fn =>
    data =>
      data ? fn(data) : throw new Error(`Received invalid data ${data}`)

compose(...[f3,f2,f1].map(validate))
  </code></pre>
</section>

---

## `Closed context`
* An object that encapsulates data and abstracts the application of an effect to this data as part of exercising business logic.
* API
** A static function to construct new containers with a value
** A function to transform this data
** A function to extract the end result from the container
* Id and []

---

## `Array`
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

const letters = ['aabbcc'] // Or Array.of('aabbcc')
  .map(unique)
  .map(join)
  .map(toUpper)
  .pop()
  </code></pre>
</section>

---

## `Identity context`
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

## `Contextual composition`
* Composing functions together over the underlying array context.

---

## `Data types`
* boolean, number, string, undefined, null, Symbol, Object
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
