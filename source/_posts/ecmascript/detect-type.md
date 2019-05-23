title: 如何判斷 ECMAScript 變數型別 ?
tags:
  - ECMAScript
feature: images/feature/ecmascript.png
date: 2019-05-22 15:34:43
---
ECMAScript 雖然是 Dynamic Language，但並不代表 Variable 沒有 Type，只是其內建獲得 Type 的方式包含太多 `驚喜`，成為備受爭議的部分。本文整理出 4 種獲得 Type 方式，各有其優缺點，最後整理出自己的 `typeof()`，可以判斷出各種 Type。

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
ECMAScript 5
Ramda 0.26.1

## Type

本文將介紹 5 種獲得 Type 方法：

1. 內建的 `typeof` operator
2. 內建的 `instanceof` operator
3. 內建的 `Object.prototype.toString()` function
4. Ramda 的 `is()` function
5. 自己寫的 `typeof()`

## Number

Number 共有 3 種建立方法：

1. Number literal
2. Number function
3. Number constructor

實務上最常使用的是的 number literal，轉型時會使用 number function (或使用 `+` operator)，但 number constructor 實務上不建議使用。

```javascript
import { is } from 'ramda';

/** typeof */
console.log(typeof 1);
console.log(typeof Number(1));
console.log(typeof new Number(1));

/** instanceof */
console.log(1 instanceof Number);
console.log(Number(1) instanceof Number);
console.log(new Number(1) instanceof Number);

/** Object.prototype.toString() */
console.log(Object.prototype.toString.call(1));
console.log(Object.prototype.toString.call(Number(1)));
console.log(Object.prototype.toString.call(new Number(1)));

/** is() */
console.log(is(Number, 1));
console.log(is(Number, Number(1)));
console.log(is(Number, new Number(1)));
```

**typeof**

* 對於 literal 或 function 所產生的 number 判斷如預期，會回傳 `number`
* 對於 constructor 所產生的 number，會回傳 `object`，這是比較困擾的地方

**instanceof**

* 對於 literal 或 function 所產生的 number 判斷如預期，會回傳 `false`
* 對於 constructor 所產生的 number 判斷如預期，會回傳 `true`

> `instanceof` 的邏輯理論上沒錯，但使得 `typeof` 與 `instanceof` 都有其不足，無法同時判斷 literal、function 與 constructor 所建立的 number，實務上並不好用

**Object.prototype.toString()**

* 無論對於 literal、function、constructor 所產生的 number，都回傳 `number`

**Ramda 之 is()**

* 無論對於 literal、function、constructor 所產生的 number，都回傳 `true`

> `Object.prototype.toString()` 與 `is()` 都能同時判斷 literal、function 與 constructor 所產生的 number，實務上比較好用

![type000](/images/ecmascript/detect-type/type000.png)

## String

String 共有 3 種建立方法：

1. String literal
2. String function
3. String constructor

實務上最常使用的是的 string literal，轉型時會使用 string function (或使用 `+ ''` operator)，但 string constructor 實務上不建議使用。

```javascript
import { is } from 'ramda';

/** typeof */
console.log(typeof 'abc');
console.log(typeof String('abc'));
console.log(typeof new String('abc'));

/** instanceof */
console.log('abc' instanceof String);
console.log(String('abc') instanceof String);
console.log(new String('abc') instanceof String);

/** Object.prototype.toString() */
console.log(Object.prototype.toString.call('abc'));
console.log(Object.prototype.toString.call(String('abc')));
console.log(Object.prototype.toString.call(new String('abc')));

/** is() */
console.log(is(String, 'abc'));
console.log(is(String, String('abc')));
console.log(is(String, new String('abc')));
```

**typeof**

- 對於 literal 或 function 所產生的 string 判斷如預期，會回傳 `string`
- 對於 constructor 所產生的 string，會回傳 `object`，這是比較困擾的地方

**instanceof**

- 對於 literal 或 function 所產生的 string 判斷如預期，會回傳 `false`
- 對於 constructor 所產生的 string 判斷如預期，會回傳 `true`

> `instanceof` 的邏輯理論上沒錯，但使得 `typeof` 與 `instanceof` 都有其不足，無法同時判斷 literal、function 與 constructor 所建立的 string，實務上並不好用

**Object.prototype.toString()**

- 無論對於 literal、function、constructor 所產生的 string，都回傳 `string`

**Ramda 之 is()**

