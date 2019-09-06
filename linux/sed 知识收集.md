# sed 知识收集

[TOC]

# sed 替换 ip 端口号

[sed, a stream editor](http://www.gnu.org/software/sed/manual/sed.html) - 官网文档

正则表达式，在 sed 中的转译用法

> The only difference between basic and extended regular expressions is in the behavior of a few characters: ‘?’, ‘+’, parentheses, braces (‘{}’), and ‘|’. While basic regular expressions require these to be escaped if you want them to behave as special characters, when using extended regular expressions you must escape them if you want them *to match a literal character*. ‘|’ is special here because ‘\|’ is a GNU extension – standard basic regular expressions do not provide its functionality.
>
> Examples:
>
> - `abc?`
>
>   becomes ‘abc\?’ when using extended regular expressions. It matches the literal string ‘abc?’.
>
> - `c\+`
>
>   becomes ‘c+’ when using extended regular expressions. It matches one or more ‘c’s.
>
> - `a\{3,\}`
>
>   becomes ‘a{3,}’ when using extended regular expressions. It matches three or more ‘a’s.
>
> - `\(abc\)\{2,3\}`
>
>   becomes ‘(abc){2,3}’ when using extended regular expressions. It matches either ‘abcabc’ or ‘abcabcabc’.
>
> - `\(abc*\)\1`
>
>   becomes ‘(abc*)\1’ when using extended regular expressions. Backreferences must still be escaped when using extended regular expressions.
>
> - `a\|b`
>
>   becomes ‘a|b’ when using extended regular expressions. It matches ‘a’ or ‘b’.

sed 替换 ip 端口号

```sh
echo 10.168.1.192:521 | sed 's#[0-9.]\+:[0-9]\{1,4\}#XXX#'
```

