# ArrayHandle
简单的数组map搞笑处理方法

>  如果固定查询数量，无论数组多少个元素，这种方法都能保证查询时间是常量的。

```js
var _ = require("underscore")._;

var ah = new ArrayHandle();
// const __ = ah.ArrData.bind(ah);

const SIZE = 2000_0000;
x = 0;
let ary = _.range(SIZE);
console.time('t1');
targets = ah.ArrData(ary).map(v => {
    x++;
    return v + 1;
}).filter(v => {
    x++;
    return v % 2 == 0;
}).reverse().slice(1, 5).take(3).value();
console.timeEnd('t1');
console.log(targets, x, ' time');

```

基于上面的测试数据：
测试代码：
```js
var ah = new ArrayHandle();
const __ = ah.ArrData.bind(ah);

const SIZE = 2000_0000;

ary = _.range(SIZE);

let x = 0;
console.time('origin_method');
targets = ary.map(v => {
    x++;
    return v + 1;
}).filter(v => {
    x++;
    return v % 2 == 0;
}).reverse().slice(1, 4);
console.timeEnd('origin_method');
console.log(targets, x, ' time');


ary = _.range(SIZE);

x = 0;
console.time('t1');
targets = __(ary).map(v => {
    x++;
    return v + 1;
}).filter(v => {
    x++;
    return v % 2 == 0;
}).reverse().slice(1, 5).take(3).value();
console.timeEnd('t1');
console.log(targets, x, ' time');
```
2000万个元素数据
```s
$ node main.js 
origin_method: 1.020s
[ 19999998, 19999996, 19999994 ] 40000000  time
t1: 0.267ms
[ 19999998, 19999996, 19999994 ] 14  time
```
200万个元素数据 
```s
$ node main.js 
origin_method: 98.868ms
[ 1999998, 1999996, 1999994 ] 4000000  time
t1: 0.371ms
[ 1999998, 1999996, 1999994 ] 14  time

```