- 無論對於 literal、function、constructor 所產生的 string，都回傳 `true`

> `Object.prototype.toString()` 與 `is()` 都能同時判斷 literal、function 與 constructor 所產生的 string，實務上比較好用

![type001](/images/ecmascript/detect-type/type001.png)

## Boolean

Boolean 共有 3 種建立方法：

1. Boolean literal
2. Boolean function
3. Boolean constructor

實務上最常使用的是的 boolean literal，轉型時會使用 boolean function (或使用 `!!` operator，且 ECMAScript 支援 boolean coercion，會自動轉型)，但 boolean constructor 實務上不建議使用。

```javascript
import { is } from 'ramda';

console.log(typeof true);
console.log(typeof Boolean(true));
console.log(typeof new Boolean(true));

console.log(true instanceof Boolean);
console.log(Boolean(true) instanceof Boolean);
console.log(new Boolean(true) instanceof Boolean);

console.log(Object.prototype.toString.call(true));
console.log(Object.prototype.toString.call(Boolean(true)));
console.log(Object.prototype.toString.call(new Boolean(true)));

console.log(is(Boolean, true));
console.log(is(Boolean, Boolean(true)));
console.log(is(Boolean, new Boolean(true)));
```

**typeof**

- 對於 literal 或 function 所產生的 boolean 判斷如預期，會回傳 `boolean`
- 對於 constructor 所產生的 boolean，會回傳 `object`，這是比較困擾的地方

**instanceof**

- 對於 literal 或 function 所產生的 boolean 判斷如預期，會回傳 `false`
- 對於 constructor 所產生的 boolean 判斷如預期，會回傳 `true`

> `instanceof` 的邏輯理論上沒錯，但使得 `typeof` 與 `instanceof` 都有其不足，無法同時判斷 literal、function 與 constructor 所建立的 boolean，實務上並不好用

**Object.prototype.toString()**

- 無論對於 literal、function、constructor 所產生的 boolen，都回傳 `boolean`

**Ramda 之 is()**

- 無論對於 literal、function、constructor 所產生的 boolean，都回傳 `true`

> `Object.prototype.toString()` 與 `is()` 都能同時判斷 literal、function 與 constructor 所產生的 boolean，實務上比較好用

![type002](/images/ecmascript/detect-type/type002.png)

## Undefined

`undefined` 在 ECMAScript 已獨立成一個 type，其值也是 `undefined`。

```javascript
import { isNil } from 'ramda';

/** typeof */
console.log(typeof undefined);

/** instanceof */
// N/A

/** Object.prototype.toString() */
console.log(Object.prototype.toString.call(undefined));

/** isNil() */
console.log(isNil(undefined));
```

**typeof**

- 判斷 `undefined` 會如預期回傳 `undefined`

**instanceof**

* 無法判斷 `undefined`，因為不存在 undefined constructor

**Object.prototype.toString()**

* 判斷 `undefined` 會如預期回傳 `undefined`

**isNil()**

* `is()` 無法判斷 `undefined`，要改用 `isNil()`，如預期回傳 `true`

> Ramda 使用 `isNil()` 使得 type 判斷分裂成兩個 function，實務上沒那麼好用，目前只有 `Object.prototype.toString()` 能達到單一 function 判斷所有 type

![type003](/images/ecmascript/detect-type/type003.png)

## Null

`null` 在 ECMAScript 已獨立成一個 type，其值也是 `null`。

```javascript
import { isNil } from 'ramda';

/** typeof */
console.log(typeof null);

/** instanceof */
// N/A

/** Object.prototype.toString() */
console.log(Object.prototype.toString.call(null));

/** isNil() */
console.log(isNil(null));
```

**typeof**

* 判斷 `null` 會如預期回傳 `object`，這是已知的 bug，但因為歷史因素，無法修改，變成 feature

**instanceof**

- 無法判斷 `null`，因為不存在 null constructor

**Object.prototype.toString()**

- 判斷 `null` 會如預期回傳 `null`

**isNil()**

- `is()` 無法判斷 `null`，要改用 `isNil()`，如預期回傳 `true`

> Ramda 使用 `isNil()` 使得 type 判斷分裂成兩個 function，實務上沒那麼好用，目前只有 `Object.prototype.toString()` 能達到單一 function 判斷所有 type

![type004](/images/ecmascript/detect-type/type004.png)

## Object

Object 共有 5 種建立方法：

