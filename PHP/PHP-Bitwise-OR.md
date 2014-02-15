PHP手记 20140213
---
#### --PHP中关于 ||、|、&&、&
---
**首先我们来看一个问题：**

	<?php
		$a = 4;
		$b = 5;
		if ($a = 4 || $b = 7)
		{
			$a++; $b++;
		}
		echo $a . " " . $b;

结果输出是什么？最开始我以为是`5 6`，但是执行结果是`1 8`，仔细就看了一下发现 `if` 条件句中不是 == 是 `=`，但是是`=`的时候为什么就输出1和8了呢？

因为在 if 条件句中的赋值会被强制转换为bool值 `false`，false的值为 0 ，所以 $a 在条件句中其实是被执行了 0++ ，而 $b 并没有被赋值，这个是因为php中 `||`（&&）的短路现象，在$a 被成功赋值后，这个 if 表达式已经被短路干掉了。所以在执行了 $a++,$b++ 后得到的值是 1和 8.

同理，把其中的 || 换成 && 也是支持短路运算的。


**再来看另一个问题：**

	<?php
		$a = 4;
		$b = 5;
		if ($a = 4 | $b = 7)
		{
			$a++; $b++;
		}
		echo $a . " " . $b;
		
结果输出是什么？

结果输出是`8 8`，因为在这里，表达式并没有被执行短路，而是分别赋值 $a = 4, $b = 7;

`|`这个操作符是将左右两边的值换算为二进制后按位比较，只要有一边为1，就将左边那位变为1，（遇1得1）,4 的 二进制为`110` , 而 7 的二进制为 111，两者遇一得1，结果为 `111`,也就是 $a 被运算为 7，执行了 $a ++   , 就是 8 .

而 $b 在 if中没有被强转类型，被成功赋值为 7 ，执行 ++ 后也是 8 。

<br>
<br>
<br>
Primo.chu
<br>

2014-02-13 18:10:26