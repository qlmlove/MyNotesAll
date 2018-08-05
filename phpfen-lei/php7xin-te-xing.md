# PHP版本特性总结\(不仅仅是PHP7,仅仅是自己可能不熟悉的部分\)

---

### 特性总结:

##### 1. 空合并运算符（??）

简化判断

```php
$param = $_GET['param'] ?? 1;
```

相当于：

```php
$param = isset($_GET['param']) ? $_GET['param'] : 1;
```

##### 2. 太空船操作符\(组合比较符\)

用于比较两个表达式.左边大于右边返回1,等于返回0,小于返回-1

```php
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
```

##### 3. **Traits**

Traits提供了一种灵活的代码重用机制，即不像interface一样只能定义方法但不能实现，又不能像class一样只能单继承。至于在实践中怎样使用，还需要深入思考。

魔术常量为** TRAIT**

```php
# 官网的一个例子：  
trait SayWorld {  
        public function sayHello() {  
                parent::sayHello();  
                echo "World!\n";  
                echo 'ID:' . $this->id . "\n";  
        }  
}  

class Base {  
        public function sayHello() {  
                echo 'Hello ';  
        }  
}  

class MyHelloWorld extends Base {  
        private $id;  

        public function __construct() {  
                $this->id = 123456;  
        }  

        use SayWorld;  
}  

$o = new MyHelloWorld();  
$o->sayHello();  

/*will output: 
Hello World! 
ID:123456 
 */
```

##### 4. 非变量array和strng也能支持下标获取

```php
echo array(1,2,3)[0];
echo [1, 2, 3][0];
echo  "foobar"[2];
```

##### 5. 函数变量类型声明

两种模式 : 强制 \( 默认 \) 和 严格模式  
类型：array,object\(对象\),string、int、float和 bool\

```php
class bar {  
function foo(bar $foo) {  
}  
//其中函数foo中的参数规定了传入的参数必须为bar类的实例，否则系统会判断出错。同样对于数组来说，也可以进行判断，比如：  
function foo(array $foo) {  
}   
foo(array(1, 2, 3)); // 正确，因为传入的是数组  
foo(123); // 不正确，传入的不是数组

function add(int $a) 
{ 
    return 1+$a; 
} 
var_dump(add(2));

function foo(int $i) { ... }  
foo(1);      // $i = 1  
foo(1.0);    // $i = 1  
foo("1");    // $i = 1  
foo("1abc"); // not yet clear, maybe $i = 1 with notice  
foo(1.5);    // not yet clear, maybe $i = 1 with notice  
foo([]);     // error  
foo("abc");  // error
```

##### 6. 三元运算符

原本格式为是`(expr1) ? (expr2) : (expr3)`

如果`expr1`结果为`True`，则返回`expr2`的结果。

新增一种书写方式，可以省略中间部分，书写为`expr1 ?: expr3`

如果`expr1`结果为`True`,则返回`expr1`的结果

```php
$expr1=1;
$expr2=2;
//原格式  
$expr=$expr1?$expr1:$expr2  
//新格式  
$expr=$expr1?:$expr2
/* 输出结果:
1
1
*/
```

##### 7. 参数跳跃\(只是一个提议,官方好像并没有采纳\)

如果你有一个函数接受多个可选的参数，目前没有办法只改变最后一个参数，而让其他所有参数为默认值。  
RFC上的例子，如果你有一个函数如下：

```php
function create_query($where, $order_by, $join_type='', $execute = false, $report_errors = true) { ... }
```

那么有没有办法设置`$report_errors=false`，而其他两个为默认值。为了解决这个跳跃参数的问题而提出：

```php
create_query("deleted=0", "name", default, default, false);
```

##### 8. 可变函数参数

代替 func\_get\_args\(\)

```php
function add(...$args)  
{  
    $result = 0;  
    foreach($args as $arg)  
        $result += $arg;  
    return $result;  
}
```

##### 9. 可为空（Nullable）类型

类型允许为空，当启用这个特性时，传入的参数或者函数返回的结果要么是给定的类型，要么是 null 。可以通过在类型前面加上一个**问号**来使之成为可为空的。

```php
function test(?string $name)
{
    var_dump($name);
}
```

以上例程会输出：

```php
string(5) "tpunt"
NULL
Uncaught Error: Too few arguments to function test(), 0 passed in...
```

##### 10. Void 函数

在PHP 7 中引入的其他返回值类型的基础上，一个新的返回值类型void被引入。 返回值声明为 void 类型的方法要么干脆省去 return 语句，要么使用一个空的 return 语句。 对于 void 函数来说，null 不是一个合法的返回值。

```php
function swap(&$left, &$right) : void
{
    if ($left === $right) {
        return;
    }
    $tmp = $left;
    $left = $right;
    $right = $tmp;
}
$a = 1;
$b = 2;
var_dump(swap($a, $b), $a, $b);
```

以上例程会输出：

```php
null
int(2)
int(1)
```

试图去获取一个 void 方法的返回值会得到 null ，并且不会产生任何警告。这么做的原因是不想影响更高层次的方法。

##### 11. 返回值类型声明

函数和匿名函数都可以指定返回值的类型

```php
function show(): array 
{ 
    return [1,2,3,4]; 
}

function arraysSum(array ...$arrays): array
{
  return array_map(function(array $array): int {
      return array_sum($array);
  }, $arrays);
}
```

##### 12. 参数解包功能

在调用函数的时候，通过 … 操作符可以把数组或者可遍历对象解包到参数列表，这和Ruby等语言中的扩张\(splat\)操作符类似

