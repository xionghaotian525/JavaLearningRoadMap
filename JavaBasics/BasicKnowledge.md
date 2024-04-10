# BasicKnowledge

## 目录
[toc]

## 一、个人总结
### 1.1语言基础
#### 1.1.1概述
![](/Res/images/Java基础-语言基础-概述.png)
#### 1.1.2数据类型
1. 基本类型：分为**四类八种**
    - 整型：byte（1字节）、short（2字节）、int（4字节）、long（8字节）
    - 浮点型：float（4字节）、double（8字节）
    - 字符型：char（2字节，表示单个字符）
    - 布尔型：boolean（表示真假值）
2. 基本数据类型都有其对应包装类
    基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用**自动装箱与拆箱**完成。  
    ```java
    Integer x = 2;     // 装箱
    int y = x;         // 拆箱
    ```
3. 枚举类型
4. 引用类型
   1. String
   1. 数组、集合
   2. 类、接口
#### 1.1.3关键字(final和static的详解)
1. **final**
   - final 关键字用于声明一个实体（变量、方法或类）是最终的，不能被修改。
   - **final变量**：一旦给final变量赋值后，就不能更改它的值。final修饰的变量效果上成为常量。例：`final int MAX_VALUE = 10;`
     - 对于基本类型，final 使数值不变；
     - 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。  
    ```java
    final int x = 1;
    // x = 2;  // cannot assign value to final variable 'x'
    final A y = new A();
    y.a = 1;
    ```
   - **final方法**：这种方法不能在子类中重写。
    ```java
    public final void showFinalMethod() {
        // 方法体
    }
    ``` 
   - **final类**：被声明为final的类不能被继承。
例：`public final class MyFinalClass { ... }`

- 使用final关键字可以让程序运行得更安全，因为它可以避免无意的赋值或继承导致的错误。同时，final 变量经常与不可变类搭配使用，以创建线程安全的常量。
     
2. **static**

   - static 关键字用于声明独立于对象的类成员。即它可以创建类级别的变量和方法。
   - **静态变量（Static Variable）**：
       - 静态变量: 又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，**可以直接通过类名来访问它**；静态变量在内存中只存在一份。
       - 实例变量: 每创建一个实例就会产生一个实例变量，它与该实例同生共死。
       
       ```java
       public class A {
           private int x;         // 实例变量
           private static int y;  // 静态变量

           public static void main(String[] args) {
               // int x = A.x;  // Non-static field 'x' cannot be referenced from a static context
               A a = new A();
               int x = a.x;
               int y = A.y;
           }
       }
       ```

   - **静态方法（Static Method）**：
       - 静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法(abstract)。
       ```java
       public abstract class A {
           public static void func1(){
           }
           // public abstract static void func2();  // Illegal combination of modifiers: 'abstract' and 'static'
       }
       ```
       - 只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字。
       ```java
       public class A {
           private static int x;
           private int y;

           public static void func1(){
               int a = x;
               // int b = y;  // Non-static field 'y' cannot be referenced from a static context
               // int b = this.y;     // 'A.this' cannot be referenced from a static context
           }
       }
       ```
   - **静态语句块**
     静态语句块在类初始化时运行一次，通常用于初始化变量。
     ```java
       public class A {
           static {
               System.out.println("123");
           }

           public static void main(String[] args) {
               A a1 = new A();
               A a2 = new A();
           }
       }
       //123
     ``` 
   - **静态内部类（Static Inner Class）**：它可以访问外部类的静态成员。
      - 非静态内部类依赖于外部类的实例，而静态内部类不需要。
       ```java
       public class OuterClass {
           class InnerClass {
           }

           static class StaticInnerClass {
           }

           public static void main(String[] args) {
               // InnerClass innerClass = new InnerClass(); 
               // 'OuterClass.this' cannot be referenced from a static context
               OuterClass outerClass = new OuterClass();
               InnerClass innerClass = outerClass.new InnerClass();
               StaticInnerClass staticInnerClass = new StaticInnerClass();
           }
       }
       ```
   - **静态导入（Static Import）**：允许导入类的静态成员而无需类前缀。
       - 在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。
    例：`import static java.lang.Math.*;`
   - **初始化顺序**：
       - 静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。
       ```java
       public static String staticField = "静态变量";
       ```
       ```java
       static {
           System.out.println("静态语句块");
       }
       ```
       ```java
       public String field = "实例变量";
       ```
       ```java
       {
           System.out.println("普通语句块");
       }
       ```
       ```java
       public InitialOrderTest() {
           System.out.println("构造函数");
       }
       ```
       - 存在继承的情况下，初始化顺序为:

           - 父类(静态变量、静态语句块)
           - 子类(静态变量、静态语句块)
           - 父类(实例变量、普通语句块)
           - 父类(构造函数)
           - 子类(实例变量、普通语句块)
           - 子类(构造函数)
   - 静态成员是类的一部分，而不是单个实例的一部分。这意味着如果一个对象修改了一个静态变量，这个修改会影响到所有的实例。

两者（static 和 final）也经常一起使用，例如在定义静态常量的时候，static final 一起使用能够创建全局常量。
#### 1.1.4变量声明与初始化
1. 变量声明：指定变量类型并赋予名字
   - 示例：`int number;`
2. 变量初始化：为变量赋予初始值
   - 示例：`number = 10;`
3. 常量声明：使用final关键字定义常量，常量一旦赋值不能改变
   - 示例：`final double PI = 3.14;`
