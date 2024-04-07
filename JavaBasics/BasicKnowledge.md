# BasicKnowledge

## 目录
[toc]

## 一、个人总结
### 语言基础
1. **数据类型**
    1. 基本类型：分为**四类八种**
        - 整型：byte（1字节）、short（2字节）、int（4字节）、long（8字节）
        - 浮点型：float（4字节）、double（8字节）
        - 字符型：char（2字节，表示单个字符）
        - 布尔型：boolean（表示真假值）
        - 引用类型：类（Class）、接口（Interface）、数组（Array）
    2. 基本数据类型都有其对应包装类
        基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用**自动装箱与拆箱**完成。  
        ```java
        Integer x = 2;     // 装箱
        int y = x;         // 拆箱
        ```
2. **变量和常量**
    1. 变量声明：指定变量类型并赋予名字
       - 示例：`int number;`
    2. 变量初始化：为变量赋予初始值
       - 示例：`number = 10;`
    3. 常量声明：使用final关键字定义常量，常量一旦赋值不能改变
       - 示例：`final double PI = 3.14;`
3. **运算符**
    1. 算术运算符：`+`、`-`、`*`、`/`、`%`（取模）、`++`（自增）、`--`（自减）
    2. 比较运算符：`==`、`!=`、`>`、`<`、`>=`、`<=`
    3. 逻辑运算符：`&&`（与）、`||`（或）、`!`（非）
    4. 赋值运算符：`=`、`+=`、`-=`、`*=`、`/=`、`%=`等
    5. 位运算符：`&`（和）、`|`（或）、`^`（异或）、`~`（取反）、`<<`（左移）、`>>`（右移）
4. **控制流程语句**
    1. 条件语句：
        - **if、else if、else**：根据条件执行不同代码块
        - **switch**：根据不同的case值执行不同代码块
        ```java
        //java7开始switch支持String对象
        //switch不支持long
        String s = "a";
        switch (s) {
            case "a":
                System.out.println("aaa");
                break;
            case "b":
                System.out.println("bbb");
                break;
        }
        ``` 
    1. 循环语句：
        **for**：一种有限次循环，通常知道循环次数时使用
        **while**：当某条件为真，执行循环体
        **do-while**：先执行循环体至少一次，然后检查条件
    2. 控制转移语句：
        **break**：退出循环或switch语句
        **continue**：跳过当前循环的剩余部分执行下一次循环
        **return**：退出当前方法
1. **类型转换**
    1. 隐式类型转换：小类型自动转换到大类型，如int自动转换成double
    2. 显式类型转换（强制类型转换）：需要显示声明新类型，可能造成数据丢失
        - 示例：`int a = (int) 3.14;`
2. **字符串处理**
    1. 字符串连接：使用+连接字符串
    2. 常用方法：length()、charAt()、substring()、indexOf()、toLowerCase()、toUpperCase()等
3. **数组**
    1. 声明：指定元素类型及数组名
        - 示例：`int[] myArray;`
    2. 初始化：创建数组并分配空间，可以在声明时或之后进行
        - 示例：`myArray = new int[10];`
    3. 访问：使用索引访问数组元素
        - 示例：`myArray[0] = 50;`
    4. 多维数组：数组的数组，如二维数组
4. **包（Packages）**
    1. 定义：将类组织在不同的命名空间里，便于管理
    2. 导入：使用import语句导入其他包的类
5. **注释**
    1. 单行注释：//
    2. 多行注释：/* ... */
    3. 文档注释：/** ... */
6. **Object通用方法**
   1. 概览
   ```java
    public final native Class<?> getClass()

    public native int hashCode()

    public boolean equals(Object obj)

    protected native Object clone() throws CloneNotSupportedException

    public String toString()

    public final native void notify()

    public final native void notifyAll()

    public final native void wait(long timeout) throws InterruptedException

    public final void wait(long timeout, int nanos) throws InterruptedException

    public final void wait() throws InterruptedException

    protected void finalize() throws Throwable {}

   ```
   1. equals()

