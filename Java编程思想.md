# 1 对象导论

> 对象具有状态、行为和标识

* 每个对象都有一个接口

| Light                       | 类型名称 |
| --------------------------- | -------- |
| on() off() brighten() dim() | 接口     |

* 每个对象都提供服务

* 将实现隐藏起来可以减少bug

* 复用

  * 使用现有类合成新类---**组合** （*has-a*），如果是动态发生的，叫做**聚合**
  * 建立新类时，应该先组合

* 继承(*is-a*)

  * 使得基类和导出类有差异：
    1. 直接添加新方法
    2. *覆盖*

* **多态**

  * *后期绑定*
  * *向上转型*：把导出类看做是它的基类

* 容器

  * ex: `ArrayList` 和 `LinkedList`
  * 范型：`<>`

* 对象的创建和生命期

  * *Java*完全采用动态内存分配方式

* 异常处理

* 并发

* *Java*与*Internet*

  * 客户/服务机计算技术

    核心思想：系统具有一个中央信息存储池，用来存储某种数据，它通常存在于数据库中，你可以根据需要将它分发给某些人员或机器集群。

    *性能问题*

  * 客户端编程

# 2 一切都是对象

**通常用`new`来创建一个新的对象**，特例：基本类型

*Java*不存在销毁对象的问题，而且有一个*垃圾储存器*，用来监视用`new`创建的所有对象

* 解决了**内存泄漏**的问题

**类**

* 字段
* 方法

变量没有赋初始值会报错，*<u>当变量作为类的成员使用时，才会给默认值</u>*

运用其他构件

```java
import java.util.ArrayList;
import java.util.*;
```

**static关键字**

一个`static`字段对每个类来说都只有一份存储空间，而非`static`字段则是每个对象都有一个存储空间

* 重要用法：不创建任何对象的前提下就可以调用之​。（直接通过类名）
* “牧羊人” 看护与其隶属同一类型的实例群

第一个*Java*程序

* **类的名字必须于文件名相同**
* 类中必须包含一个名为**main()**的方法

```java
//: object/HelloDate.java
import java.util.*;

/** The first Thinking in Java example program.
 * Displays a string and today's date.
 * @author Bruce Eckel
 * @author www.MindView.net
 * @version 4.0
*/
public class HelloDate {
  /** Entry point to class & application.
   * @param args array of string arguments
   * @throws exceptions No exceptions thrown
  */
  public static void main(String[] args) {
    System.out.println("Hello, it's: ");
    System.out.println(new Date());
  }
} /* Output: (55% match)
Hello, it's:
Wed Oct 05 14:39:36 MDT 2005
*///:~

```

`javadoc`

* 只能在`/**`中出现

# 3 操作符

* 赋值

  一个对象赋值给另一个对象 --- 引用相同 --- 指向相同对象

* 别名问题

* 关系操作符

  比较的是对象的引用

  ```java
  public class Equivalence{
      public static void main(String[] args){
          Integer n1 = new Integer(4);
          Integer n2 = new Integer(4);
          System.out.println(n1 == n2);
          System.out.println(n1 != n2);
      }
  }/* Output:
  false
  true
  *///~
  ```

  * *“短路”*

  * ```java
    float f = 1e-43f; // 10 to the power
    ```

  * 按位操作符

  * 移位操作符

  * 字符串操作符

    如果一个表达式以一个字符串起头，后续所有操作数都必须是字符串型

  * 类型转换

    * 窄化转换&扩展转换

  * 四舍五入 `Math.round()`

  * 两个足够大的`int`值执行乘法运算，结果就会溢出

# 4 控制执行流程

`Math`库中的静态方法`random()` 也有`Random`对象：`Random rand = new Random()`

`forreach`语法：`for(int i : range(10))`

* 一般的**`continue`**会退回最内层循环的开头（顶部），并继续执行
* 带标签的**`continue`**会到达标签的位置，并重新进入紧接在那个标签后面的循环
* 一般的**`break`**会中断并跳出当前循环
* 带标签的**`break`**会中断并跳出标签所指的循环，一般情况下可以理解为*跳出两层循环*

`switch`语句

* 选择因子必须是**`int`**或者**`char`**那样的整数值
* 别忘了末尾的**`break`**

# 5 初始化和清理

用构造器确保初始化

```java
class Tester{
    String s1;
    String s2 = "hello";
    String s3;
    Tester() {s3 = "world"}
}

public class ConstructorTest{
    public static void main(String[] args){
  		Tester t = new Tester();
    }
}
```

* 方法重载

  * 多种方式创建一个对象
  * 区分重载方法：*参数类型的差异*

  

* `this`关键字

  * 只能在方法内部使用
  * 如果是在方法内部调用同一个类中的另外一个方法，则不必使用
  * 常常在`return`中用于返回当前对象的引用
  * **只能调用一个构造器**，必须将构造器调用置于最起始处

再理解`static`：就是没有`this`的方法

**静态方法内部不能调用非静态方法**：

这是类加载机制的最后一步，在这个阶段，`java`程序代码才开始真正执行。我们知道，在准备阶段已经为类变量赋过一次值。在初始化阶端，程序员可以根据自己的需求来赋值了。**初始化时候才会为我们的普通成员变量赋值。**

写到这答案已经出来了，**静态方法是属于类的，动态方法属于实例对象**，在类加载的时候就会分配内存，可以通过类名直接去访问，非静态成员（变量和方法）属于类的对象，所以只有该对象初始化之后才存在，然后通过类的对象去访问。

也就是说如果我们在静态方法中调用非静态成员变量会超前，可能会调用了一个还未初始化的变量。因此编译器会报错。

* 清理
  * 对象可能不被垃圾回收
  * 垃圾回收并不等于“析构”
  * 垃圾回收只与内存有关