#### 1.1.5运算符
![](/Res/images/Java基础-语言基础-运算符.png)
1. 算术运算符：`+`、`-`、`*`、`/`、`%`（取模）、`++`（自增）、`--`（自减）
2. 比较运算符：`==`、`!=`、`>`、`<`、`>=`、`<=`
3. 逻辑运算符：`&&`（与）、`||`（或）、`!`（非）
4. 赋值运算符：`=`、`+=`、`-=`、`*=`、`/=`、`%=`等
5. 位运算符：`&`（和）、`|`（或）、`^`（异或）、`~`（取反）、`<<`（左移）、`>>`（右移）
#### 1.1.6控制流语句
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
2. 循环语句：
    **for**：一种有限次循环，通常知道循环次数时使用
    **while**：当某条件为真，执行循环体
    **do-while**：先执行循环体至少一次，然后检查条件
3. 控制转移语句：
    **break**：退出循环或switch语句
    **continue**：跳过当前循环的剩余部分执行下一次循环
    **return**：退出当前方法
#### 1.1.7类型转换
1. 隐式类型转换：小类型自动转换到大类型，如int自动转换成double
2. 显式类型转换（强制类型转换）：需要显示声明新类型，可能造成数据丢失
    - 示例：`int a = (int) 3.14;`
#### 1.1.8字符串处理
![](/Res/images/Java基础-语言基础-字符串处理.png)
```java
public class StringExample {
    public static void main(String[] args) {
        // 创建字符串
        String greeting = "Hello";

        // 字符串连接
        String name = "World";
        String message = greeting + ", " + name + "!";
        System.out.println(message);

        // 计算字符串长度
        int length = message.length();
        System.out.println("Length of the message: " + length);
        
        // 截取字符串
        String sub = message.substring(0, 5); // 截取Hello
        System.out.println("Substring: " + sub);

        // 转换大小写
        String upperMessage = message.toUpperCase();
        String lowerMessage = message.toLowerCase();
        System.out.println("Uppercase: " + upperMessage);
        System.out.println("Lowercase: " + lowerMessage);

        // 比较字符串
        String anotherGreeting = "HELLO";
        System.out.println("Are greetings equal? " + greeting.equalsIgnoreCase(anotherGreeting));

        // 检查字符串是否包含另一个字符串
        System.out.println("Does message contain name? " + message.contains(name));

        // 替换字符串中的字符
        String replacedMessage = message.replace("l", "r");
        System.out.println("Replaced message: " + replacedMessage);

        // 去掉字符串首尾空格
        String spacedString = "  lots of space  ";
        System.out.println("Trimmed string: '" + spacedString.trim() + "'");

        // 字符串分割
        String[] words = message.split(" ");
        System.out.println("The message split into words:");
        for (String word : words) {
            System.out.println(word);
        }

        // 检查字符串开头和结尾的字符或子字符串
        System.out.println("Does the message start with 'Hello'? " + message.startsWith("Hello"));
        System.out.println("Does the message end with 'world!'? " + message.endsWith("world!"));

        // 获取指定位置的字符
        char specificChar = message.charAt(1);
        System.out.println("The character at index 1 is: " + specificChar);

        // 字符串格式化
        String formatted = String.format("This is a %s formatted by %s", "string", "String.format");
        System.out.println(formatted);
    }
}
```
#### 1.1.9数组
![](/Res/images/Java基础-语言基础-数组.png)
```java
import java.util.Arrays;

public class ArrayOperations {
    public static void main(String[] args) {
        // 创建并初始化数组
        int[] numbers = {3, 5, 1, 4, 2};

        // 打印原始数组
        System.out.println("Original array: " + Arrays.toString(numbers));

        // 排序数组
        Arrays.sort(numbers);
        System.out.println("Sorted array: " + Arrays.toString(numbers));

        // 搜索数组中的元素（例如，查找'4'的位置）
        int index = Arrays.binarySearch(numbers, 4);
        System.out.println("The element 4 is at index: " + index);

        // 复制数组
        int[] copiedNumbers = Arrays.copyOf(numbers, numbers.length);
        System.out.println("Copied array: " + Arrays.toString(copiedNumbers));

        // 填充数组
        Arrays.fill(copiedNumbers, 10);
        System.out.println("Array after filling: " + Arrays.toString(copiedNumbers));

        // 比较两个数组
        System.out.println("Arrays equals? " + Arrays.equals(numbers, copiedNumbers));
    }
}
```
#### 1.1.10 Object类通用方法
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
1. **equals()**

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
      - 检查是否为同一个对象的引用，如果是直接返回 true；
      - 检查是否是同一个类型，如果不是，直接返回 false；
      - 将 Object 对象进行转型；
      - 判断每个关键域是否相等。
    ```java
    public class EqualExample {
        private int x;
        private int y;
        private int z;

        public EqualExample(int x, int y, int z) {
            this.x = x;
            this.y = y;
            this.z = z;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;

            EqualExample that = (EqualExample) o;

            if (x != that.x) return false;
            if (y != that.y) return false;
            return z == that.z;
        }
    }

    ```

3. **hashcode()**

   - hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。
   - 在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。
   - 下面的代码中，新建了两个等价的对象，并将它们添加到 HashSet 中。我们希望将这两个对象当成一样的，只在集合中添加一个对象，但是因为 EqualExample 没有实现 hasCode() 方法，因此这两个对象的散列值是不同的，最终导致集合添加了两个等价的对象
    ```java
    EqualExample e1 = new EqualExample(1, 1, 1);
    EqualExample e2 = new EqualExample(1, 1, 1);
    System.out.println(e1.equals(e2)); // true
    HashSet<EqualExample> set = new HashSet<>();
    set.add(e1);
    set.add(e2);
    System.out.println(set.size());   // 2
    ``` 
   - 理想的散列函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的散列值上。这就要求了散列函数要把所有域的值都考虑进来，可以将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数。R 一般取 31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位。
   - 一个数与 31 相乘可以转换成移位和减法: `31*x == (x<<5)-x`，编译器会自动进行这个优化。
    ```java
    @Override
    public int hashCode() {
        int result = 17;
        result = 31 * result + x;
        result = 31 * result + y;
        result = 31 * result + z;
        return result;
    }
    ```