1. Object literal
2. Object constructor
3. Constructor function
4. Class
5. Object.create()

實務上最常使用的是的 object literal，而 constructor function、class 與 `Object.create()` 都有其適用時機，但 object constructor 實務上不建議使用。

```javascript
/** object literal */
console.log(typeof {});

/** object constructor */
console.log(typeof new Object());

/** constructor function */
function Foo1() {}
console.log(typeof new Foo1());

/** class */
class Foo2 {}
console.log(typeof new Foo2());

/** Object.create() */
console.log(typeof Object.create({}));
```

**typeof**

* 無論使用哪一種方式建立 object，都會如預期回傳 `object`

![type005](/images/ecmascript/detect-type/type005.png)

```javascript
/** object literal */
console.log({} instanceof Object);

/** object constructor */
console.log(new Object() instanceof Object);

/** constructor function */
function Foo1() {}
console.log(new Foo1() instanceof Object);

/** class */
class Foo2 {}
console.log(new Foo2() instanceof Object);

/** Object.create() */
console.log(Object.create({}) instanceof Object);
```

**instanceof**

* 無論使用哪一種方式建立 object，都會如預期回傳 `true`

![type006](/images/ecmascript/detect-type/type006.png)

```javascript
/** object literal */
console.log(Object.prototype.toString.call({}));

/** object constructor */
console.log(Object.prototype.toString.call(new Object()));

/** constructor function */
function Foo1() {}
console.log(Object.prototype.toString.call(new Foo1()));

/** class */
class Foo2 {}
console.log(Object.prototype.toString.call(new Foo2()));

/** Object.create() */
console.log(Object.prototype.toString.call(Object.create({})));
```

**Object.prototype.toString()**

* 無論使用哪一種方式建立 object，都會如預期回傳 `object`

![type007](/images/ecmascript/detect-type/type007.png)

```javascript
import { is } from 'ramda';

/** object literal */
console.log(is(Object, {}));

/** object constructor */
console.log(is(Object, new Object()));

/** constructor function */
function Foo1() {}
console.log(is(Object, new Foo1()));

/** class */
class Foo2 {}
console.log(is(Object, new Foo2()));

/** Object.create() */
console.log(is(Object, Object.create({})));
```

**is()**

* 無論使用哪一種方式建立 object，都會如預期回傳 `true`

> 對於 object，無論使用 `typeof`、`instanceof`、`Object.prototype.toString()` 或 `is()`，都可以如預期判斷出 object

![type008](/images/ecmascript/detect-type/type008.png)

## Array

Array 共有 3 種建立方法：

1. Array literal
2. Array function
3. Array constructor

實務上最常使用的是的 array literal，建立 empty array 時會使用 array function，但 array constructor 實務上不建議使用 (因為效果與 array function 一樣)。

```javascript
import { is } from 'ramda';

/** typeof */
console.log(typeof []);
console.log(typeof Array(0));
console.log(typeof new Array(0));

/** instanceof */
console.log([] instanceof Array);
console.log(Array(0) instanceof Array);
console.log(new Array(0) instanceof Array);

/** Object.prototype.toString() */
console.log(Object.prototype.toString.call([]));
console.log(Object.prototype.toString.call(Array(0)));
console.log(Object.prototype.toString.call(new Array(0)));

/** is() */
console.log(is(Array, []));
console.log(is(Array, Array(0)));
console.log(is(Array, new Array(0)));
```

**typeof**

- 無論使用哪一種方式建立 array，都無法如預期回傳 `array`，而是回傳 `object`，這是比較困擾的地方

**instanceof**

- 無論使用哪一種方式建立 object，都會如預期回傳 `true`

**Object.prototype.toString()**

- 無論對於 literal、function、constructor 所產生的 array，都回傳 `array`

**Ramda 之 is()**

- 無論對於 literal、function、constructor 所產生的 array，都回傳 `true`

> `instanceof`、`Object.prototype.toString()` 與 `is()` 都能同時判斷 literal、function 與 constructor 所產生的 array，實務上比較好用，但 `typeof` 則完全誤判為 object

![type009](/images/ecmascript/detect-type/type009.png)

## Function

Function 共有 4 種建立方法：

1. Function declaration
2. Function expression
3. Function constructor
4. Arrow function

實務上最常使用的是的 function expression 與 arrow function，function declaration 則漸漸較少使用，但 function constructor 使用機會更少，除非想在 run-time 動態湊出 function。