* 垃圾回收站如何工作
  * `Java`虚拟机将采用一种*自适应*的垃圾回收技术
    * 停止-复制：堆空间出现很多碎片
    * 标记-清扫：所有对象都很稳定，垃圾回收器的效率降低

* 成员初始化
  * 方法内部必须初始化，否则会报错
  * 类的数据成员有初始值，而对象引用则是返回一个`null`
  * 即使变量定义散布于方法定义间，它们仍旧会在任何方法（包括构造器）被调用之前得到初始化
  * 静态初始化只有在必要时刻才会进行

对象的创建过程：Page 96

* 静态初始化

  * 只执行一次

* 非静态初始化（实例初始化）

  ```java
  class Tester {
  	String s;
  	{
  		s = "Initializing string in Tester";
  		System.out.println(s);
  	}
  	Tester() {
  		System.out.println("Tester()");
  	}
  }
  ```

  * 可变参数列表
    * 不用显式的编写数组语法，当你指定参数时，编译器实际上会为你填充数组，你获取的仍旧是一个数组
  
  ```java
  public class VarargEx19 {
  	static void showStrings(String... args) {
  		for(String s : args)
  			System.out.print(s + " ");
  		System.out.println();
  	}
  	public static void main(String[] args) {
  		showStrings("one", "TWO", "three", "four");
  		showStrings(new String[]{"1", "2", "3", "4"});
  	}
  }
  ```
  
  * 枚举类型
  
  ```java
  public enum Spiciness {
    NOT, MILD, MEDIUM, HOT, FLAMING
  } 
  
  public class Burrito {
    Spiciness degree;
    public Burrito(Spiciness degree) { this.degree = degree;}
    public void describe() {
      System.out.print("This burrito is ");
      switch(degree) {
        case NOT:    System.out.println("not spicy at all.");
                     break;
        case MILD:
        case MEDIUM: System.out.println("a little hot.");
                     break;
        case HOT:
        case FLAMING:
        default:     System.out.println("maybe too hot.");
      }
    }	
    public static void main(String[] args) {
      Burrito
        plain = new Burrito(Spiciness.NOT),
        greenChile = new Burrito(Spiciness.MEDIUM),
        jalapeno = new Burrito(Spiciness.HOT);
      plain.describe();
      greenChile.describe();
      jalapeno.describe();
    }
  } /* Output:
  This burrito is not spicy at all.
  This burrito is a little hot.
  This burrito is maybe too hot.
  *///:~
  ```
  

# 6 访问权限控制

*Java*中访问权限修饰词：`public`,`protected`,包访问权限（没有关键词）,`private`

**`public`：接口访问权限**

每个编译单元中只能有一个`public`类，否则编译器就不会接受，如果还有额外的类，包外面的世界是看不到这些类的，这些类主要用来为主`public`类做支持。

**`package`和`import`关键字允许你做的，是将单一的全局名字空间分割开**

* **对包使用的忠告**
  * 必须位于其名称所指定的目录中，而该目录必须`CLASSPATH`开始的目录中可以查询到的

**`private`：你无法访问**

可能想控制如何创建对象，并阻止别人直接访问

```java
class Sundae {
  private Sundae() {}
  static Sundae makeASundae() {
    return new Sundae();
  }
}

public class IceCream {
  public static void main(String[] args) {
    //! Sundae x = new Sundae();
    Sundae x = Sundae.makeASundae();
  }
```

**`protected`：继承访问权限**

* 把对它的访问权限赋予派生类
* 提供**包访问权限**，相同包内的其他类可以访问

*设计模式*

```java
class Soup2 {
  private Soup2() {}
  // (2) Create a static object and return a reference
  // upon request.(The "Singleton" pattern):
  private static Soup2 ps1 = new Soup2();
  public static Soup2 access() {
    return ps1;
  }
```

`Soup2`的对象是作为`Soup2`的一个`private static`成员而创建的，所以有且仅有一个，而且只能通过`public`方法的`access()`访问

# 7 复用类

`toString()`方法

**组合**：将对象引用置于新类中即可

* 初始化这些引用：
  * 定义对象的地方
  * 类的构造器中
  * 惰性初始化
  * 使用实例初始化

**继承**

为了继承，一般的规则是把所有的数据成员都指定为`private`，把所有的方法指定为`public`

初始化基类

* 只能在构造器中调用基类构造器来初始化
* 构建过程从基类向外扩散

带参数的构造器：用关键字`super`显地编写调用基类构造器的语句

**代理**  page 131

**确保正确清理**

**`protected`关键字**

```java
* package reusing.ex15;
* public class BasicDevice {
*	private String s = "Original";
*	public BasicDevice() {	s = "Original"; }
*	protected void changeS(String c) { // outside package, only derived 
*		s = c;			// classes can access protected method	
*	}
*	public void showS() {
*		System.out.println(s);
*	}
* }
*/

import reusing.ex15.*;

class DeviceFail {	
	public static void main(String[] s) {
		BasicDevice fail = new BasicDevice();
		fail.showS();
		// fail.changeS("good-bye"); // cannot access protected method 	
	}
}

public class Device extends BasicDevice {
	void changeBasic(String t) {
		super.changeS(t); // calls protected method
	}	
	public static void main(String[] args) {
		Device d = new Device();
		d.showS();
		d.changeBasic("Changed"); // derived class can access protected
		d.showS();
		DeviceFail.main(args);
	}
}
```

**向上转型**

```java
class Instrument{
    public void play() {}
    static void tune(Instrument i){
		i.play();
    }
}

public class Wind extends Instrument{
	public static void main(String[] args){
        Wind flute = new Wind();
        Instrument.play(flute); //upcasting
    }
}
```

**再论组合与继承**