4. **toString()**

   - `toString()` 方法是 Java 中的 `Object` 类的一个方法，用于返回对象的字符串表示。该方法通常被子类重写以返回有意义的字符串表示。

   - 默认情况下，`Object` 类中的 `toString()` 方法返回的是**对象的类名**，后跟 `@` 符号和**对象的哈希码的无符号十六进制表示**。例如，对于一个名为 `obj` 的对象，其默认的 `toString()` 返回值通常会类似于 `ClassName@hashCode`。默认返回 `ToStringExample@4554617c` 这种形式，其中 @ 后面的数值为**散列码的无符号十六进制**表示。
    ```java
    public class ToStringExample {
        private int number;

        public ToStringExample(int number) {
            this.number = number;
        }
    }
    ToStringExample example = new ToStringExample(123);
    System.out.println(example.toString());
    //输出：ToStringExample@4554617c
    ```

    ```java
    public class Person {
        private String name;
        private int age;

        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }

        // 重写toString方法
        @Override
        public String toString() {
            return "Person{name='" + name + "', age=" + age + "}";
        }

        public static void main(String[] args) {
            Person person = new Person("Alice", 30);
            System.out.println(person); // 输出：Person{name='Alice', age=30}
        }
    }
    ```
5. **clone()**

   - **cloneable**
        clone() 是 Object 的 protected 方法，默认对对象进行浅拷贝,它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。
        ```java
        public class CloneExample {
            private int a;
            private int b;
        }
        CloneExample e1 = new CloneExample();
        // CloneExample e2 = e1.clone(); // 'clone()' has protected access in 'java.lang.Object'
        ```
        Cloneable 接口是一个标记接口，用于指定对象是“可克隆的”。所谓标记接口，即没有包含方法的接口，它的作用是告诉 Object 类的 clone() 方法，当对实现了 Cloneable 接口的对象调用 clone() 方法时可以合法地进行字段到字段的复制。
        ```java
        public class MyClass implements Cloneable {
            // fields here

            @Override
            public Object clone() throws CloneNotSupportedException {
                return super.clone();
            }
        }
        ```
        当类实现了 Cloneable 接口，它就告诉 Object 的 clone() 方法：“我可以进行复制”。如果尝试克隆一个未实现 Cloneable 接口的对象，clone() 方法将抛出 `CloneNotSupportedException` 异常。

        ```java
        public class CloneExample {
            private int a;
            private int b;

            @Override
            protected CloneExample clone() throws CloneNotSupportedException {
                return (CloneExample)super.clone();
            }
        }
        CloneExample e1 = new CloneExample();
        try {
            CloneExample e2 = e1.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        //输出：java.lang.CloneNotSupportedException: CloneExample
        ```

   - **浅拷贝和深拷贝**
        
        **解惑：** 对于基本数据类型来说，不存在浅拷贝和深拷贝的区别。基本数据类型的字段在拷贝时总是创建值的一个副本，所以修改克隆对象的基本类型字段不会影响源对象的同名字段（可以理解为深拷贝）；深拷贝和浅拷贝是只针对Object和Array这样的引用数据类型的。实现cloneable接口、重写clone()方法时默认是浅拷贝，实现深拷贝需要在克隆的引用类型里重写clone()方法、修改clone方法（额外对引用类型进行一次克隆？，参考下方代码注释）。
        **浅拷贝：拷贝对象和原始对象的引用类型引用同一个对象。**
        **深拷贝：拷贝对象和原始对象的引用类型引用不同对象。**
        ```java
        class Address {
            String street;

            Address(String street) {
                this.street = street;
            }

            // 浅拷贝情况下，这里不需要实现clone方法
            // 深拷贝情况下，需要实现这段代码
            /*
            @Override
            protected Object clone() throws CloneNotSupportedException {
                return super.clone();
            }
            */
        }

        class Person implements Cloneable {
            int age;
            Address address;

            Person(int age, String street) {
                this.age = age;
                this.address = new Address(street);
            }

            @Override
            protected Object clone() throws CloneNotSupportedException {
                // 默认的clone方法执行的就是浅拷贝
                Person cloned = (Person) super.clone();

                // 若要实现深拷贝，请取消以下注释的代码段
                /*
                cloned.address = (Address) address.clone();
                */

                return cloned;
            }
        }
        ```
        ```java
        public class CloneExample {
            public static void main(String[] args) {
                Person originalPerson = new Person(30, "1234 Broadway");
                try {
                    Person clonedPerson = (Person) originalPerson.clone();

                    // 浅拷贝：此时originalPerson与clonedPerson的address属性指向同一个Address对象
                    // 深拷贝：如果取消了Person的clone方法中的注释，那么会为address创建一个新的对象拷贝

                    System.out.println("Before changes:");
                    System.out.println("Original person address: " + originalPerson.address.street);
                    System.out.println("Cloned person address: " + clonedPerson.address.street);
                    
                    // 改变原始对象的Address street字段
                    originalPerson.address.street = "5678 Main Street";

                    System.out.println("After changes to original person's address street:");
                    System.out.println("Original person address: " + originalPerson.address.street);
                    System.out.println("Cloned person address: " + clonedPerson.address.street);

                    // 在浅拷贝的情况下，clonedPerson的address也会发生变化，因为它们引用了同一个Address对象
                    // 在深拷贝的情况下，clonedPerson的address不会发生变化，因为它引用了一个全新的Address对象副本

                } catch (CloneNotSupportedException e) {
                    e.printStackTrace();
                }
            }
        }
        ```
        **浅拷贝输出结果：**
        ```java
        Before changes:
        Original person address: 1234 Broadway
        Cloned person address: 1234 Broadway
        After changes to original person's address street:
        Original person address: 5678 Main Street
        Cloned person address: 5678 Main Street
        ```
        这里可以看到，在浅拷贝后，`originalPerson` 和 `clonedPerson` 的 `address` 属性指向了同一个 `Address` 对象，因此当 `originalPerson` 的 `street` 改变后，`clonedPerson` 中的 `street` 也随之改变。

        **深拷贝输出结果：**（如果实现了 `Address` 类的 `clone` 方法并在 `Person` 的 `clone` 方法中取消了注释的代码段）
        ```java
        Before changes:
        Original person address: 1234 Broadway
        Cloned person address: 1234 Broadway
        After changes to original person's address street:
        Original person address: 5678 Main Street
        Cloned person address: 1234 Broadway
        ```
        在这种情况下，`clonedPerson` 的 `address` 指向了一个新的 `Address` 对象，该对象是 `originalPerson` 的 `address` 的一个副本。因此，当更改 `originalPerson` 的 `street` 时不会影响 `clonedPerson`。
   - **clone()的替代方案**
        使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换,可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。
        - **拷贝构造函数**
            ```java
            class Address {
                String city;
                String street;

                public Address(String city, String street) {
                    this.city = city;
                    this.street = street;
                }

                // Address类的拷贝构造函数
                public Address(Address otherAddress) {
                    this.city = otherAddress.city;
                    this.street = otherAddress.street;
                }
            }

            class Person {
                String name;
                Address address;

                public Person(String name, Address address) {
                    this.name = name;
                    this.address = address;
                }

                // Person类的拷贝构造函数
                public Person(Person otherPerson) {
                    this.name = otherPerson.name;
                    // 使用Address类的拷贝构造函数，以实现深拷贝
                    this.address = new Address(otherPerson.address);
                }
            }
            ```
            ```java
            public class CopyConstructorExample {
                public static void main(String[] args) {
                    // 创建一个Person及其Address
                    Person originalPerson = new Person("John Doe", new Address("New York", "5th Ave"));

                    // 使用拷贝构造函数创建originalPerson的一个新副本
                    Person copiedPerson = new Person(originalPerson);

                    // 输出两个对象的信息，检查它们是否相同
                    System.out.println("Original Person: " + originalPerson.name + 
                                    ", Address: " + originalPerson.address.city + 
                                    " " + originalPerson.address.street);

                    System.out.println("Copied Person: " + copiedPerson.name + 
                                    ", Address: " + copiedPerson.address.city + 
                                    " " + copiedPerson.address.street);

                    // 修改originalPerson的地址信息
                    originalPerson.address.street = "Broadway";

                    // 再次输出，检查copiedPerson的地址是否改变
                    System.out.println("After changing the original person's address:");
                    System.out.println("Original Person: " + originalPerson.name + 
                                    ", Address: " + originalPerson.address.city + 
                                    " " + originalPerson.address.street);

                    System.out.println("Copied Person: " + copiedPerson.name + 
                                    ", Address: " + copiedPerson.address.city + 
                                    " " + copiedPerson.address.street);
                }
            }
            ```
            在这个例子中，我们使用了Address类和Person类的拷贝构造函数，借此实现了深拷贝。因此，即使我们改变了originalPerson的address属性，copiedPerson的address属性也不会受到影响。
            **输出结果：**
            ```java
            Original Person: John Doe, Address: New York 5th Ave
            Copied Person: John Doe, Address: New York 5th Ave
            After changing the original person's address:
            Original Person: John Doe, Address: New York Broadway
            Copied Person: John Doe, Address: New York 5th Ave
            ```
        - **拷贝工厂方法**:与拷贝构造函数类似，不过是以静态工厂方法的形式存在。
            ```java
            public class Example {
                private int number;

                public static Example newInstance(Example another) {
                    return new Example(another.number);
                }

                private Example(int number) {
                    this.number = number;
                }
            }
            ```
        - **序列化**
            通过序列化和反序列化来创建对象的深拷贝。这通常涉及将对象写入一个流中，然后再从流中读回来，但这也是一个非常昂贵的操作，仅适合复杂对象拷贝或者对象图拷贝。
            ```java
            public static Object deepCopy(Object object) {
                try {
                    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
                    ObjectOutputStream outputStrm = new ObjectOutputStream(outputStream);
                    outputStrm.writeObject(object);
                    ByteArrayInputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
                    ObjectInputStream objInputStream = new ObjectInputStream(inputStream);
                    return objInputStream.readObject();
                } catch (Exception e) {
                    e.printStackTrace();
                    return null;
                }
            }
            ``` 
      - **Apache Commons Lang**
      `SerializationUtils.clone()`方法使用序列化来创建对象的拷贝，这要求被拷贝的对象实现`Serializable`接口。 
        ```java
            Example cloneExample = (Example) SerializationUtils.clone(originalExample);
        ```
      - **Spring Framework**
      如果你正在使用Spring框架，可以使用`BeanUtils.copyProperties()`方法来拷贝同类之间的属性（它不会拷贝属性值为null的）。

