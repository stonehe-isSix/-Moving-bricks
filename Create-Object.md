# Create-Object

#### 目录
  	
* [object构造函数]()
* [对象字面量]()
* [工厂模式]()
* [构造函数模式]()
* [原型模式]()
* [混合模式（原型模式 + 构造函数模式]()
* [动态原型模式]()
* [寄生构造函数模式]()
* [稳妥构造函数模式]()
  
#### object构造函数
  
         var object = new Object();
		 var name = new Object();
  
         缺点：使用同一个接口创建很多对象，会产生大量的重复代码。
  
  
#### 对象字面量
		
		var object = {};
		var name ={}; 

		缺点：使用同一个接口创建很多对象，会产生大量的重复代码。

#### 工厂模式
		工厂模式一种广为人知的设计模式，这种模式抽象了创建对象的过程。
		  functuon createPerson（name,age,job）{
		    var o = new Object();
		    o.name=name;
		    o.age=age;
		    o.job=job;
		    o.sayNmae=function(){
		      alert(this.name);
		    };
		    return o;
		  }
		  
		var person1  = createPerson("zhangsan1",20,"IT");
		var person2  = createPerson("zhangsan2",21,"UI"); 
		优点：创建对象，避免了大量的重复代码。
		缺点：不能识别对象的类型。

#### 构造函数模式

	ECMAScript中的构造函数可以用来创建特定类型的对象。如Object，Array这样的原生构造函数，也可以使用自定义的构造函数。例如重写工厂模式的createPerson方法。
	
	function Person(name,age,job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function (){
			alert(this.name);
		};
	}
	var person1  = new Person("zhangsan1",20,"IT");
	var person2  = new Person("zhangsan2",21,"UI"); 
>和工厂模式的区别：

1. 没有显示的创建对象。
2. 直接将属性和方法赋给了this对象。
3. 没有return语句。

>创建自定义的构造函数意味这将来可以将他的实例标识为一种特定的类型，这也是构造函数模式胜过工厂模式的地方。但是以这种方式定义的函数是定义在Global对象中的(在浏览器的window对象中)；

		缺点：虽然构造函数模式比较好用，但是也是存在缺点的，那就是，每一个方法都要在每个实例上重新创建一遍。这种方式创建函数，会导致不同的作用于链和标识符解析。但是创建Function新实例的机制还是一样的。因此不同函数上的同名函数还是不同的。如
		function Person(name,age,job){
				this.name = name;
				this.age = age;
				this.job = job;
				this.sayName = function (){alert(this.name);};
		}
		var person1  = new Person("zhangsan1",20,"IT");
		var person2  = new Person("zhangsan2",21,"UI");
		console.log(person1.sayName == person2.sayName);//false
		
		但是我们也可以使用把sayName函数定义在全局作用域。
		function Person(name,age,job){
				this.name = name;
				this.age = age;
				this.job = job;
				this.sayName = sayName;
		}
		function sayName(){
			alert(this.name);
		};
		但是这样的做，问题又来了，一个全局函数，却只能被某个对象使用，不仅不符合代码设计，也可能导致多个这样的全局函数。那接下来看看有什么方式解决。


### 原型模式

每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象。这个对象的用途是包含了特定类型实例的所有共享的属性和方法。
		
			function Task(id){  
			    this.id = id;  
			}  
			    
			Task.prototype.status = "STOPPED";  
			Task.prototype.execute = function(args){  
			    return "execute task_"+this.id+"["+this.status+"]:"+args;  
			}  
			    
			var task1 = new Task(1);  
			var task2 = new Task(2);  
			    
			task1.status = "ACTIVE";  
			task2.status = "STARTING"; 

>无论什么时候，只要创建一个新的函数，就会根据一组特定的规则创建一个prototype属性，这个属性指向函数的原型对象。在默认情况下，原型对象会自动获得一个constructor属性，这个属性包含一个指向prototype属性所在函数的指针，例如Task.prototype.constructor 指向 Task，又或者

>task1.constructor = Task.prototype.constructor;

>task1.constructor.prototype = Task.pertotype;

