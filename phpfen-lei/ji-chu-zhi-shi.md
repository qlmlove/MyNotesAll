# PHP基础知识

---

什么是PHP?

PHP（"_PHP: Hypertext Preprocessor_"，超文本预处理器的字母缩写）是一种被广泛应用的开放源代码的多用途脚本语言，它可嵌入到 HTML中，尤其适合 web 开发。

## 配置PHP

### 配置文件php.ini





### .user.ini 文件

 自 PHP 5.3.0 起，PHP 支持基于每个目录的 .htaccess 风格的 INI 文件。此类文件_仅_被 CGI／FastCGI SAPI 处理。此功能使得 PECL 的 htscanner 扩展作废。如果使用 Apache，则用 .htaccess 文件有同样效果。

 除了主 php.ini 之外，PHP 还会在每个目录下扫描 INI 文件，从被执行的 PHP 文件所在目录开始一直上升到 web 根目录（`$_SERVER['DOCUMENT_ROOT']` 所指定的）。如果被执行的 PHP 文件在 web 根目录之外，则只扫描该目录。

 在 .user.ini 风格的 INI 文件中只有具有 **`PHP_INI_PERDIR`** 和 **`PHP_INI_USER`** 模式的 INI 设置可被识别。

 两个新的 INI 指令，_user\_ini.filename_ 和 _user\_ini.cache\_ttl_ 控制着用户 INI 文件的使用。

_user\_ini.filename_ 设定了 PHP 会在每个目录下搜寻的文件名；如果设定为空字符串则 PHP 不会搜寻。默认值是 _.user.ini_。

_user\_ini.cache\_ttl_ 控制着重新读取用户 INI 文件的间隔时间。默认是 300 秒（5 分钟）。



## 数据类型

### 四种标量类型

boolean\(布尔类型\)

&gt;当转换为 [boolean](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.boolean.html) 时，以下值被认为是 **`FALSE`**：

* [布尔](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.boolean.html)值 **`FALSE`** 本身
* [整型](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.integer.html)值 0（零）
* [浮点型](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.float.html)值 0.0（零）
* 空[字符串](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.string.html)，以及[字符串](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.string.html) "0"
* 不包括任何元素的[数组](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.array.html)
* 不包括任何成员变量的[对象](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.object.html)（仅 PHP 4.0 适用）
* 特殊类型 [NULL](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.null.html)（包括尚未赋值的变量）
* 从空标记生成的 [SimpleXML](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/ref.simplexml.html) 对象

 所有其它值都被认为是 **`TRUE`**（包括任何[资源](mk:@MSITStore:D:\LAMPbrother\手册拓展及规范\php_manual_zh2016.chm::/res/language.types.resource.html)）。

integer\(整型\)

float\(浮点型\)

string\(字符串类型\)

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