自己是否需要从新类向基类进行向上转型，如果必须向上转型，继承是必要的

```java
class Amphibian {
	protected void swim() {
		println("Amphibian swim");		
	}
	protected void speak() {
		println("Amphibian speak");
	}
	void eat() {
		println("Amphibian eat");
	}
	static void grow(Amphibian a) {
		println("Amphibian grow");
		a.eat();
	}
}

public class Frog17 extends Amphibian {
	@Override protected void swim() {
		println("Frog swim");		
	}
	@Override protected void speak() {
		println("Frog speak");
	}
	@Override void eat() {
		println("Frog eat");
	}
	static void grow(Amphibian a) {
		println("Frog grow");
		a.eat();
	}
	public static void main(String[] args) {
		Frog17 f = new Frog17();
		// call overridden base-class methods:
		f.swim();
		f.speak();
		f.eat();
		// upcast Frog17 to Amphibian argument:
		f.grow(f);
		// upcast Frog17 to Amphibian and call Amphibian method:
		Amphibian.grow(f);
	}
}/*output:
Frog swim
Frog speak
Frog eat
Frog grow
Frog eat
Amphibian grow
Frog eat // !!
*///~
```

==*@override*==可以防止在不想重载的时候进行重载，想要覆写而不是重载的时候使用：当你不小心重载了就会报错

* 重写（复写）是**子类**的方法覆盖**父类**的方法，要求**方法名和参数相同**，访问修饰符的限制一定要大于被重写方法的访问修饰符，**只有非`private`方法才可以被覆盖**

* 重载是在同一个类中有两个或两个以上的方法，拥有**相同的方法名，但是参数不同**（参数个数、次序、类型不同），对返回值没有要求，可以相同，也可以不同，，方法体也不相同，但是如果参数的个数、类型、次序都相同，方法名也相同，仅返回值不同，则无法构成重载方法，最常见的重载例子就是类的构造函数。 

**`final`关键字**：“永远无法改变的”

**定义为`public`，则可以被用于包之外；定义为`static`，强调只有一份；定义为`final`，说明它是一个常量。**

空白`final`：必须在域的定义处或者每个构造器中用表达式对`final`进行赋值

`final`方法：把方法锁定 & 效率

**继承和初始化**

```java
//ex24
class Insect {
	private int i = 9;
	protected int j;
	Insect() {
		System.out.println("i = " + i + ", j = " + j);
		j = 39;
	}
	private static int x1 = printInit("static Insect.x1 initialized");
	static int printInit(String s) {
		System.out.println(s);
		return 47;
	}
}

class Beetle extends Insect {
	private int k = printInit("Beetle.k initialized");
	protected static int z = printInit("this is an order test");
	public Beetle() {
		System.out.println("k = " + k);
		System.out.println("j = " + j);
	}
	private static int x2 = printInit("static Beetle.x2 initialized");	
}

class Scarab extends Beetle {
	private int n = printInit("Scarab.n initialized");
	public Scarab() {
		System.out.println("n = " + n);
		System.out.println("j = " + j);
	}
	private static int x3 = printInit("static Scarab.x3 initialized");
	public static void main(String[] args) {
		System.out.println("Scarab constructor");
		Scarab sc = new Scarab();			
	}
}/*output
static Insect.x1 initialized
this is an order test
static Beetle.x2 initialized
static Scarab.x3 initialized
Scarab constructor
i = 9, j = 0
Beetle.k initialized
k = 47
j = 39
Scarab.n initialized
n = 47
j = 39
*///~
```



# 8 多态

继数据抽象和继承之后的第三种基本特征，作用是消除*类型*之间的耦合关系

*Java*中除了`static`方法和`final`方法，其他所有的方法都是后期绑定

后期绑定就是运行时根据对象的类型进行绑定

```java
// polymorphism/biking/Biking5.java
// TIJ4 Chapter Polymorphism, Exercise 5, page 286
/* Starting from Exercise 1, add a wheels() method in Cycle, which returns the
* number of wheels. MOdify ride() to call wheels() and verify that polymorphism
* works.
*/
package polymorphism.biking;
import static org.greggordon.tools.Print.*;

class Cycle {
	private String name = "Cycle";
	private int wheels = 0;
	public static void travel(Cycle c) {
		println("Cycle.ride() " + c);
	}
	public int wheels() { return wheels; }
	public String toString() {
		return this.name;
	}
}

class Unicycle extends Cycle {
	private String name = "Unicycle";
	private int wheels = 1;
	@Override public int wheels() { return wheels; }
	public String toString() {
		return this.name;
	}	
}

class Bicycle extends Cycle {
	private String name = "Bicycle";
	private int wheels = 2;
	@Override public int wheels() { return wheels; }
	public String toString() {
		return this.name;
	}	

}

class Tricycle extends Cycle {
	private String name = "Tricycle";
	private int wheels = 3;
	@Override public int wheels() { return wheels; }
	public String toString() {
		return this.name;
	}	
}

public class Biking5 {
	public static void ride(Cycle c) {
		c.travel(c);
		println("wheels: " + c.wheels());
	}
	public static void main(String[] args) {
		Unicycle u = new Unicycle();
		Bicycle b = new Bicycle();
		Tricycle t = new Tricycle();
		ride(u); // upcast to Cycle
		ride(b);
		ride(t);		
	}
}
```

```java
//ex10
class A {
	protected void f() { 
		System.out.println("A.f()");
		this.g(); 
	}
	protected void g() {
		System.out.println("A.g()"); 
	}
}

class B extends A {
	@Override protected void g() {
		System.out.println("B.g()");
	}
}

public class Ex10 {
	public static void main(String[] args) {
		B b = new B();
		// automatic upgrade to base-class to call base-class method A.f()
		// which,by polymorphism, will call the derived-class method B.g():
		b.f();
	}
}
```