- 等价关系
    ```java
    //自反性
    x.equals(x); // true
    //对称性
    x.equals(y) == y.equals(x); // true
    //传递性
    if (x.equals(y) && y.equals(z))
        x.equals(z); // true;
    //一致性 多次调用 equals() 方法结果不变
    x.equals(y) == x.equals(y); // true
    //与null的比较 对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false
    x.equals(null); // false;
    ```
- equals与==
  - 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
  - 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。  
    ```java
        Integer x = new Integer(1);
        Integer y = new Integer(1);
        System.out.println(x.equals(y)); // true
        System.out.println(x == y);      // false    
    ```
- 实现

   1. hashcode()
   2. toString()
   3. clone()
1. **关键字**
   1. final
   2. static
### 面向对象

1. **三大特性**
    1. **封装**

- 利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。用户无需知道对象内部的细节，但可以通过对象对外提供的接口来访问该对象。
    
-   优点：
    - 减少耦合: 可以独立地开发、测试、优化、使用、理解和修改
    - 减轻维护的负担: 可以更容易被程序员理解，并且在调试的时候可以不影响其他模块
    - 有效地调节性能: 可以通过剖析确定哪些模块影响了系统的性能
    - 提高软件的可重用性
    - 降低了构建大型系统的风险: 即使整个系统不可用，但是这些独立的模块却有可能是可用的

- 以下 Person 类封装 name、gender、age 等属性，外界只能通过 get() 方法获取一个 Person 对象的 name 属性和 gender 属性，而无法获取 age 属性，但是 age 属性可以供 work() 方法使用。
- 注意到 gender 属性使用 int 数据类型进行存储，封装使得用户注意不到这种实现细节。并且在需要修改 gender 属性使用的数据类型时，也可以在不影响客户端代码的情况下进行。

    ```java
    public class Person {

        private String name;
        private int gender;
        private int age;

        public String getName() {
            return name;
        }

        public String getGender() {
            return gender == 0 ? "man" : "woman";
        }

        public void work() {
            if (18 <= age && age <= 50) {
                System.out.println(name + " is working very hard!");
            } else {
                System.out.println(name + " can't work any more!");
            }
        }
    }
    ```

    2. **继承**

- 继承实现了 IS-A 关系，例如 Cat 和 Animal 就是一种 IS-A 关系，因此 Cat 可以继承自 Animal，从而获得 Animal 非 private 的属性和方法。
- 继承应该遵循里氏替换原则，子类对象必须能够替换掉所有父类对象。
- Cat 可以当做 Animal 来使用，也就是说可以使用 Animal 引用 Cat 对象。父类引用指向子类对象称为 向上转型 。
    
    ```java
    Animal animal = new Cat();
    ```
- 访问权限
  - public
  - private
  - protected
  - default
- 抽象类与接口
  - 抽象类
    - 抽象类和抽象方法都使用 abstract 关键字进行声明。抽象类一般会包含抽象方法，抽象方法一定位于抽象类中。
    - 抽象类和普通类最大的区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。 
  - 接口
    - 接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。
    - 从 Java 8 开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。
    - 接口的成员(字段 + 方法)默认都是 public 的，并且不允许定义为 private 或者 protected。
    - 接口的字段默认都是 static 和 final 的。
  - 对比：
    - 目的：
        - 抽象类用于表示“是一个”关系，它表达的是一种继承关系。
        - 接口用于表示“能做什么”关系，它表达的是一种能力。
    - 使用场景：
        - 当多个类之间存在较多共享状态或行为时，使用抽象类更合适。
        - 当多个类需要实现多个可选的行为时，接口更加适用。
    - 扩展性：
        - 抽象类在Java中限制了子类的扩展性，因为Java不允许多重继承。
        - 接口提供了更好的扩展性，特别是在需要多个类实现同一个行为或多个行为时。
