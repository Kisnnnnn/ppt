## 钩子

### 当页面 window.onload 事件被触发时触发。

```js
page.on('load')

page.on('load', function() {
  // TODO:做些你想做的事情
})
```

### 当页面关闭时触发

```js
page.on('close')

page.on('close', msg => {
  // TODO:做些你想做的事情
})
```

### 当 iframe 加载时触发

```js
page.on('frameattached')
```

### 当 iframe 移除时触发

```js
page.on('framedetached')
```

-n-

## 示例 API

### 创建匿名的浏览器上下文

> 匿名的浏览器上下文将不会与其他浏览器上下文分享 cookies/cache。

```js
const browser = await puppeteer.launch()
// 创建一个匿名的浏览器上下文
const context = await browser.createIncognitoBrowserContext()
// 在一个原生的上下文中创建一个新页面
const page = await context.newPage()
// 做一些事情
await page.goto('https://www.baidu.com')
```

### 获取页面元素

```js
// 在页面内执行 document.querySelector
page.$('#main|.mian')

// 在页面内执行 document.querySelectorAll
page.$$('#main|.mian')
```

-n-

### 自动表单提交

```js
//模拟用户输入账号和密码
let loginId: string = 'kisn',
  password: string = 'dwa1'

await page.type('#mobile', loginId)
await page.type('#password', password, {
  // 延迟100毫秒
  delay: 100
})
await page.click('input[type=submit]') //点击登录按钮
```

-n-

#### 如何在页面执行方法？

```js
page.evaluate(pageFunction[, ...args]

参数
- pageFunction <function|string> 要在页面实例上下文中执行的方法
- ...args <...Serializable|JSHandle> 要传给 pageFunction 的参数
- return <Promise<Serializable>> pageFunction执行的结果
```

注意：

1.如果`pageFunction`返回的是`Promise`，`page.evaluate`将等待`promise`完成，并返回其返回值。

2.如果`pageFunction`返回的是不能序列化的值，将返回`undefined`

-n-
evaluate 本质上是一个 Promise，所以我们在`pageFunction`添加 return 才能有返回值，或者等待其 Promise 完成。

`pageFunction`在`page`实例浏览器的执行上下文中执行，所以在`pageFunction`中是没有调用 `Page` 外部环境的方法和变量的，

```js
function out() {}

var num = 0

page.evaluate(function() {
  out()
  // aaa is not undefined
  num++
  // num is not defined
})
```

Note:

那怎么调用外部方法呢？
-n-

1.我们可以通过作为参数传入执行上下文，进行处理。

2.利用`page.exposeFunction()`方法，将方法挂载到全局 window 对象上。

```js
const mobile = await page.$('#mobile')

function num() {
  var one = 1
  return one
}

// 挂载一个叫「extraFn」方法，返回值是一个<Promise>
await page.exposeFunction('extraFn', number => {
  return ++number
})

await page.evaluate(
  (a, mobileEL, num) => {
    // 调用全局方法，返回是一个Pormise，所以需要await
    const newNum = await window.extraFn(num)
    console.log(a)
    // 7
    console.log(mobileEL)
    // <input class="login_input " type="text" id="mobile" placeholder="请输入用户名" onkeyup="threeH()">
    console.log(num)
    // 1
    console.log(newNum)
    // 2
  },
  7,
  mobile,
  num()
)
```

-n-

##### 示例

```js
  await page
    .evaluate(() => {
        // 执行方法
      function getBase64Image(img: any) {
      return imgB64
    })
    .then(async imgB64 => {
      await fetch('http://xxxx', {
        method: 'POST',
        body: JSON.stringify({
          imgBase64: imgB64
        }),
        headers: {
          'Content-Type': 'application/json'
        }
      })
        .then(function(response) {
          return response.json()
        })
        .then(function(data) {
            // 将会等到promise完成，才能返回。
          console.log(data)
        })
        .catch(function(e) {
          console.log('Oops, error')
        })
    })
})()
```

-n-

##### 也可以传一个字符串

```js
console.log(await page.evaluate('1 + 2'))
// 3
```

-n-

##### 当然我们还可以执行选择器，将它作为第一个元素进行传值执行

```js
page.$eval(selector, pageFunction[, ...args])
- selector <string> 选择器
- pageFunction <function> 在浏览器实例上下文中要执行的方法
- ...args <...Serializable|JSHandle> 要传给 pageFunction 的参数。
（比如你的代码里生成了一个变量，在页面中执行方法时需要用到，可以通过这个 args 传进去）
- return: <Promise<Serializable>> Promise对象，完成后是 pageFunction 的返回值
```

##### 示例

```js
// 获取标签「srh-input」的值
const searchValue = await page.$eval('#srh-input', el => el.value)
// 获取选择器的值
const text = await page.$eval('#click', a => a.innerText)

// 移除标签的属性

//移除div中id的值
await page.$eval('#div_text', div => div.removeAttribute('id'))
//更改div中class的值
await page.$eval('#input_02', div => div.setAttribute('value', 'test'))
```

-l-

### 参考资料
[Puppeteer中文文档](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/)

[Puppeteer-Github](https://github.com/GoogleChrome/puppeteer)

[puppeteer-core-NPM](https://www.npmjs.com/package/puppeteer-core)

-l-

# 谢谢观看

演讲者：袁凯忻
