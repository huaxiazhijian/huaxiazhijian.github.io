---
layout:     post
title:      "PHP面向对象编程「原」"
subtitle:   "PHP object oriented programming  "
date:       2016-11-4
author:     "LingDie | 靈蝶"
header-img: "img/blog/2016114/object.jpg"
tags:
    - PHP
    - 面向对象
    - 编程
    - 原创
---

> 下滑这里查看更多内容

### 什么是面向对象编程

把数据及对数据的操作方法放在一起，作为一个相互依存的整体——对象。对同类对象抽象出其共性，形成类。类中的大多数数据，只能用本类的方法进行处理。类通过一个简单的外部接口与外界发生关系，对象与对象之间通过消息进行通信。程序流程由用户在使用中决定。

### 什么是面向过程编程

自顶向下顺序执行，逐步求精；其程序结构是按功能划分为若干个基本模块，这些模块形成一个树状结构；各模块之间的关系尽可能简单，在功能上相对独立；每一模块内部均是由顺序、选择和循环三种基本结构组成；其模块化实现的具体方法是使用子程序。程序流程在写程序时就已决定。

### 面向对象编程和面向过程编程的区别

面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。 
面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。

### 面向对象编程类和对象

- 类和对象的区别与联系：
  - 类是抽象的，概念的，代表一类事物，比如人类，猫类；
  - 对象是具体的，实际的，代表一个具体的事物；
  - 类是对象的模板，对象是类的一个个体实例；

### 面向对象编程实例（详解）

- 以人为列子：

{% highlight ruby %}
 <?php
	header("Content-type:text/html;charset=utf-8");
	class person{
	#下面是人的成员属性
	var $name;
	#人的名字
	var $sex;
	#人的性别
	var $age;
	#人的年龄
	#定义一个构造方法参数为姓名$name,性别$sex和年龄$age
	function __construct($name,$sex,$age){
	#通过构造方法传进来的$name给成员属性$this->name赋初始值
	$this->name=$name;
	#通过构造方法传进来的$sex给成员属性$this->sex赋初始值
	$this->sex=$sex;
	#通过构造方法传进来的$age给成员属性$this->age赋初始值
	$this->age="$age";
	}
	#下面是人的成员方法
	function say()
	#这个人可以说话的方法
	{
	echo "我的名字叫：".$this->name."性别；".$this->sex."我的年龄是：".$this->age."<br>";
	}
	function run() #这个人可以走路的方法
	{
	echo "这个人在走路";
	}
	#这是一个析构函数，在对象销毁前调用
	function __destruct()
	{
	echo "再见".$this->name."<br>";
	}
	}
	#通过构造方法创建3个对象$p1,$p2,$p3，分别传入三个不同的实参为姓名性别和年龄
	$p1=new person("小明","男",20);
	$p2=new person("熊","女",30);
	$p3=new person("向日葵","男",25);
	$p1->say();
	$p2->say();
	$p3->say();
	#下面访问3个对象的说话方式$p1->say();$p2->say();$p3->say();
	?> 
{% endhighlight %}
- php面向对象的几个步骤 
  - 第一类的定义：
  {% highlight ruby %}
  <?php
  Class myobject{
    #……
	}
   ?>
  {% endhighlight  %}
  - 第二成员方法：
  {% highlight ruby %}
  <?php
	classmyobject{
	   function getobjectname($name){
	      echo "商品名称为：".$name;
	   }
	}
  ?>
  {% endhighlight %}
  - 第三类的实例化：
  {% highlight ruby %}
  <?php
	class myobject{
	  function getobjectname($name){
	     echo "商品名称为：".$name;
	   }
	}
	$c_book=new myobject();           #实例化对象
	echo $c_book->getobjectname("计算机图书");  #调用getbookname()方法
  ?>
  {% endhighlight  %}
  - 第四成员变量：
  {% highlight ruby %}
  <?php
	class myobject{
	  public $object_name;
	  functionsetobjectname($name){
	    $this->object_name=$name;
	  }
	  functiongetobjectname(){
	    return$this->object_name;
	  }
	}
	$c_book=new myobject();
	$c_book->setobjectname("计算机图书");
	echo $c_book->object_name."<br>";
	echo $c_book->getobjectname();
  ?>
  {% endhighlight %}
  - 第五常量类：
    既然有变量，当然也会有常量了。常量就是不会改变的量，是一个恒值。众所周知的一个常量就是圆周率Pi。定义常量使用关键字const如：ConstPI=3.14159; 
  {% highlight ruby %}
  <?php
   class myobject{                        
	  const book_type="计算机图书";             #声明常量book_type
	  public $object_name;                                    #声明变量
	  functionsetobjectname($name){                     #声明方法setobjectname()
	      $this->object_name=$name;                   #设置成员的变量值
	  }
	  functiongetobjectname(){                #声明方法getobject()
	    return$this->object_name;                        
	  }
	}
	$c_book=new myobject();                                 #实例化对象
	$c_book->setobjectname("PHP的类");              #调用方法setobjectname
	echo myobject::book_type."<br>";              #输出常量的值
	echo $c_book->getobjectname();                 #调用方法getobjectname
  ?>
  {% endhighlight  %}
  - 第六面向对象类的构造方法:
  {% highlight ruby %}
  <?php
	class myobject{                        
	   public $object_name;   #商品名称
	   public $object_price;              #商品价格
	   public $object_num;        #商品数量
	   public $object_agio;        #商品折扣
	   …………
   }            
  ?>
  {% endhighlight %}
  - 声明一个myobject类的对象，并对这个类的一些成员变量赋初值。代码如下：
  {% highlight ruby %}
   <?php
		class myobject{                        
		   public $object_name;
		   public $object_price;
		   public $object_num;
		   public $object_agio;
		   functiongetobjectname(){
		      return$this->object_name;
		        return$this->object_price;
		        return $this->object_num;
		        return $this->object_agio;
		   }
		}         
		$dress=new myobject();
		$dress->object_name="western-style clothes";
		$dress->object_price=1500;
		$dress->object_num=5;
		$dress->object_agio=8;
		echo $dress->getobjectname();  
		?>
		Void__construect([mixed args,[……]])
  {% endhighlight %}
  注意：函数中的__是两条下划线，不是一条。 