- super
  - 访问父类的构造函数: 可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
  - 访问父类的成员: 如果子类重写了父类的中某个方法的实现，可以通过使用 super 关键字来引用父类的方法实现。
  ```java
    public class SuperExample {
        protected int x;
        protected int y;

        public SuperExample(int x, int y) {
            this.x = x;
            this.y = y;
        }

        public void func() {
            System.out.println("SuperExample.func()");
        }
    }

    public class SuperExtendExample extends SuperExample {
        private int z;

        public SuperExtendExample(int x, int y, int z) {
            super(x, y);
            this.z = z;
        }

        @Override
        public void func() {
            super.func();
            System.out.println("SuperExtendExample.func()");
        }
    }

    SuperExample e = new SuperExtendExample(1, 2, 3);
    e.func();
  ``` 

  ```java
    SuperExample.func()
    SuperExtendExample.func()
  ```
- 重写和重载
    - 重写：

      - 存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。
      - 为了满足里式替换原则，重写有以下两个限制:
        - 子类方法的访问权限必须大于等于父类方法；
        - 子类方法的返回类型必须是父类方法返回类型或为其子类型。
      - 使用 `@Override` 注解，可以让编译器帮忙检查是否满足上面的两个限制条件。

    - 重载
        - 存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。

        - 应该注意的是，返回值不同，其它都相同不算是重载。
    3. **多态**

- 多态分为编译时多态和运行时多态:
  - 编译时多态主要指方法的重载
  - 运行时多态指程序中定义的对象引用所指向的具体类型在运行期间才确定
- 运行时多态有三个条件:
  - 继承
  - 覆盖(重写)
  - 向上转型
- 下面的代码中，乐器类(Instrument)有两个子类: Wind 和 Percussion，它们都覆盖了父类的 play() 方法，并且在 main() 方法中使用父类 Instrument 来引用 Wind 和 Percussion 对象。在 Instrument 引用调用 play() 方法时，会执行实际引用对象所在类的 play() 方法，而不是 Instrument 类的方法。

    ```java
    public class Instrument {
        public void play() {
            System.out.println("Instrument is playing...");
        }
    }

    public class Wind extends Instrument {
        public void play() {
            System.out.println("Wind is playing...");
        }
    }

    public class Percussion extends Instrument {
        public void play() {
            System.out.println("Percussion is playing...");
        }
    }

    public class Music {
        public static void main(String[] args) {
            List<Instrument> instruments = new ArrayList<>();
            instruments.add(new Wind());
            instruments.add(new Percussion());
            for(Instrument instrument : instruments) {
                instrument.play();
            }
        }
    }
    ```
2. **类图**
    1. **泛化关系 (Generalization)**
    
    用来描述继承关系，在 Java 中使用 `extends` 关键字。
    ![](/Res/images/Java基础-面向对象-类图-泛化关系.png)

    2. **实现关系 (Realization)**
    
    用来实现一个接口，在 Java 中使用 implements 关键字。
    ![](/Res/images/Java基础-面向对象-类图-实现关系.png)

    3. **聚合关系 (Aggregation)**
    
    表示整体由部分组成，但是整体和部分不是强依赖的，整体不存在了部分还是会存在。
    ![](/Res/images/Java基础-面向对象-类图-聚合关系.png)

    4. **组合关系 (Composition)**
    
    和聚合不同，组合中整体和部分是强依赖的，整体不存在了部分也不存在了。比如公司和部门，公司没了部门就不存在了。但是公司和员工就属于聚合关系了，因为公司没了员工还在。
    ![](/Res/images/Java基础-面向对象-类图-组合关系.png)

    5. **关联关系 (Association)**
    
    表示不同类对象之间有关联，这是一种静态关系，与运行过程的状态无关，在最开始就可以确定。因此也可以用 1 对 1、多对 1、多对多这种关联关系来表示。比如学生和学校就是一种关联关系，一个学校可以有很多学生，但是一个学生只属于一个学校，因此这是一种多对一的关系，在运行开始之前就可以确定
    ![](/Res/images/Java基础-面向对象-类图-关联关系.png)

    6. **依赖关系 (Dependency)**

    和关联关系不同的是，依赖关系是在运行过程中起作用的。A 类和 B 类是依赖关系主要有三种形式:
    - A 类是 B 类中的(某中方法的)局部变量；
    - A 类是 B 类方法当中的一个参数；
    - A 类向 B 类发送消息，从而影响 B 类发生变化；
    
    ![](/Res/images/Java基础-面向对象-类图-依赖关系.png)

## 二、八股问题整理