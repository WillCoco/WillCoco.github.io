---
title: Ramda-学习记录4
date: 2018-02-26 10:03:06
tags: [Ramda, technology]
---

## 七. 对象

7.1 对象的特征判断

has: 返回一个布尔值，表示对象自身是否具有该属性。

      var hasName = R.has('name')
      hasName({name: 'alice'})   //=> true
      hasName({name: 'bob'})     //=> true
      hasName({})                //=> false

      var point = {x: 0, y: 0};
      var pointHas = R.has(R.__, point); //R.__ 参数占位
      pointHas('x')  // true
      pointHas('y')  // true
      pointHas('z')  // false

hasIn：返回一个布尔值，表示对象自身或原型链上是否具有某个属性。

      function Rectangle(width, height) {
        this.width = width;
        this.height = height;
      }
      Rectangle.prototype.area = function() {
        return this.width * this.height;
      };

      var square = new Rectangle(2, 2);
      R.hasIn('width')(square)  // true
      R.hasIn('area')(square)  // true

propIs：如果属性类型等于给定类型， 返回true。

      R.propIs(Number, 'x', {x: 1, y: 2});  //=> true
      R.propIs(Number, 'x', {x: 'foo'});    //=> false
      R.propIs(Number, 'x', {});            //=> false

propEq：如果属性等于给定值，返回true。单个属性

      var abby = {name: 'Abby', age: 7, hair: 'blond'};
      var fred = {name: 'Fred', age: 12, hair: 'brown'};
      var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
      var alois = {name: 'Alois', age: 15, disposition: 'surly'};
      var kids = [abby, fred, rusty, alois];
      var hasBrownHair = R.propEq('hair', 'brown');
      R.filter(hasBrownHair)(kids) // [fred, rusty]

propOr：定义一个私有属性，如果属性名存在，则显示存在的属性

      var alice = {
        name: 'ALICE',
        age: 101
      };

      var favorite = R.prop('favoriteLibrary');
      var favoriteWithDefault = R.propOr('Ramda', 'favoriteLibrary');
      favorite(alice);  //=> undefined
      favoriteWithDefault(alice);  //=> 'Ramda'

      var favoriteName = R.propOr('Ramda', 'name');
      favoriteName(alice); // => 'ALICE'

whereEq：如果属性等于给定值，返回true。多个属性

      var pred = R.whereEq({a: 1, b: 2});

      pred({a: 1})              // false
      pred({a: 1, b: 2})        // true
      pred({a: 1, b: 2, c: 3})  // true
      pred({a: 1, b: 1})        // false

where：如果各个属性都符合指定条件，返回true。

      var pred = R.where({
        a: R.equals('foo'),
        b: R.complement(R.equals('bar')),
        x: R.gt(__, 10),
        y: R.lt(__, 20)
      });

      pred({a: 'foo', b: 'xxx', x: 11, y: 19}) // true
      pred({a: 'xxx', b: 'xxx', x: 11, y: 19}) // false
      pred({a: 'foo', b: 'bar', x: 11, y: 19}) // false
      pred({a: 'foo', b: 'xxx', x: 10, y: 19}) // false
      pred({a: 'foo', b: 'xxx', x: 11, y: 20}) // false

7.2 对象的过滤

omit：过滤指定属性。

      R.omit(['a', 'd'])({a: 1, b: 2, c: 3, d: 4})
      // {b: 2, c: 3}

filter：返回所有满足条件的属性

      var isEven = n => n % 2 === 0;
      R.filter(isEven)({a: 1, b: 2, c: 3, d: 4}) // {b: 2, d: 4}

reject：返回所有不满足条件的属性

      var isOdd = (n) => n % 2 === 1;
      R.reject(isOdd)({a: 1, b: 2, c: 3, d: 4})
      // {b: 2, d: 4}

7.3 对象的截取

dissoc：过滤掉指定属性。(和omit有什么区别？)

      R.dissoc('b')({a: 1, b: 2, c: 3})
      // {a: 1, c: 3}

assoc：添加或改写某个属性。

      R.assoc('c', 3)({a: 1, b: 2})
      // {a: 1, b: 2, c: 3}

partition：根据属性值是否满足给定条件，将属性分区。

      R.partition(R.contains('s'))({ a: 'sss', b: 'ttt', foo: 'bars' })
      // [ { a: 'sss', foo: 'bars' }, { b: 'ttt' }  ]

pick：返回指定属性组成的新对象

      R.pick(['a', 'd'])({a: 1, b: 2, c: 3, d: 4})
      // {a: 1, d: 4}

      R.pick(['a', 'e', 'f'])({a: 1, b: 2, c: 3, d: 4})
      // {a: 1}

pickAll：与pick类似，但会包括不存在的属性。

      R.pickAll(['a', 'd'])({a: 1, b: 2, c: 3, d: 4})
      // {a: 1, d: 4}

      R.pickAll(['a', 'e', 'f'])({a: 1, b: 2, c: 3, d: 4})
      // {a: 1, e: undefined, f: undefined}

