## JS数组常用方法汇总

数组在工作中无处不在，今天总结一下数组的一些高频使用方法

数组的操作咱们可以分为两部分来记忆，改变原数组及不改变原数组

# 改变原数组

## reverse()

数组翻转，即颠倒数组中元素的位置

***返回值是翻转后的数组\***

```
let a=[1,2,3,4,5]
let b=a.reverse()

b   //[5, 4, 3, 2, 1]
a   //[5, 4, 3, 2, 1]
```

## sort()

对数组中元素进行排序，但是默认排序顺序是根据字符串 Unicode 码点

***返回值是排序后的数组\***

```
let a=[1,22,3,24,15]
let b=a.sort()

b   //[1, 15, 22, 24, 3]
a   //[1, 15, 22, 24, 3]
```

## pop()

删除数组中的最后一个元素

***返回的是被删除的元素\***

```
var a1 = [1, 2, 3, 4];
a1.pop()    //4

a1//[1, 2, 3]
```

## push()

在数组的尾部添加一个或者多个元素

***返回的是插入元素后数组的长度\***

```
var a1 = [1, 2, 3, 4];
a1.push()    //4 如果没有插入的值那么返回的长度是当前数组的原长度
var a1 = [1, 2, 3, 4];
a1.push(1,2,3)   //7
a1  //[1, 2, 3,4, 1, 2, 3]
```

## shift()

删除数组中的第一个元素

***返回的是被删除的元素\***

```
var a1 = [1, 2, 3, 4];
a1.shift()  //1
a1  //[2, 3, 4]
```

## unshift()

在数组的首部添加一个或者多个元素

***返回的是插入元素后数组的长度\***

```
var a1 = [1, 2, 3, 4];
a1.unshift()    //4 如果没有插入的值那么返回的长度是当前数组的原长度
var a1 = [1, 2, 3, 4];
a1.unshift(1,2,3)   //7
a1  //[1, 2, 3, 1, 2, 3, 4]
```

## splice

具有【增】【删】【改】的功能，具体实例展示

```
//增
var a1 = [1, 2, 3, 4];
a1.splice(2,0,'插入')   //[]返回的是空数组
a1  //[1, 2, "插入", 3, 4]

//删
var a1 = [1, 2, 3, 4];
a1.splice(2,1)   //[3]返回被删除的元素
a1  //  [1, 2, 4] 返回的是删除后剩余的数组元素

//改
var a1 = [1, 2, 3, 4];
a1.splice(2,1,'改')   //[3]返回的是被替换删除掉的元素
a1  //[1, 2, "改", 4]
```

## fill()

使用给定的值填充一个数组

***返回值是填充后的数组\***

接受第二个和第三个参数，分别用于指定填充的起始位置和结束位置

```
//3个参数
var a1= ['a', 'b', 'c']
a1.fill(7, 1, 2)    //["a", 7, "c"]
a1  //["a", 7, "c"]

//1个参数
var a1= ['a', 'b', 'c']
a1.fill(7)    //["a", 7, "c"]
a1  //[7, 7, 7]
```

# 不改变原数组

## jion()

将数组转换成字符串

***返回值是转变后的字符串\***

```
var a=[1,2,3,4,5]
var b=a.join('-')

 b//返回值 "1-2-3-4-5"

 a //[1,2,3,4,5]
```

## concoat()

合并数组

***返回值是合并生成的新数组\***

```
let a=[1,22,3,24,15]
let c=[1,2,3]
let b=a.concat(c)

a   //[1, 22, 3, 24, 15]
b   //[1, 22, 3, 24, 15, 1, 2, 3]
c   //[1, 2, 3]
```

## slice()

截取数组中的一部分，从开始到结束，截取原则**左闭右开**（即不包括结束索引）

++第二个参数可选++：不写或大于数组的 length，取之将从开始索引到最后一个参数

***返回值是截取到的新数组\***

```
let a=[1,22,3,24,15]
let b=a.slice(1,2)
b   //22

let a=[1,22,3,24,15]
let b=a.slice(1,1)
b   //undefined

let a=[1,22,3,24,15]
let b=a.slice(1)
b   //[22, 3, 24, 15]
```

## map()

新数组中的每个元素，由原数组中的每一个元素执行相应的函数而来

***返回值是新创建的数组\***