```php
function add($a, $b, $c) {  
    return $a + $b + $c;  
}  
$arr = [2, 3];  
add(1, ...$arr);
```

##### 13. 实例化类

```php
class test{  
    function show(){  
return 'test';  
    }  
}  
echo (new test())->show();
```

##### 14. 支持 Class::{expr}\(\) 语法

```php
foreach ([new Human("Gonzalo"), new Human("Peter")] as $human) {  
    echo $human->{'hello'}();  
}
```

##### 15. 迭代器 yield

在旧版本中，自定义迭代器很少使用，因为它们的实现，需要大量的样板代码。生成器解决这个问题，并提供了一种简单的样板代码来创建迭代器。  
例如，你可以定义一个范围函数作为迭代器:

```php
function *xrange($start, $end, $step = 1) {  
    for ($i = $start; $i < $end; $i += $step) {  
        yield $i;  
    }  
}  
foreach (xrange(10, 20) as $i) {  
    // ...  
}
```

上述xrange函数具有与内建函数相同的行为，但有一点区别：不是返回一个数组的所有值，而是返回一个迭代器动态生成的值。

##### 16. 列表解析和生成器表达式

列表解析提供一个简单的方法对数组进行小规模操作:

```php
$firstNames = [foreach ($users as $user) yield $user->firstName];
```

上述列表解析相等于下面的代码：

```php
$firstNames = [];  
foreach ($users as $user) {  
    $firstNames[] = $user->firstName;  
}
```

也可以这样过滤数组:

```php
$underageUsers = [foreach ($users as $user) if ($user->age < 18) yield $user];
```

生成器表达式也很类似，但是返回一个迭代器\(用于动态生成值\)而不是一个数组。

##### 17. 多异常捕获处理

一个catch语句块现在可以通过管道字符\(\|\)来实现多个异常的捕获。 这对于需要同时处理来自不同类的不同异常时很有用。

```php
try {
    // some code
} catch (FirstException | SecondException $e) {
    // handle first and second exceptions
} catch (\Exception $e) {
    // ...
} finally{
//
}
```

##### 18. foreach 支持list\(\)

对于“数组的数组”进行迭代，之前需要使用两个foreach，现在只需要使用foreach + list了，但是这个数组的数组中的每个数组的个数需要一样。看文档的例子一看就明白了。

```php
$array = [  
    [1, 2],  
    [3, 4],  
];  
foreach ($array as list($a, $b)) {  
    echo "A: $a; B: $b\n";  
}
```

##### 19. 为unserialize\(\)提供过滤

这个特性旨在提供更安全的方式解包不可靠的数据。它通过白名单的方式来防止潜在的代码注入。

```php
//将所有对象分为__PHP_Incomplete_Class对象
$data = unserialize($foo, ["allowed_classes" => false]);
//将所有对象分为__PHP_Incomplete_Class 对象 除了ClassName1和ClassName2
$data = unserialize($foo, ["allowed_classes" => ["ClassName1", "ClassName2"]);
//默认行为，和 unserialize($foo)相同
$data = unserialize($foo, ["allowed_classes" => true]);
```

##### 20. intdiv\(\)

接收两个参数作为被除数和除数，返回他们相除结果的整数部分。

```php
var_dump(intdiv(7, 2));
```

输出`int(3)`

##### 21. Session options

现在，session\_start\(\)函数可以接收一个数组作为参数，可以覆盖php.ini中session的配置项。  
比如，把cache\_limiter设置为私有的，同时在阅读完session后立即关闭。

```php
session_start(['cache_limiter' => 'private',
               'read_and_close' => true,
]);
```

##### 22. $\_SERVER\[“REQUEST\_TIME\_FLOAT”\]

这个是用来统计服务请求时间的，并用ms\(毫秒\)来表示

```php
echo "脚本执行时间 ", round(microtime(true) - $_SERVER["REQUEST_TIME_FLOAT"], 2), "s";
```

23.empty\(\) 支持任意表达式

empty\(\) 现在支持传入一个任意表达式，而不仅是一个变量

```php
function always_false() {
    return false;
}

if (empty(always_false())) {
    echo 'This will be printed.';
}

if (empty(true)) {
    echo 'This will not be printed.';
}
```

输出  
`This will be printed.`

##### 24. php://input 可以被复用

php://input 开始支持多次打开和读取，这给处理POST数据的模块的内存占用带来了极大的改善。

##### 25. Upload progress 文件上传

Session提供了上传进度支持，通过$\_SESSION\[“upload\_progress\_name”\]就可以获得当前文件上传的进度信息，结合Ajax就能很容易实现上传进度条了。  
\[详细的介绍看\]\[4\]

---

_**参考:**_

\[1\]:[https://blog.csdn.net/h330531987/article/details/74364681](https://blog.csdn.net/h330531987/article/details/74364681)"php7 新特性整理"

\[2\]:[https://blog.csdn.net/fenglailea/article/details/9853645](https://blog.csdn.net/fenglailea/article/details/9853645) "php5.3 /4/5/6 新特性,使用PHP5.5/PHP5.6要注意的"

\[3\]: [https://blog.csdn.net/fenglailea/article/details/52717364](https://blog.csdn.net/fenglailea/article/details/52717364) "PHP7.0,PHP7.1.x新特性"

\[4\]: [http://www.laruence.com/2011/10/10/2217.html](http://www.laruence.com/2011/10/10/2217.html) "上传进度支持\(Upload progress in sessions\)"