多态方法允许一种类型表现出与其他相似类型的区别，只要它们是从同一基类中导出出来的

如果某个方法是静态的，它的行为不具有多态性：静态方法是与类而不是单个对象相关联的

构造器内部多态方法的行为： page 163

* 协变返回类型

用继承表达行为间的差异，用字段表达状态上的变化

* 组合对比继承
  * 继承结构,子类可以获得父类内部实现细节,破坏封装。组合结构:整体不能看到部分的内部实现细节,不会破坏封装
  * 继承模式是单继承,不支持动态继承,组合模式可以支持动态组合
  * 继承结构中,子类可以回溯父类,直到Object类,这样就可以根据业务实现多态(向上转型和向下转型)  ,组合中不可以实现多态
  * 在开发 过程中,如果复用的部分不会改变,为了安全性和封装的本质,应该使用组合,当我们不仅需要复用,而且还可能要重写或扩展,则应该使用继承

# 9 接口

**接口与内部类提供了一种将接口与实现分离的更加结构化的方法**

* 抽象方法
  * `abstract void f()`

```java
//ex3
abstract class Dad {
	protected abstract void print();
	Dad() { print(); }
}

class Son extends Dad {
	private int i = 1;
	@Override protected void print() { println("Son.i = " + i); }
}

public class Ex3 {
	public static void main(String[] args) {
		/* Process of initialization:
		* 1. Storage for Son s allocated and initialized to binary zero
		* 2. Dad() called
		* 3. Son.print() called in Dad() (s.i = 0)
		* 4. Member initializers called in order (s.i = 1)
		* 5. Body of Son() called
		*/
		Son s = new Son();
		s.print();
	}
}
```

* 接口 `interface`
  * 自动就是`public`

```java
package interfaces.music10;
import polymorphism.music.Note;
import static net.mindview.util.Print.*;

interface Instrument {
	// Compile-time constant:
	int VALUE = 5; // static and final
	// Cannot have method definitions:	
	void adjust(); 
}

interface Playable {
	void play(Note n); // Automatically public
}

class Wind implements Instrument, Playable {
	public void play(Note n) {
		print(this + ".play() " + n);
	}
	public String toString() { return "Wind"; }
	public void adjust() { print(this + ".adjust()"); }
}


class Percussion implements Instrument, Playable {
	public void play(Note n) {
		print(this + ".play() " + n);
	}
	public String toString() { return "Percussion"; }
	public void adjust() { print(this + ".adjust()"); }
}

class Stringed implements Instrument, Playable {
	public void play(Note n) {
		print(this + ".play() " + n);
	}
	public String toString() { return "Stringed"; }
	public void adjust() { print(this + ".adjust()"); }
}

class Brass extends Wind {
	public String toString() { return "Brass"; }
}

class Woodwind extends Wind {
	public String toString() { return "Woodwing"; }
}

public class Music10 {
	// Doesn't care about type, so new types
	// added to the system will work right:
	static void tune(Playable p) {
		//...
		p.play(Note.MIDDLE_C);
	}
	static void tuneAll(Playable[] e) {
		for(Playable p : e)
			tune(p);
	}
	public static void main(String[] args) {
		// Upcasting during addition to the array:
		Playable[] orchestra = {
			new Wind(),
			new Percussion(),
			new Stringed(),
			new Brass(),
			new Woodwind()
		};
		tuneAll(orchestra);
	}
}
```

* 策略设计模式：根据所传递的参数对象不同而具有不同行为的方法

* 适配器设计模式：适配器中的代码将接受的你接口，并产生你需要的接口

  * ```java
    class FilterAdapter implements Processor{
    	Filter filter;
        public FilterAdpater(Filter filter){
    		this.filter = filter;
        }
    }
    ```

**抽象类和接口的区别**

* 接口是抽象类的变体，接口中所有的方法都是抽象的；而抽象类是声明方法的存在而不去实现它的类
* 接口可以多继承，抽象类不行
* 接口定义方法，不能实现，而抽象类可以实现部分方法
* 接口中基本数据类型为static 而抽类象不是的
* 当你关注一个事物的本质的时候，用抽象类；当你关注一个操作的时候，用接口

* *Java*中的多重继承

```java
//ex13
interface CanDo {
	void doIt();
}

interface CanDoMore extends CanDo {
	void doMore();
}

interface CanDoFaster extends CanDo {
	void doFaster();
}

interface CanDoMost extends CanDoMore, CanDoFaster {
	void doMost();
}

class Doer implements CanDoMost {
	public void doIt() {}
	public void doMore() {}
	public void doFaster() {}
	public void doMost() {}
}

public class DiamondInheritance13 {
	public static void main(String[] args) {
		Doer d = new Doer();
		((CanDoMore)d).doMore();
		((CanDoFaster)d).doFaster();
		((CanDo)d).doIt();	
	}
}
```

* 通过继承来扩展接口

```java
//ex14 
//error but still working
interface Cat{
	void huntRat();
	void drinkMilk();
}

interface Dog{
	void bitCat();
	void eatBone();
}

interface Docat extends Cat, Dog{
	void scareHuman();
}

class Animal implements Docat{
	public void huntRat() { System.out.println("HuntRat");}
	public void drinkMilk() {}
	public void bitCat() {}
	public void eatBone() {}
	public void scareHuman() {System.out.println("scareHuman");}
	static void bite(Cat c) {
		c.huntRat();
	}
	static void scare(Docat d) {
		d.scareHuman();
	}
	public static void main(String[] args) {
		Animal a = new Animal();
		bite(a);
		scare(a);
	}
}
```

