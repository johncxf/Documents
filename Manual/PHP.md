## PHP知识点

#### 1.编码规范（PSR规范）

#### 2.版本特性

##### 2.1 PHP7.0

**标量类型声明**

> 标量类型声明 有两种模式: 强制 (默认) 和 严格模式。 现在可以使用下列类型参数（无论用强制模式还是严格模式）： 字符串(string), 整数 (int), 浮点数 (float), 以及布尔值 (bool)。它们扩充了PHP5中引入的其他类型：类名，接口，数组和 回调类型。

```
 <?php
 // Coercive mode
 function sumOfInts(int ...$ints)
 {
     return array_sum($ints);
 }

 var_dump(sumOfInts(2, '3', 4.1));    //9
```

**返回值类型声明**

> PHP 7 增加了对返回类型声明的支持。 类似于参数类型声明，返回类型声明指明了函数返回值的类型。可用的类型与参数声明中可用的类型相同。

```
  <?php

 function arraysSum(array ...$arrays): array
 {
     return array_map(function(array $array): int {
         return array_sum($array);
     }, $arrays);
 }

 print_r(arraysSum([1,2,3], [4,5,6], [7,8,9]));
```

以上例程会输出：

```
 Array
 (
     [0] => 6
     [1] => 15
     [2] => 24
 )
```

**null合并运算符**

> 由于日常使用中存在大量同时使用三元表达式和 isset()的情况， 我们添加了null合并运算符 (??) 这个语法糖。如果变量存在且值不为NULL， 它就会返回自身的值，否则返回它的第二个操作数。

```
 <?php
 // Fetches the value of $_GET['user'] and returns 'nobody'
 // if it does not exist.
 $username = $_GET['user'] ?? 'nobody';
 // This is equivalent to:
 $username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

 // Coalesces can be chained: this will return the first
 // defined value out of $_GET['user'], $_POST['user'], and
 // 'nobody'.
 $username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';
 ?>
```

**太空船操作符（组合比较符）**

> 太空船操作符用于比较两个表达式。当$a小于、等于或大于$b时它分别返回-1、0或1。 比较的原则是沿用 PHP 的常规比较规则进行的。

```
 <?php
 // Integers
 echo 1 <=> 1; // 0
 echo 1 <=> 2; // -1
 echo 2 <=> 1; // 1

 // Floats
 echo 1.5 <=> 1.5; // 0
 echo 1.5 <=> 2.5; // -1
 echo 2.5 <=> 1.5; // 1

 // Strings
 echo "a" <=> "a"; // 0
 echo "a" <=> "b"; // -1
 echo "b" <=> "a"; // 1
 ?>
```

**通过 define() 定义常量数组**

> Array 类型的常量现在可以通过define() 来定义。在 PHP5.6 中仅能通过 const 定义。

```
 <?php
 define('ANIMALS', [
     'dog',
     'cat',
     'bird'
 ]);

 echo ANIMALS[1]; // outputs "cat"
 ?>
```

**匿名类**

> 现在支持通过new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义。

```
 <?php
 interface Logger {
     public function log(string $msg);
 }

 class Application {
     private $logger;

     public function getLogger(): Logger {
          return $this->logger;
     }

     public function setLogger(Logger $logger) {
          $this->logger = $logger;
     }
 }

 $app = new Application;
 $app->setLogger(new class implements Logger {
     public function log(string $msg) {
         echo $msg;
     }
 });

 var_dump($app->getLogger());
 ?>
```

**Unicode codepoint 转译语法**

> 这接受一个以16进制形式的 Unicode codepoint，并打印出一个双引号或heredoc包围的 UTF-8 编码格式的字符串。 可以接受任何有效的 codepoint，并且开头的 0 是可以省略的。

```
 echo "\u{aa}";       //a
 echo "\u{0000aa}";   //a
 echo "\u{9999}";     //香
```

**Group use declarations**

> 从同一 namespace 导入的类、函数和常量现在可以通过单个 use 语句 一次性导入了。