```javascript
/** function declaration */
function fun() {}
console.log(typeof fun);

/** function expression */
console.log(typeof function() {});

/** function constructor */
console.log(typeof new Function());

/** arrow function */
console.log(typeof (() => {}));
```

**typeof**

- 無論使用哪一種方式建立 function，都會如預期回傳 `function`

![type010](/images/ecmascript/detect-type/type010.png)

```javascript
/** function declaration */
function fun() {}
console.log(fun instanceof Function);

/** function expression */
console.log(function() {} instanceof Function);

/** function constructor */
console.log(new Function() instanceof Function);

/** arrow function */
console.log((() => {}) instanceof Function);
```

**instanceof**

- 無論使用哪一種方式建立 function，都會如預期回傳 `true`

![type011](/images/ecmascript/detect-type/type011.png)

```javascript
/** function declaration */
function fun() {}
console.log(Object.prototype.toString.call(fun));

/** function expression */
console.log(Object.prototype.toString.call(function() {}));

/** function constructor */
console.log(Object.prototype.toString.call(new Function()));

/** arrow function */
console.log(Object.prototype.toString.call(() => {}));
```

**Object.prototype.toString()**

- 無論使用哪一種方式建立 function，都會如預期回傳 `function`

![type012](/images/ecmascript/detect-type/type012.png)

```javascript
import { is } from 'ramda';

/** function declaration */
function fun() {}
console.log(is(Function, fun));

/** function expression */
console.log(is(Function, function() {}));

/** function constructor */
console.log(is(Function, new Function()));

/** arrow function */
console.log(is(Function, () => {}));
```

**is()**

- 無論使用哪一種方式建立 object，都會如預期回傳 `true`

> 對於 function，無論使用 `typeof`、`instanceof`、`Object.prototype.toString()` 或 `is()`，都可以如預期判斷出 function

![type014](/images/ecmascript/detect-type/type014.png)

## typeof()

```javascript
let typeof = data => Object.prototype.toString.call(data).slice(8, -1).toLowerCase();

console.log(typeof(1));
console.log(typeof(new Number(1)));
console.log(typeof(new String('abc')));
console.log(typeof(new String('abc')));
console.log(typeof(true));
console.log(typeof(new Boolean(true)));
console.log(undefined);
console.log(null);
console.log(typeof({}));
console.log(typeof([]));
console.log(typeof(()=>{}));
```

根據以上經驗，我們發現只有一種方式能抓到所有 type 都正確，那就是 `Object.prototype.toString()`，但其回傳還包含 `[Object ]` 等不需要部分，且 type 是第一個字大寫，因此只要稍作加工，就可以與 `typeof` 完全一樣。

```javascript
let typeof = data => Object.prototype.toString.call(data).slice(8, -1).toLowerCase();
```

一樣使用 `Object.prototype.toString()`，利用 `slice()` 抓到我們要的部分，最後再 `toLowerCase()` 轉成全小寫。

![type013](/images/ecmascript/detect-type/type013.png)

## Conclusion

* 使用 literal 或 function 建立的 data，推薦使用 `typeof`，但 `typeof` 無法判斷 `null` (可判斷 `nudefined`) 與以 constructor 建立的 data，也無法判斷 array
* 使用 constructor 建立的 data，推薦使用 `instanceof`，使用 `typeof` 只會傳回 `object`
* `instanceof` 無法測試 `null` 與 `undefined`，因為沒有 constructor
* Ramda 的 `is()` 無法測試 `undefind` 與 `null`，Nullable (`undefined` 與 `null`) 必須使用 `isNil()`，因此也無法達成單一 function 判斷所有 type
* `Object.prototype.toString().call()` 最完整，單一 function 可以判斷所有 type
* `typeof null` 是 ECMAScript 有名的 bug，已經成為 feature
* 自己寫的 `typeof()`，則是以 `Object.prototype.toString()` 為根據繼續加工，再搭配 `slice()` 與 `toLowerCase()`，就能打造全型別判斷的 `typeof()`

## Reference

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString), [typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString), [instanceof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString), [Object.prototype.toString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
[MDN](https://developer.mozilla.org/en-US/), [String.prototype.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)
[MDN](https://developer.mozilla.org/en-US/), [String.prototype.toLowerCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
[Ramda](https://ramdajs.com), [is()](https://ramdajs.com/docs/#is)
[Ramda](https://ramdajs.com), [isNil()](https://ramdajs.com/docs/#isNil)