* 适配接口

  * **“你可以用任何你想要的对象来调用我的方法，只要你的对象遵循我的接口”**

  说实话这边没太看懂 page 182

接口中的域隐式的自动是`static`和`final`

* 嵌套接口

* 工厂模式：某个接口或者类的工厂对象上调用的是创建方法，而工厂对象将直接生成接口的某个实现的对象

  就是一层套一层，每一层都有对应的*方法类*和*工厂类*，最后只要在接口上调用就行

  ```java
  interface Service {
    void method1();
    void method2();
  }
  
  interface ServiceFactory {
    Service getService();
  }
  
  class Implementation1 implements Service {
    Implementation1() {} // Package access
    public void method1() {print("Implementation1 method1");}
    public void method2() {print("Implementation1 method2");}
  }	
  
  class Implementation1Factory implements ServiceFactory {
    public Service getService() {
      return new Implementation1();
    }
  }
  
  class Implementation2 implements Service {
    Implementation2() {} // Package access
    public void method1() {print("Implementation2 method1");}
    public void method2() {print("Implementation2 method2");}
  }
  
  class Implementation2Factory implements ServiceFactory {
    public Service getService() {
      return new Implementation2();
    }
  }	
  
  public class Factories {
    public static void serviceConsumer(ServiceFactory fact) {
      Service s = fact.getService();
      s.method1();
      s.method2();
    }
    public static void main(String[] args) {
      serviceConsumer(new Implementation1Factory());
      // Implementations are completely interchangeable:
      serviceConsumer(new Implementation2Factory());
    }
  } /* Output:
  Implementation1 method1
  Implementation1 method2
  Implementation2 method1
  Implementation2 method2
  *///:~
  ```

  

# 10 内部类

==内部类和组合是完全不同的概念==

如果想在外部类的非静态方法之外的任意位置创建某个内部类对象，则必须具体指明这个对象的类型：**`OuterClassName.InnerClassName`**

内部类拥有对外围类所有元素的访问权（包括`private`）

直接创建内部类的对象，必须使用外部类的对象来创建该内部类对象

* 使用`.this`与`.new`

* 内部类与向上转型

* 在方法和作用域内的内部类

* **匿名内部类 page 197**

  * 如果定义一个匿名内部类，并且希望它使用一个在其外部对应的对象，那么编译器会要求其参数引用是`final`的

  * 再访工厂方法：只需要单一的工厂对象了

    ```java
    import static net.mindview.util.Print.*;
    
    interface Service {
      void method1();
      void method2();
    }
    
    interface ServiceFactory {
      Service getService();
    }	
    
    class Implementation1 implements Service {
      private Implementation1() {}
      public void method1() {print("Implementation1 method1");}
      public void method2() {print("Implementation1 method2");}
      public static ServiceFactory factory =
        new ServiceFactory() {
          public Service getService() {
            return new Implementation1();
          }
        };
    }	
    ```

* 嵌套类

  * 如果不需要内部类对象和外围对象之间有联系，那么可以将内部类声明为`static`

  * **可以使用嵌套类来放置测试代码**

    ```java
    class TestBed {
      public void f() { System.out.println("f()"); }
      public static class Tester {
        public static void main(String[] args) {
          TestBed t = new TestBed();
          t.f();
        }
      }
    }
    ```

  * 为什么要使用内部类 page 204

    * *多重继承*的问题
    
  * 闭包与回调
  
    ```java
    //Callbacks.java
    interface Incrementable{
    	void increment();
    }
    
    class Callee1 implements Incrementable{
    	private int i = 0;
    	public void increment() {
    		i++;
    		System.out.println(i);
    	}
    }
    
    class MyIncrement{
    	public void increment() {System.out.println("other operation");}
    	static void f(MyIncrement mi) {mi.increment();}
    }
    
    class Callee2 extends MyIncrement{
    	private int i = 0;
    	public void increment() {
    		super.increment();
    		i += 10;
    		System.out.println(i);
    	}
    	private class Closure implements Incrementable{
    		public void increment(){
    			Callee2.this.increment();
    		}
    	}
    	Incrementable getCall() {
    		return new Closure();
    	}
    }
    
    	
    class Caller {
    	  private Incrementable callbackReference;
    	  Caller(Incrementable cbh) { callbackReference = cbh; }
    	  void go() { callbackReference.increment(); }
    	}
    
    class Callbacks {
      public static void main(String[] args) {
        Callee1 c1 = new Callee1();
        Callee2 c2 = new Callee2();
        MyIncrement.f(c2);
        Caller caller1 = new Caller(c1);
        Caller caller2 = new Caller(c2.getCall());
        caller1.go();
        caller1.go();
        caller2.go();
        caller2.go();
      }	
    }/*output:
    other operation
    10
    1
    2
    other operation
    20
    other operation
    30
    *///~
    ```
  
  * 内部类与控制框架
    * 主要用来响应事件的系统称为*事件驱动系统*
    * 使变化的事物与不变得事物分离
  * 内部类的继承 page 212
  * 局部内部类
    
    * 和匿名内部类功能差不多，区别在于匿名内部类只能用于实例初始化；而我们需要一个已命名的构造器，或者需要重载构造器时可以用局部内部类

# 11 持有对象

```java
import java.util.*;

public class chp11 {

}

class Gerbil{
	private int gerbilNumber;
	Gerbil(int i){
		gerbilNumber = i;
	}
	public void hop() {
		System.out.println(gerbilNumber);
	}
	public static void main(String[] args) {
		ArrayList<Gerbil> gerbil = new ArrayList<Gerbil>();
		for(int i = 0;i < 5;++i) {
			gerbil.add(new Gerbil(i));
		}
		for(Gerbil g: gerbil) {
			g.hop();
		}
	}
}
```

* 添加一组元素 page 220

