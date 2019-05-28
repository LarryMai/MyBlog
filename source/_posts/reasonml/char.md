title: ReasonML 之 Char
tags:
  - ReasonML
feature: images/feature/reasonml.png
date: 2019-05-28 11:36:28
---
ECMAScript 並沒有 char，但 ReasonML 如 C 語言有獨立出 char。

<!-- more -->

## Version

ReasonML 3.0.4
BuckleScript 3.1.0

## Single Quote

```ocaml
let chr = 'a';

chr |> Js.log;
```

char 要使用 single quote，chr 不必指定 type，當對 chr 做 let binding 時，會自動對 chr type inference 為 char。

當對 char 直接 `Js.log()` 時，只會顯示其 ASCII，不會顯示 character。

> ECMAScript 並沒有 char，只有 string，也因此 ECMAScript 的 string 可用 single quote，也可以用 double quote

![char000](/images/reasonml/char/char000.png)

## Char to String

```ocaml
let chr = 'a';

String.make(1, chr) |> Js.log;
```

`Js.log()` 無法顯示 char，必須使用 `String.make()` 將 char 轉成 string 才能顯示。

> **String.make()**
> `(int, char) => string`
> 由 char 產生 string

`int`：產生 n 個 char

`char`：指定 character

`string`：回傳 string

![char001](/images/reasonml/char/char001.png)

## String to Char

```javascript
let name = "Sam";

name.[0] |> String.make(1) |> Js.log;
String.get(name, 0) |> String.make(1) |> Js.log;
```

若要從 string 取出一部分的 char，有兩種方式：

* 使用 `.[n]` 取出 char
* 使用 `String.get()` 取出 char

> **String.get()**
> `(string, int) => char`
> 由 string 取出該 index 的 char

`string`：data 為 string

`int`：指定 index

`char`：回傳 char

![char002](/images/reasonml/char/char002.png)

## Conclusion

* ECMAScript 並沒有 char，只有 string
* Single quote 留給 char 使用，而 double quote 只能用在 string
* 若直接使用 `Js.log()` 印出 char，只會印出其 ASCII，必須靠 `String.make()` 轉成 string 才會印出 character
* 可使用 `.[n]` 由 string 取出部分 char

## Reference

[ReasonML](https://reasonml.github.io/en/), [String & Char](https://reasonml.github.io/docs/en/string-and-char)
[Dr.Axel Rauschmayer](https://twitter.com/rauschma), [Exploring ReasonML and functional programming](http://reasonmlhub.com/exploring-reasonml/ch_basic-types.html#characters)
[Nick Graf](https://egghead.io/instructors/nik-graf), [Basic Datatypes and Operators in ReasonML](https://egghead.io/lessons/reason-basic-datatypes-and-operators-in-reason)
[ReasonML](https://reasonml.github.io/en/), [String.make()](https://reasonml.github.io/api/String.html)
[ReasonML](https://reasonml.github.io/en/), [String.get()](https://reasonml.github.io/api/String.html)