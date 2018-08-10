# PHP基础知识

---

什么是PHP?

PHP（"_PHP: Hypertext Preprocessor_"，超文本预处理器的字母缩写）是一种被广泛应用的开放源代码的多用途脚本语言，它可嵌入到 HTML中，尤其适合 web 开发。

## 配置PHP

### 配置文件php.ini

### .user.ini 文件

自 PHP 5.3.0 起，PHP 支持基于每个目录的 .htaccess 风格的 INI 文件。此类文件_仅_被 CGI／FastCGI SAPI 处理。此功能使得 PECL 的 htscanner 扩展作废。如果使用 Apache，则用 .htaccess 文件有同样效果。

除了主 php.ini 之外，PHP 还会在每个目录下扫描 INI 文件，从被执行的 PHP 文件所在目录开始一直上升到 web 根目录（`$_SERVER['DOCUMENT_ROOT']` 所指定的）。如果被执行的 PHP 文件在 web 根目录之外，则只扫描该目录。

在 .user.ini 风格的 INI 文件中只有具有 `PHP_INI_PERDIR` 和 `PHP_INI_USER` 模式的 INI 设置可被识别。

两个新的 INI 指令，_user\_ini.filename_ 和 _user\_ini.cache\_ttl_ 控制着用户 INI 文件的使用。

_user\_ini.filename_ 设定了 PHP 会在每个目录下搜寻的文件名；如果设定为空字符串则 PHP 不会搜寻。默认值是 _.user.ini_。

_user\_ini.cache\_ttl_ 控制着重新读取用户 INI 文件的间隔时间。默认是 300 秒（5 分钟）。

## 数据类型

### 四种标量类型

#### boolean\(布尔类型\)

> 当转换为 boolean 时，以下值被认为是 `FALSE`：

* 布尔值 `FALSE` 本身
* 整型值 0（零）
* 浮点型值 0.0（零）
* 空字符串，以及字符串 "0"
* 不包括任何元素的数组
* 不包括任何成员变量的对象（仅 PHP 4.0 适用）
* 特殊类型 NULL（包括尚未赋值的变量）
* 从空标记生成的 SimpleXML 对象

  所有其它值都被认为是 `TRUE`（包括任何资源）。

#### integer\(整型\)

1. 要使用八进制表达，数字前必须加上 _0_（零）。要使用十六进制表达，数字前必须加上 _0x_。要使用二进制表达，数字前必须加上 _0b_。

2. Integer 值的字长可以用常量 `PHP_INT_SIZE`来表示，自 PHP 4.4.0 和 PHP 5.0.5后，最大值可以用常量 `PHP_INT_MAX` 来表示。

3. 整数溢出

   > 如果给定的一个数超出了 integer 的范围，将会被解释为 float。同样如果执行的运算结果超出了 integer 范围，也会返回 float。

4. PHP 中没有整除的运算符。_1/2_ 产生出 [float](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.float.html)_0.5_。值可以舍弃小数部分强制转换为 [integer](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.integer.html)，或者使用 [round\(\)](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/function.round.html) 函数可以更好地进行四舍五入。

5. 强制转换为整型

   > 要明确地将一个值转换为 [integer](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.integer.html)，用 _\(int\)_ 或 _\(integer\)_ 强制转换。不过大多数情况下都不需要强制转换，因为当运算符，函数或流程控制需要一个 [integer](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.integer.html) 参数时，值会自动转换。还可以通过函数 [intval\(\)](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/function.intval.html) 来将一个值转换成整型。

#### float\(浮点型\)

#### string\(字符串类型\)

### 两种复合类型

array\(数组\)

object\(对象\)

### 特殊类型

resource\(资源\)

NULL\(无类型\)

### 三种伪类型

mixed\(混合类型\)

number\(数字类型\)

callback\(回调类型\)

