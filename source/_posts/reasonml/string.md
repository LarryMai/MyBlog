title: ReasonML 之 String
tags:
  - ReasonML
feature: images/feature/reasonml.png
date: 2019-05-28 10:05:56
---
ReasonML 是 Static Type Language，因此 Type 特別重要，我們就從最基本的 String 談起。

<!-- more -->

## Version

ReasonML 3.0.4
BuckleScript 3.1.0

## Double Quote

```ocaml
let name = "Sam";

name |> Js.log;
```

string 要使用 double quote， `name` 不必指定 type，當對 `name` 做 let binding 時，會自動對 `name` type inference 為 string。

> 在 ECMAScript，string 可用 single quote，也可以用 double quote；但 ReasonML 的 string 只能用 double quote，single quote 是 char 所用

![string000](/images/reasonml/string/string000.png)

## Escape Character

```ocaml
let name = "Sam\n Xiao";

name |> Js.log;
```

特殊字元必須使用 `\` 加以 escape，如 `\n` (換行)。

> 與 ECMAScript 相同

![string001](/images/reasonml/string/string001.png)

## String Concatenation

```javascript
let firstName = "Sam";
let lastName = "Xiao";
let fullName = firstName ++ " " ++ lastName;

fullName |> Js.log;
```

string 相加時，使用 `++`。

> ECMAScript 使用 `+`

![string002](/images/reasonml/string/string002.png)

> Q：為什麼 ReasonML 要另外為 string 提供 `++` ?

因為 ReasonML 支援 type inference，看到 `++` 時，就能推導出 parameter 的 type 是 string，且回傳也是 string。

## Multiline String

```ocaml
let greeting = "
  Hello World
  Sam Xiao
";

greeting |> Js.log;
```

若要將  string 以多行顯示，直接以 double quote 刮起來，換行不必加上 escape character， ReasonML 會自動加上 `\n` 換行。


![string007](/images/reasonml/string/string007.png)

```ocaml
let greeting = {|
  Hello World
  Sam Xiao
|};

greeting |> Js.log;
```


若要將  string 以多行顯示，也可以使用 `{|` 與 `|}` 刮起來，換行不必加上 escape character， ReasonML 會自動加上 `\n` 換行。

> 相當於 ES6 的 template string 使用 backtick 支援多行 string

![string003](/images/reasonml/string/string003.png)

## Interpolation

```ocaml
let firstName = "Sam";
let lastName = "Xiao";
let fullName = {j|$firstName $lastName|j};

fullName |> Js.log;
```

ReasonML 目前並沒有支援 interpolation，只能使用 concatenation。

但若要使用 interpolation，必須依賴 BuckleScript，可使用 `{j|` 與 `|j}` 刮起來，binding 名稱前需加上 `$`。

當 BuckleScript 編譯成 ECMAScript 時，看到 `{j|}` 與 `|j}`，會啟動執行 interpolation 取代 binding。

> ES6 的 template string 已經支援 interpolation，這點優於 ReasonML

![string004](/images/reasonml/string/string004.png)

## Unicode

```ocaml
let greeting = {j|嗨 世界|j};
let greeting2 = {js|嗨 台灣|js};

greeting |> Js.log;
greeting2 |> Js.log;
```

ReasonML 目前只支援 UTF-8，尚未如 ECMAScript 支援 UTF-16，必須得在 `{j|} {|j}` 或 `{js|} {|js}` 內使用 UTF-16，BuckleScript 會加以處理。

> ECMAScript 原生支援 UTF-16，這點優於 ReasonML

![string005](/images/reasonml/string/string005.png)

## String Library

```ocaml
let greeting = "Hello World!";

greeting |> String.length |> Js.log;
greeting |> Js.String.length |> Js.log;
```

ReasonML 共有兩套 string library 可用，其中 `String` module 為 ReasonML 所提供，而 `Js.String` module 為 BuckleScript 所提供。

> ECMAScript 提供 `String.prototype`

![string006](/images/reasonml/string/string006.png)

## Conclusion

* ReasonML 的 `string` 使用 double quote；ECMAScript 使用 single quote 或 double quoute 皆可
* ReasonML 與 ECMAScript 的 escape character 都使用 `\`
* ReasonML 的 string concatenation 使用 `++`；ECMAScript 使用 `+`
* ReasonML 的 multiline string 可直接使用 double quote；ECMAScript 要使用 template string 的 backtick
* ReasonML 不支援 interpolation，必須使用 BuckleScript 的  `{j|}` 與 `|j}`；ECMAScript 的 template string 直接支援 interpolation
* ReasonML 不支援 Unicode，必須使用 BuckleScript 的  `{j|} {|j}` 或 `{js|} {|js}`；ECMAScript 原生支援 Unicode
* ReasonML 有 `String` 與 `Js.String` 兩套 module 可用；ECMAScript 有 `String.prototype` 

## Reference

[ReasonML](https://reasonml.github.io/en/), [String & Char](https://reasonml.github.io/docs/en/string-and-char)
[Dr.Axel Rauschmayer](https://twitter.com/rauschma), [Exploring ReasonML and functional programming](http://reasonmlhub.com/exploring-reasonml/ch_basic-types.html#strings)
[Nick Graf](https://egghead.io/instructors/nik-graf), [Basic Datatypes and Operators in ReasonML](https://egghead.io/lessons/reason-basic-datatypes-and-operators-in-reason)