### 1.2面向对象
思维导图
![](/Res/images/Java基础-面向对象.png)
#### 1.2.1类
1. **自定义类**
格式：[访问修饰符] [修饰符] class 自定义类名 [extends 父类] [implement 接口1,接口2...]{ }
2. **内部类**
内部类提供了一种实现类的嵌套和封装的机制
   - 内部类是定义在其他类内部的类。
   - 内部类可以访问外部类的所有成员，包括私有成员。
   - 内部类分为四种类型：成员内部类、静态内部类、局部内部类和匿名内部类。
   - 内部类可以实现面向对象设计的一些模式，如事件监听器、迭代器等。

1. **抽象类**
抽象类提供了一种定义子类的模板和规范的机制
   - 抽象类是不能被实例化的类，它用于定义子类的结构和行为。
   - 抽象类中可以包含抽象方法和非抽象方法。
   - 抽象方法是没有方法体的方法，必须在子类中实现。
   - 子类继承抽象类时，必须要实现抽象类中的所有抽象方法，除非子类也是抽象类。

1. **接口**
接口提供了一种定义公共行为和多重继承的机制
   - 接口是一种抽象数据类型，定义了一组方法的规范，而不包含方法的实现。
   - 接口中的方法默认是公有的抽象方法，可以省略 public abstract 关键字。
   - 类可以实现一个或多个接口，通过关键字 implements。
   - 接口之间可以通过关键字 extends 进行继承，一个接口可以继承多个接口。
   - 接口提供了一种多重继承机制，可以帮助解决Java单继承的限制。
   - 接口常用于定义不同类之间的共同行为，实现类可以根据需要选择实现哪些接口。

