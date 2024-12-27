---
layout: post
title: "Javascrpit中的正则表达式"
date: 2012-02-10 23:10
comments: true
categories: [正则表达式，Javascript]
---

建立正则表达式对象语法
`re = new RegExp(/pattern/[flags])`

flags
{% highlight ruby %}
g （全文查找出现的所有 pattern）
i （忽略大小写）
m （多行查找）
{% endhighlight %}

普通字符
{% highlight ruby %}
\ 	将下一个字符标记为一个特殊字符、或一个原义字符、或一个 后向引用、或一个八进制转义符。例如，’n’ 匹配字符 “n”。’\n’ 匹配一个换行符。序列 ‘\\’ 匹配 “\” 而 “\(” 则匹配 “(”。
. 	匹配除 “\n” 之外的任何单个字符。要匹配包括 ‘\n’ 在内的任何字符，请使用象 ‘[.\n]‘ 的模式。
x|y 	匹配 x 或 y。例如，’z|food’ 能匹配 “z” 或 “food”。’(z|f)ood’ 则匹配 “zood” 或 “food”。
[xyz] 	字符集合。匹配所包含的任意一个字符。例如， ‘[abc]‘ 可以匹配 “plain” 中的 ‘a’。
[^xyz] 	负值字符集合。匹配未包含的任意字符。例如， ‘[^abc]‘ 可以匹配 “plain” 中的’p'。
[a-z] 	字符范围。匹配指定范围内的任意字符。例如，’[a-z]‘ 可以匹配 ‘a’ 到 ‘z’ 范围内的任意小写字母字符。
[^a-z] 	负值字符范围。匹配任何不在指定范围内的任意字符。例如，’[^a-z]‘ 可以匹配任何不在 ‘a’ 到 ‘z’ 范围内的任意字符。
\cx 	匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。 x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 ‘c’ 字符。
\d 	匹配一个数字字符。等价于 [0-9]。
\D 	匹配一个非数字字符。等价于 [^0-9]。
\w 	匹配包括下划线的任何单词字符。等价于’[A-Za-z0-9_]‘。
\W 	匹配任何非单词字符。等价于 ‘[^A-Za-z0-9_]‘。
\xn 	匹配 n，其中 n 为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如， ‘\x41′ 匹配 “A”。’\x041′ 则等价于 ‘\x04′ & “1″。正则表达式中可以使用 ASCII 编码。.
\num 	匹配 num，其中 num 是一个正整数。对所获取的匹配的引用。例如，’(.)\1′ 匹配两个连续的相同字符。
\n 	标识一个八进制转义值或一个后向引用。如果 \n 之前至少 n 个获取的子表达式，则 n 为后向引用。否则，如果 n 为八进制数字 (0-7)，则n 为一个八进制转义值。
\nm 	标识一个八进制转义值或一个后向引用。如果 \nm 之前至少有is preceded by at least nm 个获取得子表达式，则 nm 为后向引用。如果 \nm 之前至少有 n 个获取，则 n 为一个后跟文字 m 的后向引用。如果前面的条件都不满足，若? n 和 m 均为八进制数字 (0-7)，则 \nm将匹配八进制转义值 nm。
\nml 	如果 n 为八进制数字 (0-3)，且 m 和 l 均为八进制数字 (0-7)，则匹配八进制转义值 nml。
\un 	匹配 n，其中 n 是一个用四个十六进制数字表示的 Unicode 字符。例如， \u00A9 匹配版权符号 (?)。
{% endhighlight %}

特殊字符
{% highlight ruby %}
$ 	匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 ‘\n’ 或 ‘\r’。要匹配 $ 字符本身，请使用 \$。
( ) 	标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用 \( 和 \)。
* 	匹配前面的子表达式零次或多次。要匹配 * 字符，请使用 \*。
+ 	匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 +。
. 	匹配除换行符 \n之外的任何单字符。要匹配 .，请使用 \。
[ 	标记一个中括号表达式的开始。要匹配 [，请使用 \[。
? 	匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 \?。
\ 	将下一个字符标记为或特殊字符、或原义字符、或后向引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\' 匹配 "\"，而 '\(' 则匹配 "("。
^ 	匹配输入字符串的开始位置，除非在方括号表达式中使用，此时它表示不接受该字符集合。要匹配 ^ 字符本身，请使用\^。
{ 	标记限定符表达式的开始。要匹配 {，请使用 \{。
| 	指明两项之间的一个选择。要匹配 |，请使用 \|。
{% endhighlight %}

<!-- more -->

非打印字符
{% highlight ruby %}
\cx 	匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。 x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。
\f 	匹配一个换页符。等价于 \x0c 和 \cL。
\n 	匹配一个换行符。等价于 x0a 和 cJ。
\r 	匹配一个回车符。等价于 x0d 和 cM。
\s 	匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [?\f\n\r\t\v]。
\S 	匹配任何非空白字符。等价于 [^?\f\n\r\t\v]。
\t 	匹配一个制表符。等价于 \x09 和 \cI。
\v 	匹配一个垂直制表符。等价于 \x0b 和 \cK。
{% endhighlight %}

限定符
{% highlight ruby %}
* 	匹配前面的子表达式零次或多次。例如，zo* 能匹配 “z” 以及 “zoo”。 * 等价于{0,}。
+ 	匹配前面的子表达式一次或多次。例如，’zo+’ 能匹配 “zo” 以及 “zoo”，但不能匹配 “z”。+ 等价于 {1,}。
? 	匹配前面的子表达式零次或一次。例如，”do(es)?” 可以匹配 “do” 或 “does” 中的”do” 。? 等价于 {0,1}。
{n} 	n 是一个非负整数。匹配确定的 n 次。例如，’o{2}’ 不能匹配 “Bob” 中的 ‘o’，但是能匹配 “food” 中的两个 o。
{n,} 	n 是一个非负整数。至少匹配n 次。例如，’o{2,}’ 不能匹配 “Bob” 中的 ‘o’，但能匹配 “foooood” 中的所有 o。’o{1,}’ 等价于 ‘o+’。’o{0,}’ 则等价于 ‘o*’。
{n,m} 	m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。刘， “o{1,3}” 将匹配 “fooooood” 中的前三个 o。’o{0,1}’ 等价于 ‘o?’。请注意在逗号和两个数之间不能有空格。
{% endhighlight %}

定位符
{% highlight ruby %}
^ 	匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 ‘\n’ 或 ‘\r’ 之后的位置。
$ 	匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 ‘\n’ 或 ‘\r’ 之前的位置。
\b 	匹配一个单词边界，也就是指单词和空格间的位置。
\B 	匹配非单词边界。
{% endhighlight %}

>是否有匹配

regexpObject.test(string)

返回值为Boolean型

    var re = new RegExp(/\bbe\b/g);
    var str = "To be, or not to be:That is the question:";
    alert(str.search(re));

string.search(regexpObject)

返回匹配字符的位置，无匹配返回-1

    var re = new RegExp(/\bbe\b/g);
    var str = "To be, or not to be:That is the question:";
    alert(re.test(str));

取得正则匹配信息

regexpObject.exec(string)

    var re = new RegExp(/be/g);
    var str = "To be, or not to be:That is the question:";
    var f;
    do
    {
        f = re.exec(str);
        alert(f + ":" + f.index);
    } while (f!=null);

使用正则表达式进行字符串替换

string.replace(re, replaceString)

    var re = new RegExp(/be/g);
    var str = "To be, or not to be:That is the question:";
    alert(str.replace(re, "*"));



