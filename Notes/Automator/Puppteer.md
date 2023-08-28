# Puppteer

Puppeteer 是一个 Node 库，它提供了一个高级 API 来通过 [DevTools](https://chromedevtools.github.io/devtools-protocol/) 协议控制 Chromium 或 Chrome

- github：https://github.com/puppeteer/puppeteer
- 官方文档：https://pptr.dev/
- 中文文档：https://puppeteer.bootcss.com/#faq

## 安装使用

puppteer 提供了`puppteer`和`puppteer-core`两个库可供使用

- `puppeteer` 是用于浏览器自动化的*产品*。安装后，它将下载一个版本的 `Chromium`, 然后使用 `puppeteer-core` 驱动

- `puppeteer-core` 是一个的轻量级的 Puppeteer 版本，在安装后不会自动下载 Chromium

选择对应库进行安装：

```shell
$ npm i puppeteer
$ npm i puppeteer-core
```

使用示例：

```javascript
// const puppeteer = require('puppeteer');
const puppeteer = require('puppeteer-core');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```

## API

