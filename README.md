[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide() {

**用更合理的方式写 JavaScript**

ES5 的编码规范请查看[版本一](https://github.com/sivan/javascript-style-guide/blob/master/es5/README.md)，[版本二](https://github.com/adamlu/javascript-style-guide)。

翻译自 [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) 。

<a name="table-of-contents"></a>
## 目录

  1. [类型](#types)
  1. [引用](#references)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [解构](#destructuring)
  1. [字符串](#strings)
  1. [函数](#functions)
  1. [箭头函数](#arrow-functions)
  1. [类 & 构造函数](#classes--constructors)
  1. [模块](#modules)
  1. [Iterators & Generators ](#iterators-and-generators)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [作用域提升](#hoisting)
  1. [比较运算符 & 等号](#comparison-operators--equality)
  1. [代码块](#blocks)
  1. [注释](#comments)
  1. [空格](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转换](#type-casting--coercion)
  1. [命名规则](#naming-conventions)
  1. [存取器](#accessors)
  1. [事件](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 兼容性](#ecmascript-5-compatibility)
  1. [ECMAScript 6 (ES 2015+) 编码规范](#ecmascript-6-es-2015-styles)
  1. [测试](#testing)
  1. [性能](#performance)
  1. [资源](#resources)
  1. [使用人群](#in-the-wild)
  1. [翻译](#translation)
  1. [JavaScript 编码规范说明](#the-javascript-style-guide-guide)
  1. [一起来讨论 JavaScript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)

<a name="types"></a>
##  类型

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **基本类型**: 直接存取基本类型。

    + `字符串`
    + `数值`
    + `布尔类型`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex) **复杂类型**: 通过引用的方式存取复杂类型。

    + `对象`
    + `数组`
    + `函数`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="references"></a>
## 引用

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) 对所有的引用使用 `const` ；不要使用 `var`。 eslint: [`prefer-const`](http://eslint.cn/docs/rules/prefer-const), [`no-const-assign`](http://eslint.cn/docs/rules/no-const-assign)

    > 为什么？这能确保你无法对引用重新赋值，也不会导致 bug 和难以理解的代码。

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) 如果你需要对引用重新赋值，使用 `let` 代替 `var`。

    > 为什么？因为 `let` 是块级作用域，而 `var` 是函数作用域。

    ```javascript
    // bad
    var count = 1;
    if (true) {
        count += 1;
    }

    // good
    let count = 1;
    if (true) {
        count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) `let` 和 `const` 都是块级作用域。

    ```javascript
    // const 和 let 只存在于它们被定义的区块内。
    {
        let a = 1;
        const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="objects"></a>
## 对象

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) 使用字面值创建对象。eslint: [`no-new-object`](http://eslint.cn/docs/rules/no-new-object)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.2"></a>
  - [3.2](#3.2) 创建有动态属性名的对象时，使用可被计算的属性名称。

    > 为什么？因为这样可以让你在一个地方定义所有的对象属性。

    ```javascript
    function getKey(k) {
        return `a key named ${k}`;
    }

    // bad
    const obj = {
        id: 5,
        name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
        id: 5,
        name: 'San Francisco',
        [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.3"></a>
  - [3.3](#es6-object-shorthand) 使用对象方法的简写。eslint: [`object-shorthand`](http://eslint.cn/docs/rules/object-shorthand)

    ```javascript
    // bad
    const atom = {
        value: 1,

        addValue: function (value) {
            return atom.value + value;
        },
    };

    // good
    const atom = {
        value: 1,

        addValue(value) {
            return atom.value + value;
        },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.4"></a>
  - [3.4](#es6-object-concise) 使用对象属性值的简写。eslint: [`object-shorthand`](http://eslint.cn/docs/rules/object-shorthand)

    > 为什么？因为这样更简短而且更形象。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.5"></a>
  - [3.5](#objects--grouped-shorthand) 声明对象属性之前对简写的属性分组。

    > 为什么？因为这样能清楚地看出哪些属性使用了简写。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        episodeOne: 1,
        twoJedisWalkIntoACantina: 2,
        lukeSkywalker,
        episodeThree: 3,
        mayTheFourth: 4,
        anakinSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
        anakinSkywalker,
        episodeOne: 1,
        twoJedisWalkIntoACantina: 2,
        episodeThree: 3,
        mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.6"></a>
  - [3.6](#objects--quoted-props) 对无效的标识符使用引号。eslint: [`quote-props`](http://eslint.cn/docs/rules/quote-props)

    > 为什么? 一般我们主观上认为这样可读性更好，这样可以改善语法高亮，而且更容易被 JS 引擎优化。

    ```javascript
    // bad
    const bad = {
        'foo': 3,
        'bar': 4,
        'data-blah': 5,
    };

    // good
    const good = {
        foo: 3,
        bar: 4,
        'data-blah': 5,
    };
    ```

  <a name="objects--rest-spread"><a name="3.7"></a>
  - [3.7](#objects--rest-spread) 最好使用[`扩展运算符`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)代替 [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 去浅拷贝对象，重新定义一个对象去得到一个去除确定属性的的新对象。

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrays"></a>
## 数组

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) 使用字面值创建数组。eslint: [`no-array-constructor`](http://eslint.cn/docs/rules/no-array-constructor)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) 向数组添加元素时使用 [`Array.push`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 替代直接赋值。

    ```javascript
    const someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) <a name='4.3'></a> 使用扩展运算符 `...` 复制数组。

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a><a name="4.4"></a>
  - [4.4](#arrays--from) 使用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 把一个类数组对象转换成数组。

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.5](#arrays--callback-return) 在数组方法的回调函数中使用 return 语句。如果函数部分只有一个单行申明例如 [8.2](#8.2)，return 也可以省略掉。 eslint: [`array-callback-return`](http://eslint.cn/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
        const flatten = memo.concat(item);
        flat[index] = flatten;
    });

    // good
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
        const flatten = memo.concat(item);
        flat[index] = flatten;
        return flatten;
    });

    // bad
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === 'Mockingbird') {
            return author === 'Harper Lee';
        } else {
            return false;
        }
    });

    // good
    inbox.filter((msg) => {
        const { subject, author } = msg;
        if (subject === 'Mockingbird') {
            return author === 'Harper Lee';
        }

        return false;
    });
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="destructuring"></a>
## 解构

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) 使用解构存取和使用多属性对象。jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

    > 为什么？因为解构能减少临时引用属性。

    ```javascript
    // bad
    function getFullName(user) {
        const firstName = user.firstName;
        const lastName = user.lastName;

        return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
        const { firstName, lastName } = obj;
        return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
        return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) 对数组使用解构赋值。jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) 需要回传多个值时，使用对象解构，而不是数组解构。jscs: [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)

    > 为什么？增加属性或者改变排序不会改变调用时的位置。

    ```javascript
    // bad
    function processInput(input) {
        // then a miracle occurs
        return [left, right, top, bottom];
    }

    // 调用时需要考虑回调数据的顺序。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
        // then a miracle occurs
        return { left, right, top, bottom };
    }

    // 调用时只选择需要的数据
    const { left, right } = processInput(input);
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="strings"></a>
## Strings

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) 字符串使用单引号 `''` 。eslint: [`quotes`](http://eslint.cn/docs/rules/quotes)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) 字符串超过 100 个字节不要使用字符串连接符写成多行。

    > 为什么? 拆成多行的字符串很难维护而且不易于搜索。

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.3"></a>
  - [6.3](#es6-template-literals) 程序化生成字符串时，使用模板字符串代替字符串连接。

    > 为什么？模板字符串更为简洁，更具可读性。

    ```javascript
    // bad
    function sayHi(name) {
        return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
        return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
        return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
        return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.4"></a>
  - [6.4](#strings--eval) 永远不要对字符串使用 `eval()`，这个函数会引起很多问题。

  <a name="strings--escaping"></a><a name="6.5"></a>
  - [6.5](#strings--escaping) 不要在字符串中使用无意义的转义 eslint: [`no-useless-escape`](http://eslint.cn/docs/rules/no-useless-escape)

    > 为什么? 反斜杠使可读性变差，因此只在必要的时候使用。

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"';

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="functions"></a>
## 函数

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) 使用函数表达式代替函数声明。

    > 为什么？函数声明会把整个函数提升（hoisted），这意味着我们很容易在函数定义的位置之前去引用这个函数，会影响到代码的可维护性和可读性。

    ```javascript
    // bad
    function foo() {
    }

    // good
    const foo = function () {
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) 把需要立即执行的函数包裹起来。eslint: [`wrap-iife`](http://eslint.cn/docs/rules/wrap-iife)

    ```javascript
    // 立即调用的函数表达式 (IIFE)
    (() => {
        console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) 永远不要在一个非函数代码块（`if`、`while` 等）中声明一个函数。把那个函数赋给一个变量再使用。虽然浏览器允许你这么做，但它们的解析表现不一致。eslint: [`no-loop-func`](http://eslint.cn/docs/rules/no-loop-func)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **注意:** ECMA-262 把 `block` 定义为一组语句。函数声明不是语句。[阅读 ECMA-262 关于这个问题的说明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // bad
    if (currentUser) {
        function test() {
            console.log('Nope.');
        }
    }

    // good
    let test;
    if (currentUser) {
        test = () => {
            console.log('Yup.');
        };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) 永远不要把参数命名为 `arguments`。这将覆盖原来函数作用域内的 `arguments` 对象。

    ```javascript
    // bad
    function nope(name, options, arguments) {
        // ...
    }

    // good
    function yup(name, options, args) {
        // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) <a name='7.6'></a> 不要使用 `arguments`。可以选择 rest 语法 `...` 替代。

    > 为什么？使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

    ```javascript
    // bad
    function concatenateAll() {
        const args = Array.prototype.slice.call(arguments);
        return args.join('');
    }

    // good
    function concatenateAll(...args) {
        return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) 直接给函数的参数指定默认值，不要改变函数参数。

    ```javascript
    // really bad
    function handleThings(opts) {
        // 不！我们不应该改变函数参数。
        // 更加糟糕: 如果参数 opts 是 false 的话，它就会被设定为一个对象。
        // 但这样的写法会造成一些 Bugs。
        //（译注：例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象。）
        opts = opts || {};
        // ...
    }

    // still bad
    function handleThings(opts) {
        if (opts === void 0) {
            opts = {};
        }
        // ...
    }

    // good
    function handleThings(opts = {}) {
        // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) 直接给函数参数赋值时需要避免副作用。

    > 为什么？因为这样的写法让人感到很困惑。

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
        console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) 把默认的参数放在最后面。

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
        // ...
    }

    // good
    function handleThings(name, opts = {}) {
        // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) 永远不要使用 `Function` 去构造一个新的函数。eslint: [`no-new-func`](http://eslint.cn/docs/rules/no-new-func)

    > 为什么？用这种方式去创建一个新的函数相当于对字符串使用 `eval()`，会引起很多问题。

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');

    // good
    var x = function (a, b) {
        return a + b;
    };
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) 在函数的圆括号前后添加空格。 eslint: [`space-before-function-paren`](http://eslint.cn/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.cn/docs/rules/space-before-blocks)

    > 为什么? 一致性很重要，而且在添加或者删除函数名字的时候不需要添加或者删除空格了。

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) 永远不要操作参数。eslint: [`no-param-reassign`](http://eslint.cn/docs/rules/no-param-reassign)

    > 为什么？对函数参数中的变量进行操作可能会误导读者，导致混乱，也会改变 `arguments` 对象。

    ```javascript
    // bad
    function f1(obj) {
        obj.key = 1;
    }

    // good
    function f2(obj) {
        const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) 永远不要对参数重新赋值。eslint: [`no-param-reassign`](http://eslint.cn/docs/rules/no-param-reassign)

    > 为什么? 重新赋值参数会导致一些不可预料的情况发生，比如当访问 `arguments` 对象的时候。这也会影响一些性能优化问问题，特别是在 [`v8`](https://github.com/v8/v8) 引擎中。

    ```javascript
    // bad
    function f1(a) {
        a = 1;
        // ...
    }

    function f2(a) {
        if (!a) { a = 1; }
        // ...
    }

    // good
    function f3(a) {
        const b = a || 1;
        // ...
    }

    function f4(a = 1) {
        // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) 最好使用扩展运算符 `...` 调用可变参数函数。eslint: [`prefer-spread`](http://eslint.cn/docs/rules/prefer-spread)

    > Why? It's cleaner, you don't need to supply a context, and you can not easily compose `new` with `apply`.

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // good
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // good
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"><a name="7.15"></a>
  - [7.15](#functions--signature-invocation-indentation) 有多个参数的函数在调用或者定义的时候，如果要使用换行缩进，应该和其它多行列表使用一个标准：每一条语句占用一行，在最后加上一个逗号。

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
        // ...
    }

    // good
    function foo(
        bar,
        baz,
        quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
        bar,
        baz);

    // good
    console.log(
        foo,
        bar,
        baz,
    );
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrow-functions"></a>
## 箭头函数

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。eslint: [`prefer-arrow-callback`](http://eslint.cn/docs/rules/prefer-arrow-callback), [`arrow-spacing`](http://eslint.cn/docs/rules/arrow-spacing)

    > 为什么?因为箭头函数创造了新的一个 `this` 执行环境（译注：参考 [Arrow functions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 和 [ES6 arrow functions, syntax and lexical scoping](http://toddmotto.com/es6-arrow-functions-syntaxes-and-lexical-scoping/)），通常情况下都能满足你的需求，而且这样的写法更为简洁。

    > 为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
        const y = x + 1;
        return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return)如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。eslint: [`arrow-parens`](http://eslint.cn/docs/rules/arrow-parens), [`arrow-body-style`](http://eslint.cn/docs/rules/arrow-body-style)

    > 为什么？语法糖。在链式调用中可读性很高。

    ```javascript
    // bad
    [1, 2, 3].map(number => {
        const nextNumber = number + 1;
        `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    //bad
    [1, 2, 3].map(number => {
        const nextNumber = number + 1;
        return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number) => {
        const nextNumber = number + 1;
        return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
        [index]: number,
    }));
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) 当表达式占用了多行的时候，为了更好的可读性，用括号包裹起来。

    > 为什么? 函数内容看起来会很清晰。

    ```javascript
    // bad
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
            httpMagicObjectWithAVeryLongName,
            httpMethod,
        )
    );

    // good
    ['get', 'post', 'put'].map(httpMethod => (
        Object.prototype.hasOwnProperty.call(
            httpMagicObjectWithAVeryLongName,
            httpMethod,
        )
    ));
    ```

   <a name="arrows--confusing"></a><a name="8.4"></a>
  - [8.4](#arrows--confusing) 避免箭头函数语法 (`=>`) 和一些比较操作 (`<=`, `>=`) 对人造成困惑。eslint: [`no-confusing-arrow`](http://eslint.cn/docs/rules/no-confusing-arrow)

    ```javascript
    // bad
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
        const { height, largeSize, smallSize } = item;
        return height > 256 ? largeSize : smallSize;
    };
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="class--constructors"></a>
## 类 & 构造器

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) 总是使用 `class`。避免直接操作 `prototype` 。

    > 为什么? 因为 `class` 语法更为简洁更易读。

    ```javascript
    // bad
    function Queue(contents = []) {
        this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
    }


    // good
    class Queue {
        constructor(contents = []) {
            this._queue = [...contents];
        }
        pop() {
            const value = this._queue[0];
            this._queue.splice(0, 1);
            return value;
        }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) 使用 `extends` 继承。

    > 为什么？因为 `extends` 是一个内建的原型继承方法并且不会破坏 `instanceof`。

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
        Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
        return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
        peek() {
            return this._queue[0];
        }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) 方法可以返回 `this` 来帮助链式调用。

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
        this.jumping = true;
        return true;
    };

    Jedi.prototype.setHeight = function(height) {
        this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
        jump() {
            this.jumping = true;
            return this;
        }

        setHeight(height) {
            this.height = height;
            return this;
        }
    }

    const luke = new Jedi();

    luke.jump()
        .setHeight(20);
    ```


  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) 可以写一个自定义的 `toString()` 方法，但要确保它能正常运行并且不会引起副作用。

    ```javascript
    class Jedi {
        constructor(options = {}) {
            this.name = options.name || 'no name';
        }

        getName() {
            return this.name;
        }

        toString() {
            return `Jedi - ${this.getName()}`;
        }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 没有指定构造函数的类都有一个默认的构造函数，没有必要提供一个空的构造函数或只是简单的调用父类这样的构造函数。 eslint: [`no-useless-constructor`](http://eslint.cn/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
        constructor() {}

        getName() {
            return this.name;
        }
    }

    // bad
    class Rey extends Jedi {
        constructor(...args) {
            super(...args);
        }
    }

    // good
    class Rey extends Jedi {
        constructor(...args) {
            super(...args);
            this.name = 'Rey';
        }
    }
    ```

    <a name="classes--no-duplicate-members"><a name="9.6"></a>
  - [9.6](#classes--no-duplicate-members) 不允许类成员中有重复的名称。 eslint: [`no-dupe-class-members`](http://eslint.cn/docs/rules/no-dupe-class-members)

    > 为什么？重复的声明类成员都会被最后一个覆盖。

    ```javascript
    // bad
    class Foo {
        bar() { return 1; }
        bar() { return 2; }
    }

    // good
    class Foo {
        bar() { return 1; }
    }

    // good
    class Foo {
        bar() { return 2; }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="modules"></a>
## 模块

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) 总是使用 (`import`/`export`) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。

    > 为什么？模块就是未来，让我们开始迈向未来吧。

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) 不要使用通配符 import。

    > 为什么？这样能确保你只有一个默认 `export`。

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) 不要从 `import` 中直接 export。

    > 为什么？虽然一行代码简洁明了，但让 import 和 export 各司其职让事情能保持一致。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"><a name="10.4"></a>
  - [10.4](#modules--no-duplicate-imports) 相同路径只使用一次 `import`。eslint: [`no-duplicate-imports`](http://eslint.cn/docs/rules/no-duplicate-imports)

    > 为什么？同一个路径使用多个 `import` 会使代码变的难以维护。

    ```javascript
    // bad
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
        named1,
        named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"><a name="10.5"></a>
  - [10.5](#modules--no-mutable-exports) 不要 `export` 可变的绑定。eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

    ```javascript
    // bad
    let foo = 3;
    export { foo };

    // good
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"><a name="10.6"></a>
  - [10.6](#modules--prefer-default-export) 在一个只有一个 export 的模块中，最好用默认 export 覆盖名字 export。eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--imports-first"><a name="10.7"></a>
  - [10.7](#modules--imports-first) 把所有的 `import` 放在 no-import 声明之前。eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > 为什么？因为 `import` 作用域提升，把它们放在最前面防止意外.

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"><a name="10.8"></a>
  - [10.8](#modules--multiline-imports-over-newlines) 多行的 imports 应该和多行数组以及字面值对象一样的缩进规则。

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // good
    import {
        longNameA,
        longNameB,
        longNameC,
        longNameD,
        longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"><a name="10.9"></a>
  - [10.9](#modules--no-webpack-loader-syntax) 禁止 Webpack 的加载语法出现在模块 import 语句中。eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > 为什么？在 imports 中使用 Webpack 语法会导致最后打包的代码重复，最好在 `webpack.config.js` 中使用加载语法。

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // good
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="iterators-and-generators"></a>
## Iterators and Generators

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) 不要使用 iterators。使用高阶函数替代 `for-in` 和 `for-of`。eslint: [`no-iterator`](http://eslint.cn/docs/rules/no-iterator) [`no-restricted-syntax`](http://eslint.cn/docs/rules/no-restricted-syntax)

    > 为什么？这加强了我们不变的规则。处理纯函数的回调值更易读，这比它带来的副作用更重要。

    > 使用 `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... iterate 数组，`Object.keys()` / `Object.values()` / `Object.entries()` 产生数组然后 iterate 对象。

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
        sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // bad
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
        increasedByOne.push(numbers[i] + 1);
    }

    // good
    const increasedByOne = [];
    numbers.forEach(num => increasedByOne.push(num + 1));

    // best (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) 现在还不要使用 generators。

    > 为什么？因为它们现在还没法很好地编译到 ES5。 (译者注：目前(2016/03) Chrome 和 Node.js 的稳定版本都已支持 generators)

  <a name="generators--spacing"><a name="11.3"></a>
  - [11.3](#generators--spacing) 如果你一定要使用 generators，不想搭理 [11.2](#generators--nope), 请强制 generator 函数中 * 周围使用正确的空格。eslint: [`generator-star-spacing`](http://eslint.cn/docs/rules/generator-star-spacing)

    > 为什么？`function` 和 `*` 同一个概念的关键字中的一部分， `*` 不是修饰 `function` 的， `function*` 和 `function` 不是一个东西.

    ```javascript
    // bad
    function * foo() {
        // ...
    }

    // bad
    const bar = function * () {
        // ...
    };

    // bad
    const baz = function *() {
        // ...
    };

    // bad
    const quux = function*() {
        // ...
    };

    // bad
    function*foo() {
        // ...
    }

    // bad
    function *foo() {
        // ...
    }

    // very bad
    function
    *
    foo() {
        // ...
    }

    // very bad
    const wat = function
    *
    () {
        // ...
    };

    // good
    function* foo() {
        // ...
    }

    // good
    const foo = function* () {
        // ...
    };
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="properties"></a>
## 属性

  - [12.1](#12.1) <a name='12.1'></a> 使用 `.` 来访问对象的属性。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> 当通过变量访问属性时使用中括号 `[]`。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="variables"></a>
## 变量

  - [13.1](#13.1) <a name='13.1'></a> 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。[地球队长](http://www.wikiwand.com/en/Captain_Planet)已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。）

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> 使用 `const` 声明每一个变量。

    > 为什么？增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> 将所有的 `const` 和 `let` 分组

  > 为什么？当你需要把已赋值变量赋值给未赋值变量时非常有用。

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> 在你需要的地方给变量赋值，但请把它们放在一个合理的位置。

  > 为什么？`let` 和 `const` 是块级作用域而不是函数作用域。

    ```javascript
    // good
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - unnecessary function call
    function(hasName) {
      const name = getName();

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function(hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="hoisting"></a>
## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> `var` 声明会被提升至该作用域的顶部，但它们赋值不会提升。`let` 和 `const` 被赋予了一种称为「[暂时性死区（Temporal Dead Zones, TDZ）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)」的概念。这对于了解为什么 [type of 不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)相当重要。

    ```javascript
    // 我们知道这样运行不了
    // （假设 notDefined 不是全局变量）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 由于变量提升的原因，
    // 在引用变量后再声明变量是可以运行的。
    // 注：变量的赋值 `true` 不会被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 编译器会把函数声明提升到作用域的顶层，
    // 这意味着我们的例子可以改写成这样：
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // 使用 const 和 let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> 匿名函数表达式的变量名会被提升，但函数内容并不会。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> 函数声明的名称和函数体都会被提升。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 想了解更多信息，参考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)。

**[⬆ 返回目录](#table-of-contents)**

<a name="comparison-operators--equality"></a>
## 比较运算符 & 等号

  - [15.1](#15.1) <a name='15.1'></a> 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`.
  - [15.2](#15.2) <a name='15.2'></a> 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则：

    + **对象** 被计算为 **true**
    + **Undefined** 被计算为 **false**
    + **Null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0、-0、或 NaN** 被计算为 **false**, 否则为 **true**
    + **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> 使用简写。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - [15.4](#15.4) <a name='15.4'></a> 想了解更多信息，参考 Angus Croll 的 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)。

**[⬆ 返回目录](#table-of-contents)**

<a name="blocks"></a>
## 代码块

  - [16.1](#16.1) <a name='16.1'></a> 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  - [16.2](#16.2) <a name='16.2'></a> 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="comments"></a>
## 注释

  - [17.1](#17.1) <a name='17.1'></a> 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - [17.2](#17.2) <a name='17.2'></a> 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [17.3](#17.3) <a name='17.3'></a> 给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  - [17.4](#17.4) <a name='17.4'></a> 使用 `// FIXME`: 标注问题。

    ```javascript
    class Calculator {
      constructor() {
        // FIXME: shouldn't use a global here
        total = 0;
      }
    }
    ```

  - [17.5](#17.5) <a name='17.5'></a> 使用 `// TODO`: 标注问题的解决方式。

    ```javascript
    class Calculator {
      constructor() {
        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="whitespace"></a>
## 空白

  - [18.1](#18.1) <a name='18.1'></a> 使用 2 个空格作为缩进。

    ```javascript
    // bad
    function() {
    ∙∙∙∙const name;
    }

    // bad
    function() {
    ∙const name;
    }

    // good
    function() {
    ∙∙const name;
    }
    ```

  - [18.2](#18.2) <a name='18.2'></a> 在花括号前放一个空格。

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [18.3](#18.3) <a name='18.3'></a> 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - [18.4](#18.4) <a name='18.4'></a> 使用空格把运算符隔开。

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name='18.5'></a> 在文件末尾插入一个空行。

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - [18.5](#18.5) <a name='18.5'></a> 在使用长方法链时进行缩进。使用前面的点 `.` 强调这是方法调用而不是新语句。

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [18.6](#18.6) <a name='18.6'></a> 在块末和新语句前插入空行。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="commas"></a>
## 逗号

  - [19.1](#19.1) <a name='19.1'></a> 行首逗号：**不需要**。

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [19.2](#19.2) <a name='19.2'></a> 增加结尾的逗号: **需要**。

  > 为什么? 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的[尾逗号问题](es5/README.md#commas)。

    ```javascript
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    }

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    }

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="semicolons"></a>
## 分号

  - [20.1](#20.1) <a name='20.1'></a> **使用分号**

    ```javascript
    // bad
    (function() {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    })();

    // good (防止函数在两个 IIFE 合并时被当成一个参数)
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 返回目录](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 类型转换

  - [21.1](#21.1) <a name='21.1'></a> 在语句开始时执行类型转换。
  - [21.2](#21.2) <a name='21.2'></a> 字符串：

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name='21.3'></a> 对数字使用 `parseInt` 转换，并带上类型转换的基数。

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - [21.4](#21.4) <a name='21.4'></a> 如果因为某些原因 parseInt 成为你所做的事的瓶颈而需要使用位操作解决[性能问题](http://jsperf.com/coercion-vs-casting/3)时，留个注释说清楚原因和你的目的。

    ```javascript
    // good
    /**
     * 使用 parseInt 导致我的程序变慢，
     * 改成使用位操作转换数字快多了。
     */
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name='21.5'></a> **注:** 小心使用位操作运算符。数字会被当成 [64 位值](http://es5.github.io/#x4.3.19)，但是位操作运算符总是返回 32 位的整数（[参考](http://es5.github.io/#x11.7)）。位操作处理大于 32 位的整数值时还会导致意料之外的行为。[关于这个问题的讨论](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整数是 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [21.6](#21.6) <a name='21.6'></a> 布尔:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="naming-conventions"></a>
## 命名规则

  - [22.1](#22.1) <a name='22.1'></a> 避免单字母命名。命名应具备描述性。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - [22.2](#22.2) <a name='22.2'></a> 使用驼峰式命名对象、函数和实例。

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - [22.3](#22.3) <a name='22.3'></a> 使用帕斯卡式命名构造函数或类。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - [22.4](#22.4) <a name='22.4'></a> 使用下划线 `_` 开头命名私有属性。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [22.5](#22.5) <a name='22.5'></a> 别保存 `this` 的引用。使用箭头函数或 Function#bind。

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - [22.6](#22.6) <a name='22.6'></a> 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - [22.7](#22.7) <a name='22.7'></a> 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [22.8](#22.8) <a name='22.8'></a> 当你导出单例、函数库、空对象时使用帕斯卡式命名。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="accessors"></a>
## 存取器

  - [23.1](#23.1) <a name='23.1'></a> 属性的存取函数不是必须的。
  - [23.2](#23.2) <a name='23.2'></a> 如果你需要存取函数时使用 `getVal()` 和 `setVal('hello')`。

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - [23.3](#23.3) <a name='23.3'></a> 如果属性是布尔值，使用 `isVal()` 或 `hasVal()`。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [23.4](#23.4) <a name='23.4'></a> 创建 `get()` 和 `set()` 函数是可以的，但要保持一致。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="events"></a>
## 事件

  - [24.1](#24.1) <a name='24.1'></a> 当给事件附加数据时（无论是 DOM 事件还是私有事件），传入一个哈希而不是原始值。这样可以让后面的贡献者增加更多数据到事件数据而无需找出并更新事件的每一个处理器。例如，不好的写法：

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    更好的写法：

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ 返回目录](#table-of-contents)**


## jQuery

  - [25.1](#25.1) <a name='25.1'></a> 使用 `$` 作为存储 jQuery 对象的变量名前缀。

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');
    ```

  - [25.2](#25.2) <a name='25.2'></a> 缓存 jQuery 查询。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [25.3](#25.3) <a name='25.3'></a> 对 DOM 查询使用层叠 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [25.4](#25.4) <a name='25.4'></a> 对有作用域的 jQuery 对象查询使用 `find`。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="ecmascript-5-compatibility"></a>
## ECMAScript 5 兼容性

  - [26.1](#26.1) <a name='26.1'></a> 参考 [Kangax](https://twitter.com/kangax/) 的 ES5 [兼容性](http://kangax.github.com/es5-compat-table/).

**[⬆ 返回目录](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6 规范

  - [27.1](#27.1) <a name='27.1'></a> 以下是链接到 ES6 的各个特性的列表。

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[⬆ 返回目录](#table-of-contents)**

<a name="testing"></a>
## 测试

  - [28.1](#28.1) <a name='28.1'></a> **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="performance"></a>
## 性能

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ 返回目录](#table-of-contents)**

<a name="resources"></a>
## 资源

**Learning ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Tools**

  - Code Style Linters
    + [ESlint](http://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[⬆ 返回目录](#table-of-contents)**

<a name="in-the-wild"></a>
## 使用人群

  This is a list of organizations that are using this style guide. Send us a pull request or open an issue and we'll add you to the list.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ 返回目录](#table-of-contents)**

<a name="translation"></a>
## 翻译

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [yuche/javascript](https://github.com/yuche/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

<a name="the-javascript-style-guide-guide"></a>
## JavaScript 编码规范说明

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

<a name="chat-with-us-about-javascript"></a>
## 一起来讨论 JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 返回目录](#table-of-contents)**

# };
