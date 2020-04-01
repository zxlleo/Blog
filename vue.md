# 简易Vue实现

## Observer

```js
function Observer(obj) {
  if (obj && typeof obj !== 'object') {
    return
  }

  let keys = Object.keys(obj)
  keys.forEach(key => {
    defineReactive(obj, key, obj[key])
  })
}

function defineReactive(obj, key, val) {
  Observer(val)

  let dep = new Dep()
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get() {
      dep.depend()
      return val
    },
    set(newVal) {
      val = newVal
      dep.notify()
    }
  })
}

//订阅器
class Dep {
  constructor (){
    this.subs = []
  }

  addSub(watcher){
    this.subs.push(watcher)
  }

  depend(){
    if (Dep.target) {
      this.addSub(Dep.target)
    }
  }

  notify() {
    this.subs.forEach(sub => {
      sub.update()
    })
  }
}

//订阅者
class Watcher {
  constructor (vm, expr, cb){
    this.vm = vm
    this.expr = expr
    this.value = this.get()
  }

  update(){
    let newVal = this.vm.data[this.expr]
    let oldVal = this.value
    if (newVal !== oldVal) {
      this.value = newVal
      this.cb.call(this.vm, newVal)
    }
  }

  get(){
    Dep.target = this
    let value = this.vm.data[this.expr]
    Dep.target = null
    return value
  }
}
```
