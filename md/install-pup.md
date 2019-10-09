### 安装 puppeteer

```
npm i puppeteer
// or
yarn add puppeteer
```

##### 如果想跳过安装 Chromium，我们可以选**puppeteer-core**，不会默认下载 Chromium

<!-- .element: class="fragment" data-fragment-index=1 -->

```
npm i puppeteer-core
// or
yarn add puppeteer-core
```

<!-- .element: class="fragment" data-fragment-index=2 -->

Note:
当安装 Puppeteer 时，它会下载最新版本的 Chromium（\~170MB Mac，\~282MB Linux，\~280MB Win），以保证可以使用 API。

-n-

<!-- .slide: data-background="#c2ccd0"-->

#### 开发前需要注意的一些事项

- Node v6.4.0+
- Puppeteer.js 可以使用 async/await，但是需要 Node v7.6.0 +支持
