title: 使用 reverse() 將 Array 顛倒
tags:
  - Ramda
  - Ramda/reverse()
feature: images/feature/ramda.png
date: 2019-05-18 17:08:27
---
將 String 或 Array 前後顛倒在實務上可能不太常見，不過用來練功倒是不錯。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.209
Ramda 0.26.1

## ECMAScript

```javascript
let data = 'sam';

// reverse :: String -> String
let reverse = str => str.split('').reverse().join('');

console.log(reverse(data));
```

string 沒有 `reverse()`，但 array 有：

* 先用 `split()` 轉成 array
* 使用 array 的 `reverse()`
* 最後再使用 `join()` 還原成 string

![reverse002](/images/ramda/reverse/reverse002.png)

## reverse()

```javascript
import { reverse } from 'ramda';

let data = 'sam';

console.log(reverse(data));
```

事實上 Ramda 的 `reverse()`，除了用於 array 外，也可以用在 string。

> **reverse()**
> `[a] -> [a]`
> `String -> String`
> 將 array 或 string 顛倒

![reverse000](/images/ramda/reverse/reverse000.png)

> Q：為什麼 `reverse()` 這麼神，可同時處理 array 或 string ?

從 [reverse() source code](https://github.com/ramda/ramda/blob/v0.26.1/source/reverse.js) 發現，其實一點也不神。

```javascript
var reverse = _curry1(function reverse(list) {
  return _isString(list)
    ? list.split('').reverse().join('')
    : Array.prototype.slice.call(list, 0).reverse();
});
```

Ramda 先判斷是否為 string，若是 string 一樣走 `spilt()`、`reverse()`、`join()` 流程；若是 array，則先 `slice()` 一份新的 array，再 `reverse()` 之，因為 ECMAScript 內建的 `reverse()` 是直接改變原有 array，屬 side effect 寫法。

> Ramda 的 `reverse()` 雖然同時支援 array 與 string，實則經過 `_isString()` 判斷而分別處理

## Conclusion

* `split()` 與 `join()` 是常見的組合，`split()` 將 string 轉成 array 後，可借用 array 豐富的 function 處理，最後再使用 `join()` 還原成 string，可視為 pattern 使用
* 別忘了 Ramda 的 `reverse()` 除了支援 array 外，也支援 string

## Reference

[Samantha Ming](https://medium.com/@samanthaming), [How to Reverse a String in JavaScript ?](https://medium.com/@samanthaming/how-to-reverse-a-string-in-javascript-pictorial-b630a3480200)
[Ramda](https://ramdajs.com), [reverse()](https://ramdajs.com/docs/#reverse)