1. **类的访问修饰符**
    - 顶级类
      - public
      - 默认
    - 方法、属性、内部类
      - public
      - protected
      - private
      - 默认
2. **类与类的关系**
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

#### 1.2.2对象
1. 一切皆为对象
2. 对象有方法和属性
3. 对象都是唯一的
4. 对象都是某个类的实例
#### 1.2.3方法
1. 方法的声明格式：
   
    **[访问修饰符] [修饰符] 返回类型 方法名([参数列表]) throws [异常类型] { }**
    ```java
    public static int add(int a, int b) throws ArithmeticException {
        // 方法体
        return a + b;
    }
    ```
    说明：方法名为 `add`，接受两个参数 `a` 和 `b`，返回类型为 `int`，可能抛出 `ArithmeticException` 异常。
    - 访问修饰符
      - public
      - private
      - protected
    - 修饰符
      - static
      - abstract
      - final
      - synchronized
      - native

2. 方法调用
    - 静态方法：类名.方法名(实参列表);
    - 非静态方法：对象.方法名(实参列表)
#### 1.2.4三大特性
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
    
    **访问修饰符** | **同一个类** | **同一个包** | **子类** | **任何地方**
    ---|---|---|---|---|
    public|Y|Y|Y|Y
    protected|Y|Y|Y
    default|Y|Y|
    pravite|Y|

  - public
    - public修饰符表示公开的，公共的。不同类、不同包下都可以访问

    - 1个java文件中只可以有一个public修饰的类，并且类名需要和文件名相同
  - private
    - 可用来修饰内部类、属性、方法

    - “私有的”，即被private修饰的属性、方法、类**只能被该类的对象访问**，其子类不能访问，更不能允许跨包访问

    - 注意：private可以修饰内部类，不可以修饰外部类
    ```java
    class test{//private不能修饰外部类
        private String name;//private修饰属性
        private void test(){//private修饰方法
            System.out.println("private修饰方法");
        }
        private class innerClass{//private修饰内部类

        }
    }
    ```
  - protected
    - protected修饰符表示受保护的，它主要的作用是保护子类，子类可以用它修饰的成员，其他的不可以

    - protected修饰符可以被本类、同一个包中的类、不同包中的子类所访问到

    - protected可以修饰属性、方法，但是不能修饰外部类，可以修饰内部类    
    ```java
    class test{//不能用protected修饰外部类
        protected String name;//protected修饰属性
        protected void demo(){//protected修饰方法
            System.out.println("protected修饰方法");
        }
        protected class innerClass{//protected修饰内部类

        }
    }
    ``` 
  - default  包级可见 
  
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

#### 1.2.5设计原则
- 单一职责原则
  - 专业化
  - 模块化
- 开闭环原则
  - 对修改关闭
  - 对扩展开放
  - 抽象是关键
  - 封装可变性
- 依赖倒转原则
- 里氏替换原则
- 接口隔离原则
- 合成聚合复用原则
- 迪米特法则
## 二、八股问题整理

### (1).基本数据类型及其对应封装类
Java中有8种基本数据类型，它们不是对象，为了能将这些基本数据类型当作对象操作，Java为每个基本数据类型都提供了对应的包装类。

**基本数据类型** | **大小 (字节)** | **默认值** | **值域** | **封装类**
---|---|---|---|---|
byte | 1 | 0 | -128至127 | Byte
short | 2 | 0 | -32768至32767 | Short
int | 4 | 0 | `-2^31`至`2^31-1` | Integer
long | 8 | 0L | `-2^63`至`2^63-1` |Long
float | 4 | 0.0f | 它用于保存单精度、分数值等小数点可能变化的值。 |Float
double | 8 | 0.0d | 提供比float更大的值域和精度。 |Double
boolean | 1 bit | false | 通常是true或false |Boolean
char | 2 | '\u0000' | 用于存储单个字符（如'A'或'9'） |Character

