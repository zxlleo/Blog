# JS 相关

- <a href="#debounce">防抖</a>
- <a href="#throttle">节流</a>

## 防抖

<a name="debounce"></a>

<center>

![函数防抖](imgs/函数防抖.jpg '函数防抖')

</center>

```js
function debounce(fn, wait) {
  var timer = null
  return function() {
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

<a name="throttle"></a>

<center>

![函数节流](imgs/函数节流.jpg '函数节流')

</center>

- 时间撮节流

```js
var throttle = function(func, delay) {
  var prev = Date.now()
  return function() {
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
var throttle = function(func, delay) {
  var timer = null
  return function() {
    var context = this
    var args = arguments
    if (!timer) {
      timer = setTimeout(function() {
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

## 洗牌算法

```js
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let index = Math.floor(Math.random() * (i + 1))
    let tem = arr[index]
    arr[index] = arr[i]
    arr[i] = tem
  }
  return arr
}
```