# this 指向

<center>

![this指向](https://github.com/zxlleo/Blog/blob/master/imgs/this.jpg 'this指向')

</center>

# 防抖 和 节流

## 防抖

> 函数防抖（debounce）：当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。如下图，持续触发 scroll 事件时，并不执行 handle 函数，当 1000 毫秒内没有触发 scroll 事件时，才会延时触发 scroll 事件。

<center>

![函数防抖](https://github.com/zxlleo/Blog/blob/master/imgs/函数防抖.jpg '函数防抖')

</center>

```js
function debounce(fn, wait) {
  var timer = null
  return function () {
    if (!timer) clearTimeout(timer)
    timer = setTimeout(fn, wait)
  }
}

// 处理函数
function handle() {
  console.log(Math.random())
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000))
```

## 节流(#throttle)

> 函数节流（throttle）：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。节流通俗解释就比如我们水龙头放水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴。如下图，持续触发 scroll 事件时，并不立即执行 handle 函数，每隔 1000 毫秒才会执行一次 handle 函数。

<center>

![函数节流](https://github.com/zxlleo/Blog/blob/master/imgs/函数节流.jpg '函数节流')

</center>

- 时间撮节流

```js
var throttle = function (func, delay) {
  var prev = Date.now()
  return function () {
    var context = this
    var args = arguments
    var now = Date.now()
    if (now - prev >= delay) {
      func.apply(context, args)
      prev = Date.now()
    }
  }
}

function handle() {
  console.log(Math.random())
}
window.addEventListener('scroll', throttle(handle, 1000))
```

- 定时器节流

```js
var throttle = function (func, delay) {
  var timer = null
  return function () {
    var context = this
    var args = arguments
    if (!timer) {
      timer = setTimeout(function () {
        func.apply(context, args)
        timer = null
      }, delay)
    }
  }
}

function handle() {
  console.log(Math.random())
}
window.addEventListener('scroll', throttle(handle, 1000))
```

# 数组方法

|  方法名   | 对应版本 |                 功能                 | 原数组是否改变 |
| :-------: | :------: | :----------------------------------: | :------------: |
| concat()  |   ES5-   |    合并数组，并返回合并之后的数据    |       n        |
|  join()   |   ES5-   |  使用分隔符，将数组转为字符串并返回  |       n        |
|   pop()   |   ES5-   |    删除最后一位，并返回删除的数据    |       y        |
|  shift()  |   ES5-   |     删除第一位，并返回删除的数据     |       y        |
|  push()   |   ES5-   | 在最后一位新增一或多个数据，返回长度 |       y        |
| reverse() |   ES5-   |          反转数组，返回结果          |       y        |
|  slice()  |   ES5-   |      截取指定位置的数组，并返回      |       n        |
| sort()  |   ES5-   |    排序（字符规则），返回结果    |       y        |
| splice()  |   ES5-   |    删除指定位置，并替换，返回删除的数据	    |       y        |
| toString()  |   ES5-   |    直接转为字符串，并返回	    |       n        |
| valueOf()  |   ES5-   |    返回数组对象的原始值	    |       n        |
| indexOf()  |   ES5-   |    查询并返回数据的索引	    |       n        |
| lastIndexOf()  |   ES5-   |    反向查询并返回数据的索引	    |       n        |
| forEach()  |   ES5-   |    参数为回调函数，会遍历数组所有</br>的项，回调函数接受三个参数，分别为value，</br>index，self；forEach没有返回值    |       n        |
| map()  |   ES5-   |    同forEach，同时回调函数返回数据，</br>组成新数组由map返回	    |       n        |
| filter()  |   ES5-   |    同forEach，同时回调函数返回布尔</br>值，为true的数据组成新数组由filter返回	    |       n        |
| every()  |   ES5-   |    同forEach，同时回调函数返回布尔</br>值，全部为true，由every返回true	    |       n        |
| some()  |   ES5-   |    同forEach，同时回调函数返回布尔</br>值，只要由一个为true，由some返回true	    |       n        |
| reduce()  |   ES5-   |    归并，同forEach，迭代数组的所有</br>项，并构建一个最终值，由reduce返回	    |       n        |

# 对象拷贝
## 浅拷贝


## 深拷贝
```js
// vue router使用
function clone (value) {
  if (Array.isArray(value)) {
    return value.map(clone) // 递归数组
  } else if (value && typeof value === 'object') {
    const res = {}
    for (const key in value) {
      res[key] = clone(value[key])
    }
    return res
  } else {
    return value
  }
}
```

# Vue 换场动画实现

> 用数组实现一个路由栈，管理路由的进出
```js
// ./router/Histroy.js

import storage from 'good-storage'
export default class History {
  constructor() {
    // 设置缓存，解决从其他路由回到单页面应用后，路由栈清空了
    let cache = storage.session.get('routeCache')
    this.stack = cache || []
  }

  push(path) {
    this.stack.push(path)
    this.setCache()
  }

  find(path) {
    return this.stack.indexOf(path)
  }

  cutOff(index) {
    this.stack.splice(index)
    this.setCache()
  }

  setCache() {
    storage.session.set('routeCache', this.stack)
  }
}
```

> 通过全局后置钩子，如果路由栈里找到了路由，说明是返回，动画右滑；没有找到说明是新加，动画左滑
```js
// ./router/index.js

import Vue from 'vue'
import Router from 'vue-router'
import History from './History'
import routes from './routes'

Vue.use(Router)

let router = new Router({
  routes
})

let history = new History()
router.stack = history.stack
router.direction = '' // 动画方向

router.afterEach((to, from) => {
  if (!to.path) return

  // 首次进入，无动画
  if (from.path === '/') {
    history.push(to.path)
    router.direction = ''
    return
  }

  const index = history.find(to.path)
  if (index !== -1) {
    router.direction = 'slide-right'
    history.cutOff(index + 1)
  } else {
    router.direction = 'slide-left'
    history.push(to.path)
  }
})

export default router
```

> 入口的 transition 使用后添加的`router`属性 `direction`
```js
// ./App.vue
<template>
  <div id="app">
    <transition :name="$router.direction">
      <router-view class="view"></router-view>
    </transition>
  </div>
</template>
<style lang="less" scope>
#app {
  width: 100%;
  height: 100%;
  background-color: #f9f9f9;
}

.view {
  position: absolute;
  top: 0;
  width: 100%;
  min-height: 100%;
  transition: all 0.3s cubic-bezier(0.51, 0.18, 0.31, 0.99);
}

.slide-left-enter,
.slide-right-leave-active {
  -webkit-transform: translate(100%, 0);
  transform: translate(100%, 0);
}
.slide-left-leave-active,
.slide-right-enter {
  -webkit-transform: translate(-100%, 0);
  transform: translate(-100%, 0);
}
</style>
```