```
let a=[1,22,3,24,15]
let b=a.map(item=>item*2)

a   //[1, 22, 3, 24, 15]
b   //[2, 44, 6, 48, 30]
```

## filter()

过滤判断条件生成新的数组

***返回值是过滤符合条件的新数组\***

```
let a=[1,2,4,'']
let b=a.filter(item=> item>1)


b  //[2, 4]
a  //[1, 2, 4, ""]
```

## every()

查询数组中每一个元素是否都匹配条件

***如果都匹配返回 true\*，只要有一个不符合返回 false**

**注意如果是空数组，直接返回是 true**

```
let a=[1,22,3,24,15]
let b=a.every(item=>item>15)

b   // false


let a=[1,22,3,24,15]
let b=a.every(item=>item>=1)

b   // true
a   //[1, 22, 3, 24, 15]

let a=[]
let b=a.every(item=>item)

b   //true
```

## some()

查询数组中是否都匹配条件的元素，

***只要有一个符合就返回 true，但是如果都不匹配，返回 false\***

**注意如果是空数组，直接返回是 false**

```
let a=[]
let b=a.some(item=>item)

b //false

let a=[1,2,4,'']
let b=a.some(item=>!item)

b //true ''代表false，所以!item 代表搜索到了，所以返回true
```

## forEach()

***没有返回值\***，不会改变原数组，可以通过此方法循环判断数组中的每一个元素，执行相对应的回调

比如说我们要给某一个外部变量赋值时，需要从此数组中取出符合条件的数据即用到，可能有人会有疑问，用 filter 不行吗？接下来我们试一下

```
//forEach
let a=[
  {
    name:'张三',
  	order:0
  },
    {
    name:'李思',
  	order:1
  },  {
    name:'小鱼',
  	order:2
  }
]
let obj1Name
a.forEach(item=>{
  if(item.order==1){
		obj1Name=item.name
	}
})
console.log(obj1Name)   //李思

//filter

let a=[
  {
    name:'张三',
  	order:0
  },
    {
    name:'李思',
  	order:1
  },  {
    name:'小鱼',
  	order:2
  }
]
let obj1Name
obj1Name=a.filter(item=>{
  return item.order==1
})
console.log(obj1Name)   //[{name: "李思",order: 1}]
```

## reduce()

相当于一个累加器，第一个参数表示上一次累计的返回值，第二个参数表示当前的返回值

返回结果是最后累加的总和

```
const array1 = [1, 2, 3, 4];
const reducer = (prev, curr) => prev + curr;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
//  15
```

## find()

找出数组中第一个符合条件的元素

它的参数是一个回调函数，数组中的所有元素依次执行该回调函数，

***直到第一个返回值为 true，那么就直接返回这个符合条件的元素，反之如果找不到符合条件的，那么返回值是 undefined\***

```
var a1=[1, 4, -5, 10]
a1.find((n) => n < 0)   // -5
a1  //[1, 4, -5, 10]

var a1=[1, 4, -5, 10]
a1.find((n) => n == 0)   // undefined
a1  //[1, 4, -5, 10]
```

## findIndex()

数组实例的 findIndex 方法的用法与 find 方法非常类似

***返回第一个符合条件的数组成员的索引位置，如果所有成员都不符合条件，则返回-1\***

```
//没有符合元素
var a1=[1, 4, -5, 10]
a1.findIndex((n) => n == 0) //-1
a1  //[1, 4, -5, 10]

//有符合元素
var a1=[1, 4, -5, 10]
a1.findIndex((n) => n <0)  //2
```

## includes()

判断数组中是否包含给定的值

***返回值是一个布尔值，如果包含返回 true，反之 false\***

## entries()、keys()、values()

用来遍历数组，返回的是一个遍历器对象，可以通过使用 for...of 进行遍历

不同点：

- keys 是对键的遍历
- values 是对值的遍历
- entries 是对键和值一起的遍历

```
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
```

分享到此，希望对小伙伴有所帮助～


![img](https://mmbiz.qpic.cn/mmbiz_png/gH31uF9VIibShzIzled4Zc5Kyzriaos31SRXmHOc6MTE3u7HVfiaWrrmY30jMVEuFCZictC8rlUq9JU2YevFVskCuA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在看支持小布一下吧❤️

阅读 361

 在看8