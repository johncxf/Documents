## Mocha

- 中文文档：https://mochajs.cn/

### 断言

#### chai

一般在测试用例文件的顶部，声明引用的断言库

```
const expect = require('chai').expect;
```

上面这行代码引入的断言库是chai，并且制定使用它的expect断言风格。

基本上，expect断言的写法都是一样的。头部是expect方法，尾部是断言方法，比如equal、a/an、ok、match等。两者之间使用to或to.be连接。

