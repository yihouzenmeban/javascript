**用更合理的方式写 JavaScript**

翻译自 [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) 。
根据咱们自己项目所用的规则有删减和修改, 每条后面带 `已删除` 标签的表示经过投票已经删除。

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
  1. [编辑器配置格式化](SETTING.md)

<a name="types"></a>
##  类型

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **基本类型**: 直接存取基本类型。

    + `string`
    + `number`
    + `boolean`
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

    + `object`
    + `array`
    + `function`

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

    > 为什么？因为这样可以让你一次定义所有的对象属性。

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
  - [6.2](#strings--line-length) 字符串超过 120 个字节不要使用字符串连接符写成多行。

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
  - ~~[7.1](#functions--declarations) 使用函数表达式代替函数声明。~~ `已删除`

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
  - [7.2](#functions--iife) 把需要立即执行的函数包裹起来（包裹 function 表达式）。eslint: [`wrap-iife`](http://eslint.cn/docs/rules/wrap-iife)

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
  - [7.9](#functions--defaults-last) 把有默认值的参数放在最后面。

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
  - ~~[7.12](#functions--mutate-params) 永远不要操作参数。eslint: [`no-param-reassign`](http://eslint.cn/docs/rules/no-param-reassign)~~ `已删除`

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
## 模块 `咱们项目目前使用的是 seajs, 不支持 import 方式引用模块`

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
  - [11.3](#generators--spacing) 如果你一定要使用 generators，不想搭理 [11.2](#generators--nope)，请强制 generator 函数中 * 周围使用正确的空格。eslint: [`generator-star-spacing`](http://eslint.cn/docs/rules/generator-star-spacing)

    > 为什么？`function` 和 `*` 同一个概念的关键字中的一部分，`*` 不是修饰 `function` 的，`function*` 和 `function` 不是一个东西。

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

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) 使用 `.` 来访问对象的属性。eslint: [`dot-notation`](http://eslint.cn/docs/rules/dot-notation)

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

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) 当通过变量访问属性时使用 `[]`。

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

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。[地球队长](http://www.wikiwand.com/en/Captain_Planet)已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。）eslint: [`no-undef`](http://eslint.cn/docs/rules/no-undef) [`prefer-const`](http://eslint.cn/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) 使用 `const` 声明每一个变量。eslint: [`one-var`](http://eslint.cn/docs/rules/one-var)

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

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) 将所有的 `const` 和 `let` 分组

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

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) 在你需要的地方给变量赋值，但请把它们放在一个合理的位置。

    > 为什么？`let` 和 `const` 是块级作用域而不是函数作用域。

    ```javascript
    // bad - 无意义的函数调用
    function checkName(hasName) {
        const name = getName();

        if (hasName === 'test') {
            return false;
        }

        if (name === 'test') {
            this.setName('');
            return false;
        }

        return name;
    }

    // good
    function checkName(hasName) {
        if (hasName === 'test') {
            return false;
        }

        const name = getName();

        if (name === 'test') {
            this.setName('');
            return false;
        }

        return name;
    }
    ```

  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) 不要链式定义。

    > 为什么? 链式定义会引起隐式的全局定义。

    ```javascript
    // bad
    (function example() {
        // JavaScript 解释为
        // let a = ( b = ( c = 1 ) );
        // let 只作用于 a，b 和 c 变成了全局定义。
        let a = b = c = 1;
    }());

    console.log(a); // undefined
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
        let a = 1;
        let b = a;
        let c = a;
    }());

    console.log(a); // undefined
    console.log(b); // undefined
    console.log(c); // undefined

    // `const` 也一样。
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - ~~[13.6](#variables--unary-increment-decrement) 不要使用一元操作符 (++, --)。 eslint [`no-plusplus`](http://eslint.cn/docs/rules/no-plusplus)~~ `已删除`

    > 为什么? 按照 eslint 的文档，一元操作符 `++` 和 `--` 会自动添加分号，不同的空白可能会改变源代码的语义。而且还会在项目中导致一些意外。

    ```javascript
    // bad

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
        let value = array[i];
        sum += value;
        if (value) {
            truthyCount++;
        }
    }

    // good

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="hoisting"></a>
## Hoisting

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var` 声明会被提升至该作用域的顶部，但它们赋值不会提升。`let` 和 `const` 被赋予了一种称为「[暂时性死区（Temporal Dead Zones, TDZ）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)」的概念。这对于了解为什么 [type of 不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)相当重要。

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

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) 匿名函数表达式的变量名会被提升，但函数内容并不会。

    ```javascript
    function example() {
        console.log(anonymous); // => undefined

        anonymous(); // => TypeError anonymous is not a function

        var anonymous = function() {
            console.log('anonymous function expression');
        };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expresions) 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。

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

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) 声明函数的名称和函数体都会被提升。

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

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - ~~[15.1](#comparison--eqeqeq) 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`。eslint: [`eqeqeq`](http://eslint.cn/docs/rules/eqeqeq)~~ `已删除`

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则：

    + **对象** 被计算为 **true**
    + **undefined** 被计算为 **false**
    + **null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0、-0、或 NaN** 被计算为 **false**, 否则为 **true**
    + **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

    ```javascript
    if ([0] && []) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) 使用简写，显示的字符串和数字对比除外。

    ```javascript
    // bad
    if (isValid === true) {
        // ...
    }

    // good
    if (isValid) {
        // ...
    }

    // bad
    if (name) {
        // ...
    }

    // good
    if (name !== '') {
        // ...
    }

    // bad
    if (collection.length) {
        // ...
    }

    // good
    if (collection.length > 0) {
        // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) 想了解更多信息，参考 Angus Croll 的 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)。

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) 使用花括号创建块作用域来包裹 `case` 和 `default`，如果子句包含词法声明(例如：`let`, `const`, `function`, `class`)。eslint: [`no-case-declarations`](http://eslint.cn/docs/rules/no-case-declarations).

    > 为什么? 因为词法声明在整个 switch 作用域都有效，但是它只有在运行到它定义的 `case` 语句时，才会进行初始化操作。这回导致很多 `case` 的子句定义同一个东西。

    ```javascript
    // bad
    switch (foo) {
        case 1:
            let x = 1;
            break;
        case 2:
            const y = 2;
            break;
        case 3:
            function f() {
              // ...
            }
            break;
        default:
            class C {}
    }

    // good
    switch (foo) {
        case 1: {
            let x = 1;
            break;
        }
        case 2: {
            const y = 2;
            break;
        }
        case 3: {
            function f() {
              // ...
            }
            break;
        }
        case 4:
            bar();
            break;
        default: {
            class C {}
        }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) 禁止使用嵌套的三元运算符。eslint rules: [`no-nested-ternary`](http://eslint.cn/docs/rules/no-nested-ternary)

    ```javascript
    // bad
    const foo = maybe1 > maybe2
        ? "bar"
        : value1 > value2 ? "baz" : null;

    // better
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2
        ? 'bar'
        : maybeNull;

    // best
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) 不要使用无意义的三元运算符。eslint rules: [`no-unneeded-ternary`](http://eslint.cn/docs/rules/no-unneeded-ternary)

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="blocks"></a>
## 代码块

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) 使用大括号包裹所有的多行代码块。

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

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。eslint: [`brace-style`](http://eslint.cn/docs/rules/brace-style)

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

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [17.1](#comments--multiline) 使用 `/** ... */` 作为多行注释。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

        // ...

        return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

        // ...

        return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [17.2](#comments--singleline) 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

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

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a><a name="17.3"></a>
  - [17.3](#comments--spaces) 在所有的注释内容开始之前添加一个空格使它更容易被阅读。eslint: [`spaced-comment`](http://eslint.cn/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

        // ...

        return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

        // ...

        return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.4"></a>
  - [17.4](#comments--actionitems) 给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  <a name="comments--fixme"></a><a name="17.5"></a>
  - [17.5](#comments--fixme) 使用 `// FIXME`: 标注问题。

    ```javascript
    class Calculator {
        constructor() {
            // FIXME: shouldn't use a global here
            total = 0;
        }
    }
    ```

  <a name="comments--todo"></a><a name="17.6"></a>
  - [17.6](#comments--todo) 使用 `// TODO`: 标注问题的解决方式。

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

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [18.1](#whitespace--spaces) 使用 4 个空格作为缩进。eslint: [`indent`](http://eslint.cn/docs/rules/indent)

    ```javascript
    // bad
    function() {
    ∙∙const name;
    }

    // bad
    function() {
    ∙const name;
    }

    // good
    function() {
    ∙∙∙∙const name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [18.2](#whitespace--before-blocks) 在花括号前放一个空格。

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

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [18.3](#whitespace--around-keywords) 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。eslint: [`keyword-spacing`](http://eslint.cn/docs/rules/keyword-spacing)

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

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [18.4](#whitespace--infix-ops) 使用空格把运算符隔开。eslint: [`space-infix-ops`](http://eslint.cn/docs/rules/space-infix-ops)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [18.5](#whitespace--newline-at-end) 在文件末尾插入一个空行。eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
        // ...
    export default es6;
    ```

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
        // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // good
    import { es6 } from './AirbnbStyleGuide';
        // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [18.5](#whitespace--chains) 在使用长方法链时进行缩进（超过两个方法时）。使用前面的点 `.` 强调这是方法调用而不是新语句。eslint: [`newline-per-chained-call`](http://eslint.cn/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.cn/docs/rules/no-whitespace-before-property)

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
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
        .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
        .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [18.6](#whitespace--after-blocks) 在块末和新语句前插入空行。

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

    // bad
    const arr = [
        function foo() {
        },
        function bar() {
        },
    ];
    return arr;

    // good
    const arr = [
        function foo() {
        },

        function bar() {
        },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [18.8](#whitespace--padded-blocks) 禁止块内填充空行。eslint: [`padded-blocks`](http://eslint.cn/docs/rules/padded-blocks)

    ```javascript
    // bad
    function bar() {

        console.log(foo);

    }

    // also bad
    if (baz) {

        console.log(qux);
    } else {
        console.log(foo);

    }

    // good
    function bar() {
        console.log(foo);
    }

    // good
    if (baz) {
        console.log(qux);
    } else {
        console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [18.9](#whitespace--in-parens) 不要在圆括号内加空格。eslint: [`space-in-parens`](http://eslint.cn/docs/rules/space-in-parens)

    ```javascript
    // bad
    function bar( foo ) {
        return foo;
    }

    // good
    function bar(foo) {
        return foo;
    }

    // bad
    if ( foo ) {
        console.log(foo);
    }

    // good
    if (foo) {
        console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [18.10](#whitespace--in-brackets) 不要在方括号内加空格。eslint: [`array-bracket-spacing`](http://eslint.cn/docs/rules/array-bracket-spacing)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [18.11](#whitespace--in-braces) 在花括号内加空格。eslint: [`object-curly-spacing`](http://eslint.cn/docs/rules/object-curly-spacing)

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [18.12](#whitespace--max-len) 单行代码不要超过 120 个字符串（空格计算在内, 这个是大家投票出来确定的数值）。 注意：[above](#strings--line-length), 长字符串不遵循这条规则而且长字符串不应该被折行。eslint: [`max-len`](http://eslint.cn/docs/rules/max-len)

    > 为什么？为了可读性和可维护性。

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
        && jsonData.foo
        && jsonData.foo.bar
        && jsonData.foo.bar.baz
        && jsonData.foo.bar.baz.quux
        && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
        method: 'POST',
        url: 'https://airbnb.com/',
        data: { name: 'John' },
    })
        .done(() => console.log('Congratulations!'))
        .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="commas"></a>
## 逗号

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [19.1](#commas--leading-trailing) 行首逗号：**不需要**。eslint: [`comma-style`](http://eslint.cn/docs/rules/comma-style)

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

  <a name="commas--dangling"></a><a name="19.2"></a>
  - ~~[19.2](#commas--dangling) 增加结尾的逗号: **需要**。eslint: [`comma-dangle`](http://eslint.cn/docs/rules/comma-dangle)~~ `已删除`

    > ~~为什么? 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的[尾逗号问题](es5/README.md#commas)。~~

    > 禁止添加结尾的逗号。

    ```diff
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
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

    // bad
    function createHero(
        firstName,
        lastName,
        inventorOf
    ) {
        // does nothing
    }

    // good
    function createHero(
        firstName,
        lastName,
        inventorOf,
    ) {
        // does nothing
    }

    // good (note that a comma must not appear after a "rest" element)
    function createHero(
        firstName,
        lastName,
        inventorOf,
        ...heroArgs
    ) {
        // does nothing
    }

    // bad
    createHero(
        firstName,
        lastName,
        inventorOf
    );

    // good
    createHero(
        firstName,
        lastName,
        inventorOf,
    );

    // good (note that a comma must not appear after a "rest" element)
    createHero(
        firstName,
        lastName,
        inventorOf,
        ...heroArgs
    );
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="semicolons"></a>
## 分号

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [20.1](#20.1) **使用分号** eslint: [`semi`](http://eslint.cn/docs/rules/semi)

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

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214)

**[⬆ 返回目录](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 类型转换

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [21.1](#coercion--explicit) 在语句开始时执行类型转换。

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [21.2](#coercion--strings) 字符串：

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + ''; // 调用 this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // 不能保证返回一个字符串

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [21.3](#coercion--numbers) 对数字使用 `Number` 和 `parseInt` 转换，`parseInt` 要带上类型转换的基数。eslint: [`radix`](http://eslint.cn/docs/rules/radix)

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

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [21.4](#coercion--comment-deviations) 如果因为某些原因 parseInt 成为你所做的事的瓶颈而需要使用位操作解决[性能问题](http://jsperf.com/coercion-vs-casting/3)时，留个注释说清楚原因和你的目的。

    ```javascript
    // good
    /**
     * 使用 parseInt 导致我的程序变慢，
     * 改成使用位操作转换数字快多了。
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [21.5](#coercion--bitwise) **注:** 使用位操作运算符要小心。数字会被当成 [64 位值](http://es5.github.io/#x4.3.19)，但是位操作运算符总是返回 32 位的整数（[参考](http://es5.github.io/#x11.7)）。位操作处理大于 32 位的整数值时还会导致意料之外的行为。[关于这个问题的讨论](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整数是 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [21.6](#coercion--booleans) 布尔:

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

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [22.1](#naming--descriptive) 避免单字母命名。命名应具备描述性。eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

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

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [22.2](#naming--camelCase) 使用驼峰式命名对象、函数和实例。eslint: [`camelcase`](http://eslint.cn/docs/rules/camelcase)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [22.3](#naming--PascalCase) 使用帕斯卡式命名（首字母大写）构造函数或类。eslint: [`new-cap`](http://eslint.cn/docs/rules/new-cap)

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

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - ~~[22.4](#naming--leading-underscore) 不要使用下划线 `_` 开头或者结尾进行命名。eslint: [`no-underscore-dangle`](http://eslint.cn/docs/rules/no-underscore-dangle)~~ `已删除`

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - ~~[22.5](#naming--self-this) 别保存 `this` 的引用。使用箭头函数或 [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)。~~ `已删除`

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

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [22.6](#naming--filename-matches-export) 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。

    ```javascript
    // file 1 contents
    class CheckBox {
        // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [22.7](#naming--camelCase-default-export) 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [22.8](#naming--PascalCase-singleton) 当你导出单例、函数库、空对象时使用帕斯卡式命名。

    ```javascript
    const AirbnbStyleGuide = {
        es6: {
        }
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"><a name="22.9"></a></a>
  - [22.9](#naming--Acronyms-and-Initialisms) 缩略词或者缩写的单词应该全部大写或者小写。

    > 为什么？为了更好的可读性。

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
        // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
        // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const Requests = [
        // ...
    ];
    ```
**[⬆ 返回目录](#table-of-contents)**

<a name="accessors"></a>
## 存取器

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [23.1](#accessors--not-required) 属性的存取函数不是必须的。

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [23.2](#accessors--no-getters-setters) 不要使用 JavaScript `getters/setters`，因为它们会引起副作用，而且会导致代码很难测试，维护和阅读。如果创建获取函数，使用 getVal() 和 setVal('hello')。

    ```javascript
    // bad
    class Dragon {
        get age() {
            // ...
        }

        set age(value) {
            // ...
        }
    }

    // good
    class Dragon {
        getAge() {
            // ...
        }

        setAge(value) {
            // ...
        }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [23.3](#accessors--boolean-prefix) 如果属性是布尔值，使用 `isVal()` 或 `hasVal()`。

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

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [23.4](#accessors--consistent) 创建 `get()` 和 `set()` 函数是可以的，但要保持一致。

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

  <a name="events--hash"></a><a name="24.1"></a>
  - [24.1](#events--hash) 当给事件附加数据时（无论是 DOM 事件还是私有事件），传入一个哈希而不是原始值。这样可以让后面的贡献者增加更多数据到事件数据而无需找出并更新事件的每一个处理器。例如：

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

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [25.1](#jquery--dollar-prefix) 使用 `$` 作为存储 jQuery 对象的变量名前缀。jscs: [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [25.2](#jquery--cache) 缓存 jQuery 查询。

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

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [25.3](#jquery--queries) 对 DOM 查询使用层叠 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [25.4](#jquery--find) <a name='25.4'></a> 对有作用域的 jQuery 对象查询使用 `find`。

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

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [26.1](#es5-compat--kangax) 参考 [Kangax](https://twitter.com/kangax/) 的 ES5 [兼容性](http://kangax.github.com/es5-compat-table/).

**[⬆ 返回目录](#table-of-contents)**
