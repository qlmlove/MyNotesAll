# 更好的函数

性能更好或者另一废弃函数的代替

### 1. mt\_rand\(\)

**对比函数:**rand\(\)

**用法:** mt\_rand\(mix,max\)

**说明:**

如果没有提供可选参数min和max，mt\_rand\(\) 返回 0 到 RAND\_MAX 之间的伪随机数。例如想要 5 到 15（包括 5 和 15）之间的随机数，用 mt\_rand\(5, 15\)。

很多老的 libc 的随机数发生器具有一些不确定和未知的特性而且很慢。PHP 的 rand\(\) 函数默认使用 libc 随机数发生器。mt\_rand\(\) 函数是非正式用来替换它的。该函数用了 Mersenne Twister 中已知的特性作为随机数发生器，它可以产生随机数值的平均速度比 libc 提供的 rand\(\) 快四倍。

### 2. round\(\)

四舍五入

```php
<?php
var_dump(25/7);         // float(3.5714285714286) 
var_dump((int) (25/7)); // int(3)
var_dump(round(25/7));  // float(4) 
?>
```



