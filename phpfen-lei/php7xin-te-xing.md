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

##### 26. 新增函数 boolval

PHP已经实现了strval、intval和floatval的函数。为了达到一致性将添加boolval函数。它完全可以作为一个布尔值计算，也可以作为一个回调函数。

##### 27. array\_column

```php
//从数据库获取一列，但返回是数组。  
$userNames = [];  
foreach ($users as $user) {  
}  
//以前获取数组某列值，现在如下  
$userNames = array_column($users, 'name');
```

##### 28. Unicode codepoint 转译语法

这接受一个以16进制形式的 Unicode codepoint，并打印出一个双引号或heredoc包围的 UTF-8 编码格式的字符串。 可以接受任何有效的 codepoint，并且开头的 0 是可以省略的。

```php

echo "\u{9876}"  
1  
旧版输出：\u{9876}   
新版输入：顶
```
    ---

    ### PHP7.1不兼容性

    ---

    ##### 1.当传递参数过少时将抛出错误

    在过去如果我们调用一个用户定义的函数时，提供的参数不足，那么将会产生一个警告\(warning\)。 现在，这个警告被提升为一个错误异常\(Error exception\)。这个变更仅对用户定义的函数生效， 并不包含内置函数。例如：

    ```php
    function test($param){}
    test();

输出：

```php
Uncaught Error: Too few arguments to function test(), 0 passed in %s on line %d and exactly 1 expected in %s:%d
```

##### 2.禁止动态调用函数

禁止动态调用函数如下

1. assert\(\) - with a string as the first argument
2. compact\(\)
3. extract\(\)
4. func\_get\_args\(\)
5. func\_get\_arg\(\)
6. func\_num\_args\(\)
7. get\_defined\_vars\(\)
8. mb\_parse\_str\(\) - with one arg
9. parse\_str\(\) - with one arg 

```php
(function () {
    'func_num_args'();
})();
```

输出

```php
Warning: Cannot call func_num_args() dynamically in %s on line %d
```

##### 3.无效的类，接口，trait名称命名

以下名称不能用于 类，接口或trait 名称命名：  
1. `void`  
2. `iterable`

##### 4.Numerical string conversions now respect scientific notation

Integer operations and conversions on numerical strings now respect scientific notation. This also includes the \(int\) cast operation, and the following functions: intval\(\) \(where the base is 10\), settype\(\), decbin\(\), decoct\(\), and dechex\(\).

##### 5.mt\_rand 算法修复

mt\_rand\(\) will now default to using the fixed version of the Mersenne Twister algorithm. If deterministic output from mt\_srand\(\) was relied upon, then the MT\_RAND\_PHP with the ability to preserve the old \(incorrect\) implementation via an additional optional second parameter to mt\_srand\(\).

##### 6.rand\(\) 别名 mt\_rand\(\) 和 srand\(\) 别名 mt\_srand\(\)

rand\(\) and srand\(\) have now been made aliases to mt\_rand\(\) and mt\_srand\(\), respectively. This means that the output for the following functions have changes: rand\(\), shuffle\(\), str\_shuffle\(\), and array\_rand\(\).

##### 7.Disallow the ASCII delete control character in identifiers

The ASCII delete control character \(0x7F\) can no longer be used in identifiers that are not quoted.

##### 8.error\_log changes with syslog value

If the error\_log ini setting is set to syslog, the PHP error levels are mapped to the syslog error levels. This brings finer differentiation in the error logs in contrary to the previous approach where all the errors are logged with the notice level only.

##### 9.在不完整的对象上不再调用析构方法

析构方法在一个不完整的对象（例如在构造方法中抛出一个异常）上将不再会被调用

##### 10.call\_user\_func\(\)不再支持对传址的函数的调用

call\_user\_func\(\) 现在在调用一个以引用作为参数的函数时将始终失败。

##### 11.字符串不再支持空索引操作符 The empty index operator is not supported for strings anymore

对字符串使用一个空索引操作符（例如x）将会抛出一个致命错误， 而不是静默地将其转为一个数组

##### 12.ini配置项移除

下列ini配置项已经被移除：  
1. session.entropy\_file  
2.  session.entropy\_length  
3. session.hash\_function  
4. session.hash\_bits\_per\_character

---

### PHP7.0 不兼容性

---

##### 1、foreach不再改变内部数组指针

在PHP7之前，当数组通过 foreach 迭代时，数组指针会移动。现在开始，不再如此，见下面代码。

```php
$array = [0, 1, 2];
foreach ($array as &$val) {
    var_dump(current($array));
}
```

**PHP5输出：**

```php
int(1)
int(2)
bool(false)
```

PHP7输出：

```php
int(0)
int(0)
int(0)
```

##### 2、foreach通过引用遍历时，有更好的迭代特性

当使用引用遍历数组时，现在 foreach 在迭代中能更好的跟踪变化。例如，在迭代中添加一个迭代值到数组中，参考下面的代码：

```php
$array = [0];
foreach ($array as &$val) {
    var_dump($val);
    $array[1] = 1;
}
```

PHP5输出：  
int\(0\)  
PHP7输出：  
int\(0\)  
int\(1\)

##### 3、十六进制字符串不再被认为是数字

含十六进制字符串不再被认为是数字

```php
var_dump("0x123" == "291");
var_dump(is_numeric("0x123"));
var_dump("0xe" + "0x1");
var_dump(substr("foo", "0x1"));
```

PHP5输出：  
bool\(true\)  
bool\(true\)  
int\(15\)  
string\(2\) “oo”  
PHP7输出：  
bool\(false\)  
bool\(false\)  
int\(0\)  
Notice: A non well formed numeric value encountered in /tmp/test.php on line 5  
string\(3\) “foo”

##### 4、PHP7中被移除的函数

被移除的函数列表如下：  
call\_user\_func\(\) 和 call\_user\_func\_array\(\)从PHP 4.1.0开始被废弃。  
已废弃的 mcrypt\_generic\_end\(\) 函数已被移除，请使用mcrypt\_generic\_deinit\(\)代替。  
已废弃的 mcrypt\_ecb\(\), mcrypt\_cbc\(\), mcrypt\_cfb\(\) 和 mcrypt\_ofb\(\) 函数已被移除。  
set\_magic\_quotes\_runtime\(\), 和它的别名 magic\_quotes\_runtime\(\)已被移除. 它们在PHP 5.3.0中已经被废弃,并且 在in PHP 5.4.0也由于魔术引号的废弃而失去功能。  
已废弃的 set\_socket\_blocking\(\) 函数已被移除，请使用stream\_set\_blocking\(\)代替。  
dl\(\)在 PHP-FPM 不再可用，在 CLI 和 embed SAPIs 中仍可用。  
GD库中下列函数被移除：imagepsbbox\(\)、imagepsencodefont\(\)、imagepsextendfont\(\)、imagepsfreefont\(\)、imagepsloadfont\(\)、imagepsslantfont\(\)、imagepstext\(\)  
在配置文件php.ini中，always\_populate\_raw\_post\_data、asp\_tags、xsl.security\_prefs被移除了。

##### 5、new 操作符创建的对象不能以引用方式赋值给变量

new 操作符创建的对象不能以引用方式赋值给变量

```php
class C {}
$c =& new C;
```

PHP5输出：  
Deprecated: Assigning the return value of new by reference is deprecated in /tmp/test.php on line 3  
PHP7输出：  
Parse error: syntax error, unexpected ‘new’ \(T\_NEW\) in /tmp/test.php on line 3

##### 6、移除了 ASP 和 script PHP 标签

使用类似 ASP 的标签，以及 script 标签来区分 PHP 代码的方式被移除。 受到影响的标签有：&lt;% %&gt;、&lt;%= %&gt;、

##### 7、从不匹配的上下文发起调用

在不匹配的上下文中以静态方式调用非静态方法， 在 PHP 5.6 中已经废弃， 但是在 PHP 7.0 中， 会导致被调用方法中未定义 $this 变量，以及此行为已经废弃的警告。

```php
class A {
    public function test() { var_dump($this); }
}
// 注意：并没有从类 A 继承
class B {
    public function callNonStaticMethodOfA() { A::test(); }
}
(new B)->callNonStaticMethodOfA();
```

PHP5输出：  
Deprecated: Non-static method A::test\(\) should not be called statically, assuming $this from incompatible context in /tmp/test.php on line 8  
object\(B\)\#1 \(0\) {  
}  
PHP7输出：  
Deprecated: Non-static method A::test\(\) should not be called statically in /tmp/test.php on line 8  
Notice: Undefined variable: this in /tmp/test.php on line 3  
NULL

##### 8、在数值溢出的时候，内部函数将会失败

将浮点数转换为整数的时候，如果浮点数值太大，导致无法以整数表达的情况下， 在之前的版本中，内部函数会直接将整数截断，并不会引发错误。 在 PHP 7.0 中，如果发生这种情况，会引发 E\_WARNING 错误，并且返回 NULL。

##### 9、JSON 扩展已经被 JSOND 取代

JSON 扩展已经被 JSOND 扩展取代。  
对于数值的处理，有以下两点需要注意的：  
第一，数值不能以点号（.）结束 （例如，数值 34. 必须写作 34.0 或 34）。  
第二，如果使用科学计数法表示数值，e 前面必须不是点号（.） （例如，3.e3 必须写作 3.0e3 或 3e3）。

##### 10、INI 文件中 \# 注释格式被移除

在配置文件INI文件中，不再支持以 \# 开始的注释行， 请使用 ;（分号）来表示注释。 此变更适用于 php.ini 以及用 parse\_ini\_file\(\) 和 parse\_ini\_string\(\) 函数来处理的文件。

##### 11、$HTTP\_RAW\_POST\_DATA 被移除

不再提供 $HTTP\_RAW\_POST\_DATA 变量。 请使用 php://input 作为替代。

##### 12、yield 变更为右联接运算符

在使用 yield 关键字的时候，不再需要括号， 并且它变更为右联接操作符，其运算符优先级介于 print 和 =&gt; 之间。 这可能导致现有代码的行为发生改变。可以通过使用括号来消除歧义。

```php
echo yield -1;
// 在之前版本中会被解释为：
echo (yield) - 1;
// 现在，它将被解释为：
echo yield (-1);
yield $foo or die;
// 在之前版本中会被解释为：
yield ($foo or die);
// 现在，它将被解释为：
(yield $foo) or die;
```

---

### PHP 7.1.x 中废弃的特性

---

##### 1.ext/mcrypt

mcrypt 扩展已经过时了大约10年，并且用起来很复杂。因此它被废弃并且被 OpenSSL 所取代。 从PHP 7.2起它将被从核心代码中移除并且移到PECL中。

##### 2.mb\_ereg\_replace\(\)和mb\_eregi\_replace\(\)的Eval选项

对于mb\_ereg\_replace\(\)和mb\_eregi\_replace\(\)的 e模式修饰符现在已被废弃

##### 弃用或废除

下面是被弃用或废除的 INI 指令列表. 使用下面任何指令都将导致 错误.  
define\_syslog\_variables  
register\_globals  
register\_long\_arrays  
safe\_mode  
magic\_quotes\_gpc  
magic\_quotes\_runtime  
magic\_quotes\_sybase  
弃用 INI 文件中以 ‘\#’ 开头的注释.  
**弃用函数:**  
call\_user\_method\(\) \(使用 call\_user\_func\(\) 替代\)  
call\_user\_method\_array\(\) \(使用 call\_user\_func\_array\(\) 替代\)  
define\_syslog\_variables\(\)  
dl\(\)  
ereg\(\) \(使用 preg\_match\(\) 替代\)  
ereg\_replace\(\) \(使用 preg\_replace\(\) 替代\)  
eregi\(\) \(使用 preg\_match\(\) 配合 ‘i’ 修正符替代\)  
eregi\_replace\(\) \(使用 preg\_replace\(\) 配合 ‘i’ 修正符替代\)  
set\_magic\_quotes\_runtime\(\) 以及它的别名函数 magic\_quotes\_runtime\(\)  
session\_register\(\) \(使用\_SESSION 超全部变量替代\)  
session\_is\_registered\(\) \(使用 $\_SESSION 超全部变量替代\)  
set\_socket\_blocking\(\) \(使用 stream\_set\_blocking\(\) 替代\)  
split\(\) \(使用 preg\_split\(\) 替代\)  
spliti\(\) \(使用 preg\_split\(\) 配合 ‘i’ 修正符替代\)  
sql\_regcase\(\)  
mysql\_db\_query\(\) \(使用 mysql\_select\_db\(\) 和 mysql\_query\(\) 替代\)  
mysql\_escape\_string\(\) \(使用 mysql\_real\_escape\_string\(\) 替代\)  
废弃以字符串传递区域设置名称. 使用 LC\_\* 系列常量替代.  
mktime\(\) 的 is\_dst 参数. 使用新的时区处理函数替代.  
**弃用的功能:**  
弃用通过引用分配 new 的返回值.  
调用时传递引用被弃用.  
已弃用的多个特性 allow\_call\_time\_pass\_reference、define\_syslog\_variables、highlight.bg、register\_globals、register\_long\_arrays、magic\_quotes、safe\_mode、zend.ze1\_compatibility\_mode、session.bug\_compat42、session.bug\_compat\_warn 以及 y2k\_compliance。

---

_**参考:**_

\[1\]: [https://blog.csdn.net/h330531987/article/details/74364681](https://blog.csdn.net/h330531987/article/details/74364681)"php7 新特性整理"

\[2\]: [https://blog.csdn.net/fenglailea/article/details/9853645](https://blog.csdn.net/fenglailea/article/details/9853645) "php5.3 /4/5/6 新特性,使用PHP5.5/PHP5.6要注意的"

\[3\]: [https://blog.csdn.net/fenglailea/article/details/52717364](https://blog.csdn.net/fenglailea/article/details/52717364) "PHP7.0,PHP7.1.x新特性"

\[4\]: [http://www.laruence.com/2011/10/10/2217.html](http://www.laruence.com/2011/10/10/2217.html) "上传进度支持\(Upload progress in sessions\)"

