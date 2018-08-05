# PHP版本特性总结\(仅仅是自己可能不熟悉的部分\)

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

原本格式为是\(expr1\) ? \(expr2\) : \(expr3\)

如果expr1结果为True，则返回expr2的结果。

新增一种书写方式，可以省略中间部分，书写为expr1 ?: expr3

如果expr1结果为True,则返回expr1的结果

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