```
 <?php

 // Pre PHP 7 code
 use some\namespace\ClassA;
 use some\namespace\ClassB;
 use some\namespace\ClassC as C;

 use function some\namespace\fn_a;
 use function some\namespace\fn_b;
 use function some\namespace\fn_c;

 use const some\namespace\ConstA;
 use const some\namespace\ConstB;
 use const some\namespace\ConstC;

 // PHP 7+ code
 use some\namespace\{ClassA, ClassB, ClassC as C};
 use function some\namespace\{fn_a, fn_b, fn_c};
 use const some\namespace\{ConstA, ConstB, ConstC};
 ?>
```

**整数除法函数**

新加的函数 [intdiv()](https://www.php.net/manual/zh/function.intdiv.php) 用来进行 整数的除法运算。

```
<?phpvar_dump(intdiv(10, 3));?>
```

以上例程会输出：

```
int(3)
```



更多：https://www.php.net/manual/zh/migration70.new-features.php

#### 3.面向对象

##### 3.1 OOA,OOD,OOP

###### OOA

> Object-Oriented Analysis：面向对象分析方法

###### OOA主要原则

- 抽象：过程抽象和数据抽象
- 封装：把对象的属性和服务结合为一个不可分的系统单位，并尽可能隐蔽对象的内部细节
- 继承：类的继承
- 分类：就是把具有相同属性和服务的对象划分为一类，用类作为这些对象的抽象描述
- 聚合：又称组装，原则：把一个复杂的事物看成若干比较简单的事物的组装体，从而简化对复杂事物的描述
- 关联：通过一个事物联想到另外的事物
- 消息通信：这一原则要求对象之间只能通过消息进行通信，而不允许在对象之外直接地存取对象内部的属性
- 粒度控制：考虑全局时，注意其大的组成部分，暂时不详察每一部分的具体的细节；考虑某部分的细节时则暂时撇开其余的部分
- 行为分析：由大量的事物所构成的问题域中各种行为往往相互依赖、相互交织

###### OOA三种模型

- 对象模型
- 动态模型
- 功能模型(即用例模型à作为输入)

###### OOD

> 面向对象设计（Object-Oriented Design，OOD）方法是OO方法中一个中间过渡环节。其主要作用是对OOA分析的结果作进一步的规范化整理，以便能够被OOP直接接受。

> - 决定你要的类；
>
> - 给每个类提供完整的一组操作；
>
> - 明确地使用继承来表现共同点。

###### OOP

> 面向对象编程（Object Oriented Programming，OOP，面向对象程序设计）是一种计算机编程架构。OOP 的一条基本原则是计算机程序是由单个能够起到子程序作用的单元或对象组合而成。OOP 达到了软件工程的三个主要目标：重用性、灵活性和扩展性。为了实现整体运算，每个对象都能够接收信息、处理数据和向其它对象发送信息。

##### 3.2 抽象类和接口

###### 抽象类/Abstract Class

定义Animal抽象类

```php
<?php
abstract class Animal {
  public $name;
  abstract public function eat($food);
}
?>
```

继承抽象类

```php
<?php
class Whale extends Animal {
  public function __construct() {
    $this->name = "Whale";
  }
  public function eat($food) {
    echo $this->name . " eat " . $food . ".\n";
  }
}
?>
```

调用

```php
<?php
  $whale = new Whale();
  $whale->eat("fish");
?>
```

###### 接口/Interface

定义接口

```php
<?php
interface IAction {
  public function eat($food);
  public function swim();
}
?>
```

继承

```php
<?php
class Whale implements IAction {
  public function eat($food) {
    echo "Whale eat " . $food . "\n.";
  }
  public swim() {
    echo "Whale is swimming.\n";
  }
}
?>
```

调用

```php
<?php
  $whale = new Whale();
  $whale->eat("fish");
?>
```

###### 抽象类vs接口

对于PHP编程来说，抽象类可以实现的功能，接口也可以实现。 抽象类的接口的区别，不在于编程实现，而在于程序设计模式的不同。 一般来讲，抽象用于不同的事物，而接口用于事物的行为。 如：水生生物是鲸鱼的抽象概念，但是水生生物并不是鲸鱼的行为，吃东西才是鲸鱼的行为。 对于大型项目来说，对象都是由基本的抽象类继承实现，而这些类的方法通常都由接口来定义。 此外，对于事物属性的更改，建议使用接口，而不是直接赋值或者别的方式

###### 二者异同

- 对接口的使用方式是通过关键字implements来实现的，而对于抽象类的操作是使用类继承的关键字exotends实现的，使用时要特别注意。
- 接口没有数据成员，但是抽象类有数据成员，抽象类可以实现数据的封装。 接口可以定义常量
- 接口没有构造函数，抽象类可以有构造函数。
- 接口中的方法都是public类型，而抽象类中的方法可以使用private、protected或public来修饰。
- 一个类可以同时实现多个接口，但是只能实现一个抽象类。
- 相同点：函数体内不能写任何东西，连两个大括号都不能写！！！如：function getName();这样就行了

##### 3.3 封装

###### 封装方法

| private                    | protected            | public                     |
| -------------------------- | -------------------- | -------------------------- |
| 私有                       | 保护                 | 公共                       |
| 仅对象内部可见，外部不可见 | 仅自身类和继承类可见 | 任何类、任何事物都可以访问 |

##### 3.4 继承

> 类和类之间应该是低耦合的。 继承通常是继承自抽象类，而不是具体类。 通常直接继承抽象类的具体类只有一层，在抽象类中用protected来限定

##### 3.5 多态

#### 4 设计模式

##### 4.1 单例模式

> 通过提供对自身共享实例的访问， 单例设计模式用于限制特定对象只能被创建一次。单例设计模式最常用于构建数据库连接对象

```php
<?php
class Singleton
{
    // 保存类实例在此属性中
    private static $instance;
    // 构造方法声明为private，防止直接创建对象
    private function __construct() 
    {
        echo 'I am constructed';
    }
    // singleton 方法
    public static function singleton() 
    {
        if (!isset(self::$instance)) {
            $c = __CLASS__;
            self::$instance = new $c;
        }
        return self::$instance;
    }
    // Example类中的普通方法
    public function bark()
    {
        echo 'Woof!';
    }
    // 阻止用户复制对象实例
    public function __clone()
    {
        trigger_error('Clone is not allowed.', E_USER_ERROR);
    }
}

?>
```

这样我们可以得到一个独一无二的Singleton类的对象。

```php
<?php
// 这个写法会出错，因为构造方法被声明为private
$test = new Singleton;

// 下面将得到Example类的单例对象
$test = Singleton::singleton();
$test->bark();

// 复制对象将导致一个E_USER_ERROR.
$test_clone = clone $test;

?>
```

##### 4.2 工厂模式

> 工厂模式（Factory）允许你在代码执行时实例化对象。它之所以被称为工厂模式是因为它负责“生产”对象。工厂方法的参数是 你要生成的对象对应的类名称。

```php
<?php
class Example
{
    // The parameterized factory method
    public static function factory($type)
    {
        if (include_once 'Drivers/' . $type . '.php') {
            $classname = 'Driver_' . $type;
            return new $classname;
        } else {
            throw new Exception ('Driver not found');
        }
    }
}
?>
```

按上面的方式可以动态加载drivers。如果Example类是一个数据库抽象类，那么 可以这样来生成MySQL和 SQLite驱动对象：

```php
<?php
// Load a MySQL Driver
$mysql = Example::factory('MySQL');
// Load a SQLite Driver
$sqlite = Example::factory('SQLite');
?>
```

##### 4.3 抽象工厂模式

抽象工厂模式:用来生成一组相关或相互依赖的对象。

> 抽象工厂模式与工厂方法模式的区别： 抽象工厂模式是工厂方法模式的升级版本，他用来创建一组相关或者相互依赖的对象。 他与工厂方法模式的区别就在于，工厂方法模式针对的是一个产品等级结构； 而抽象工厂模式则是针对的多个产品等级结构。 在编程中，通常一个产品结构，表现为一个接口或者抽象类， 也就是说，工厂方法模式提供的所有产品都是衍生自同一个接口或抽象类， 而抽象工厂模式所提供的产品则是衍生自不同的接口或抽象类。在抽象工厂模式中， 有一个产品族的概念：所谓的产品族，是指位于不同产品等级结构中功能相关联的产品组成的家族。 抽象工厂模式所提供的一系列产品就组成一个产品族；而工厂方法提供的一系列产品称为一个等级结构。 我们依然拿生产汽车的例子来说明他们之间的区别。

```php
<?php
/**
 *汽车抽象类
 */
abstract class vehicle{
    /**
     *属性
     *@var array
     */
    public $props = array();
    /**
     *设置属性
     *@param $key 属性名
     *@param $value 属性值
     */
    public function setProperty( $key, $value ){
       $this->props[$key] = $value; 
    }
    /**
     *获取属性值
     *@param $key
     *
     */
    public function getProperty( $key ){
       return $this->props[$key]; 
    }
}
/**
 *公交车抽象类
 */
abstract class bus extends vehicle {
   /**
    *形体大小：两个门，三个门
    *@var string
    *
    */ 
    public $shape;
    /**
     *设置形体
     *@param $value
     */
    public function setShape( $value ){
        $this->shape = $value;
    }
    /**
     *抽象方法
     *@param
     */
    abstract function run();
}
/**
 *小汽车抽象类
 */
abstract class car extends vehicle{
    /**
     *两厢，三厢
     *@var string
     */    
    public $room;
    /**
     *设置属性值
     *@param $key 属性名
     *@param $value 属性值
     */
    public function setRoom( $value ){
       $this->room = $value; 
    }
    /**
     *获取属性值
     *
     */
    public function getRoom(){
        return $this->room;
    }
    /**
     *抽象方法
     *
     */
    abstract function run();
}
/**
 *抽象工厂类
 */
abstract class factory{
    abstract static function create( $className );
}
/**
 *小汽车工厂类
 *@author li.yonghuan
 *@version 2014.01.13
 */
class carFactory extends factory {

    /**
     *实现工厂方法
     *@param $className 类名
     */
    public static function create( $className ){
        $class = $className;
        return new $class();
    }
}
/**
 *奥迪车
 */
class audi extends car{

    public function run(){
        parent::setProperty('brand','audi');
        $brand = parent::getProperty('brand');
        $this->setRoom('threeRoom');
       return $this->room.' '.$brand.' car is running'; 
    }
}

$car = carFactory::create('audi');
echo $car->run();
```

#### 5 算法

##### 5.1 冒泡排序

> 每次比较相邻的两个元素，如果它们顺序错误就交换过来
>
> 时间复杂度：O(n2)

```php
<?php
    $a = array(3,8,1,4,11,7);
	$len = count($a);
	for ($i=1; $i<$len; $i++) {
        for($j=$len-1; $j>=$i;$j--) {
            if ($a[$j] < $a[$j-1]){//如果改成从大到小的话，只要将if改成if ($a[$j] > $a[$j-1])
                $temp = $a[$j];
                $a[$j] = $a[$j-1];
                $a[$j-1] = $temp;
            }
        }
	}
	print_r($a);
```

##### 5.2 快速排序

> 选择一个基准元素，通常选择第一个元素或者最后一个元素。通过一趟扫描，将待排序列分成两部分，一部分比基准元素小，一部分大于等于基准元素。此时基准元素在其排好序后的正确位置，然后再用同样的方法递归地排序划分的两部分
>
> 时间复杂度：O(nlogN) 最差：O(n2)

**方法一**

```php
<?php
    /**
    * 快速排序.
  	* @param  array $value 待排序数组
  	* @param  array $left  左边界
  	* @param  array $right 右边界
  	* @return array
  	*/
    function quick(&$value, $left, $right) {
        //左右边界重合，跳出
        if ($left >= $right) {
            return;
        }
        //左边的设为基准数
        $base = $left;
        //执行
        do {
            // 从最右边开始找到第一个比基准小的值，互换位置
            // 找到基准索引为止
            for ($i=$right; $i > $base; --$i) {
                if ($value[$i] < $value[$base]) {
                    $tmp = $value[$i];
                    $value[$i] = $value[$base];
                    $value[$base] = $tmp;
                    $base = $i; // 更新基准值索引
                    break;
                }
            }
            // 从最左边开始找到第一个比基准大的值，互换位置
            // 找到基准索引为止
            for ($j=$left; $j < $base; ++$j) {
                if ($value[$j] > $value[$base]) {
                    $tmp = $value[$j];
                    $value[$j] = $value[$base];
                    $value[$base] = $tmp;
                    $base = $j; // 更新基准值索引
                    break;
                }
            }
        } while($i > $j);//直到左右索引重合为止
        // 开始递归
        // 以当前索引为分界
        // 开始排序左部分
        quick($value, $left, $i-1);
        // 开始排序右边部分
        quick($value, $i+1, $right);
        return $value;
	}
```

**方法二**

```php
<?php
   	/**
   	* 快速排序.while版本
   	* @param  array $value 待排序数组
   	* @param  array $left  左边界
   	* @param  array $right 右边界
   	* @return array
   	*/
    function quick_while(&value, $left, $right) {
        if ($left >= $right) {
            return;
        }
        $point = $left;
        $i = $right;
        $j = $left;
        //查右边值
        while ($i > $j) {
            if ($value[$i] < $value[$point]) {
                $tmp = $value[$i];
                $value[$i] = $value[$point];
                $value[$point] = $tmp;
                $point = $i;
                break;
            }
            --$i;
        }
        //查左边值
        while ($j < $point) {
            if ($value[$j] > $value[$point]) {
                $tmp = $value[$j];
                $value[$j] = $value[$point];
                $value[$point] = $tmp;
                $point = $j;
                break;
            }
            ++$j;
        }
        // 开始递归
        // 以当前索引为分界
        // 开始排序左部分
        quick_while($value, $left, $i-1);
        // 开始排序右边部分
        quick_while($value, $i+1, $right);
        return $value;
	}
```

##### 5.3 选择排序

> 在要排序的一组数中，选出最小的一个数与第一个位置的数交换。然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。

```php
$list = array(10,3,5,7,18,11,45,64,74,23,21,6);
$list = selectSort($list)
var_dump($list);

function selectSort($arr) {
//双重循环完成，外层控制轮数，内层控制比较次数
 $len=count($arr);
    for($i=0; $i<$len-1; $i++) {
        //先假设最小的值的位置
        $p = $i;

        for($j=$i+1; $j<$len; $j++) {
            //$arr[$p] 是当前已知的最小值
            if($arr[$p] > $arr[$j]) {
            //比较，发现更小的,记录下最小值的位置；并且在下次比较时采用已知的最小值进行比较。
                $p = $j;
            }
        }
        //已经确定了当前的最小值的位置，保存到$p中。如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可。
        if($p != $i) {
            $tmp = $arr[$p];
            $arr[$p] = $arr[$i];
            $arr[$i] = $tmp;
        }
    }
    //返回最终结果
    return $arr;
}
```

##### 5.4 插入排序

> 在要排序的一组数中，假设前面的数已经是排好顺序的，现在要把第n个数插到前面的有序数中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。

```php
$list = array(10,3,5,7,18,11,45,64,74,23,21,6);
$list = insertSort($list);
var_dump($list);

function insertSort($arr) {
    $len=count($arr); 
    for($i=1, $i<$len; $i++) {
        $tmp = $arr[$i];
        //内层循环控制，比较并插入
        for($j=$i-1;$j>=0;$j--) {
            if($tmp < $arr[$j]) {
                //发现插入的元素要小，交换位置，将后边的元素与前面的元素互换
                $arr[$j+1] = $arr[$j];
                $arr[$j] = $tmp;
            } else {
                //如果碰到不需要移动的元素，由于是已经排序好是数组，则前面的就不需要再次比较了。
                break;
            }
        }
    }
    return $arr;
}
```

#### 6 web基础

##### 6.1 TCP/IP协议

> 详细见“计算机网络面试知识点”

##### 6.2 HTTP协议

> HTTP-超文本传输协议-应用层
>
> 特点：简单快速、灵活、无连接、无状态、支持B/S或C/S模式

###### URL

> UniformResourceLocator-统一资源定位符
>
> scheme://host[:port#]/path/.../[?query-string][#anchor] 

**组成**

1. 协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符
2. 域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用
3. 端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口
4. 虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”
5. 文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名
6. 锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分
7. 参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

###### URL和URI的区别

> URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源
>
> URL，是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源

###### HTTP请求消息Request

> 请求行（request line）、请求头部（header）、空行和请求数据四个部分

**GET**

```
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host    img.mukewang.com
User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept    image/webp,image/*,*/*;q=0.8
Referer    http://www.imooc.com/
Accept-Encoding    gzip, deflate, sdch
Accept-Language    zh-CN,zh;q=0.8

```

- 第一部分：请求行，用来说明请求类型,要访问的资源以及所使用的HTTP版本.

GET说明请求类型为GET,[/562f25980001b1b106000338.jpg]为要访问的资源，该行的最后一部分说明使用的是HTTP1.1版本。

- 第二部分：请求头部，紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息

从第二行起为请求头部，HOST将指出请求的目的地.User-Agent,服务器端和客户端脚本都能访问它,它是浏览器类型检测逻辑的重要基础.该信息由你的浏览器来定义,并且在每个请求中自动发送等等

- 第三部分：空行，请求头部后面的空行是必须的

即使第四部分的请求数据为空，也必须有空行。

- 第四部分：请求数据也叫主体，可以添加任意的其他数据。

这个例子的请求数据为空。

**POST**

```
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```

name=Professional%20Ajax&publisher=Wiley 第一部分：请求行，第一行明了是post请求，以及http1.1版本。 第二部分：请求头部，第二行至第六行。 第三部分：空行，第七行的空行。 第四部分：请求数据，第八行。

###### HTTP响应消息Response

> 状态行、消息报头、空行和响应正文

![img](https://upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/683/format/webp)

```
HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Content-Type: text/html; charset=UTF-8

<html>
      <head></head>
      <body>
            <!--body goes here-->
      </body>
</html>
```

- 第一部分：状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成。

  第一行为状态行，（HTTP/1.1）表明HTTP版本为1.1版本，状态码为200，状态消息为（ok）

- 第二部分：消息报头，用来说明客户端要使用的一些附加信息

  第二行和第三行为消息报头， Date:生成响应的日期和时间；Content-Type:指定了MIME类型的HTML(text/html),编码类型是UTF-8

- 第三部分：空行，消息报头后面的空行是必须的

- 第四部分：响应正文，服务器返回给客户端的文本信息。

  空行后面的html部分为响应正文。

###### HTTP之状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

> 1xx：指示信息--表示请求已接收，继续处理
>
> 2xx：成功--表示请求已被成功接收、理解、接受
>
> 3xx：重定向--要完成请求必须进行更进一步的操作
>
> 4xx：客户端错误--请求有语法错误或请求无法实现
>
> 5xx：服务器端错误--服务器未能实现合法的请求

常见状态码：

```
200 OK                        //客户端请求成功
400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
403 Forbidden                 //服务器收到请求，但是拒绝提供服务
404 Not Found                 //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error     //服务器发生不可预期的错误
503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
```

更多状态码<http://www.runoob.com/http/http-status-codes.html>

###### HTTP请求方法

> 根据HTTP标准，HTTP请求可以使用多种请求方法。 HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。 HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

- GET 请求指定的页面信息，并返回实体主体。
- HEAD 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
- POST 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
- PUT 从客户端向服务器传送的数据取代指定的文档的内容。
- DELETE 请求服务器删除指定的页面。
- CONNECT HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
- OPTIONS 允许客户端查看服务器的性能。
- TRACE 回显服务器收到的请求，主要用于测试或诊断。

###### HTTP 工作原理

> 客户端连接Web服务器-->发送HTTp请求-->服务器接受请求并返回HTTP响应-->释放连接TCP连接-->客户端浏览器解析HTML内容-

###### GET和POST请求的区别

参考https://www.cnblogs.com/logsharing/p/8448446.html

#### 7 web服务

##### 7.1 Socket

> TCP Socket和UDP Socket
>
> ip地址，协议，端口

PHP socket https://www.swoole.com/

##### 7.2 REST

> RESTful，是目前最为流行的一种互联网软件架构
>
> 结构清晰、符合标准、易于理解、扩展方便
>
> REST(REpresentational State Transfer)这个概念，首次出现是在 2000年Roy Thomas Fielding（他是HTTP规范的主要编写者之一）的博士论文中，它指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是RESTful的
>
> REST是一种架构风格，汲取了WWW的成功经验：无状态，以资源为中心，充分利用HTTP协议和URI协议，提供统一的接口定义，使得它作为一种设计Web服务的方法而变得流行

**三个概念**

- 资源（Resources） REST是"表现层状态转化"，其实它省略了主语。"表现层"其实指的是"资源"的"表现层"。

  那么什么是资源呢？就是我们平常上网访问的一张图片、一个文档、一个视频等。这些资源我们通过URI来定位，也就是一个URI表示一个资源。

- 表现层（Representation）

  资源是做一个具体的实体信息，他可以有多种的展现方式。而把实体展现出来就是表现层，例如一个txt文本信息，他可以输出成html、json、xml等格式，一个图片他可以jpg、png等方式展现，这个就是表现层的意思。

  URI确定一个资源，但是如何确定它的具体表现形式呢？应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

- 状态转化（State Transfer）

  访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，肯定涉及到数据和状态的变化。而HTTP协议是无状态的，那么这些状态肯定保存在服务器端，所以如果客户端想要通知服务器端改变数据和状态的变化，肯定要通过某种方式来通知它。

  客户端能通知服务器端的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

##### 7.3 RPC

> RPC（Remote Procedure Call Protocol）——远程过程调用协议，是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议

#### 8.web安全

##### 8.1 CSRF攻击

> CSRF（Cross-site request forgery），中文名称：跨站请求伪造，也被称为：one click attack/session riding，缩写为：CSRF/XSRF。

要完成一次CSRF攻击，受害者必须依次完成两个步骤 ：

- 1.登录受信任网站A，并在本地生成Cookie 。
- 2.在不退出A的情况下，访问危险网站B。

服务端的预防CSRF攻击的方式方法有多种，但思想上都是差不多的，主要从以下2个方面入手：

- 1、正确使用GET,POST和Cookie；
- 2、在非GET请求中增加伪随机数；

##### 8.2 XSS攻击

> 跨站脚本攻击(Cross-Site Scripting)，为了不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS

XSS目前主要的手段和目的如下：

- 盗用cookie，获取敏感信息。
- 利用植入Flash，通过crossdomain权限设置进一步获取更高权限；或者利用Java等得到类似的操作。
- 利用iframe、frame、XMLHttpRequest或上述Flash等方式，以（被攻击者）用户的身份执行一些管理动作，或执行一些如:发微博、加好友、发私信等常规操作，前段时间新浪微博就遭遇过一次XSS。
- 利用可被攻击的域受到其他域信任的特点，以受信任来源的身份请求一些平时不允许的操作，如进行不当的投票活动。
- 在访问量极大的一些页面上的XSS可以攻击一些小型网站，实现DDoS攻击的效果

目前防御XSS主要有如下几种方式：

- 过滤特殊字符

  避免XSS的方法之一主要是将用户所提供的内容进行过滤，Go语言提供了HTML的过滤函数：

  text/template包下面的HTMLEscapeString、JSEscapeString等函数

- 使用HTTP头指定类型

  `w.Header().Set("Content-Type","text/javascript")`

  这样就可以让浏览器解析javascript代码，而不会是html输出。

##### 8.3 SQL注入

> SQL注入攻击（SQL Injection），简称注入攻击，是Web开发中最常见的一种安全漏洞。可以用它来从数据库获取敏感信息，或者利用数据库的特性执行添加用户，导出文件等一系列恶意操作，甚至有可能获取数据库乃至系统用户最高权限

如何来防治

1. 严格限制Web应用的数据库的操作权限，给此用户提供仅仅能够满足其工作的最低权限，从而最大限度的减少注入攻击对数据库的危害。
2. 检查输入的数据是否具有所期望的数据格式，严格限制变量的类型.
3. 对进入数据库的特殊字符（'"\尖括号&*;等）进行转义处理，或编码转换。
4. 所有的查询语句建议使用数据库提供的参数化查询接口，参数化的语句使用参数而不是将用户输入变量嵌入到SQL语句中，即不要直接拼接SQL语句
5. 在应用发布之前建议使用专业的SQL注入检测工具进行检测，以及时修补被发现的SQL注入漏洞。网上有很多这方面的开源工具，例如sqlmap、SQLninja等。
6. 避免网站打印出SQL错误信息，比如类型错误、字段不匹配等，把代码里的SQL语句暴露出来，以防止攻击者利用这些错误信息进行SQL注入。
