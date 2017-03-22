# 对象(Object)

#### 目录

* [概念]()
* [创建对象]()
* [检索对象]()
* [更新]()
* [引用]()
* [原型]()
* [反射]()
* [遍历]()
* [删除]()
* [对象Clone]()


#### 概念

（其实有点尴尬，可能没有人不知道对象，java object 、 javaScript object、 PHP object等等一系列的对象。哪我简单谈谈对象。）

在所有面向对象（Object-Oriented，OO）的语言中有一个标志，那就是"类"的概念。并且这个类可以创建任意多的有相同属性和方法的对象。但是在javaScript中（ESMAScript5之前）是没有类的概念，但是在ESMAScript6开始慢慢在开始引入类的概念。

ESMA-262把把对象定义为："无序属性的集合"，其中属性就可以是任意的值。对象的每一个属性都有一个名称，这个名称会映射到一个对应的值。简单来说，对象就是一个无序的散列表。

对象是属性的容器，每个属性的名字可以包含空字符串在内的任意字符串，属性的值可以是除undefined值之外的所有值。
>w3c定义对象

![W3C定义对象](https://ooo.0o0.ooo/2017/03/22/58d275e9bcfeb.png)

#### 创建对象

创建对象非常简单，可能最为大家熟悉的就是对象字面量和构造函数创建对象。
但是创建对象的方式是有很多的。

* 对象字面量
* Object构造函数
* 工厂模式创建对象
* 构造函数模式创建对象
* 原型创建对象
* 组合模式创建对象
* 动态原型模式创建对象
* 寄生构造函数模式创建对象
* 稳妥构造函数创建对象

>每一种创建对象的方法都存在好处和坏处，下一次我将为大家写一篇通过上诉的方法创建对象的文档。

#### 检索对象

要检索对象包含的值，可以采用在[]后缀中包括一个字符串表达式的方式。如果字符串表达式是一个字符串字面量。而且它是一个合法的javaScript标识符并且不是保留字，那么也可以使用.代替。优先考虑使用.表示法，因为它更加的紧凑、可读性好。

var object = {
	"first-name": "A"，
	"last-name": "B"
};

object["first-name"];
>注意在定义属性的时候，不能直接使用first-name，必须使用引号括起来。这在javaScript中标识符包含-是不合法的，但是允许使用下划线(_);

在检索对象属性的时候，如果检索一个不存在的对象属性，会显示undefined。尝试从undefined中检索属性则会导致TypeErrer异常。这是可以通过&&运算符来避免错误。
>object.name --undefined
>object.name.state --- throw TypeError
>object.name && object.name.state --- undefined

#### 更新

对象的值可以通过赋值语句来更新。如果属性名已存在与对象中，那么这个属性的值就会被替换掉。如果对象之前没有拥有这个属性名，那么该属性就会被扩从到对象中。

#### 引用

谈到对象引用，可能就要说道不同数据的类型。ECMAScirpt 变量有两种不同的数据类型：基本类型，引用类型。也有其他的叫法，比如原始类型和对象类型，拥有方法的类型和不能拥有方法的类型，还可以分为可变类型和不可变类型，其实这些叫法都是依据这两种的类型特点来命名！

* **基本类型**
>基本的数据类型有：`undefined，boolean，number，string，null.基本类型的访问是按值访问的，就是说你可以操作保存在变量中的实际的值。基本类型有以下几个特点：

+ 基本类型的值是不可变得。
+ 基本类型的比较是值的比较。
+ 基本类型的变量是存放在栈区的（栈区指内存里的栈内存）。

		var name = 'jozo';
		var city = 'guangzhou';
		var age = 22;
		
	![](https://ooo.0o0.ooo/2017/03/22/58d27f6073b04.png)

* **引用类型**

javascript中除了上面的基本类型(number,string,boolean,null,undefined)之外就是引用类型了，也可以说是就是对象了。对象是属性和方法的集合。也就是说引用类型可以拥有属性和方法，属性又可以包含基本类型和引用类型。来看看引用类型的一些特性：

+ 引用类型的值是可变的。
+ 引用类型的值是同时保存在栈内存和堆内存中的对象。

		javascript和其他语言不同，其不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间，那我们操作啥呢？ 实际上，是操作对象的引用，所以引用类型的值是按引用访问的。
		准确地说，引用类型的存储需要内存的栈区和堆区（堆区是指内存里的堆内存）共同完成，栈区内存保存变量标识符和指向堆内存中该对象的指针，也可以说是该对象在堆内存的地址。

		var person1 = {name:'jozo'};
		var person2 = {name:'xiaom'};
		var person3 = {name:'xiaoq'};
![](https://ooo.0o0.ooo/2017/03/22/58d280b5894ef.png)

+ 引用类型的比较是引用的比较

		var person1 = '{}';
		var person2 = '{}';
		console.log(person1 == person2); // true
		上面讲基本类型的比较的时候提到了当两个比较值的类型相同的时候，相当于是用 === ，所以输出是true了。再看看：
		
		var person1 = {};
		var person2 = {};
		console.log(person1 == person2); // false
		可能你已经看出破绽了，上面比较的是两个字符串，而下面比较的是两个对象，为什么长的一模一样的对象就不相等了呢？
		
		别忘了，引用类型时按引用访问的，换句话说就是比较两个对象的堆内存中的地址是否相同，那很明显，person1和person2在堆内存中地址是不同的：
![](https://ooo.0o0.ooo/2017/03/22/58d2813bbd84c.png)

		当从一个变量向另一个变量赋值引用类型的值时，同样也会将存储在变量中的对象的值复制一份放到为新变量分配的空间中。前面讲引用类型的时候提到，保 存在变量中的是对象在堆内存中的地址，所以，与简单赋值不同，这个值的副本实际上是一个指针，而这个指针指向存储在堆内存的一个对象。那么赋值操作后，两 个变量都保存了同一个对象地址，则这两个变量指向了同一个对象。因此，改变其中任何一个变量，都会相互影响：
		
		var a = {}; // a保存了一个空对象的实例
		var b = a;  // a和b都指向了这个空对象
		
		a.name = 'jozo';
		console.log(a.name); // 'jozo'
		console.log(b.name); // 'jozo'
		
		b.age = 22;
		console.log(b.age);// 22
		console.log(a.age);// 22
		
		console.log(a == b);// true
![](https://ooo.0o0.ooo/2017/03/22/58d281fa88dfa.png)

#### 原型
每一个对下对象都会连接到一个原型对象，并且它可以从中继承属性。所有通过对象字面量创建的对象都会链接到object.prototype,它是JavaScript中的标配对象。

当你创建一个对象的时候，你可以选择某个对象作为他的原型。JavaScript提供的实现机制杂乱而复杂，但其实可以简化。我将给Object增肌一个create方法，这个方法创建一个是使用原对象作为原型的新对象。

			if (typeof Object.beget !== 'function') {
				Object.create = function (o){
					var F = function (){};
					F.prototype = o;
					return new F();
				}
			}

原型连接在更新值的时候是不起租用的，当我们对某个对象做出改变的时候，不会触及到该对象的原型。

原型链接只有在检索值的时候才会使用到。如果我们尝试去获取对象的某个属性值，但该对象没有此属性名，那么JavaScript会尝试从原型对象中获取属性值，如果那个原型对象也没有该属性，那么再从他的原型中需找，以此类推，直到到达终点Object.portotype。如果该属性完全不存在该原型练中，结果就为undefined值，这个过程称为 **委托**。

#### 反射

检查对象属性并确定对象有什么属性是很容易的事情。

* typeof 对确定对象属性的类型有帮助。

				typeof object.number //'number'
				typeof object.string //'string'
				typeof object.object //'object'
				typeof object.unde // 'undefined'
				但是不能检查null的类型，typeof检查null返回的的object

>检查对象是否存在某个属性

* 使用in关键字。
			
		var o={x:1};
		"x" in o;            //true，自有属性存在
		"y" in o;            //false
		"toString" in o;     //true，是一个继承属性
* 使用对象的hasOwnProperty()方法。

		var o={x:1};
		o.hasOwnProperty("x");    　　 //true，自有属性中有x
		o.hasOwnProperty("y");    　　 //false，自有属性中不存在y
		o.hasOwnProperty("toString"); //false，这是一个继承属性，但不是自有属性

* 用undefined判断

		var o={x:1};
		o.x!==undefined;        //true
		o.y!==undefined;        //false
		o.toString!==undefined  //true
该方法存在一个问题，如果属性的值就是undefined的话，该方法不能返回想要的结果，如下。

		var o={x:undefined};
		o.x!==undefined;        //false，属性存在，但值是undefined
		o.y!==undefined;        //false
		o.toString!==undefined  //true

* 在条件语句中直接判断

		var o={};
		if(o.x) o.x+=1;  //如果x是undefine,null,false," ",0或NaN,它将保持不变

#### 遍历

* 遍历可枚举的自身属性

**可枚举的意思就是该属性的[[Enumerable]]特性为true,自身属性的意思就是该属性不是从原型链上继承下来的.**
>
>var propertys = Object.keys(window);

* 遍历所有的自身属性
>var propertys = Object.getOwnPropertyNames(window);

* 遍历可枚举的自身属性和继承属性

			var getEnumPropertyNames = function (obj) {
		        var props = [];
		        for (prop in obj) {
		            props.push(prop);
		        }
		        return props;
		    }
		    var propertys = getEnumPropertyNames(window);

* 遍历所有的自身属性和继承属性

		var getAllPropertyNames = function (obj) {
		        var props = [];
		        do {
		            props = props.concat(Object.getOwnPropertyNames(obj));
		        } while (obj = Object.getPrototypeOf(obj));
		        return props;
		    }
		    var propertys = getAllPropertyNames(window);


#### 删除

delete运算符可以用来删除对象的属性，如果对象包括该属性，那么该属性会被移除。他不会触击到原型练中的任何对象。但是删除对象的属性会让原型链的属性暴露出来。

#### 对象的Clone

有时候我们在使用数组或者对象进行操作的时候，需要将数组或者对象进行备份，但是，有时候当我们简单的将一个数组或者对象复制给其他变量，
那么我们只要改变其中一个，另外一个也会随之改变，这样的复制拷贝，叫做浅拷贝。

		数组的深拷贝：
		1、JavaScript的slice()方法。
		2、JavaScript的concat()方法。
		3、循环复制。

		对象的深拷贝：
		1、对象的深拷贝就是把对象的所有属性全部遍历一遍，复制给新对象。

		数值和对象深拷贝的公共方法：
		function clone(obj){
		  var buf;
		  if(obj instanceof Array){
		    buf=[];
		    buf = obj.concat();//buf = obj.slice(0);
		    return buf;
		  }
		  else if(obj instanceof Object){
		    var buf={};
		    for(var ob in obj){
		      buf[ob]=obj[ob];
		    }
		    return buf;
		  }
		  else{
		    return obj;
		  }
		}
		但是光这样的拷贝是不行的，这样的拷贝存在问题，当存在对象时多层的时候，是不能正常拷贝的。
		function clone(obj){
		  var buf;
		  if(obj instanceof Array){
		    buf=[];
		    buf = obj.concat();//buf = obj.slice(0);
		    return buf;
		  }
		  else if(obj instanceof Object){
		    if(typeof obj !="object"){
		      return obj;
		    }
		    var buf={};
		    for(var ob in obj){
		      buf[ob]=clone(obj[ob]);
		    }
		    return buf;
		  }
		  else{
		    return obj;
		  }
		}

3/22/2017 10:37:50 PM 谢谢大家！