- 实例2：
  {% highlight ruby %}
   <?php
		class myobject{                        
		  public $object_name;
		  public $object_price;
		  public $object_num;
		  public $object_agio;
		function__construct($name,$price,$num,$agio){   #通过参数给成员变量赋值
		      $this->object_name=$name;
		          $this->object_price=$price;
		          $this->object_num=$num;
		          $this->object_agio=$agio;
		   }
		function setobjectname($name){
		    $this->object_name=$name;
		}
		function getobjectname1(){
		    return $this->object_name;
		}
		function getobjectname2(){
		        return $this->object_price;
		}
		}            
		$c_book=new myobject("western-styleclothes",1500,5,8);
		echo $c_book->getobjectname1();
		echo "<br>";
		echo $c_book->getobjectname2();
  ?>
  {% endhighlight %}
  - 第七析构方法：
  概念
  析构方法的作用和构造方法正好相反，是对象被销毁时被调用的，作用是释放内存。析构方法的格式为：Void__destruct(void)
  例：
  {% highlight ruby %}
  <?php
		class myobject{                        
		  public $object_name;
		  public $object_price;
		  public $object_num;
		  public $object_agio;
		function__construct($name,$price,$num,$agio){   #通过参数给成员变量赋值
		      $this->object_name=$name;
		          $this->object_price=$price;
		          $this->object_num=$num;
		          $this->object_agio=$agio;
		   }
		function setobjectname($name){
		    $this->object_name=$name;
		}
		function getobjectname1(){
		    return $this->object_name;
		}
		function getobjectname2(){
		        return $this->object_price;
		}
		function __destruct(){
		  echo "<p><b>对象被销毁，调用析构函数。</b></p>";
		}
		}            
		$c_book=new myobject("western-styleclothes",1500,5,8);
		echo $c_book->getobjectname1();
		echo "<br>";
		echo $c_book->getobjectname2();
		unset($c_book);
  ?>
  {% endhighlight %}
  PHP使用的是一种“垃圾回收”机制，自动清除不再使用的对象，释放内存。就是说即使不使用unset函数，析构方法也会自动被调用，这里只是明确一下析构函数在何时被调用。一般情况下是不需要手动创建析构方法的。
  {% highlight ruby %}
   <?php
	class myobject{                        
	   public $object_name;
	   public $object_price;
	   public $object_num;
	   public $object_agio;
	function __construct($name,$price,$num,$agio){    #通过参数给成员变量赋值
	       $this->object_name=$name;
	          $this->object_price=$price;
	          $this->object_num=$num;
	          $this->object_agio=$agio;
	  }
	function setobjectname($name){
	    $this->object_name=$name;
	}
	function getobjectname1(){
	     return$this->object_name;
	}
	function getobjectname2(){
	        return $this->object_price;
	}
	function __destruct(){
	   echo"<p><b>对象被销毁，调用析构函数。</b></p>";
	}
	}            
	$c_book=new myobject("western-styleclothes",1500,5,8);
	echo $c_book->getobjectname1();
	echo "<br>";
	echo $c_book->getobjectname2();
   ?>
  {% endhighlight %}
  - 第八继承和多状态的实现:
  {% highlight ruby %}
  Class subclass extends superclass{
   ……
  }
  {% endhighlight %}
  说明：subclass为子类的名称，superclass为父类名称。
  例：
  {% highlight ruby %}
   <?php
		class myobject{                        
		  public $object_name;
		  public $object_price;
		  public $object_num;
		  public $object_agio;
		function__construct($name,$price,$num,$agio){   #通过参数给成员变量赋值
		      $this->object_name=$name;
		          $this->object_price=$price;
		          $this->object_num=$num;
		          $this->object_agio=$agio;
		   }
		function showme(){
		    echo "这句话会输出吗？答案是不会。";
		}
		}
		class book extends myobject{         
		   public $book_type;
		       function__construct($type,$num){
		         $this->book_type=$type;
		         $this->object_num=$num;  
		       }
		       functionshowme(){                            #重写父类中的showme()方法。
		         return "本次新进".$this->book_type."图书".$this->object_num."本"."<br>";
		       }
		}
		class elec extends myobject{
		   function showme(){                             #重写父类中的showme()方法
		         return "热卖商品：".$this->object_name."<br>"."原 价：".$this->object_price."<br>"."特 价".$this->object_price*$this->object_agio;
		       }
		}
		$c_book=new book("计算机类",1000);   #声明一个book子类对象。
		$h_elec=new elec("待机王XX系列",1200,3,0.8);    //声明一个elec子类对象。
		echo$c_book->showme()."<br>";   #输出book子类的showme()方法
		echo $h_elec->showme();          #输出elec子类的是showme()方法
   ?>
  {% endhighlight %}
  子类继承了父类的所有成员变量和方法，包括构造函数。这就是继承的实现。
  当子类被创建时，PHP会先在子类中查找构造方法。如果子类有自己的构造方法，PHP会先调用子类中的方法，当子类中没有时，PHP则会去调用父类中的构造方法。
  两个子类重写了父类的方法showme()，所以两个对象虽然调用的都是showme()方法，但返回的却是两段不同的信息。这就是多态性的实现。
  - 第九this−>和操作符的使用1、this-> 
  在 前面类的实例化中，对如何调用成员方法有了基本的了解，那就是用对象名加方法名，格式为“对象名->方法名”。但在定义类时(如 myobject)，根本无法得知对象的名称是什么。这时如果调用类中的方法，就要用伪变量this−>。this的意思就是本身，所 以$this->只可以在类的内部使用。
  例：
  {% highlight ruby %}
  <?php
	classexample{   #创建类example
	  function exam(){   #创建成员方法
	     if(isset($this)){            #判断变量$this是否存在
	           echo "\$this的值为：".get_class($this);    #如果存在，输出$this所属类的名字
	        }else{
	           echo "$this未定义。";
	        }
	  }
	}
	$class_name=newexample();   #实例化对象
	$class_name->exam();          #调用方法exam()
  ?>
  {% endhighlight %}
  Get_class函数返回对象所属类的名字，如果不是对象，则返回false。 
  - 第十公共、私有和保护 
    - public公共成员
    顾名思义，就是可以公开的、没有必要隐藏的数据信息。可以在程序的任何地点(类内、类外)被其他的类和对象调用。子类可以继承和使用父类中所有的公共成员。
	所有的变量都被声明为public，而所有的方法在默认的状态下也是public。所以对变量
	和方法的调用显示得十分混乱。为了解决这个问题，就需要使用第二个关键字：private。 
    -  private私有成员 
    被private关键字修饰的变量和方法，只能在所属类的内部被调用和修改，不可以在类外被访问。在子类中也不可以。
    例：
    {% highlight ruby %}
    <?php
		class book{
		private $name="computer";
		public function setname($name){
		   $this->name=$name;
		  }
		public function getname(){
		   return $this->name;
		  }
		}
		class lbook extends book{
		}
		$lbook=new lbook();
		echo "正确操作私有变量的方法：";
		$lbook->setname("PHP应用开发！");
		echo $lbook->getname();
		echo "直接操作私有变量的结果：";
		echo book:name;
    ?>
    {% endhighlight %}
    对于成员方法，如果没有写关键字，那么默认就是public。从本节开始，以后所有的方法及变量都会带个关键字，这是一种良好的书写习惯。 
    - protected保护成员 
    private 关键字可以将数据完全隐藏起来，除了在本类外，其他地方都不可以调用。子类也不可以。但对于有些变量希望子类能够调用，但对另外的类来说，还要做到封装。 这时，就可以使用protected。被protected修改的类成员，可以在类和子类中被调用，其他地方则不可以被调用。
    例：
    {% highlight ruby %}
    <?php
		class book{
		  protected $name="computer";
		}
		class lbook extends book{
		  public function showme(){
		      echo "对于protected修饰的变量，在子类中是可以直接调用的。如：\$name=".$this->name."<br>";
		   }
		}
		$lbook=new lbook();
		$lbook->showme();
		echo "但在其他的地方是不可以调用的，否则：";
		$lbook->name="history";
    ?>
    {% endhighlight %}

