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

    通过init函数new jQuery.fn.init( selector, context );生成jQuery实例华函数

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

        // @cloughzhang
        // 若target不是Object，则设target为{}
        // Handle case when target is a string or something (possible in deep copy)
        if ( typeof target !== "object" && !jQuery.isFunction( target ) ) {
            target = {};
        }

        // @cloughzhang
        // 如果传入的Obejct只有一个，则target就为jQuery对象
        // 为什么会有这个feature？单纯为了扩展jQuery对象吗?
        // Extend jQuery itself if only one argument is passed
        if ( i === length ) {
            target = this;
            i--;
        }

        // @cloughzhang
        // extend的核心部分
        for ( ; i < length; i++ ) {

            // Only deal with non-null/undefined values
            if ( ( options = arguments[ i ] ) != null ) {

                // Extend the base object
                for ( name in options ) {
                    src = target[ name ];
                    copy = options[ name ];

                    // @cloughzhang
                    // 避免循环引用
                    // Prevent never-ending loop
                    if ( target === copy ) {
                        continue;
                    }

                    // @cloughzhang
                    // 深拷贝
                    // 区分array和Obecjt两种情况
                    // target的属性要和需要copy的type保持一致
                    // target原来属性消失
                    // 例如：
                    // var taret = {a: {b:1}}
                    // jQuery.extend(true, target, {a:[1,2,3]})
                    // return   Object {a: Array[3]}  b没有了
                    // copy会覆盖target

                    // Recurse if we're merging plain objects or arrays
                    if ( deep && copy && ( jQuery.isPlainObject( copy ) ||
                        ( copyIsArray = jQuery.isArray( copy ) ) ) ) {

                        if ( copyIsArray ) {
                            copyIsArray = false;
                            clone = src && jQuery.isArray( src ) ? src : [];

                        } else {
                            clone = src && jQuery.isPlainObject( src ) ? src : {};
                        }

                        // @cloughzhang
                        // 递归extend
                        // Never move original objects, clone them
                        target[ name ] = jQuery.extend( deep, clone, copy );

                    // @cloughzhang
                    // 浅拷贝
                    // Don't bring in undefined values
                    } else if ( copy !== undefined ) {
                        target[ name ] = copy;
                    }
                }
            }
        }

        // Return the modified object
        return target;
    };

### 3. 利用jQuery.extend扩展jQuery对象

    只传入了一个Object参数


## 注意的点

### 1. jQuery.fn.init

    该方法并没有在./src/core.js中 而是在./src/core/init.js里。

### 2. ./src/core.js

    在生成的dist目录下也有core.js文件。
    在合并生成的最终文件./dist/jQuery.js里，./src/core.js部分代码在最开始的位置。















