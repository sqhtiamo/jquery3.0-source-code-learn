# 关于jQuery Core

## 相关文件

    ./src/core.js

## 相关依赖

    "./var/arr",
    "./var/document",
    "./var/getProto",
    "./var/slice",
    "./var/concat",
    "./var/push",
    "./var/indexOf",
    "./var/class2type",
    "./var/toString",
    "./var/hasOwn",
    "./var/fnToString",
    "./var/ObjectFunctionString",
    "./var/support",
    "./core/DOMEval"

## 返回

    jQuery对象

## 作用

    通过init函数new jQuery.fn.init( selector, context );生成jQuery对象

## 重点分析


### 1. jQuery.fn对象

    jQuery.fn = jQuery.prototype = {...}

    声明jQuery的原型对象

### 2. jQuery.extend对象

    作用将若干个对象merge为一个对象。（其他对象不做改变）
    例如：
    var target = {c: 1};
    jQuery.extend(target, {a:1}, {b:2, c:3});
    target = {
        c: 3,
        a: 1,
        b: 2
    }

    jQuery.extend = jQuery.fn.extend = function() {
        var options, name, src, copy, copyIsArray, clone,

            // @target为jQuery.extend的第一个参数，即最终想合并生成的对象
            // 如果没有则为空对象
            target = arguments[ 0 ] || {},
            i = 1,
            length = arguments.length,
            deep = false;

        // @cloughzhang
        // Handle a deep copy situation
        // 若第一个对象为boolean类型
        // 则第一个参数为是否深拷贝（默认为否）
        // 最终生成的target默认为第二个参数
        if ( typeof target === "boolean" ) {
            deep = target;

            // Skip the boolean and the target
            target = arguments[ i ] || {};
            i++;
        }
## 注意的点

### 1. jQuery.fn.init

    该方法并没有在./src/core.js中 而是在./src/core/init.js里。

### 2. ./src/core.js 地位有点特殊

    在生成的dist目录下也有core.js文件。
    在合并生成的最终文件./dist/jQuery.js里，./src/core.js部分代码在最开始的位置。