pickBy：返回符合条件的属性

      var isUpperCase = (val, key) => key.toUpperCase() === key;
      R.pickBy(isUpperCase)({a: 1, b: 2, A: 3, B: 4})
      // {A: 3, B: 4}

keys：返回对象自身属性的属性名组成的新数组。

      R.keys({a: 1, b: 2, c: 3}) // ['a', 'b', 'c']

keysIn：返回对象自身的和继承的属性的属性名组成的新数组。

      var F = function() { this.x = 'X'; };
      F.prototype.y = 'Y';
      var f = new F();
      R.keysIn(f) // ['x', 'y']

values：返回对象自身的属性的属性值组成的数组。

      R.values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]

valuesIn：返回对象自身的和继承的属性的属性值组成的数组。

      var F = function() { this.x = 'X'; };
      F.prototype.y = 'Y';
      var f = new F();
      R.valuesIn(f) // ['X', 'Y']

invertObj：将属性值和属性名互换。如果多个属性的属性值相同，只返回最后一个属性。

      var raceResultsByFirstName = {
        first: 'alice',
        second: 'jake',
        third: 'alice',
      };
      R.invertObj(raceResultsByFirstName)
      // {"alice": "third", "jake": "second"}

invert：将属性值和属性名互换，每个属性值对应一个数组。

      var raceResultsByFirstName = {
        first: 'alice',
        second: 'jake',
        third: 'alice',
      };
      R.invert(raceResultsByFirstName)
      // { 'alice': ['first', 'third'], 'jake':['second'] }


7.4 对象的运算

prop：返回对象的指定属性

      R.prop('x')({x: 100})
      // 100

      R.prop('x')({})
      // undefined

map：对象的所有属性依次执行某个函数。

      var double = x => x * 2;
      R.map(double)({x: 1, y: 2, z: 3})
      // {x: 2, y: 4, z: 6}

mapObjIndexed：与map类似，但是会额外传入属性名和整个对象。

      var values = { x: 1, y: 2, z: 3 };
      var prependKeyAndDouble = (num, key, obj) => key + (num * 2);

      R.mapObjIndexed(prependKeyAndDouble)(values)
      // { x: 'x2', y: 'y4', z: 'z6' }

forEachObjIndexed：每个属性依次执行给定函数，给定函数的参数分别是属性值和属性名，返回原对象。

      var printKeyConcatValue = (value, key) => console.log(key + ':' + value);
      R.forEachObjIndexed(printKeyConcatValue)({x: 1, y: 2}) // {x: 1, y: 2}
      // logs x:1
      // logs y:2

merge：合并两个对象，如果有同名属性，后面的值会覆盖掉前面的值。

      R.merge({ 'name': 'fred', 'age': 10 })({ 'age': 40 })
      // { 'name': 'fred', 'age': 40 }

      var resetToDefault = R.merge(R.__, {x: 0});
      resetToDefault({x: 5, y: 2}) // {x: 0, y: 2}

mergeWith：合并两个对象，如果有同名属性，会使用指定的函数处理。

      R.mergeWith(
        R.concat,
        { a: true, values: [10, 20] },
        { b: true, values: [15, 35] }
      );
      // { a: true, b: true, values: [10, 20, 15, 35] }

eqProps：比较两个对象的指定属性是否相等。

      var o1 = { a: 1, b: 2, c: 3, d: 4 };
      var o2 = { a: 10, b: 20, c: 3, d: 40 };
      R.eqProps('a', o1)(o2) // false
      R.eqProps('c', o1)(o2) // true

R.evolve：对象的属性分别经过一组函数的处理，返回一个新对象。

      var tomato  = {
        firstName: '  Tomato ',
        data: {elapsed: 100, remaining: 1400},
        id: 123
      };
      var transformations = {
        firstName: R.trim,
        lastName: R.trim, // 不会被调用
        data: {elapsed: R.add(1), remaining: R.add(-1)}
      };
      R.evolve(transformations)(tomato)
      // {
      //   firstName: 'Tomato',
      //   data: {elapsed: 101, remaining: 1399},
      //   id: 123
      // }


7.5 复合对象

path：取出数组中指定路径的值。

      R.path(['a', 'b'], {a: {b: 2}}) // 2
      R.path(['a', 'b'], {c: {b: 2}}) // undefined

pathEq：返回指定路径的值符合条件的成员

      var user1 = { address: { zipCode: 90210 } };
      var user2 = { address: { zipCode: 55555 } };
      var user3 = { name: 'Bob' };
      var users = [ user1, user2, user3 ];
      var isFamous = R.pathEq(['address', 'zipCode'], 90210);
      R.filter(isFamous)(users) // [ user1 ]

assocPath：添加或改写指定路径的属性的值。

      R.assocPath(['a', 'b', 'c'], 42)({a: {b: {c: 0}}})
      // {a: {b: {c: 42}}}

      R.assocPath(['a', 'b', 'c'], 42)({a: 5})
      // {a: {b: {c: 42}}}