* `Collection`：每个槽中只能保存一个元素，此类容器包括：`List`，以特定顺序保存一组元素；`Set`，元素不能重复；`Queue`，只允许在容器的一端插入对象，并从另一端移除对象

* `Map`：每个槽内保存了两个对象

* `List`:`ArrayList`和`LinkedList`
  * `contains()`：确定某个对象是否在列表中
  * `indexOf()`
  * `subList(start, end)`
  * `containAll()`
  * `XXX.retianAll()`:在`XXX`中保留交集元素

* 迭代器

  * 只能单向移动

  * 准备好返回序列的第一个元素，==要先`.next()`==

  * 接受容器对象并传递它，从而在每个对象上都执行操作，这种思想十分强大

    ```java
    Iterator<Gerbil> it = gerbil.iterator();
    while(it.hasNext()) 
        it.next().hop();
    ```

  * `ListIterator`

* `LinkedList` page 228

  * 栈、队列、双端队列

* `Stack`

* `Set`

* `Map`

  * `.keySet()`
  * `.values()`

* `Queue`

  * `peek()` `element()`

  * `poll()` `remove()`

  * `offer()`：插入队列

    public static void commander(Queue==<Command>== q) {
    		while(qc.peek() != null)
    			qc.poll().operation();
    	}

  * `PriorityQueue`

* `ForReach`与迭代器 page 241

  * 如果要创建自己的`iterator`，需要用适配器方法惯用法-两层

    ```java
    public Iterable<Pet> reversed() {
    		return new Iterable<Pet>() {
    			public Iterator<Pet> iterator() {
    				return new Iterator<Pet>() {
    					int current = pets.length - 1;
    					public boolean hasNext() {
    						return current > -1;
    					}
    					public Pet next() {
    						return pets[current--];
    					}
    					public void remove() {
    						throw new
    						UnsupportedOperationException();
    					}
    				};
    			}
    		};
    	}
    ```

  * 没有创建自己的`Iterator`:

    ```java
    public Iterable<Pet> randomized() {
    		return new Iterable<Pet>() {
    			public Iterator<Pet> iterator() {
    				List<Pet> shuffled = new
    				  	ArrayList<Pet>(Arrays.asList(pets));
    				Collections.shuffle(shuffled, new Random());
    				return shuffled.iterator();
    			}
    		};
    	}
    ```

* **总结**

  ![QQ截图20200404115217](D:\Typora_Image\QQ截图20200404115217-1585972362534.png)

# 12 通过异常处理错误

所有标准异常类都有两个构造器：一个是默认，一个接受**字符串**作为参数

* 异常与记录日志

  ```java
  //Ex7
  public class Ex7 {
  	private static int[] ia = new int[2];	
  	private static Logger logger = Logger.getLogger("Ex7 Exceptions");
  	static void logException(Exception e) { // Exception e argument
  		StringWriter trace = new StringWriter();
  		e.printStackTrace(new PrintWriter(trace));
  		logger.severe(trace.toString()); 	
  	}
  	public static void main(String[] args) {
  		try {
  			ia[2] = 3;	
  		} catch(ArrayIndexOutOfBoundsException e) {
  			System.err.println(
  				"Caught ArrayIndexOutOfBoundsException");
  			e.printStackTrace();
  			// call logging method:
  			logException(e);
  		} 
  	}	
  }
  ```

* 异常说明

* 捕获所有异常

  * `Exception`
  * ex 9 好像有点问题

* 栈轨迹

* 重新抛出异常

  * 会把异常抛给上一级环境中的异常处理程序

* 异常链

  * 在捕获一个异常后抛出另一个异常，并且希望把原始异常的信息保存下来

* `initCause()`方法

* 关于`RuntimeException`

  * 只能在代码中忽略`RuntimeException`（及其子类）类型的异常，其他类型异常的处理都是由编译器强制实施的

* 使用`finally`进行清理

  * 当需要把除内存之外的资源恢复到它们的初始状态时，就要用到`finally`字句
  * 当涉及`continue`和`break`语句的时候，`finally`字句也会得到执行
  * 即使在try块中间`return`也会起作用
  * 异常丢失

* 异常的限制 page 271

* 异常匹配

# 13 字符串

* 不可变String

* `StringBuilder`：**效率问题**

* `String`上的操作 page 288

* 格式化输出
  * `Formatter`类
  * 格式化说明符
  * `Formatter`转换
  * `String.format()`
  
