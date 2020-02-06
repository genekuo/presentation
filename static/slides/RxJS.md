# `RxJS`

---

## `Operators`
* Operators are pure functions that create a new observable based on the current one

---

## `Explicit concellation`
<section>
	<pre><code data-trim data-noescape>
  var progressBar$=Rx.Observable.create(
      observer=>{
          const OFFSET=3000
          const SPEED=50
          let val=0
          let timeoutId=0
          function progress() {
              if(++val<=100){
                  observer.next(val)
                  timeoutId=setTimeout(progress, SPEED)
              }else{
                  observer.complete()
              }
          }
          timeoutId=setTimeout(progress,OFFSET)
          return ()=>{
              clearTimeout(timeoutId)
          }
      })
  var subscription = progressBar$.subscribe(console.log)
  subscription.unsubscribe()
  </code></pre>
</section>

---

## `Promise not to be concelled`

---

## `Operators`
* An operator is a pure, higher-order function