**为什么要使用封装类？**

* **对象化操作**: 封装类允许将基本数据类型作为对象处理，可以调用方法进行操作，例如类型转换、比较等。
* **集合类**: Java的集合类，例如List、Map等，只能操作对象类型，不能直接存储基本数据类型。使用封装类可以将基本数据类型存储到集合中。
* **空值**: 基本数据类型不能表示空值，而封装类可以表示空值，例如 `Integer i = null;`。
* **实用方法**: 封装类提供了许多实用类方法，例如类型转换、进制转换、字符串处理等，方便开发者操作数据。

### (2).自动装箱与拆箱
从Java 5开始，提供了自动装箱和拆箱的功能，可以方便地将基本数据类型和封装类进行转换。

* **自动装箱**: 自动将基本数据类型转换为对应的封装类对象。例如：`Integer i = 10;` 等价于 `Integer i = Integer.valueOf(10);`
* **自动拆箱**: 自动将封装类对象转换为对应的基本数据类型。例如：`int n = i;` 等价于 `int n = i.intValue();`

### (3).类型转换
在Java中，类型转换是将一种数据类型的值转换为另一种数据类型的过程。类型转换主要分为两种：隐式类型转换和显式类型转换。

1. 隐式类型转换（自动类型转换）：当一个数据类型的值赋给另一个数据类型的变量时，如果目标数据类型的范围大于源数据类型的范围，Java会自动进行隐式类型转换。例如，将一个整数赋给一个浮点数类型的变量，或者将一个小范围的整数类型赋给一个大范围的整数类型变量。

```java
int numInt = 10;
double numDouble = numInt; // 隐式类型转换，将int类型转换为double类型
```

2. 显式类型转换（强制类型转换）：当目标数据类型的范围小于源数据类型的范围时，需要使用显式类型转换来将数据类型转换为目标类型。这种转换在编码时需要显式地指定，并且需要使用强制类型转换操作符`(目标数据类型)`。

```java
double numDouble = 10.5;
int numInt = (int) numDouble; // 显式类型转换，将double类型转换为int类型
```

显式类型转换可能会导致数据丢失或溢出
### (4).object类中的常用方法
![](/Res/images/object类常用方法.png)
**clone():**
protected方法，实现对象的浅复制，只有实现了 Cloneable 接口才可以调用该方法，否则抛出CloneNotSupportedException 异常，深拷贝也需要实现 Cloneable，同时其成员变量为**引用类型**的也需要实现 Cloneable，然后重写 clone 方法。
**finalize():**
该方法和垃圾收集器有关系，判断一个对象是否可以被回收的最后一步就是判断是否重写了此方法。
**equals():**
在 Object 类中，`equals` 方法用于比较两个对象是否相等。默认实现是比较两个对象的内存地址，也就是说，在没有被重写的情况下，当且仅当两个引用指向同一对象时，equals 方法才返回 true。

以下是 Object 类中 equals 方法的默认实现：
```java
public boolean equals(Object obj) {
    return (this == obj);
}
```
这种实现方式在许多情况下并不适用，因为我们通常需要根据对象的内容来判断两个对象是否相等，而不是它们在内存中的位置。因此，当创建自定义类时，通常需要根据类的实际需求重写 equals 方法。

例如，假设有一个 Person 类，我们希望比较两个 Person 对象时基于它们的 id 字段：
```java
public class Person {
    private int id;
    private String name;

    // 构造器、getter、setter 省略

    @Override
    public boolean equals(Object obj) {
        // 检查是否为同一对象的引用，如果是，返回 true
        if (this == obj) {
            return true;
        }
        // 检查是否为同一类型，如果不是，返回 false
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        // 强制类型转换后比较对应字段是否相等
        Person person = (Person) obj;
        return this.id == person.id;
    }
}
```
这里 equals 方法首先检查是否是相同的引用。如果不是，则检查是否为 null 或者是否为同类型的实例。如果这些基本检查通过了，它接着会将传入的 Object 类型的参数转换（或称类型强制转换）为 Person 类型，并比较对应字段（在此例中是 id 字段）是否相同。

重写 equals 方法时，还要注意的是，通常也应该同时重写 hashCode 方法，以保持 equals 方法和 hashCode 方法的一致性，这是因为在使用基于哈希码的集合（如 HashSet 和 HashMap）时，equals 和 hashCode 需要遵守一定的规则，确保对象能够正确地存储和检索。
**hashcode():**
`hashCode()` 是Java对象的一个方法，返回一个代表对象内存地址的整数。它主要用于优化hash-based集合（如HashMap, HashSet）的性能。如果两个对象通过`equals()`方法相等，则它们的hashCode()值必须相同。相反，不同对象可以有相同的哈希码，但这可能会降低哈希表的效率。通常，重写`equals()`方法时要同步重写`hashCode()`，以保持二者间的一致性。
**toString():**
toString() 方法在Java中用于生成对象的字符串表示形式
**wait()、notify()、notifyAll():**
wait(), notify(), 和 notifyAll()用于协调多线程之间的同步。
- `wait()` 方法使当前线程等待，直到另一个线程在同一个对象上调用`notify()`或`notifyAll()`，或者直到超时。
- `notify()` 方法唤醒在此对象监视器上等待的单个线程，如果有多个线程等待，会随机选择一个。
- `notifyAll()` 方法唤醒在此对象监视器上等待的所有线程。