* 正则表达式

  * `string.matches()`

  * 创建正则表达式 page 298

    ```java
    //Ex11
    import java.util.regex.*;
    
    public class RegEx11 {
    	public static void main(String[] args) {
    		Pattern p = 
    	Pattern.compile("(?i)((^[aeiou])|(\\s+[aeiou]))\\w+[aeiou]\\b");
    		Matcher m = p.matcher("Arline ate eight apples and one orange while Anita hadn't any");
    		while(m.find())
    			System.out.println("Match \"" + m.group() + "\" at " + m.start());
    	}
    }
    ```

    * `find()`可以在输入的任何位置定位正则表达式，而`lookingAt()`和`matches()`只有在表达式与输入的最开始处就开始匹配才会成功

    * `split()` page 305

      * 将输入字符串断开成字符串**数组**

    * `group()` page 302

    * 替换操作

    * 正则表达式和Java I/O

    * 扫描输入

      * `Scanner`类

        * `useDelimiter()`方法来设置定界符，默认为空白字符

        ```java
        //Ex20
        class Scanner20 {
        	int i;
        	long L;
        	float f;
        	double d;
        	String s;
        	Scanner20(String s) {
        		Scanner sc = new Scanner(s);
        		i = sc.nextInt();
        		L = sc.nextLong();
        		f = sc.nextFloat();
        		d = sc.nextDouble();
        		this.s = sc.next(); 		
        	}
        	public String toString() {
        		return i + " " + L + " " + f + " " + d + " " + s;
        	}
        	public static void main(String[] args) {
        		Scanner20 s20 = new Scanner20("17 56789 2.7 3.61412 hello");
        		System.out.println(s20);
        	}
        }
        ```

      * 使用正则表达式扫描

        ```java
        import java.util.regex.*;
        import java.util.*;
        
        public class ThreatAnalyzer {
          static String threatData =
            "58.27.82.161@02/10/2005\n" +
            "204.45.234.40@02/11/2005\n" +
            "58.27.82.161@02/11/2005\n" +
            "58.27.82.161@02/12/2005\n" +
            "58.27.82.161@02/12/2005\n" +
            "[Next log section with different data format]";
          public static void main(String[] args) {
            Scanner scanner = new Scanner(threatData);
            String pattern = "(\\d+[.]\\d+[.]\\d+[.]\\d+)@" +
              "(\\d{2}/\\d{2}/\\d{4})";
            while(scanner.hasNext(pattern)) {
              scanner.next(pattern);
              MatchResult match = scanner.match();
              String ip = match.group(1);
              String date = match.group(2);
              System.out.format("Threat on %s from %s\n", date,ip);
            }
          }
        } /* Output:
        Threat on 02/10/2005 from 58.27.82.161
        Threat on 02/11/2005 from 204.45.234.40
        Threat on 02/11/2005 from 58.27.82.161
        Threat on 02/12/2005 from 58.27.82.161
        Threat on 02/12/2005 from 58.27.82.161
        *///:~
        
        ```

# 14 类型信息

**`RTTI`**：在运行时，识别一个对象的类型

Runtime Type Information

* `Class`类

  * `forName()`是取得`Class`对象引用的一种方法
    * 参数是字符串
    * 必须使用全限定名（包括类名）
  * 使用类而做准备工作：
    * 加载
    * 链接
    * 初始化
  * 使用泛型语法，可以让编译器强制执行额外的类型检查 page 320
  * 用于`Class`引用的转型语法，即`cast()`方法

* 注册工厂

  * 工厂方法设计模式

* `instanceof`与直接比较（`==`）的等价性

* **反射：运行时的类信息 **page 335

  * 类方法提取器：动态地提取某个类的信息

  * `java.lang.reflect.*`

    ```java
    //Ex19
    import java.lang.reflect.*;
    
    interface HasBatteries {}
    interface Waterproof {}
    interface Shoots {}
    
    class Toy {
    	Toy() {
    		System.out.println("Creating Toy() object");	
    	}
    	Toy(int i) {
    		System.out.println("Creating Toy(" + i + ") object");
    	}
    }
    
    class FancyToy extends Toy 
    	implements HasBatteries, Waterproof, Shoots {
    		FancyToy() { super(1); }
    }
    
    class ToyTest19 {
    	public static void main(String[] args) {
    		// get appropriate constructor and create new instance:
    		try {
    			Toy.class.getDeclaredConstructor(int.class).newInstance(1);
    		// catch four exceptions:
    		} catch(NoSuchMethodException e) {
    			System.out.println("No such method: " + e);
    		} catch(InstantiationException e) {
    			System.out.println("Unable make Toy: " + e);
    		} catch(IllegalAccessException e) {
    			System.out.println("Unable access: " + e);
    		} catch(InvocationTargetException e) {
    			System.out.println("Target invocation problem: " + e);
    		}
    	}
    }
    ```

    

* 动态代理

  * 需要一个类加载器；一个希望代理实现的接口列表；`InvocationHandler`接口的一个实现

* 接口与类型信息

  * `callHiddenMethod()`：运用反射，可以到达并调用所有方法，甚至是`private`方法

# 15 泛型

`草草略过`

* 一个元组类库
* 泛型接口
* 泛型方法
* 匿名内部类
* 构建复杂模型
* 擦除神秘之处
* 通配符
* 自限定
* 混型

# 16 数组

# 17 容器深入研究

![1589330855603](D:\Typora_Image\1589330855603.png)

*享元*设计模式

使用情况：普通的解决方案需要过多的对象/产生普通对象太占空间

每个`Map.Entry`对象都只储存了其索引而不是实际的键和值（P468例子）

* 填充容器
  * `shuffle()`：打乱顺序
  * `map.headMap(key_point)`：返回map中键值严格小于key_point的键（`headSet()`同理）
* Collection相关功能方法
* 可选操作
  * `Arrays.asList()`生成的List是基于一个固定大小的数组，仅支持那些不会改变数组大小的操作
* Set和存储顺序
  * `instancof` 是 Java 的保留关键字。它的作用是测试它左边的对象是否是它右边的类的实例，返回`boolean `的数据类型。 
* 优先级队列

Comparable接口： https://www.cnblogs.com/xiatom/p/10784850.html 

* 双向队列

* 理解Map
  * `hashCode()`是Object类中的方法

  * `linkedHashMap`有五个构造函数，其中

    * ```java
      //真正有点特殊的就是这个，多了一个参数accessOrder。存储顺序，LinkedHashMap关键的参数之一就在这个，
      　　//true：指定迭代的顺序是按照访问顺序(近期访问最少到近期访问最多的元素)来迭代的。 false：指定迭代的顺序是按照插入顺序迭代，也就是通过插入元素的顺序来迭代所有元素
      //如果你想指定访问顺序，那么就只能使用该构造方法，其他三个构造方法默认使用插入顺序。
      
      22     public LinkedHashMap(int initialCapacity,
      23                          float loadFactor,
      24                          boolean accessOrder) {
      25         super(initialCapacity, loadFactor);
      26         this.accessOrder = accessOrder;
      27     }
      ```

