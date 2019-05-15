---
title: 一些 JavaScript 中的代码小技巧
url: 161.html
id: 161
categories:
  - js
date: 2018-08-23 10:06:06
tags:
---

一些 JavaScript 中的代码小技巧
---------------------

**·  ·  ·**

**JSON.stringify**
------------------

我们平时经常会用到`JSON` 对象，比如当我们要实现对象的深拷贝时，我们可以用`JSON` 对象的`JSON.stringify`和`JSON.parse` 来拷贝一个完全一样的对象，而不会对原对象产生任何引用关系。在使用`localStorage` 时，也会用到它，因为`localStorage` 只能存储字符串格式的内容，所以，我们在存之前，将数值转换成`JSON字符串`，取出来用的时候，再转成对象或数组。 对于`JSON.stringify` 方法，它可以帮我们把一个对象或数组转换成一个`JSON字符串`。我们通常只会用到它的第一个参数，其实它还有另外两个参数，可以让它实现一些非常好用的功能。 首先来看语法：

> JSON.stringify(value\[, replacer \[, space\]\])

参数：

*   value：将要被序列化的变量的值
*   replacer：替代器。可以是函数或者是数组，如果是一个函数，则`value` 每个属性都要经过这个函数的处理，该函数的返回值就是最后被序列化后的值。如果是一个数组，则要求该数组的元素是字符串，且这些元素会被当做`value` 的键（`key`）进行匹配，最后序列化的结果，是只包含该数组每个元素为`key` 的值。
*   space：指定输出数值的代码缩进，美化格式之用，可以是数字或者字符串。如果是数字（最大为10）的话，代表每行代码的缩进是多少个空格。如果是字符串的话，该字符串（最多前十个字符）将作显示在每行代码之前。

这时候，你应该知道了。我们可以用`JSON.stringify` 来做序列化时的过滤，相当于我们可以自定义`JSON.stringify` 的解析逻辑。 使用函数过滤并序列化对象：

1.  `// 使用“函数”当替代器`
2.  `function replacer(key, value) {`
3.  `if (typeof value === "string") {`
4.  `return undefined;`
5.  ` }`
6.  `return value;`
7.  `}`

9.  `var foo = {`
10.  `foundation: "Mozilla",`
11.  `model: "box",`
12.  `week: 45,`
13.  `transport: "car",`
14.  `month: 7`
15.  `};`
16.  `var jsonString = JSON.stringify(foo, replacer);`

18.  `// {"week":45,"month":7}`

使用数组过滤并序列化对象：

1.  `// 使用“数组”当替代器`
2.  `const user = {`
3.  `name: 'zollero',`
4.  `nick: 'z',`
5.  ` skills: ['JavaScript', 'CSS', 'HTML5']`
6.  `};`
7.  `JSON.stringify(user, ['name', 'skills'], 2);`

9.  `// "{`
10.  `//   "name": "zollero",`
11.  `//   "skills": [`
12.  `//     "JavaScript",`
13.  `//     "CSS",`
14.  `//     "HTML5"`
15.  `//   ]`
16.  `// }"`

还有一个有意思的东西，是对象的`toJSON` 属性。 如果一个对象有`toJSON` 属性，当它被序列化的时候，不会对该对象进行序列化，而是将它的`toJSON` 方法的返回值进行序列化。 见下面的例子：

1.  `var obj = {`
2.  `foo: 'foo',`
3.  `toJSON: function () {`
4.  `return 'bar';`
5.  ` }`
6.  `};`
7.  `JSON.stringify(obj);      // '"bar"'`
8.  `JSON.stringify({x: obj}); // '{"x":"bar"}'`

**·  ·  ·**

**用 Set 来实现数组去重**
-----------------

在`ES6` 中，引入了一个新的数据结构类型：`Set`。而`Set` 与`Array` 的结构是很类似的，且`Set` 和`Array` 可以相互进行转换。 数组去重，也算是一个比较常见的前端面试题了，方法有很多种，这里不多赘述。下面我们看看用`Set` 和`...（拓展运算符）`可以很简单的进行数组去重。

1.  `const removeDuplicateItems = arr => [...new Set(arr)];`
2.  `removeDuplicateItems([42, 'foo', 42, 'foo', true, true]);`
3.  `//=> [42, "foo", true]`

**·  ·  ·**

**用块级作用域避免命名冲突**
----------------

在开发的过程中，通常会遇到命名冲突的问题，就是需要根据场景不同来定义不同的值来赋值给同一个变量。下面介绍一个使用`ES6` 中的`块级作用域` 来解决这个问题的方法。 比如，在使用`switchcase` 时，我们可以这样做：

1.  `switch (record.type) {`
2.  `case 'added': {`
3.  `const li = document.createElement('li');`
4.  `   li.textContent = record.name;`
5.  `   li.id = record.id;`
6.  `   fragment.appendChild(li);`
7.  `break;`
8.  ` }`

10.  `case 'modified': {`
11.  `const li = document.getElementById(record.id);`
12.  `   li.textContent = record.name;`
13.  `break;`
14.  ` }`
15.  `}`

**·  ·  ·**

**函数参数值校验**
-----------

我们知道，在`ES6` 中，为函数增加了参数默认值的特性，可以为参数设定一些默认值，可以让代码更简洁，可维护。 其实，我们可以通过这个特性来做函数参数值的校验。 首先，函数的参数可以是任意类型的值，也可以是函数，比如下面这个：

