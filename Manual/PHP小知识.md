## PHP小知识

#### PHP中 $$ 是什么意思？

首先，我们先给出一段代码：

```
<?php
$a = 'abc';
$$a = '789';
echo $abc;
```

运行结果：

```
789
```

$$a可以理解为先解析后边这个$a，然后在进行解析，最终解析成为$abc

所以直接打印$abc就是一个变量，打印出来就是789

#### PHP页面间传值方法

**require_once**

```
`//Page a:``    ``<?php``            ``$a` `= ``"hello"``;``    ``?>``//Page b:``    ``<?php``        ``require_once` `"A.php"``;``        ``echo` `$a``.``" world!"``;``    ``?>`
```

访问b.php会得到：hello world！

**通过页面跳转时携带参数传值**

```
`//Page a:``<?php``    ``$a` `= ``"world"``;``?>``    ``<a href=``"b.php?m=$a"``>点我跳到b.php</a>``//Page b:``<?php``    ``echo` `"hello"``.``$_GET``[``'m'``];``?>`
```

**表单提交**

```
`<form name=``"form1"` `method=``"post"` `action=``"2.php"``>``  ``<input type=``"text"` `name=``"val"` `/>``  ``<input type=``"submit"` `name=``"Submit"` `value=``"提交"` `/>``</form>``//2.php：``<?php``    ``echo` `$_POST``[``'val'``];``?>`
```

**SESSION会话**

（SESSION是全局变量，只要被声明，在不关闭网页或者没有到SESSION的周期在所有页面都是可用的，而POST和GET只要php执行完毕就会立刻被释放没有）

```
`<?php``    ``session_start();``    ``$_SESSION``[``'val'``]=``'123'``;``    ``echo` `$_SESSION``[``'val'``];``?>``<?php``    ``session_start();``    ``echo` `$_SESSION``[``'val'``];    ``//直接输出全局变量val.``?>`
```

**COOKIE**

cookie是存放在客户端上（也是全局变量），session是存放在服务器上。这是两者唯一的不同。

```
`<?php   ``     ``setcookie(``"user"``, ``"SUVLLIAN"``, time()+3600);    ``//创建一个名为user的cookie变量，它的值是Alex Porter。它将在一个小时以后过期，也就是不能访问了``     ``echo` `$_COOKIE``[``'user'``];    ``//还要刷新一下页面才可以生效``?>`
```

**存入数据库**

优点：能够长期存储。

缺点：每次需要使用时，都需要在数据库中查询，耗费资源非常大。

#### PHP数组对象之间的转换

```php
/**
 * 数组 转 对象
 * @param array $arr 数组
 * @return object
 */
function array_to_object($arr) {
    if (gettype($arr) != 'array') {
        return;
    }
    foreach ($arr as $k => $v) {
        if (gettype($v) == 'array' || getType($v) == 'object') {
            $arr[$k] = (object)array_to_object($v);
        }
    }
    return (object)$arr;
}
/**
 * 对象 转 数组
 * @param object $obj 对象
 * @return array
 */
function object_to_array($obj) {
    $obj = (array)$obj;
    foreach ($obj as $k => $v) {
        if (gettype($v) == 'resource') {
            return;
        }
        if (gettype($v) == 'object' || gettype($v) == 'array') {
            $obj[$k] = (array)object_to_array($v);
        }
    }
    return $obj;

}
```

