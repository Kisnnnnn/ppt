一个简单的 Puppeteer 应用

```ts
// index.ts
// 引入puppeteer
import * as puppeteer from 'puppeteer'
;(async () => {
  // 加载一个浏览器示例
  const browser = await puppeteer.launch({
      // 请输入你的Chrome的目录地址
      executablePath: './application/Chromium.app/Contents/MacOS/Chromium',
      //   如果需要打开浏览器，请讲无头模式关闭
      headless: false
    }),
    page = await browser.newPage()

  // 设置窗口大小
  await page.setViewport({
    width: 1366,
    height: 768
  })
  //  转到需要进入的网址
  await page.goto('http://58.211.61.83:8100/ciap/login.jsp', {})
  //延迟1秒输入
  await page.waitFor(1000)

  //模拟用户输入账号和密码
  let loginId: string = 'kisn',
    password: string = 'dwa1'

  await page.type('#mobile', loginId)
  await page.type('#password', password, {
    // 延迟100毫秒
    delay: 100
  })
})()
```

-n-

#### 在目标页面上下文也可以执行你的代码

```js
let num = 5
const result = await page.evaluate(x => {
  return Promise.resolve(8 * x)
}, num) //
console.log(result) // 输出 "40"
```
