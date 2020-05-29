### Javascript对象

我们先来说说什么是对象，因为翻译的原因，中文语境下我们很难理解“对象”的真正含义。事实上，Object（对象）在英文中，是一切事物的总称，这和面向对象编程的抽象思维有互通之处。

中文的“对象”却没有这样的普适性，我们在学习编程的过程中，更多是把它当作一个专业名词来理解。

但不论如何，我们应该认识到，对象并不是计算机领域凭空造出来的概念，它是顺着人类思维模式产生的一种抽象（于是面向对象编程也被认为是：更接近人类思维模式的一种编程范式）。



对象这一概念在人类的幼儿期形成，这远远早于我们编程逻辑中常用的值、过程等概念。在幼年期，我们总是先认识到某一个苹果能吃（这里的某一个苹果就是一个对象），继而认识到所有的苹果都可以吃（这里的所有苹果，就是一个类），再到后来我们才能意识到三个苹果和三个梨之间的联系，进而产生数字“3”（值）的概念。



#### JavaScript 对象的特征

不论我们使用什么样的编程语言，我们都先应该去理解对象的本质特征（参考 Grandy Booch《面向对象分析与设计》）。总结来看，对象有如下几个特点。

- 对象具有唯一标识性：即使完全相同的两个对象，也并非同一个对象。
- 对象有状态：对象具有状态，同一对象可能处于不同状态之下。
- 对象具有行为：即对象的状态，可能因为它的行为产生变迁。

我们先来看第一个特征，对象具有唯一标识性。一般而言，各种语言的对象唯一标识性都是用内存地址来体现的， 对象具有唯一标识的内存地址，所以具有唯一的标识。

所以，JavaScript 程序员都知道，任何不同的 JavaScript 对象其实是互不相等的，我们可以看下面的代码，o1 和 o2 初看是两个一模一样的对象，但是打印出来的结果却是 false。

```javascript
var o1 = { a: 1 };
var o2 = { a: 1 };
console.log(o1 == o2); // false
```



关于对象的第二个和第三个特征“状态和行为”，不同语言会使用不同的术语来抽象描述它们，比如 C++ 中称它们为“成员变量”和“成员函数”，Java 中则称它们为“属性”和“方法”。

在 JavaScript 中，将状态和行为统一抽象为“属性”，考虑到 JavaScript 中将函数设计成一种特殊对象，所以 JavaScript 中的行为和状态都能用属性来抽象。

下面这段代码其实就展示了普通属性和函数作为属性的一个例子，其中 o 是对象，d 是一个属性，而函数 f 也是一个属性，尽管写法不太相同，但是对 JavaScript 来说，d 和 f 就是两个普通属性。

```javascript
var o = { 
    d: 1,
    f() {
        console.log(this.d);
    }    
};
```

所以，总结一句话来看，在 JavaScript 中，对象的状态和行为其实都被抽象为了属性。如果你用过 Java，一定不要觉得奇怪，尽管设计思路有一定差别，但是二者都很好地表现了对象的基本特征：标识性、状态和行为。

在实现了对象基本特征的基础上, 我认为，JavaScript 中对象独有的特色是：对象具有高度的动态性，这是因为 JavaScript 赋予了使用者在运行时为对象添改状态和行为的能力。

下面这段代码就展示了运行时如何向一个对象添加属性，一开始我定义了一个对象 o，定义完成之后，再添加它的属性 b，这样操作是完全没问题的。

```javascript
var o = { a: 1 };
o.b = 2;
console.log(o.a, o.b); //1 2
```



#### JavaScript 对象的两类属性

对 JavaScript 来说，属性并非只是简单的名称和值，JavaScript 用一组特征（attribute）来描述属性（property）。

先来说第一类属性，数据属性。它比较接近于其它语言的属性概念。数据属性具有四个特征。

- value：就是属性的值。
- writable：决定属性能否被赋值。
- enumerable：决定 for in 能否枚举该属性。
- configurable：决定该属性能否被删除或者改变特征值。

在大多数情况下，我们只关心数据属性的值即可。

第二类属性是访问器（getter/setter）属性，它也有四个特征。

- getter：函数或 undefined，在取属性值时被调用。
- setter：函数或 undefined，在设置属性值时被调用。
- enumerable：决定 for in 能否枚举该属性。
- configurable：决定该属性能否被删除或者改变特征值。

访问器属性使得属性在读和写时执行代码，它允许使用者在写和读属性时，得到完全不同的值，它可以视为一种函数的语法糖。

我们通常用于定义属性的代码会产生数据属性，其中的 writable、enumerable、configurable 都默认为 true。我们可以使用内置函数 Object.getOwnPropertyDescripter 来查看，如以下代码所示：

```javascript
var o = { a: 1 };
o.b = 2;
//a和b皆为数据属性
Object.getOwnPropertyDescriptor(o,"a") // {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(o,"b") // {value: 2, writable: true, enumerable: true, configurable: true}
```

如果我们要想改变属性的特征，或者定义访问器属性，我们可以使用 Object.defineProperty，示例如下：

```javascript
var o = { a: 1 };
Object.defineProperty(o, "b", {value: 2, writable: false, enumerable: false, configurable: true});
//a和b都是数据属性，但特征值变化了
Object.getOwnPropertyDescriptor(o,"a"); // {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(o,"b"); // {value: 2, writable: false, enumerable: false, configurable: true}
o.b = 3;
console.log(o.b); // 2
```

