# PHP基础知识

### 特性总结:

1. 空合并运算符（??）

简化判断

```php
$param = $_GET['param'] ?? 1;
```

相当于：

```php
$param = isset($_GET['param']) ? $_GET['param'] : 1;
```

1. 太空船操作符\(组合比较符\)

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

1. **Traits**

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