这些方法必须在同步块或同步方法中调用，即在对象的监视器锁内。它们被设计用来创建一个使线程能够等待某个条件的满足，然后再继续执行的方式。
### (5).浅拷贝与深拷贝
- 浅拷贝创建一个新对象，但只复制原始对象的引用类型字段的引用，而不是引用的对象本身。这意味着新对象和原始对象的引用类型字段将指向相同的对象。**拷贝对象和原始对象的引用类型引用同一个对象。**
- 深拷贝创建一个新对象，同时递归地复制原始对象的所有字段，包括所有的引用类型字段指向的对象，从而实现完全独立的复制。**拷贝对象和原始对象的引用类型引用不同对象。**

简而言之，浅拷贝只复制对象的一层属性，而深拷贝则复制对象的多层结构。
### (6).面向对象三大特性
- 封装
- 继承
- 多态
### (7).访问修饰符
- default：默认访问修饰符，在同一包内可见
- private：在同一类内可见，不能修饰类
- protected：对同一包内的类和所有子类可见，不能修饰类
- public：对所有类可见
### (8).抽象类和接口
抽象类和接口都是 Java 中用于实现抽象类型的机制，它们的作用和用法有所不同。

**抽象类（Abstract Class）：**

1. **定义**：抽象类是一种不能实例化的类，通常用于表示一类对象的共性特征，其中可能包含抽象方法和具体方法。
2. **特点**：
   - 可以包含抽象方法和非抽象方法。
   - 可以有构造方法，但无法直接实例化。
   - 可以包含成员变量和普通方法。
   - 子类必须实现所有抽象方法，或者声明自己为抽象类。

**接口（Interface）：**

1. **定义**：接口是一种完全抽象的类型，其中只包含抽象方法、常量和默认方法（Java 8及以上版本）、静态方法（Java 8及以上版本）。
2. **特点**：
   - 只包含常量和抽象方法，不包含具体实现。
   - 不允许包含普通方法和成员变量，除非是默认方法或静态方法。
   - 用于描述类的行为或功能，实现接口的类必须提供接口中定义的所有方法的实现。
   - 一个类可以实现多个接口，但只能继承一个父类。

**区别总结**：

1. 抽象类可以有构造方法，而接口不能有构造方法。
2. 抽象类可以包含成员变量和普通方法，而接口只能包含常量和抽象方法。
3. 一个类只能继承一个抽象类，但可以实现多个接口。
4. 抽象类的目的是为了继承和代码复用，接口的目的是为了实现类的多态和解耦。
5. 抽象类用于表示"is-a"关系，接口用于表示"has-a"关系或"can-do"关系。

在设计中，抽象类通常用于描述一类对象的通用行为和属性，而接口用于定义类的功能和能力，以提供更灵活的设计方案。
### (9).super关键字
- 访问父类的构造函数: 可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
- 访问父类的成员: 如果子类重写了父类的中某个方法的实现，可以通过使用 super 关键字来引用父类的方法实现。
### (10).重载和重写
1. 重写
   - 重写(Override)存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

    - 为了满足里式替换原则，重写有以下两个限制:
      - 子类方法的访问权限必须大于等于父类方法；
      - 子类方法的返回类型必须是父类方法返回类型或为其子类型。

    - 使用 @Override 注解，可以让编译器帮忙检查是否满足上面的两个限制条件。

2. 重载
   - 重载(Overload)存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。
   - 应该注意的是，返回值不同，其它都相同不算是重载。

### (11).关键字static
- 静态变量
  - 静态变量: 又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，可以直接通过类名来访问它；静态变量在内存中只存在一份。
  - 实例变量: 每创建一个实例就会产生一个实例变量，它与该实例同生共死。
- 静态方法
  - 静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法(abstract)。
  - 只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字。
- 静态内部类
  - 非静态内部类依赖于外部类的实例，而静态内部类不需要。
  - 静态内部类不能访问外部类的非静态的变量和方法。
- 静态语句块
  - 静态语句块在类初始化时运行一次。
- 静态导包
  - 在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。
### (12).关键字final
- 数据
  - 对于基本类型，final 使数值不变；
  - 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。
- 方法
  - 声明方法不能被子类重写。
- 类
  - 声明类不允许被继承。

### (13).类的初始化顺序

- 静态变量
- 静态语句块
- 实例变量
- 普通语句块
- 构造函数

存在继承的情况下，初始化顺序为:

- 父类(静态变量、静态语句块)
- 子类(静态变量、静态语句块)
- 父类(实例变量、普通语句块)
- 父类(构造函数)
- 子类(实例变量、普通语句块)
- 子类(构造函数)