![](https://ooo.0o0.ooo/2017/03/23/58d3d7ceb6d62.png)

		  优点：实例共享属性和方法。
		  缺点：不能为构造函数传递参数，实例不能修改原型的属性。最严重的问题是共享机制导致的，在原型中使用了引用类型，问题就比较严重。
  
#### 混合模式（原型模式 + 构造函数模式）

构造函数模式用于定义实例属性；原型模式用于定义方法和共享的属性。好处：每个实例都有自己的实例属性副本，同时有共享方法的引用，节省了内存；支持向构造函数传递参数。**这种方式是目前ECMAScript中使用最广泛、认同度最高的一种方法。**

		function Person(name,age,job){
		          this.name=name;
		          this.age=age;
		          this.job=job;
		          this.friends=["shelby","Court"];
		}
		Person.prototype={
		        contructor:Person,
		        sayName:function(){
		             alert(this.name);
		   }
		}
		var person1=new Person("Nicholas",29,"Engineer");
		var person2-new Person("Greg",27,"Doctor");
  
#### 动态原型模式

		  function Person(name,age,job){
		    this.name=name;
		    this.gae=age;
		    this.job=job;
		    //方法
		    if(typeof this.sayName !="function"){
		      Person.prototype.sayName = function(){
		        alert(this.name);
		      }
		    }
		  }
>这里的if判断主要是判断是否是初次运行构造函数。

1. 初次运行的时候，this指向的实例对象没有sayName方法，实例对象的隐式原型也没有sayName方法，因此会进入到if方法体中，进行原型方法和属性的定义。
2. 第二次运行的时候，this指向的实例对象的原型已经有了sayName方法，可以通过this.sayName访问到，因此不会进入到if方法中，避免原型方法的重复定义。

>不能使用字面量的写法，是因为如果用字面量重写的话，初次创建的实例会有问题，因为使用构造函数时，是先创建了一个隐式原型指向原型的实例对象，然后再运行构造函数的代码的。


#### 寄生构造函数模式。

寄生构造函数模式：其基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象。

		  function Person(name,age,job){
		    var o =new Object();
		    o.name=name;
		    o.age=age;
		    o.job=job;
		    o.sayName=function(){
		      alert(this.name);
		    };
		    return o;
		  }
		  var person =new Person("zhang",20,"IT");
			工厂模式的区别:
			1、寄生模式创建对象时使用了New关键字
			2、寄生模式的外部包装函数是一个构造函数

>作用:寄生模式可以在特殊的情况下为对象来创建构造函数,原因在于我们可以通过构造函数重写对象的值，并通过return返回  重写调用构造函数(创建的对象的实例)之后的对象实例的新的值。

假设我们想创建一个具有额外方法的特殊数组。由于不能直接修改Array构造函数,所以我们可以使用寄生模式。代码如下:

		function SpecialArray() {
		    //创建数组
		    var array=new Array();
		    //添加值  arguments获取的是实参,不是形参,所以SpecialArray()并没有形参接收传递过来的参数
		    array.push.apply(array,arguments);
		    array.toPipedString=function(){
		        return this.join("|");
		    }
		    return array;
		}
		var colors=new SpecialArray("red","blue","black");
		alert(colors.toPipedString());  //输出:red|blue|black
  
### 稳妥构造函数模式
(道格拉斯发明了JavaScript中的稳妥对象：指没有公共属性而且其方法也不引用this的对象）稳妥构造函数模式和寄生构造函数模式有两点不同：

1. 是新创建的对象不使用this，
2. 不使用new操作符调用构造函数。

这种模式适合在一些安全的环境中，或者防止数据被其他数据改动时使用。

		  function Person(name,age,job){
		    var o =new Object();
		    o.name=name;
		    o.age=age;
		    o.job=job;
		    o.sayName=function(){
		      alert(name);
		    };
		    return o;
		  }
		  var person = Person("zhang",20,"IT");
>上面的代码定义了一个person变量，里面保存的是一个稳妥对象,而除了吊用他的sayName()方法外,没有别的方法可以访问其数据成员。即使有其他的代码会给这个对象添加方法和数据成员，但也不可能有别的方法访问到传入到构造函数中的原始数据。稳妥构造函数模式提供的这种安全性。是的它非常适合在某些安全执行环境中。

3/23/2017 10:33:56 PM 谢谢！
