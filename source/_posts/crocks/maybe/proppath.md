title: 使用 propPath() 取得 Nested Object 的 Property
tags:
  - Crocks
  - Ramda
  - Crocks/propPath
  - Ramda/path
feature: images/feature/crocks.png
date: 2019-05-27 21:12:19
---
Ramda 的 `path()` 可能回傳 `undefined`，這也是常見 Bug 來源之一；而 Crocks 的 `propPath()` 則回傳 `Maybe`，可確保 ECMAScript 不再回傳不預期結果。 

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

## ECMAScript

```javascript
let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = obj => obj.address.postalCode;

console.log(fn(user));
```

`user` 為 nested object，若想取得 `user.address.postalCode`，可直接使用 `.` 取得。

> 若傳入的 object 真的有 `address` object 與 `postalCode` property，則一切順利如預期

![proppath000](/images/crocks/maybe/proppath/proppath000.png)

```javascript
let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    _postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = obj => obj.address.postalCode;

console.log(fn(user));
```

若傳入 object 不存在 `postalCode` property，則會回傳 `undefined`，這就是常見 bug 來源。

![proppath001](/images/crocks/maybe/proppath/proppath001.png)

```javascript
let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    _postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = obj => obj.address.postalCode || 'N/A';

console.log(fn(user));
```

常見寫法會使用 `||` 判斷，若 `postalCode` 不存在則顯示其他 string。

這種寫法雖然可行，但有幾點必須注意：

* `||` 是利用 falsy value 判斷，`undefined` 固然是 falsy value，但 empty string 也是 falsy value，若需求是要顯示 empty string 的話，這種寫法就會誤判造成 bug
* 要時時小心 property 可能不存在加上 `||` 判斷，常因為粗心而忘記使用 `||` 造成 bug
* `||` 並非商業邏輯一部分，只是為了防止 property 不存在而判斷，因此將 code 變髒了


![proppath002](/images/crocks/maybe/proppath/proppath002.png)

```javascript
let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  _address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = obj => obj.address.postalCode || 'N/A';

console.log(fn(user));
```

`address` object 也可能不存在，因此使用 `||` 判斷並不夠完整，依然產生 `Cannot read property of undefined` 的 runtime 錯誤。

![proppath003](/images/crocks/maybe/proppath/proppath003.png)

```javascript
let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  _address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    _postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = obj => (obj.address && obj.address.postalCode !== undefined) ? obj.address.postalCode : 'N/A';

console.log(fn(user));
```

最完整的判斷需同時加上 `obj.address` 與 `obj.address.postalCode !== undefined` 雙重判斷，若都成立才能使用 `obj.address.postalCode` 取值，否則都回傳 `N/A`。

這種寫法雖然可行，但有幾點必須注意：

* 若 nested object 很深，則判斷會很驚人
* 必須很小心的每一層都判斷，常因為粗心而造成 bug
* 這些判斷並非商業邏輯的一部分，只是為了防止 property 不存在而已，因此將 code 變髒了

![proppath004](/images/crocks/maybe/proppath/proppath004.png)

## Ramda

```javascript
import { path } from 'ramda';

let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  _address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    _postalCode: '11101'
  }
};

// fn :: Object -> String
let fn = path([
  'address',
  'postalCode'
]);

console.log(fn(user));
```

> **path()**
> `[Idx] -> {a} -> a | Undefined`
> `Idx = String | Int`
>
> 針對 nested object 取得其 property 值

若 property 找不到，`path()` 會回傳 `undefined`，所以儘管使用了 Ramda 的 `path()`，問題依舊沒解決，一樣是 `undefined`。

![proppath005](/images/crocks/maybe/proppath/proppath005.png)

## Crocks

```javascript
import { propPath } from 'crocks';

let user = {
  userName: 'Sam',
  email: 'oomusou@gmail.com',
  address: {
    street: '111 E. West Rd.',
    city: 'Taipei',
    postalCode: '11101'
  }
};

// fn :: Object -> Maybe String
let fn = propPath([
  'address',
  'postalCode'
]);

console.log(fn(user).option('N/A'));
```

Crocks 提供了 `propPath()`，用法與 Ramda 的 `path()` 完全相同，但回傳的是 `Maybe`。

> **propPath()**
> `Foldable f => f (String | Integer) -> a -> Maybe b`
> 針對 nested object 取得其 property 值，但回傳為 `Maybe`

`Foldable f => f (String | Integer)`：以 array 傳入 property

`a`：data 為 object 或 array

`Maybe b`：回傳 object 的 value 或 array 的 element，但會包成 `Maybe`

由於 `propPath()` 回傳為 `Maybe`，需使用 `option()` 轉回 string，並順便處理 `undefined`。

使用了 `Maybe` 後，我們發現：

* 無論 nested object 有多深，我們都不須擔心 object 或 property 不存在；也不用擔心 falsy value 判斷有潛在 bug
* 不用擔心因粗心而造成 bug，只要記得 nested object 使用 `propPath()` 即可
* Function 內不再有判斷 property 存在與否邏輯，只剩下原本商業邏輯，非常乾淨
* 因為 `Maybe` 一定要透過 `option()` 取出，不會忘記處理 `undefined`

![proppath006](/images/crocks/maybe/proppath/proppath006.png)

## Conclusion

* Ramda 與 ECMAScript 妥協，有些 function 回傳 `undefined`，如 `prop()` 與 `path()`，這很容易產生 bug
* Crocks 的 `propPath()` 除了用在 object，也能用在 array
* Crocks 的 `propPath()` 回傳的是 `Maybe`，強迫我們要透過 `option()` 處理 `undefined`，實務上建議使用 Crocks 的 `propPath()` 取代 Ramda 的 `path()`

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Safely Access Nested Object Properties with propPath()](https://egghead.io/lessons/javascript-safely-access-nested-object-properties-with-proppath)
[Ramda](https://ramdajs.com), [path()](https://ramdajs.com/docs/#path)
[Crocks](https://evilsoft.github.io/crocks/), [propPath()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#proppath)

