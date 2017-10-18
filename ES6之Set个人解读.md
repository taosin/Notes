# ES6之Set个人解读
参考链接：[ECMAScript 6 入门- Set 和 Map 数据结构](http://es6.ruanyifeng.com/#docs/set-map)

#### 定义：
Set 本身是一个构造函数，用来生成Set数据结构。
Set 是 ES6 中提供的新的数据结构，结构类似于数组，但 Set 中的成员的值都是唯一的，没有重复值。

Set 函数可以将一个数组作为参数，将其初始化。
```js
const arr = [1, 2, 3, 4, 4];
const set = new Set(arr);
// [1, 2, 3, 4, 4];
```

从上述代码可得出一种数组去重的方法：
```js
[... new Set(Array)]
// 去除数组中重复的成员
```

#### Set 实例的属性
* Set 实例有以下两个属性：

-`Set.prototype.constructor`：构造函数，默认就是 Set 函数。

-`Set.prototype.size`：返回 Set 实例的成员总数。


#### Set 实例的方法
* Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。四个操作方法如下：

 -`add(value)`: 添加某个值，返回 Set 结构本身。
 
 -`delete(value)`: 删除某个值，返回一个布尔值，表示删除是否成功。
 
 -`has(value)`: 返回一个布尔值，表示该值是否为Set的成员。
 
 -`clear(value)`: 清楚所有成员，没有返回值。

实例如下：
```js
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2); // true
s.has(2) // false
```