* 微基准测试

* 对List的选择

  * ArrayList是首选，需要经常做增加删除的话使用LinkedList

* 对Set的选择

  * 只有需要排好序的Set的时候才有TreeSet
  * 注意``LinkedHashSet`的插入代价比`HashSet`高
  * `LinkedHashSet`的特点是内部使用链表维护元素的**插入顺序**

* 对Map的选择

  * `HashMap`的性能因子
    * 容量
    * 初始容量
    * 尺寸
    * 负载因子：尺寸/容量

* List的排序与查询

* 设定Collection或Map为不可更改

* 快速报错

* 持有引用

  * `SoftReference`
  * `WeakReference`
  * `PhantomReference`


# 21 并发

* 用并发解决的问题大体可分为：
  * 速度
  * 设计可管理性

实现并发最直接的方式是在操作系统级别使用**进程**

进程是运行在它自己的地址空间内的自包容的程序

一个**线程**就是在进程中的一个单一的顺序控制流

* `Thread`类

任何线程都可以启动另一个线程

线程调度机制是非确定性的

```java
package concurrency;
class Printer implements Runnable {
    private static int taskCount;
    private final int id = taskCount++;
    Printer() {
    System.out.println("Printer started, ID = " + id);
}
public void run() {
    System.out.println("Stage 1..., ID = " + id);
	Thread.yield();	
	System.out.println("Stage 2..., ID = " + id);
	Thread.yield();
	System.out.println("Stage 3..., ID = " + id);
	Thread.yield();
	System.out.println("Printer ended, ID = " + id);
}
}
public class E01_Runnable {
	public static void main(String[] args) {
	for(int i = 0; i < 5; i++)
		new Thread(new Printer()).start();
	}
}
```

* 使用`Exexutor`
  * `newCachedThreadPool`
  * `newFixedThreadPool`
  * `newSingleThreadPool`：线程数量为1的`FixedThreadPool`
* 从任务中产生返回值
  * 使用`Callable`接口
  * `ExecutorService.submit()`方法来调用
* 休眠
  * 对`sleep()`的调用可以抛出`InterruptedException`异常
  * 异常不能跨线程传播回main()，所以必须本地处理所有任务内部产生的异常
* 优先级
  * 优先权不会导致死锁
* 让步
  * `yield()`：给线程调度机制一个暗示
  * 大体上对于任何重要的控制或者在调整应用时，不能依赖于`yield()`
* 后台线程
  * 是指程序运行的时候在后台提供一种通用服务的线程
  * 所有非后台线程结束时，程序就终止了
* 编码的变体
* 加入一个线程
  * `.join()`
* 捕获异常
  * `Thread.UncaughtExceptionHandler`接口

***

* 共享受限资源
  *  加锁 or 根除对变量的共享
  * 互斥量
  * `synchronized`：将域设置为`private`是十分重要的
  * 使用显式的`Lock`对象
  * 原子性和易变性
  * 使用`volatile`而不是`synchronized`唯一安全的情况是类中只有一个可变的域
  * 原子类
  * 临界区
    * 使用同步控制块而不是对整个方法进行同步控制的原因：在保证安全的前提下，对象不加锁的时间更长，使得其他线程能够更多的访问
* 终结任务
  * 在阻塞时终结
  * **线程状态**
    * 新建
    * 就绪
    * 阻塞
    * 死亡
  * 中断
    * I/O和在`sybchronized`块上的等待是不可中断的，意味着I/O具有锁住你的多线程的潜在可能
    * 只要任务以不可中断的方式被阻塞，就会有潜在的会锁住程序的可能
    * 检查中断
  * 线程之间的协作
    * 握手问题 `wait()`和`notifyAll()`
    * *忙等待*：任务测试这个条件的同时，不断进行空循环
    * 只能在同步控制方法或者同步控制块里调用
    * **错失的信号**
    * 当`notifyAll()`因某个特定锁而被调用是，只有等待这个锁的任务才会被唤醒
    * **消费者与生产者**
      * 同步队列`BlockingQueue`
    * 任务间使用管道进行输入/输出
      * `PipedReader`和`PipedWriter`
  * **死锁**
    * 某个任务在等待另一个任务，后者又等待别的任务，这样一直下去，直到这个链条上的任务又在等待第一个任务释放锁。这得到了一个任务之间相互等待的连续循环，没有哪个线程能继续。
    * 哲学家就餐问题
    * **发生的条件：**
      * 互斥条件
      * 至少有一个任务持有资源并且正在等待一个当前被别的任务持有的资源
      * 资源不能被任务抢占，任务必须把资源释放当做普通事件
      * 必须有循环等待
  * 新类库中的构件
    * `CountDownLatch`：用来同步一个或多个任务，强制他们等待由其他任务执行的一组操作完成
      * 典型用法：将一个程序分割成n个相互独立的可解决任务，并创建一个值为0的`CountDownLatch`
      * 只能触发一次
    * `CyclicBarrier`：
      * 可以多次重用
      * 可以提供一个“栅栏动作”，当计数值到达0时自动执行
    * `DelayQueue`：无界的`BlockingQueue`，其中的对象只能在到期时才能从队列中取走
    * `PriorityBlockingQueue`
    * `ScheduledExecutor`
    * `Semaphore`
      * 计数信号量允许n个任务同时访问这个资源
    * `Exchanger`
      * 两个任务间交换对象的栅栏
    * 仿真
  * 性能调优
    * 乐观锁
    * 何为乐观，书中解释为：保持数据为未锁定状态，并希望没有任何其他任务插入修改它
  * 活动对象

# 22 图像化用户界面

* `Jframe`
* `JLabel`
* 创建按钮：`JButton`
* 捕获事件
* 文本区域
* 控制布局