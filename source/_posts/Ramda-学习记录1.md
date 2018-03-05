---
title: Ramda学习记录1
date: 2018-02-24 17:30:11
tags: [Ramda, technology]
---

## 一. 比较运算

gt：判断第一个参数是否大于第二个参数。

    R.gt(2)(1) // true
    R.gt('a')('z') // false

gte：判断第一个参数是否大于等于第二个参数。

    R.gte(2)(2) // true
    R.gte('a')('z') // false

lt：判断第一个参数是否小于第二个参数。

    R.lt(2)(1) // false
    R.lt('a')('z') // true

lte：判断第一个参数是否小于等于第二个参数。

    R.lte(2)(2) // true
    R.lte('a')('z') // true

equals：比较两个值是否相等（支持对象的比较）。

    R.equals(1)(1) // true
    R.equals(1)('1') // false
    R.equals([1, 2, 3])([1, 2, 3]) // true

    var a = {};
    a.v = a;
    var b = {};
    b.v = b;
    R.equals(a)(b)
    // true
    
eqBy：比较两个值传入指定函数的运算结果是否相等。

    R.eqBy(Math.abs, 5)(-5)
    // true


## 二. 数学运算

  add：返回两个值的和。

      R.add(7)(10) // 17

  subtract：返回第一个参数减第二个参数的差。

      R.subtract(10)(8) // 2

  multiply：返回两个值的积。

      R.multiply(2)(5)  // 10

  divide：返回第一个参数除以第二个参数的商。

      R.divide(71)(100) // 0.71

## 三. 逻辑运算

either：接受两个函数作为参数，只要有一个返回true，就返回true，否则返回false。相当于||运算。

      var gt10 = x => x > 10;
      var even = x => x % 2 === 0;

      var f = R.either(gt10, even);
      f(101) // true
      f(8) // true


both：接受两个函数作为参数，只有它们都返回true，才返回true，否则返回false，相当于&&运算。

      var gt10 = x => x > 10;
      var even = x => x % 2 === 0;

      var f = R.both(gt10, even);
      f(15) // false
      f(30) // true


allPass：接受一个函数数组作为参数，只有它们都返回true，才返回true，否则返回false。

      var gt10 = x => x > 10;
      var even = x => x % 2 === 0;

      var isEvenAndGt10 = R.allPass([gt10, even]);
      isEvenAndGt10(15) // false
      isEvenAndGt10(30) // true  

## 四. 字符串          

split：按照指定分隔符将字符串拆成一个数组。

      R.split('.')('a.b.c.xyz.d')
      // ['a', 'b', 'c', 'xyz', 'd']

test：判断一个字符串是否匹配给定的正则表达式。

      R.test(/^x/)('xyz')
      // true

      R.test(/^y/)('xyz')
      // false

match：返回一个字符串的匹配结果。

      R.match(/([a-z]a)/g)('bananas')
      // ['ba', 'na', 'na']

      R.match(/a/)('b')
      // []

      R.match(/a/)(null)
      // TypeError: null does not have a method named "match"
