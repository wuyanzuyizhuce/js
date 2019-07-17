# 2.JS方法速查

**近期有两个JS函数式编程库：[Underscore](https://www.html.cn/doc/underscore/#each) 和 [lodashjs](https://www.lodashjs.com/)，这两者的源码看懂之后，将会大幅度的提高JS方法速查中的方法和质量，现在里面的方法不是很好，敬请期待** 

## 🌲数组

### 1.数组去重

```js
let arrs = [1,2,2,3,3,6,5,5];

// ES6
[...new Set(arr)] // [1,2,3,6,5]
// 此方法也能去除数组中重复的项：[...new Set('ababbc')].join('') // abc

// 其他方法
function uniq(array){
    let temp = [];
    let l = array.length;
    for(let i = 0; i < l; i++) {
        for(let j = i + 1; j < l; j++){
            if (array[i] === array[j]){
                i++;
                j = i;
            }
        }
        temp.push(array[i]);
    }
    return temp;
}
console.log(uniq(arrs)); // [1,2,3,6,5]
```

**数组去重拓展（传参数 指定去除哪个重复的，未完成）**

---

### 2.数组合并

```js
let arr1 = [1,2,3]
let arr2 = [4,5,6]

// ES6
[...arr1, ...arr2] // [1, 2, 3, 4, 5, 6]


// 方法2：concat方法（挂载Array原型链上）
let c = a.concat(b);
console.log(c); // [1, 2, 3, 4, 5, 6]
console.log(a); // [1, 2, 3]  不改变本身
// 备注：看似concat似乎是 数组对象的深拷贝，其实，concat 只是对数组的第一层进行深拷贝

// 方法3：apply方法
Array.prototype.push.apply(a, b);
console.log(a); // [1, 2, 3, 4, 5, 6] 改变原目标数组
console.log(b); // [4, 5, 6]
```

---

### 3.数组排序（sort）

```js
let objArr = [
  {name: 'test1', age: 20},
  {name: 'test1', age: 22},
  {name: 'test1', age: 21}
]

// 第一参数a， 第二参数b ---> a-b升序（从小到大）;b-a降序（从大到小），原理就是 两数计算，如果返回的是负数，就保留前者（我可能说的不对，欢迎纠正）
objArr.sort((a, b) => {
  return a.age - b.age
}) 
// 结果会按照年龄从小到大的顺序排列


```

---

### 4.多维数组转一维数组（flat）

```js
let arr = [1, [2], [[3], 4], 5];

// ES6 数组的flat()
arr.flat() // [1, 2, Array(1), 4, 5] 如果这样写的话只能展开二维数组，但是可以加参数Infinity，就是能展开多维数组
arr.flat(Infinity) // [1, 2, 3, 4, 5] 注意如果原数组有空位，flat()方法会跳过空位

// 其他方法
const deepFlatten = arr => [].concat(...arr.map(v => (Array.isArray(v) ? deepFlatten(v) : v)));
deepFlatten(arr); // [1,2,3,4,5]


// 执行效率验证（拓展）
// let start = new Date().getTime();
// console.log('reduceDimension: ', deepFlatten([1, [2], [[3], 4], 5]);
// console.log('耗时: ', new Date().getTime() - start); // *ms

// ES6 数组的flatMap() 方法大家可以自行查阅一下，拓展下自己的知识面
```

---

### 5.过滤数组（filter）

```js
let json = [
  { id: 1, name: 'john', age: 24 },
  { id: 2, name: 'zkp', age: 21 },
  { id: 3, name: 'mike', age: 50 }
];

// ES6
json.filter( item => item.age > 22) // [{id: 1, name: 'john', age: 24}, { id: 3, name: 'mike', age: 50 }]

// ES5

```

---

### 6.判断数组中的项是否满足于某个条件（some，every）

```javascript
let arr = [4, 2, 3]

// ES6 some方法（有符合）
arr.some( item => item > 1) // true
arr.some( item => item > 3) // true

// ES5 every（全符合）

arr.every(item => item > 1) // true
arr.every(item => item > 3) // false

// 注意：上面两个有不同哦，一个是有符合的判定，一个是全符合的判定
```

---

### 7.操作数组中的每一项，并使其按照一定的逻辑返回（map）

```js
var potatos = [
  { id: '1001', weight: 50 },
  { id: '1002', weight: 80 },
  { id: '1003', weight: 120 },
  { id: '1004', weight: 40 }
]

// ES6写法
const fn = (arr, key) => arr.map(arr =>  arr[key])

fn(potatos, 'id') // ["1001", "1002", "1003", "1004"]
fn(potatos, 'weight') // [50, 80, 120, 40]
```

---

### 8.其他常用的ES6 Array方法

```js
// forEach() 遍历数组

// pop() 删除数组中最后一个元素，并返回该元素的值。此方法更改数组的长度

// shift() 删除数组中第一个元素，并返回该元素的值。此方法更改数组的长度

// push() 将一个或多个元素添加到数组的末尾，并返回该数组的新长度

// unshift() 将一个或多个元素添加到数组的开头，并返回该数组的新长度(该方法修改原有数组)

// 🔥Array.prototype.filter() 创建一个新数组, 其包含通过所提供函数实现的测试的所有元素，不会改变原有值，如果没符合的返回[]
let arr = [1, 2, 3]
arr.filter( x => x > 1) // [2, 3]

// Array.prototype.join() 将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串
['Fire', 'Air', 'Water'].join() // "Fire,Air,Water"

// Array.prototype.slice() 取出任意元素, | 参数一：从哪开始，参数二（可选）结束位置，不选的话 就节选到最后了
[1, 2, 3].slice(0, 1) // [1]

// Array.prototype.splice() 删除任意元素，操作任意元素 | 参数一：从哪开始 | 参数二：操作元素的个数 | 参数三：插入元素的值...（可以写多个参数三）
[1, 2, 3].splice(0, 1) // 删除 [2, 3]

// Array.prototype.includes() 用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。
[1, 2, 3].includes(1) // true

// Array.prototype.reverse() 颠倒数组
[1, 2, 3].reverse() // [3, 2, 1]

```

[点击此处，查看更详细链接](https://zhukunpenglinyutong.github.io/2.note/3.JavaScript/4.ES6%E4%BB%A5%E5%8F%8AJS%E5%86%85%E7%BD%AE%E6%96%B9%E6%B3%95%E6%A2%B3%E7%90%86.html#%F0%9F%8C%B2%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95%E6%A2%B3%E7%90%86)

---

### 9.获得数组最大最小值

```js

// 使用 Math 中的 max/min 方法
let arr = [22,13,6,55,30];

// ES6
Math.max(...arr); // 55
Math.min(...arr); // 6

// ES5
Math.max.apply(null, arr); // 55
Math.min.apply(null, arr); // 6

```

---

### 10.获取数组交集

```js
// ES6 写法
const similarity = (arr1, arr2) => arr1.filter(v => arr2.includes(v));
similarity([1, 2, 3], [1, 2, 4]); // [1,2]

// ES5 写法
// function similarity(arr1, arr2) {
//   return arr2.filter(function(v) {
//     return arr1.includes(v)
//   })
// }
```

---

### 11.数组对象去重

```js
let arr = [
  {id: 1, name: 'Jhon1'},
  {id: 2, name: 'sss'},
  {id: 3, name: 'Jhon2'},
  {id: 4, name: 'Jhon3'}
]

// ES6
const uniqueElementsBy = (arr, fn) =>arr.reduce((acc, v) => {if (!acc.some(x => fn(v, x))) acc.push(v);return acc;}, []);

// 下面的示例表示，去重依据是 id ，就是 id一样的，只留下一个
uniqueElementsBy(arr, (a, b) => a.id === b.id) // [{id: 1, name: 'Jhon1'}, {id: 2, name: 'sss'}]

```

---

### 12.数组乱序

```js
function shuffle(arr) {
  let array = arr
  let index = array.length

  while (index) {
    index -= 1
    let randomInedx = Math.floor(Math.random() * index)
    let middleware = array[index]
    array[index] = array[randomInedx]
    array[randomInedx] = middleware
  }

  return array
}

let arr = [1,2,3,4,5]
shuffle(arr) // [3, 4, 2, 5, 1] 结果不定
```

**还有更简单的方式，欢迎来撩**

---

### 13.检查数组中某元素出现的次数

```js
function countOccurrences(arr, value) {
  return arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
}

let arr = [1,2,3,4,1,2,4]
countOccurrences(arr, 1) // 2
```

---

### 14.检查数组中的所有元素是否相等

```javascript
const allEqual = arr => arr.every(val => val === arr[0]);

// 示例
allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```

---

### 15.数组对象，求某一列属性的总和

```js
var potatos = [
  { id: '1001', weight: 50 },
  { id: '1002', weight: 80 },
  { id: '1003', weight: 120 },
  { id: '1004', weight: 40 }
]

// ES6写法
const fn = (arr, key) => arr.reduce((sum, p) => { return p[key] + sum },0)

fn(potatos, 'weight') // 290
fn(potatos, 'id') // "10041003100210010" 字符串相加就是这个结果，如果有各自的需求，可以自己加上
```

---

### 16.分割数组，并操作数组每一项（函数）

```js
/**
 * 数组分隔方法，并且可以传入一个处理函数，用来分隔之前处理数组的每一项
 * 
 * @category Array
 * @param {Array} array 需要处理的数组
 * @param {number} [size = 1] 每个数组区块的长度
 * @param {Function} [fn = item => item] 函数
 * @returns {Array} 返回一个包含拆分区块的新数组（相当于一个二维数组）。
 * @example
 *
 * chunk(['a', 'b', 'c', 'd'], 2)
 * // => [['a', 'b'], ['c', 'd']]
 *
 * chunk(['a', 'b', 'c', 'd'], 3)
 * // => [['a', 'b', 'c'], ['d']]
 *
 * chunk([1, 2, 3, 4], 3, item => item * 2)
 * // => [[2, 4, 6], [8]]
 */

function chunk(array, size = 1, fn = item => item) {
    
    array = array.map(fn)

    size = Math.max(size, 0) // 这一句就很妙，当传入值小于0的时候，置为0，大于0的时候，不写，但不知道性能怎么样
    const length = array == null ? 0 : array.length
    if (!length || size < 1) {
      return []
    }
    let index = 0
    let resIndex = 0
    const result = new Array(Math.ceil(length / size))
  
    while (index < length) {
      result[resIndex++] = array.slice(index, (index += size))
    }
    return result
}
```

---

## 🕰时间

### 1.Date 常用API

```js
new Date() // 创建一个时间对象 Fri Jul 12 2019 19:59:59 GMT+0800 (中国标准时间)

// 返回自1970年1月1日 00:00:00 UTC到当前时间的毫秒数。
Date.now(); // 1562932828164

// 解析一个表示某个日期的字符串，并返回从1970-1-1 00:00:00 UTC 到该日期对象（该日期对象的UTC时间）的毫秒数
Date.parse('2019.7.12') // 1562860800000

// 年月日时分秒 获取
let dateMe = new Date()

dateMe.getFullYear() // 2019 | 根据本地时间返回指定日期的年份
dateMe.getMonth() // 6 | 根据本地时间，返回一个指定的日期对象的月份，为基于0的值（0表示一年中的第一月）。
dateMe.getDate() // 12 | 根据本地时间，返回一个指定的日期对象为一个月中的哪一日（从1--31）
dateMe.getHours() // 20 |根据本地时间，返回一个指定的日期对象的小时。
dateMe.getMinutes() // 11 | 根据本地时间，返回一个指定的日期对象的分钟数。
dateMe.getSeconds() // 29 | 方法根据本地时间，返回一个指定的日期对象的秒数
dateMe.getMilliseconds() // 363 | 根据本地时间，返回一个指定的日期对象的毫秒数。

dateMe.toJSON() // 🔥 "2019-07-12T12:05:15.363Z" | 返回 Date 对象的字符串形式
dateMe.getDay() // 5 | 根据本地时间，返回一个具体日期中一周的第几天，0 表示星期天（0 - 6）
dateMe.getTime() // 1562933115363 | 方法返回一个时间的格林威治时间数值。
dateMe.toString() // "Fri Jul 12 2019 20:05:15 GMT+0800 (中国标准时间)" | 返回一个字符串，表示该Date对象
dateMe.getTimezoneOffset() // -480（说明比正常时区慢480分钟，所以要加480分钟才对） | 返回协调世界时（UTC）相对于当前时区的时间差值，单位为分钟。
dateMe.toDateString() // "Fri Jul 12 2019" | 以美式英语和人类易读的形式返回一个日期对象日期部分的字符串。

```

[MDN 更多详细](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)

---

### 2.一个时间戳格式的数字，是多少 天小时分钟秒毫秒

```javascript
const formatDuration = ms => {
  if (ms < 0) ms = -ms;
  const time = {
    day: Math.floor(ms / 86400000),
    hour: Math.floor(ms / 3600000) % 24,
    minute: Math.floor(ms / 60000) % 60,
    second: Math.floor(ms / 1000) % 60,
    millisecond: Math.floor(ms) % 1000
  };
  return Object.entries(time)
    .filter(val => val[1] !== 0)
    .map(([key, val]) => `${val} ${key}${val !== 1 ? 's' : ''}`)
    .join(', ');
};

formatDuration(3161012); // 52 minutes, 41 seconds, 12 milliseconds
```

---

### 3.格林尼治时间 转 北京时间（可传格林尼治时间 或者 时间戳）

```javascript
function myTimeToLocal(inputTime){
	if(!inputTime && typeof inputTime !== 'number'){
		return '';
	}
	let localTime = '';
	inputTime = new Date(inputTime).getTime();
	const offset = (new Date()).getTimezoneOffset();
	localTime = (new Date(inputTime - offset * 60000)).toISOString();
	localTime = localTime.substr(0, localTime.lastIndexOf('.'));
	localTime = localTime.replace('T', ' ');
	return localTime;
}

console.log(myTimeToLocal(1530540726443)); // 2018-07-02 22:12:06
console.log(myTimeToLocal('2017-11-16T05:23:20.000Z')); // 2017-11-16 13:23:20
```

---

### 4.获取两个日期相差天数

```js
function getDaysDiffBetweenDates (dateInitial, dateFinal) {
    return (dateFinal - dateInitial) / (1000 * 3600 * 24);
}

getDaysDiffBetweenDates(new Date('2017-12-13'), new Date('2017-12-22')); // 9
```

---

### 5.一个数组中，有时间，需要将这个数组按照时间进行排序


```javascript
let data = [
{
  id: 1,
  publishTime: "2019-05-14 18:10:29"
},
{
  id: 2,
  publishTime: "2019-05-14 18:17:29"
},
{
  id: 3,
  publishTime: "2019-05-14 15:09:25"
}]

data.sort((a, b) => b.publishTime - a.publishTime);

// 0: {id: 2, publishTime: "2019-05-14 18:17:29"}
// 1: {id: 1, publishTime: "2019-05-14 18:10:29"}
// 2: {id: 3, publishTime: "2019-05-14 15:09:25"}

```

---

## 👫对象

### 1.对象合并

##### ES6方法

```js

let obj1 = {
    a:1,
    b:{
        b1:2
    }
}

let obj2 = {
    c:3,
    d:4
}
console.log({...obj1, ...obj2}) // {a: 1, b: {…}, c: 3, d: 4}

// 支持无限制合并，但如果对象之间存在相同属性，则后面属性会覆盖前面属性。*请注意，这仅适用于浅层合并。
```



##### 方法一：Obj.assign()：可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象

```javascript

let o1 = { a: 1 };
let o2 = { b: 2 };

let obj = Object.assign(o1, o2);
console.log(obj); // { a: 1, b: 2 }
console.log(o1);  // { a: 1, b: 2 }, 且 **目标对象** 自身也会改变（也就是assign第一个对象）
console.log(o2); // { b: 2 } 不改变

// 备注：Object.assign() 拷贝的是属性值。假如源对象的属性值是一个指向对象的引用，它也只拷贝那个引用值
// 备注：数组合并用 concat() 方法
```

##### 方法二：$.extend()（抽时间看一下实现原理）

---

### 2.浅拷贝，深拷贝

##### 方法一：浅拷贝：意思就是只复制引用（指针），而未复制真正的值

https://www.cnblogs.com/dabingqi/p/8502932.html

```javascript

// 最简单的利用 = 赋值操作符实现了一个浅拷贝
let obj1 = {
  name: '朱昆鹏'
}

let obj2 = obj1; // 完成了浅拷贝

```

##### 方法二：深拷贝：目标的完全拷贝（只要进行了深拷贝，它们老死不相往来，谁也不会影响谁）

```javascript
// 目前实现深拷贝的方法不多，主要是两种：
// 01：利用递归来实现每一层都重新创建对象并赋值（推荐使用）
// 02：利用 JSON 对象中的 parse 和 stringify（有些属性会被忽略，所以不能使用）


// 01递归方法案例：对每一层的数据都实现一次 创建对象->对象赋值 的操作
function deepClone(source){
  const targetObj = source.constructor === Array ? [] : {}; // 判断复制的目标是数组还是对象
  for(let keys in source){ // 遍历目标
    if(source.hasOwnProperty(keys)){
      if(source[keys] && typeof source[keys] === 'object'){ // 如果值是对象，就递归一下
        targetObj[keys] = source[keys].constructor === Array ? [] : {};
        targetObj[keys] = deepClone(source[keys]);
      }else{ // 如果不是，就直接赋值
        targetObj[keys] = source[keys];
      }
    }
  }
  return targetObj;
}

const obj1 = {
  name:'axuebin',
  sayHello:function(){
    console.log('Hello World');
  }
}
const obj2 = deepClone(obj1);
console.log(obj2); // {name: "axuebin", sayHello: ƒ}
console.log(obj1 === obj2); // false


// 02方法示例
const obj3 = {
  name:'axuebin',
  sayHello:function(){
    console.log('Hello World');
  }
}
console.log(JSON.parse(JSON.stringify(obj3)); // {name: "axuebin"} ???
// undefined、function、symbol 会在转换过程中被忽略，所以就不能用这个方法进行深拷贝

```

### 3.拓展：首层浅拷贝

```javascript

function shallowClone(source) {
  const targetObj = source.constructor === Array ? [] : {}; // 判断复制的目标是数组还是对象
  for (let keys in source) { // 遍历目标
    if (source.hasOwnProperty(keys)) {
      targetObj[keys] = source[keys];
    }
  }
  return targetObj;
}

const originObj = {
  a:'a',
  b:'b',
  c:[1, 2, 3],
  d:{ dd: 'dd' }
};

const cloneObj = shallowClone(originObj);
console.log(cloneObj === originObj); // false
cloneObj.a = 'aa';
cloneObj.c = [1, 1, 1];
cloneObj.d.dd = 'surprise';

console.log(cloneObj); // {a:'aa',b:'b',c:[1,1,1],d:{dd:'surprise'}}
console.log(originObj); // {a:'a',b:'b',c:[1,2,3],d:{dd:'surprise'}}

```

---

### 4.判断对象是否为空对象

```javascript

// 参考：https://www.cnblogs.com/HKCC/p/6083575.html

if (JSON.stringify(对象) === '{}') {
  console.log('空');
}

```

---

### 5.判断对象中属性的个数

```js
let obj = {name: '朱昆鹏', age: 21}

// ES6
Object.keys(obj).length // 2

// ES5
let attributeCount = obj => {
    let count = 0;
    for(let i in obj) {
        if(obj.hasOwnProperty(i)) {  // 建议加上判断,如果没有扩展对象属性可以不加
            count++;
        }
    }
    return count;
}

attributeCount(obj) // 2
```

---

### 6.JS 对象转 url 查询字符串

```js
const objectToQueryString = (obj) => Object.keys(obj).map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(obj[key])}`).join('&');
objectToQueryString({name: 'Jhon', age: 18, address: 'beijing'})
// name=Jhon&age=18&address=beijing
```

---

### 7.对象遍历

```js
let objs = {
    1: {
        name: '朱昆鹏'
    },
    2: {
        name: '林雨桐'
    }
}

Object.keys(objs).forEach( ket => {
  console.log(key,objs[key])
})

// 1 {name: '朱昆鹏'} 2 {nama:'林雨桐'}

```

---

### 8.字符串与对象相互转换

[链接](https://mmww1024.iteye.com/blog/2427471)

---

## 🌴DOM

### 1.判断当前位置是否为页面底部

```js
function bottomVisible() {
  return document.documentElement.clientHeight + window.scrollY >= (document.documentElement.scrollHeight || document.documentElement.clientHeight)
}

bottomVisible() // 返回值为true/false
```

---

### 2.全屏

##### 1.进入全屏

```js
function launchFullscreen(element) {
  if (element.requestFullscreen) {
    element.requestFullscreen()
  } else if (element.mozRequestFullScreen) {
    element.mozRequestFullScreen()
  } else if (element.msRequestFullscreen) {
    element.msRequestFullscreen()
  } else if (element.webkitRequestFullscreen) {
    element.webkitRequestFullScreen()
  }
}

launchFullscreen(document.documentElement) // 整个页面进入全屏
launchFullscreen(document.getElementById("id")) //某个元素进入全屏
```

##### 退出全屏

```js
function exitFullscreen() {
  if (document.exitFullscreen) {
    document.exitFullscreen()
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen()
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen()
  } else if (document.webkitExitFullscreen) {
    document.webkitExitFullscreen()
  }
}

exitFullscreen()
```

##### 全屏事件

```js
document.addEventListener("fullscreenchange", function (e) {
  if (document.fullscreenElement) {
    console.log('进入全屏')
  } else {
    console.log('退出全屏')
  }
})
```

---

### 3.判断dom元素是否具有某个className

方法一：使用HTML5新增classList 来操作类名

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="test" class="te"></div>

  <script>
    
    let div = document.getElementById('test')
    console.log(div.classList.contains("te")) // true

  </script>
</body>
</html>
```
拓展：
- classList.add(newClassName)；添加新的类名，如已经存在，取消添加
- classList.contains(oldClassName)：确定元素中是否包含指定的类名，返回值为true，false
- classList.remove(oldClassName)：移除已经存在的类名;
- classList.toggle(className)：如果classList中存在给定的值，删除它，否则，添加它；

🦀**感谢掘金用户tjNane分享此方法**

---

方法二：

```js
const  hasClass = (el, className) => new RegExp(`(^|\\s)${className}(\\s|$)`).test(el.className);
```

---

## 🌱BOM

### 1.返回当前网页地址

```js
function currentURL() {
  return window.location.href
}

currentURL() // "https://juejin.im/timeline"
```

---

### 2.获取滚动条位置

```js
function getScrollPosition(el = window) {
  return {
    x: (el.pageXOffset !== undefined) ? el.pageXOffset : el.scrollLeft,
    y: (el.pageYOffset !== undefined) ? el.pageYOffset : el.scrollTop
  }
}

getScrollPosition() // {x: 0, y: 692}
```

---

### 3.获取url中的参数

```js
function getURLParameters(url) {
  const params = url.match(/([^?=&]+)(=([^&]*))/g)
  return params?params.reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  ):[]
}

getURLParameters('http://www.baidu.com/index?name=tyler') // {name: "tyler"}
```

---

### 4.检测设备类型

```js
const detectDeviceType = () =>/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|OperaMini/i.test(navigator.userAgent) ? 'Mobile' : 'Desktop';

detectDeviceType() // "Desktop"
```

---

## 💍处理JS原生具有的一些问题

### 1.加减法精度缺失问题

```javascript
// 加法函数（因为JS小数计算 丢失精度）
function add(arg1, arg2) { 
    let r1, r2, m; 
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 } 
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 } 
    m = Math.pow(10, Math.max(r1, r2)) 
    return (arg1 * m + arg2 * m) / m 
}
```

```javascript
// 减法函数（因为JS小数计算 丢失精度）
function sub(arg1, arg2) { 
    let r1, r2, m, n; 
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 } 
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 } 
    m = Math.pow(10, Math.max(r1, r2)); 
    n = (r1 >= r2) ? r1 : r2; 
    return Number(((arg1 * m - arg2 * m) / m).toFixed(n)); 
}
```

---

### 2.递归优化（尾递归）

```javascript
// 尾递归函数 摘自阮一峰ES6 | 自己懒得写了
function tco(f) {
  let value;
  let active = false;
  let accumulated = [];

  return function accumulator() {
    accumulated.push(arguments);
    if (!active) {
      active = true;
      while (accumulated.length) {
        value = f.apply(this, accumulated.shift());
      }
      active = false;
      return value;
    }
  };
}

// 使用
新的函数 = tco(递归函数)

```

---

### 3.防抖 && 节流（此处不是最好，可以再进行优化）

**防抖：任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行（例如搜索框，只有当用户搜索输入完成之后，停个零点几秒才会服务器发送请求，如果停顿的时间小于设定的，那就还不执行，这个就是防抖）**

**函数防抖就是在函数需要频繁触发情况时，只有足够空闲的时间，才执行一次。经常用于 搜索和拖拽**

```js
function debounce(fn) {
  let timeout = null; // 创建一个标记用来存放定时器的返回值
  return function () {
    clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
    timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
      fn.apply(this, arguments);
    }, 500);
  };
}

debounce(fn) // 使用

```

---

**节流：指定时间间隔内只会执行一次任务（例如一个一秒内连续触发的事件，如果用了节流，让其300ms触发一次，那么1s就只能触发3次）**

```js
function throttle(fn) {
  let canRun = true; // 通过闭包保存一个标记
  return function () {
    if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
    canRun = false; // 立即设置为false
    setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
      fn.apply(this, arguments);
      // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
      canRun = true;
    }, 500);
  };
}

throttle(fn) // 使用
```

[节流和防抖其他好的文章](https://juejin.im/entry/58c0379e44d9040068dc952f)

---

**防抖节流其他实现**

```js
/**
 * Difference between debounce and throttle
 * In summary, 
 * throttle says, “Hey, we heard you the first time, but if you want to keep going, no problem. 
 * We’ll just ignore you for a while.” 
 * Debounce says, “Looks like you’re still busy. No problem, 
 * we’ll wait for you to finish!”
 */
/**
 * 快速书写一个防抖函数
 * @description 只要一直调用, callback 将不会被触发
 * 在一次调用结束后, 只有等待 timeout ms 时间, 才能继续调用 callback
 * immediate 决定触发时机
 * @example 
 * 1. 点击按钮发送请求（保存数据之类的）
 * 2. 搜索时自动联想
 * 3. 自动保存
 * 4. Debouncing a resize/scroll event handler
 */
function debounce(callback, timeout, immediate) {
  let timer;
  return function () {
    const context = this; // 持有执行上下文
    const args = arguments; // 记录传参
    const later = function () {
      timer = null; // 贤者时间过了，重振旗鼓，重置为初始状态
      if (!immediate) callback.apply(context, args); // 设置为尾部调用才延时触发
    }
    const callNow = immediate && !timer; // 如果确认允许首部调用，且首次调用，那么本次立即触发
    clearTimeout(timer); // 杀掉上次的计时器，重新计时
    timer = setTimeout(later, timeout); // 重启一个计时器，过了贤者时间之后才触发
    callNow && callback.apply(context, args); // 设置为首部调用立即触发
  }
}

/**
 *  快速书写一个节流函数
 * @description 一直调用 callback, 每隔 timeout ms 时间 callback 触发一次
 * 在 timeout ms 时间内的调用将不触发
 * @example
 * 1. Throttling a button click so we can’t spam click 控制疯狂按钮的响应频率
 * 2. Throttling an API call 控制 API 的调用频率
 * 3. Throttling a mousemove/touchmove event handler 控制频繁触发事件的相应频率
 */
// solution1 记录时间比较
function throttle(callback, timeout) {
  let triggerTime; // 记录每次真正触发时间
  return function () {
    const context = this; // 持有执行上下文
    const args = arguments; // 记录传参
    if (triggerTime === undefined // 首次调用
      || Date.now() - triggerTime > timeout) { // 贤者时间已经过去
      triggerTime = Date.now(); // 记录真正触发时间
      callback.apply(context, args); // 可以触发回调
    }
  }
}
// solution2 间隔时间反转标志位
function throttle(callback, timeout) {
  let disable; // 触发回调是否禁用
  return function () {
    const context = this; // 持有执行上下文
    const args = arguments; // 记录传参
    if (!disable) { // 首次调用或者贤者时间过了，禁用解除
      callback.apply(context, args); // 可以触发回调
      disable = true; // 马上禁用
      setTimeout(_ => disable = false, timeout); // 贤者时间过了，禁用解除
    }
  }
}
```

---

## 🙏其他

### 1.Math函数的一些应用

```js
parseInt(5.12) // 5 | 只保留整数部分（丢弃小数部分）
Math.floor(5.12) // 5 | 向下取整（效果和parseInt一样）
Math.ceil(5.12) // 6 | 向上取整（有小数，整数就+1）

Math.round(5.499) // 5 | 四舍五入
Math.round(5.501) // 6 | 四舍五入

Math.abs(-5) // 5 | 绝对值

Math.max(5, 6) // 6 | 返回两者中较大的数
Math.min(5, 6) // 5 | 返回两者中较小的数

Math.random() // 随机数 (0-1)
```

---

### 2.常用正则速查

**消除字符串首尾两端的空格（替换）**

```js
let reg = /^\s+|\s+$/g;

let str = ' #id div.class '; 
str.replace(reg, '') // "#id div.class"
```

**把手机号码替换成 *（替换）**

```js
var reg = /1[24578]\d{9}/;

var str = '姓名：朱昆鹏 手机：15932638907'; // 手记号瞎写的
str.replace(reg, '***') //"姓名：朱昆鹏 手机：***"
```

**替换敏感字（替换）**

```js
let str = '中国共产党中国人民解放军中华人民共和国';
    
let r = str.replace(/中国|军/g, input => {
    let t = '';
    for (let i = 0; i<input.length; i++) {
        t += '*';
    }
    return t;
})
     
console.log(r); //**共产党**人民解放*中华人民共和国   
```

**千位分隔符（替换）**

```js
let reg = /(\d)(?=(?:\d{3})+$)/g

let str = '100002003232322';    
let r = str.replace(, '$1,'); //100,002,003,232,322
```

**匹配网页标签（匹配）**

```js
var reg = /<(.+)>.+<\/\1>/;

var str = '朱昆鹏<div>2707509@.qq.com</div>朱昆鹏';    
str.match(reg); // ["<div>2707509@.qq.com</div>"]
```

**验证手记号（验证）**

```js
let reg = /^1((3[\d])|(4[5,6,9])|(5[0-3,5-9])|(6[5-7])|(7[0-8])|(8[1-3,5-8])|(9[1,8,9]))\d{8}$/;

reg.test('15932539095'); //true
reg.test('234554568997'); //false
```

**验证邮箱地址（验证）**

```js
let reg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/

reg.test('2775033@hotmail.com'); //true
reg.test('abc@'); //false
```

**验证身份证（验证）**

```js
let reg = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/

reg.test('31210119651230518X'); //true 自己瞎写的
reg.test('2101196523s230518X'); //false 自己瞎写的
```

**验证中国邮箱编码（验证）**

```js
let reg = /^(0[1-7]|1[0-356]|2[0-7]|3[0-6]|4[0-7]|5[1-7]|6[1-7]|7[0-5]|8[013-6])\d{4}$/

reg.test('065900'); //true
reg.test('999999'); //false
```

**验证ipv4地址正则（验证）**

```js
let reg = /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/

reg.test('192.168.1.192'); //true
reg.test('127.0.0.1s'); //false
```

**验证银行卡号（16或19位）**

```js
let reg = /^([1-9]{1})(\d{15}|\d{18})$/

reg.test('6222026006705354218') // true
```

**验证中文姓名（验证）**

```js
let reg = /^([\u4e00-\u9fa5\·]{2,10})$/

reg.test('朱昆鹏'); //true
reg.test('Zhu Kunpeng'); //false
```

---

### 3.变换变量

```js

// [letA，letB] = [letB，letA];

let a = 1;
let b = 2;
[a, b] = [b, a] // a = 2 b = 1

```

---

### 4.格式化对象为JSON代码

```js
const formatted = JSON.stringify({name: 'Jhon', age: 18, address: 'sz'}, null, 4);
/*
{
    "name": "Jhon",
    "age": 18,
    "address": "sz"
}
*/

// 该字符串化命令有三个参数。第一个是Javascript对象。第二个是可选函数，可用于在JSON进行字符串化时对其执行操作。最后一个参数指示要添加多少空格作为缩进以格式化JSON。省略最后一个参数，JSON将返回一个长行。如果myObj中存在循环引用，则会格式失败。
```
---

### 5.随机生成六位数字验证码

```js
const code = Math.floor(Math.random() * 1000000).toString().padStart(6, "0") // 942377
```

---

### 6.RGB 颜色转 16进制颜色

```js
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
RGBToHex(255, 165, 1); // 'ffa501'
```

---

### 7.生成随机整数

```js
function randomNum(min, max) {
  switch (arguments.length) {
    case 1:
      return parseInt(Math.random() * min + 1, 10)
    case 2:
      return parseInt(Math.random() * (max - min + 1) + min, 10)
    default:
      return 0
  }
}

randomNum(1,10) // 随机 [1,10]的整数
```

---

### 8.好的笔记文章

- [JS原生事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)
- [JS选择器 new](https://blog.csdn.net/major_zhang/article/details/78118823)

---

## 📚参考列表

- [30s-code](https://30secondsofcode.org/)
- [2019.6.17日整理](https://juejin.im/post/5d01bd04f265da1b7a4b6e03#heading-2)
- [js tricks](https://qishaoxuan.github.io/js_tricks/)
- [正则参考，感谢](https://github.com/any86/any-rule)