> 来自[pdai.tech-Java基础-QA](https://pdai.tech/md/java/basic/java-basic-lan-sum.html)

### (14).Java 中应该使用什么数据类型来代表价格?
如果不是特别关心内存和性能的话，使用BigDecimal，否则使用预定义精度的 double 类型。
### (14).怎么将 byte 转换为 String?
可以使用 String 接收 byte[] 参数的构造器来进行转换，需要注意的点是要使用的正确的编码，否则会使用平台默认编码，这个编码可能跟原来的编码相同，也可能不同。

### (14).Java 中怎样将 bytes 转换为 long 类型?
String接收bytes的构造器转成String，再Long.parseLong
### (14).我们能将 int 强制转换为 byte 类型的变量吗? 如果该值大于 byte 类型的范围，将会出现什么现象?
是的，我们可以做强制转换，但是 Java 中 int 是 32 位的，而 byte 是 8 位的，所以，如果强制转化是，int 类型的高 24 位将会被丢弃，byte 类型的范围是从 -128 到 127

### (14).存在两个类，B 继承 A，C 继承 B，我们能将 B 转换为 C 么? 如 C = (C) B；
可以，向下转型。但是不建议使用，容易出现类型转型异常.
### (14).哪个类包含 clone 方法? 是 Cloneable 还是 Object?
java.lang.Cloneable 是一个标示性接口，不包含任何方法，clone 方法在 object 类中定义。并且需要知道 clone() 方法是一个本地方法，这意味着它是由 c 或 c++ 或 其他本地语言实现的。

### (14).Java 中 ++ 操作符是线程安全的吗?（重要）
不是线程安全的操作。它涉及到多个指令，如读取变量值，增加，然后存储回内存，这个过程可能会出现多个线程交差。还会存在竞态条件(读取-修改-写入)。
### (14).a = a + b 与 a += b 的区别
`+=` 隐式的将**加操作的结果类型强制转换为持有结果的类型**。如果两个整型相加，如 byte、short 或者 int，首先会将它们提升到 int 类型，然后在执行加法操作。
```java
byte a = 127;
byte b = 127;
b = a + b; // error : cannot convert from int to byte
b += a; // ok
```

(因为 a+b 操作会将 a、b 提升为 int 类型，所以将 int 类型赋值给 byte 就会编译出错)

### (14).我能在不进行强制转换的情况下将一个 double 值赋值给 long 类型的变量吗?
不行，你不能在没有强制类型转换的前提下将一个 double 值赋值给 long 类型的变量，因为 double 类型的范围比 long 类型更广，所以必须要进行强制转换。

### (15).3*0.1 == 0.3 将会返回什么? true 还是 false?
false，因为有些浮点数不能完全精确的表示出来。

### (15).int 和 Integer 哪个会占用更多的内存?
Integer 对象会占用更多的内存。Integer 是一个对象，需要存储对象的元数据。但是 int 是一个原始类型的数据，所以占用的空间更少。
### (15).为什么 Java 中的 String 是不可变的(Immutable)?
Java 中的 String 不可变是因为 Java 的设计者认为字符串使用非常频繁，将字符串设置为不可变可以允许多个客户端之间共享相同的字符串。更详细的内容参见答案。

### (15).我们能在 Switch 中使用 String 吗?
从 Java 7 开始，我们可以在 switch case 中使用字符串，但这仅仅是一个语法糖。内部实现在 switch 中使用字符串的 hash code。

### (15).Java 中的构造器链是什么?
当你从一个构造器中调用另一个构造器，就是Java 中的构造器链。这种情况只在重载了类的构造器的时候才会出现。
### (15).什么是不可变对象(immutable object)? Java 中怎么创建一个不可变对象?
不可变对象指对象一旦被创建，状态就不能再改变。任何修改都会创建一个新的对象，如 String、Integer及其它包装类。

如何在Java中写出Immutable的类?要写出这样的类，需要遵循以下几个原则:
- immutable对象的状态在创建之后就不能发生改变，任何对它的改变都应该产生一个新的对象。
- Immutable类的所有的属性都应该是final的。
- 对象必须被正确的创建，比如: 对象引用在对象创建过程中不能泄露(leak)。
- 对象应该是final的，以此来限制子类继承父类，以避免子类改变了父类的immutable特性。
- 如果类中包含mutable类对象，那么返回给客户端的时候，返回该对象的一个拷贝，而不是该对象本身(该条可以归为第一条中的一个特例)

### (15).我们能创建一个包含可变对象的不可变对象吗?
是的，我们是可以创建一个包含可变对象的不可变对象的，你只需要谨慎一点，不要共享可变对象的引用就可以了，如果需要变化时，就返回原对象的一个拷贝。最常见的例子就是对象中包含一个日期对象的引用。

在Java中，可以创建一个**包含可变对象的不可变对象**，**即使它包含可变的对象引用**。不可变性意味着一旦对象被创建，它的状态就不能改变。要实现这个目标，您需要确保：
  - 对象一旦被创建，其所有属性都不应该被修改。
  - 对象的所有可变字段都应该是私有的，并且不提供直接修改这些字段的方法。
  - 对象的类不能有子类，所以类应该被声明为`final`。

当您的不可变对象持有其他可变对象的引用时，必须小心地遵循以上原则。确保您不共享这些可变对象的参考至任何可以修改它们的方法。通常，这可以通过以下做法来实现：
   - 对于可变类型的字段，提供一个生成副本的构造器或方法。
   - 不提供修改可变字段的直接方法。
   - 当需要返回内部状态的可变对象时，提供其不可变视图或其深复制副本，而不是实际的对象引用。

下面是一个例子，表明如何创建一个包含可变对象引用（如Date对象）的不可变类：

```java
import java.util.Date;

public final class ImmutablePerson {
    private final String name;
    private final Date birthDate; // Date is mutable

    public ImmutablePerson(String name, Date birthDate) {
        this.name = name;
        // Make a defensive copy of mutable objects
        this.birthDate = new Date(birthDate.getTime());
    }

    public String getName() {
        return name;
    }

    public Date getBirthDate() {
        // Return a defensive copy of mutable objects
        return new Date(birthDate.getTime());
    }

    // No setters provided for any of the fields
}
```
说明：在这个`ImmutablePerson`类中，尽管`Date`类是可变的，通过在构造器中进行防御性复制，并在`getter`方法中返回一个新的`Date`对象而不是实际的对象引用，确保了`ImmutablePerson`类对象的不可变性。这个类没有提供修改字段状态的`setter`方法，并且这个类是`final`的，所以它不可以被继承。
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
### (15).