1.  `function fix(a = getA()) {`
2.  ` console.log('a', a)`
3.  `}`

5.  `function getA() {`
6.  ` console.log('get a')`
7.  `return 2`
8.  `}`

10.  `fix(1);`
11.  `// a 1`

13.  `fix();`
14.  `// get a`
15.  `// a 2`

可以看出，如果在调用`fix` 时传了参数`a` ，则不会执行函数`getA`，只有当不传递参数`a` 时，才会执行函数`getA`。 这时候，我们可以利用这一特性，为参数`a` 添加一个必传的校验，代码如下：

1.  `function fix(a = require()) {`
2.  ` console.log('a', a)`
3.  `}`

5.  `function require() {`
6.  `throw new Error('缺少了参数 a')`
7.  `}`

9.  `fix(1);`
10.  `// a 1`

12.  `fix();`
13.  `// Uncaught Error: 缺少了参数 a`

**·  ·  ·**

**用解构赋值过滤对象属性**
---------------

在前面我们介绍了使用`JSON.stringify` 来过滤对象的属性的方法。这里，我们介绍另外一种使用`ES6` 中的`解构赋值` 和`拓展运算符` 的特性来过滤属性的方法。 比如，下面这段示例：

1.  `// 我们想过滤掉对象 types 中的 inner 和 outer 属性`
2.  `const { inner, outer, ...restProps } = {`
3.  `inner: 'This is inner',`
4.  `outer: 'This is outer',`
5.  `v1: '1',`
6.  `v2: '2',`
7.  `v4: '3'`
8.  `};`
9.  `console.log(restProps);`
10.  `// {v1: "1", v2: "2", v4: "3"}`

**·  ·  ·**

**用解构赋值获取嵌套对象的属性**
------------------

`解构赋值` 的特性很强大，它可以帮我们从一堆嵌套很深的对象属性中，很方便地拿到我们想要的那一个。比如下面这段代码：

1.  `// 通过解构赋值获取嵌套对象的值`
2.  `const car = {`
3.  `model: 'bmw 2018',`
4.  `   engine: {`
5.  `v6: true,`
6.  `turbo: true,`
7.  `vin: 12345`
8.  `   }`
9.  `};`
10.  `// 这里使用 ES6 中的简单写法，使用 { vin } 替代 { vin: vin }`
11.  `const modalAndVIN = ({ model, engine: { vin }}) => {`
12.  ``   console.log(`model: ${model}, vin: ${vin}`);``
13.  `}`

15.  `modalAndVIN(car);`
16.  `// "model: bmw 2018, vin: 12345"`

**·  ·  ·**

**合并对象**
--------

`ES6` 中新增的`拓展运算符`，可以用来解构数组，也可以用来解构对象，它可以将对象中的所有属性展开。 通过这个特性，我们可以做一些对象合并的操作，如下：

1.  `// 使用拓展运算符合并对象，在后面的属性会重写前面相同属性的值`
2.  `const obj1 = { a: 1, b: 2, c: 3 };`
3.  `const obj2 = { c: 5, d: 9 };`
4.  `const merged = { ...obj1, ...obj2 };`
5.  `console.log(merged);`
6.  `// {a: 1, b: 2, c: 5, d: 9}`

8.  `const obj3 = { a: 1, b: 2 };`
9.  `const obj4 = { c: 3, d: { e: 4, ...obj3 } };`
10.  `console.log(obj4);`
11.  `// {c: 3, d: {a: 1, b: 2, e: 4} }`

**·  ·  ·**

**使用 === 代替 ==**
----------------

在`JavaScript` 中，`===` 和`==` 是有很大的不同的，`==` 会将两边的变量进行转义，然后将转义后的值进行比较，而`===` 是严格比较，要求两边的变量不仅值要相同，它们自身的类型也要相同。 `JavaScript` 经常被调侃成一个神奇的语言，就是因为它的转义的特性，而用`==` 可能会引入一些深埋的bug。远离 bug，还是要用`===`：

1.  `[10] ==  10 // true`
2.  `[10] === 10 // false`

4.  `'10' ==10 // true`
5.  `'10' === 10 // false`

7.  `[]  ==  0 // true`
8.  `[]  === 0 // false`

10.  `'' ==false // true`
11.  `'' === false // false`

当然，在用`===` 时，也会出问题，比如：

1.  `NaN === NaN // false`

`ES6` 中提供了一个新的方法：`Object.is()`，它具有`===` 的一些特点，而且更好、更准确，在一些特殊场景下变现的更好：

1.  `Object.is(0 , ' ');         //false`
2.  `Object.is(null, undefined); //false`
3.  `Object.is([1], true);       //false`
4.  `Object.is(NaN, NaN);        //true`

下图，是关于`==`、`===` 和`Object.is` 的对比： ![](https://mmbiz.qpic.cn/mmbiz_png/s1EnB5eYnlW7HqgnC94TicT01udHQdaBiaicOC1VUENgSqMdw7tSDe2g9yKAyqb54ibtklPeszCwGgSibyyliafGkicfQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1) **·  ·  ·** 引用： https://developer.mozilla.org/ http://www.jstips.co/ _\- The End -_