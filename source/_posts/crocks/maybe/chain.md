title: 使用 chain() 將兩層 Maybe 攤平
tags:
  - Crocks
  - Maybe/map
  - Maybe/chain
feature: images/feature/crocks.png
date: 2019-05-28 21:08:25
---
Function 若接收其他 Function 傳來的 `Maybe`，而又想使用 `map()` 對該 `Maybe` 內的值加以改變，很容易寫出兩層 Maybe，藉由 `chain()` 取代 `map()`，可將兩層 Maybe 攤平。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Crocks 0.11.1

## map()

```javascript
import { prop, propPath } from 'crocks';

// fn :: unit -> Maybe Object
let getBody = () => {
  let result = {
    status: 200,
    body: {
      id: 1,
      userName: 'Sam',
      email: 'oomusou@gmail.com',
      address: {
        street: '111 E. West Rd.',
        city: 'Taipei',
        postalCode: '11101'
      }
    }
  };

  return prop('body', result);
};

// fn :: unit -> Maybe Maybe String
let fn = () => getBody().map(propPath(['address', 'postalCode']));

console.dir(fn());
```

第 3 行

```javascript
// fn :: unit -> Maybe 
let getBody = () => {
  let result = {
    ...
  };

  return prop('body', result);
};
```

由於 `getBody()` 使用了 Crocks 的 `prop()` 取得 `body` property，因此回傳為 `Maybe`。

22 行

```javascript
// fn :: unit -> Maybe 
let fn = () => getBody().map(propPath(['address', 'postalCode']));
```

由於 `getBody()` 回傳為 `Maybe`，`fn()` 為了改變 `Maybe` 內部的值，只想取得 `postalCode` property 即可，因此繼續使用 `map()`，其 callback 使用了 `propPath()` 取得 `address.postalCode`。

因為 `propPath()` 也是回傳 `Maybe`，因此 `fn()` 變成回傳兩層 `Maybe`。

![chain000](/images/crocks/maybe/chain/chain000.png)

## chain()

```javascript
import { prop, propPath } from 'crocks';

// fn :: unit -> Maybe Object
let getBody = () => {
  let result = {
    status: 200,
    body: {
      id: 1,
      userName: 'Sam',
      email: 'oomusou@gmail.com',
      address: {
        street: '111 E. West Rd.',
        city: 'Taipei',
        postalCode: '11101'
      }
    }
  };

  return prop('body', result);
};

// fn :: unit -> Maybe String
let fn = () => getBody().chain(propPath(['address', 'postalCode']));
  
console.dir(fn());
```

22 行

```javascript
// fn :: unit -> Maybe String
let fn = () => getBody().chain(propPath(['address', 'postalCode']));
```

> **chain()**
> `Maybe a ~> (a -> Maybe b) -> Maybe b`
> 將兩層 `Maybe` 打層一層 `Maybe`

與 `map()` 比較一下：

> **map()**
> `Maybe a ~> (a -> b) -> Maybe b`
> 傳入 function 直接修改 `Maybe` 內的值

可以發現 `chain()` 與 `map()` 的差異在於傳入的 callback：

* `map()` 為 `a -> b`，故直接修改 `Maybe` 內的值
* `chain()` 為 `a -> Maybe b`，故可能產生兩層 `Maybe`

而最後 `chain()` 會將兩層 `Maybe` 攤平成一層 `Maybe`。

`Maybe a`：data 為 `Maybe`

`(a -> b)`：傳入 `a -> Maybe b` 的 function

`Maybe b`：將 `Maybe Maybe b` 轉成 `Maybe b`


![chain001](/images/crocks/maybe/chain/chain001.png)

## option()

```javascript
import { prop, propPath } from 'crocks';

// fn :: unit -> Maybe Object
let getBody = () => {
  let result = {
    status: 200,
    body: {
      id: 1,
      userName: 'Sam',
      email: 'oomusou@gmail.com',
      address: {
        street: '111 E. West Rd.',
        city: 'Taipei',
        postalCode: '11101'
      }
    }
  };

  return prop('body', result);
};

// fn :: unit -> Maybe String
let fn = () => getBody().chain(propPath(['address', 'postalCode']));
  
console.log(fn().option('N/A'));
```

25 行

```javascript
console.log(fn().option('N/A'));
```

既然是一層 `Maybe`，就能簡單用 `option()` 將 `Maybe` 轉回 string 了。

![chain002](/images/crocks/maybe/chain/chain002.png)

## Conclusion

* 實務上很容易遇到兩層 `Maybe`，別忘了使用 `chain()` 取代 `map()`，將兩層 `Maybe` 攤平

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Flatten Nested Maybes with chain()](https://egghead.io/lessons/javascript-flatten-nested-maybes-with-chain)
[Crocks](https://evilsoft.github.io/crocks/), [chain()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#chain)