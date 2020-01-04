---
title: 4小时java入门
date: 2019-05-07 19:07:57
tags: [Java, Java入门]
categories: [Learn X in Y minutes, Learn Java in Y minutes]
---

译自：[https://learnxinyminutes.com/docs/java/](https://learnxinyminutes.com/docs/java/)

ps：好多类都写到一起了，自己要运行的话记得写到不同的java文件中哦

```java
// 导入java.util包中的ArrayList类
import java.io.BufferedReader;
import java.io.FileReader;
import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.*;
// 导入java.util包中的Scanner类
// 导入java.security包中的所有类
import java.security.*;
import java.util.function.Supplier;


public class LearnJava {

    // 要运行一个java程序，必须要有一个main方法作为入口
    public static void main(String[] args) {
    
    ///////////////////////////////////////
    // 输入/输出
    ///////////////////////////////////////

        /*
        * 输出
        */

        //通过 System.out.println() 来输出一行数据
        System.out.println("Hello World!");
        System.out.println(
            "Integer: " + 10 +
            " Double: " + 3.14 +
            " Boolean: " + true
        );

        //如果输出之后不想换行，可以使用System.out.print()
        System.out.print("Hello ");
        System.out.print("World");

//        /*
//        * 输入
//        */
//
//        //通过Scanner读取输入
//        //必须import java.util.Scanner;
//        Scanner scanner = new Scanner(System.in);
//
//        //读取string输入
//        String name = scanner.next();
//
//        //读取byte输入
//        byte numByte = scanner.nextByte();
//
//        //读取int输入
//        int numInt = scanner.nextInt();
//
//        //读取long输入
//        float numFloat = scanner.nextFloat();
//
//        //读取oduble输入
//        double numDouble = scanner.nextDouble();
//
//        //读取boolean输入
//        boolean bool = scanner.nextBoolean();

        ///////////////////////////////////////
        // 变量
        ///////////////////////////////////////

        /*
        * 变量声明
        */
        //通过<type> <name>声明变量
        int fooInt;
        //声明多个变量：
        // <type> <name1>, <name2>, <name3>
        int fooInt1, fooInt2, fooInt3;

        /*
         *  变量初始化
         */

        // 通过<type> <name> = <val>来初始变量
        int barInt = 1;
        // 初始化多个变量：
        // <type> <name1>, <name2>, <name3>
        // <name1> = <name2> = <name3> = <val>
        int barInt1, barInt2, barInt3;
        barInt1 = barInt2 = barInt3 = 1;

        /*
         *  变量类型
         */
        // Byte - 8位有符号二进制补码整数
        // (-128 <= byte <= 127)
        byte fooByte = -100;

        // 如果您想将一个字节解释为无符号整数
        // 只需要下边这句
        int unsignedIntLessThan256 = 0xff & fooByte;
        // 下边这句
        int signedInt = (int) fooByte;


        // Short - 16位有符号二进制补码整数
        // (-32,768 <= short <= 32,767)
        short fooShort = 10000;

        // Integer - 32位有符号二进制补码整数
        // (-2,147,483,648 <= int <= 2,147,483,647)
        int bazInt = 1;

        // Long - 64位有符号二进制补码整数
        // (-9,223,372,036,854,775,808 <= long <= 9,223,372,036,854,775,807)
        long fooLong = 100000L;
        // 数值后边的'L'指示这个变量是long类型
        // 没有L的话依然会默认用int存储

        // 注意: byte, short, int 和 long 都是有符号的. 也就是说可以为正数或负数
        // 没有无符号变量
        // 然而，char就是16位无符号的

        // Float - 单精度32位IEEE 754浮点数
        // 2^-149 <= float <= (2-2^-23) * 2^127
        float fooFloat = 234.5f;
        // 数字末尾的f 或者 F 是用来标识这个变量通过float进行存储
        // 否则就会默认按照double存储

        // Double - 双精度64位IEEE 754浮点数
        // 2^-1074 <= x <= (2-2^-52) * 2^1023
        double fooDouble = 123.4;

        // Boolean - true & false
        boolean fooBoolean = true;
        boolean barBoolean = false;

        // Char - 一个16位的unicode字符
        char fooChar = 'A';

        // final 变量不能被重新赋值
        final int HOURS_I_WORK_PER_WEEK = 9001;
        // 但是可以稍后初始化
        final double E;
        E = 2.71828;

        // BigInteger - 不可变的任意精度整数
        //
        // BigInteger是一类数据类型，允许程序员操作超过64位长度的整数
        // 这样生成的整数存储为字节组成的数组，并通过内建在BigInteger中的函数进行操作
        //
        // BigInteger 可以通过一个字节组成的数组或者字符串初始化
        String fooByteArray = "1000000000000000000000000000000000000000000";
        BigInteger fooBigInteger = new BigInteger(fooByteArray);

        // BigDecimal - 不可变的，任意精度的带符号十进制数
        //
        // BigDecimal包含两个部分:一个任意精度的整数无标度值和一个32位整数标度
        //
        // BigDecimal允许程序员完全控制十进制舍入
        // 建议将BigDecimal与精度值一起使用，并在需要精确小数精度的地方使用BigDecimal
        //
        // BigDecimal 可以使用int、long、double或String初始化
        // 也可以初始化未缩放值(BigInteger)和scale (int)
        fooInt = 1;
        BigDecimal fooBigDecimal = new BigDecimal(fooBigInteger, fooInt);

        //警惕采用float或double的构造函数
        //float/double的不准确性也会被拷贝到BigDecimal中
        //当您需要精确值时，首选String构造函数
        BigDecimal tenCents = new BigDecimal("0.1");

        // String：字符串
        String fooString = "My String Is Here!";

        // \n 是一个转义字符，开始一个新行
        String barString = "Printing on a new line?\nNo Problem!";
        // \t 是一个转义字符，用于添加制表符
        String bazString = "Do you want to add a tab?\tNo Problem!";
        System.out.println(fooString);
        System.out.println(barString);
        System.out.println(bazString);

        // 字符串构建
        // #1 - 通过加号"+"运算符
        // 这是实现它的基本方法（在通过引擎优化）
        String plusConcatenated = "Strings can " + "be concatenated " + "via + operator.";
        System.out.println(plusConcatenated);
        // 输出: 字符串可以通过+运算符连接

        // #2 - 通过StringBuilder
        //这种方式不会创建任何中间字符串。 它只存储字符串片段，并在调用toString（）时将它们连接在一起
        //提示：此类不是线程安全的。 StringBuffer是一种线程安全的替代方案（对性能有一些影响）。
        StringBuilder builderConcatenated = new StringBuilder();
        builderConcatenated.append("You ");
        builderConcatenated.append("can use ");
        builderConcatenated.append("the StringBuilder class.");
        System.out.println(builderConcatenated.toString()); //字符串只在现在构建
        // 输出: You can use the StringBuilder class.

        // 在完成某些处理结束之前不需要完全构造的String时，StringBuilder非常有效。
        StringBuilder stringBuilder = new StringBuilder();
        String inefficientString = "";
        for (int i = 0 ; i < 10; i++) {
            stringBuilder.append(i).append(" ");
            inefficientString += i + " ";
        }
        System.out.println(inefficientString);
        System.out.println(stringBuilder.toString());
        // inefficientString需要更多的工作来生成，因为它在每次循环迭代时都会生成一个String。
        // 使用+进行简单字符串连接，编译后相当于同时调用StringBuilder和toString（）
        // 使用StringBuilder可以避免循环中的字符串连接。

        // #3 - 使用String格式化器
        // 另一种创建字符串的替代方法，快速且可读。
        String.format("%s may prefer %s.", "Or you", "String.format()");
        // 输出: Or you may prefer String.format().


        // Arrays：数组
        // 必须在实例化时确定数组大小
        // 以下格式用于声明数组
        // <datatype>[] <var name> = new <datatype>[<array size>];
        // <datatype> <var name>[] = new <datatype>[<array size>];
        int[] intArray = new int[10];
        String[] stringArray = new String[1];
        boolean boolArray[] = new boolean[100];

        // 声明和初始化数组的另一种方法
        int[] y = {9000, 1000, 1337};
        String names[] = {"Bob", "John", "Fred", "Juan Pedro"};
        boolean bools[] = {true, false, false};

        // 索引数组 - 访问元素
        System.out.println("intArray @ 0: " + intArray[0]);

        // 数组是零索引且可变的
        intArray[1] = 1;
        System.out.println("intArray @ 1: " + intArray[1]);

        // 其他值得一试的数据类型
        // ArrayLists  - 除了提供更多功能之外的类似数组，且数组长度是可变的
        // LinkedLists - 双向链表的实现，全部操作按照双向链表执行。
        // Maps - 键对象到值对象的映射。 Map是一个接口，因此无法实例化。 必须在实现类的实例化时指定Map中包含的键和值的类型。 每个键可以仅映射到一个对应的值，并且每个键可以仅出现一次（没有重复）。
        // HashMaps - 此类使用哈希表来实现Map接口。 这允许基本操作的执行时间（例如get和insert元素）即使对于大型集合也保持不变。
        // TreeMap - 按键排序的映射。 每个修改都维护由实例化时提供的Comparator定义的排序，或者如果它们实现Comparable接口则对每个Object进行比较。 密钥实现Comparable失败以及无法提供Comparator将导致ClassCastExceptions。 插入和删除操作需要O(log(n))时间，因此除非您正在利用排序，否则请避免使用此数据结构。

        ///////////////////////////////////////
        // Operators：运算符
        ///////////////////////////////////////
        System.out.println("\n->Operators");

        int i1 = 1, i2 = 2; // 多个声明的简写

        // 算数很直接
        System.out.println("1+2 = " + (i1 + i2)); // => 3
        System.out.println("2-1 = " + (i2 - i1)); // => 1
        System.out.println("2*1 = " + (i2 * i1)); // => 2
        System.out.println("1/2 = " + (i1 / i2)); // => 0 (int/int 返回 int)
        System.out.println("1/2.0 = " + (i1 / (double)i2)); // => 0.5

        // Modulo：模运算
        System.out.println("11%3 = "+(11 % 3)); // => 2

        // 比较运算符
        System.out.println("3 == 2? " + (3 == 2)); // => false
        System.out.println("3 != 2? " + (3 != 2)); // => true
        System.out.println("3 > 2? " + (3 > 2)); // => true
        System.out.println("3 < 2? " + (3 < 2)); // => false
        System.out.println("2 <= 2? " + (2 <= 2)); // => true
        System.out.println("2 >= 2? " + (2 >= 2)); // => true

        // 布尔运算符
        System.out.println("3 > 2 && 2 > 3? " + ((3 > 2) && (2 > 3))); // => false
        System.out.println("3 > 2 || 2 > 3? " + ((3 > 2) || (2 > 3))); // => true
        System.out.println("!(3 == 2)? " + (!(3 == 2))); // => true

        // 位运算符!
        /*
        ~      一元位补
        <<     带符号左移
        >>     带符号/算术右移
        >>>    无符号/逻辑右移
        &      按位与运算
        ^      按位异或运算
        |      按位或运算
        */

        // 增量运算符
        int i = 0;
        System.out.println("\n->Inc/Dec-rementation");
        // ++ 和 -- 运算符分别表示递增和递减1
        // 如果它们放在变量之前，它们会先递增后返回
        // 放在变量之后表示先返回后递增
        System.out.println(i++); // i = 1, prints 0 (后递增)
        System.out.println(++i); // i = 2, prints 2 (先递增)
        System.out.println(i--); // i = 1, prints 2 (后递减)
        System.out.println(--i); // i = 0, prints 0 (先递减)

        ///////////////////////////////////////
        // Control Structures：控制结构
        ///////////////////////////////////////
        System.out.println("\n->Control Structures");

        // if语句和c语言里边差不多
        int j = 10;
        if (j == 10) {
            System.out.println("I get printed");
        } else if (j > 10) {
            System.out.println("I don't");
        } else {
            System.out.println("I also don't");
        }

        // While循环
        int fooWhile = 0;
        while(fooWhile < 100) {
            System.out.println(fooWhile);
            fooWhile++;
        }
        System.out.println("fooWhile Value: " + fooWhile);

        // Do While 循环
        int fooDoWhile = 0;
        do {
            System.out.println(fooDoWhile);
            fooDoWhile++;
        } while(fooDoWhile < 100);
        System.out.println("fooDoWhile Value: " + fooDoWhile);

        // For 循环
        // for 循环声明的结构 => for(<start_statement>; <conditional>; <step>)
        for (int fooFor = 0; fooFor < 10; fooFor++) {
            System.out.println(fooFor);
            // 迭代10次, fooFor 0->9
        }
        // System.out.println("fooFor Value: " + fooFor); 这句会报错，因为fooFor在for循环内部声明，无法在循环外部调用

        // 嵌套的for循环，通过标签退出
        outer:
        for (int ii = 0; ii < 10; ii++) {
            for (int jj = 0; jj < 10; jj++) {
                if (ii == 5 && jj ==5) {
                    break outer;
                    // 跳出外部的outer循环，而不是只有内部的
                }
            }
        }

        // For Each 循环
        // for循环还能够遍历数组和通过Iterable接口实现的对象
        int[] fooList = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        // for each 循环结构 => for (<object> : <iterable>)
        // 可以读成: 对于迭代对象中的每个元素
        // 注意: 对象类型必须匹配可迭代对象中的元素类型
        for (int bar : fooList) {
            System.out.println(bar);
            //迭代9次并输出1-9
        }

        // Switch Case
        // switch使用byte，short，char和int数据类型
        // 它也适用于枚举类型（在枚举类型中讨论）
        // String类，以及一些包装基本类型的特殊类：字符，字节，短整数和整数
        // 从Java 7及更高版本开始，我们也可以使用String类型
        // 注意：请记住，不在任何特定情况下添加“break”会导致其接着执行下一个case（假设它满足所提供的条件）。
        int month = 3;
        String monthString;
        switch (month) {
            case 1: monthString = "January";
                break;
            case 2: monthString = "February";
                break;
            case 3: monthString = "March";
                break;
            default: monthString = "Some other month";
                break;
        }
        System.out.println("Switch Case Result: " + monthString);

        // Try-with-resources语句（Java 7+）
        // Try-catch-finally语句在Java中按预期工作，但在Java 7+中，try-with-resources语句也可用
        // Try-with-resources通过自动关闭资源简化了try-catch-finally语句

        // 为了使用try-with-resources，先在try语句内包含一个类的实例。 该类必须应用java.lang.AutoCloseable。
        try (BufferedReader br = new BufferedReader(new FileReader("foo.txt"))) {
            // 你可以试着干一些会抛出异常的事情
            System.out.println(br.readLine());
            // 在Java 7中，即使抛出了异常，资源也会被关闭
        } catch (Exception ex) {
            // 在catch语句生效之前，资源就会关闭
            System.out.println("readLine() failed.");
        }
        // 在这个例子中，不需要finally声明，BufferReader就已经关闭了
        // 这可以被用来避免在一些边缘情况中，有些finally声明不会被调用的情况
        // 了解更多请访问:
        // https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html


        // 条件语句的简写
        // 你可以使用'？'操作符来进行快速分配或逻辑分叉
        // 可以解读成 "If (statement) is true, use <first value>, otherwise, use <second value>"
        int foo = 5;
        String bar = (foo < 10) ? "A" : "B";
        System.out.println("bar : " + bar); // 打印 "bar : A", 因为声明foo<10 为 true.
        // 或者就简单的这么写
        System.out.println("bar : " + (foo < 10 ? "A" : "B"));


        ////////////////////////////////////////
        // Converting Data Types：转换数据类型
        ////////////////////////////////////////

        // 转换数据

        // 字符串 到 整数
        Integer.parseInt("123");//返回一个整数123

        // 整数 到 字符串
        Integer.toString(123);//返回一个字符串123

        // 对于其他转换，可以看一下下面的类
        // Double
        // Long
        // String

        ///////////////////////////////////////
        // Classes And Functions：类和函数
        ///////////////////////////////////////

        System.out.println("\n->Classes & Functions");

        // (Bicycle类的定义在后边)

        // 通过new来实例化一个类
        Bicycle trek = new Bicycle();

        // 调用对象的方法
        trek.speedUp(3); // You should always use setter and getter methods
        trek.setCadence(100);

        // toString 会返回这个对象的字符串表示
        System.out.println("trek info: " + trek.toString());

    }// main方法的结束
}// LearnJava类的结束


// 您可以在一个.java文件中包含其他非公共外层类，但这不是一个好习惯
// 正确的做法是将不同的类拆分为单独的文件

// Class 声明语法：
// <public/private/protected> class <class name> {
//    // 数据字段，构造函数，函数都在里面
//    // 函数在Java中称为方法
// }


class Bicycle {

    // Bicycle类的字段/变量
    public int cadence; // Public: 可以从任何地方访问
    private int speed;  // Private: 只能从类内部访问
    protected int gear; // Protected: 可以从类和子类中访问
    String name; // 默认只能从这个包中访问
    static String className; // 静态的类变量

    // Static block：静态块
    // Java没有静态构造函数的实现，但有一个静态块可用于初始化类变量（静态变量）
    // 加载类时将调用此块
    static {
        className = "Bicycle";
    }

    // Double Brace Initialization：双大括号初始化
    // Java语言没有关于如何以简单方式创建静态集合的语法。 通常您最终会使用一下方式：
    private static final Set<String> COUNTRIES = new HashSet<String>();
    static {
        COUNTRIES.add("DENMARK");
        COUNTRIES.add("SWEDEN");
        COUNTRIES.add("FINLAND");
    }

    // 但是通过使用一种叫做Double Brace Initialization的东西，可以通过一种更简单的方式实现同样的事情。
    private static final Set<String> COUNTRIES2 = new HashSet<String>() {{
        add("DENMARK");
        add("SWEDEN");
        add("FINLAND");
    }};
    // 第一个大括号是创建一个新的AnonymousInnerClass，第二个大括号是声明一个实例初始化块
    // 创建匿名内部类时调用此块
    // 这不仅适用于集合，它适用于所有非最终类

    // 构造函数是一种创建类的方法
    // 这就是一个构造函数
    public Bicycle() {
        // 你也可以调用另一个构造函数: this(1, 50, 5, "Bontrager");
        gear = 1;
        cadence = 50;
        speed = 5;
        name = "Bontrager";
    }
    // 这是一个带参数的构造函数
    public Bicycle(int startCadence, int startSpeed, int startGear,
                   String name) {
        this.gear = startGear;
        this.cadence = startCadence;
        this.speed = startSpeed;
        this.name = name;
    }

    // Method 语法:
    // <public/private/protected> <return type> <function name>(<args>)

    // Java类通常为其字段实现getter和setter

    // Method 声明语法:
    // <access modifier：访问修饰符> <return type> <method name>(<args>)
    public int getCadence() {
        return cadence;
    }

    // void方法不需要return
    public void setCadence(int newValue) {
        cadence = newValue;
    }
    public void setGear(int newValue) {
        gear = newValue;
    }
    public void speedUp(int increment) {
        speed += increment;
    }
    public void slowDown(int decrement) {
        speed -= decrement;
    }
    public void setName(String newName) {
        name = newName;
    }
    public String getName() {
        return name;
    }

    //显示此对象的属性值的方法
    @Override // 从Object类中继承.
    public String toString() {
        return "gear: " + gear + " cadence: " + cadence + " speed: " + speed +
                " name: " + name;
    }
} // Bicycle类的结束

// PennyFarthing 是 Bicycle 的一个子类
class PennyFarthing extends Bicycle {
    // (Penny Farthings是那些带有大前轮的自行车，这种自行车没有齿轮)

    public PennyFarthing(int startCadence, int startSpeed) {
        // 通过super调用父类的构造函数
        super(startCadence, startSpeed, 0, "PennyFarthing");
    }

    // 如果要覆盖一个父类的方法，您应该用@annotation标记一下（这里是@Override）
    // 了解更多请访问: http://docs.oracle.com/javase/tutorial/java/annotations/
    @Override
    public void setGear(int gear) {
        this.gear = 0;
    }
}

// Object casting：对象转换
// 由于PennyFarthing类扩展自Bicycle类，我们可以说PennyFarthing是一个Bicycle并写道：
// Bicycle bicycle = new PennyFarthing();
// 这称为对象转换，其中一个对象被用于另一个对象。 这里有许多细节和处理更多中间概念： https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html

// Interfaces：接口
// Interface 声明语法
// <access-level> interface <interface-name> extends <super-interfaces> {
//     // Constants
//     // Method declarations
// }

// 例子 - Food:
public interface Edible {
    public void eat(); // 任何实现此接口的类都必须实现此方法
}

public interface Digestible {
    public void digest();
    // 从Java 8开始，接口可以有默认方法
    public default void defaultMethod() {
        System.out.println("Hi from default method ...");
    }
}

// 我们现在可以创建一个实现这两个接口的类。
public class Fruit implements Edible, Digestible {
    @Override
    public void eat() {
        // ...
    }

    @Override
    public void digest() {
        // ...
    }
}

// 在Java中，你只能extend一个类，但是你可以实现多个接口，如下：
public class ExampleClass extends ExampleClassParent implements InterfaceOne,
        InterfaceTwo {
    @Override
    public void InterfaceOneMethod() {
    }

    @Override
    public void InterfaceTwoMethod() {
    }

}

// Abstract Classes：抽象类

// 抽象类声明语法
// <access-level> abstract class <abstract-class-name> extends
// <super-abstract-classes> {
//     // Constants and variables
//     // Method declarations
// }

// 抽象类不能被实例化
// 抽象类可以定义抽象方法
// 抽象方法没有正文并且标记为抽象，非抽象子类必须通过@Override来覆盖来自其超类的所有抽象方法
// 在将重复逻辑与自定义行为相结合时，抽象类可能很有用，但由于抽象类需要继承，因此它们违反了“基于继承的组合”，因此需要组合时请考虑使用其他方法
// https://en.wikipedia.org/wiki/Composition_over_inheritance

public abstract class Animal
{
    private int age;

    public abstract void makeSound();

    // 非抽象方法可以有正文
    public void eat()
    {
        System.out.println("I am an animal and I am Eating.");
        // 注意，在这里我们可以获取私有变量
        age = 30;
    }

    public void printAge()
    {
        System.out.println(age);
    }

    // 抽象类可以有main函数
    public static void main(String[] args)
    {
        System.out.println("I am abstract");
    }
}

class Dog extends Animal
{
    // 注意，仍需重写抽象类中的抽象方法
    @Override
    public void makeSound()
    {
        System.out.println("Bark");
        // age = 30;    ==> ERROR!    这里会报错，age是Animal的私有变量
    }

    // 注意，如果你在这里使用@Override注解会发生错误，因为java不允许重写静态方法
    // 这种情况被成为方法隐藏
    // 了解更多请访问: http://stackoverflow.com/questions/16313649/
    public static void main(String[] args)
    {
        Dog pluto = new Dog();
        pluto.makeSound();
        pluto.eat();
        pluto.printAge();
    }
}


// Final Classes：final类

// Final Class 声明语法
// <access-level> final <final-class-name> {
//     // Constants and variables
//     // Method declarations
// }

// final类不能被继承，因此最终一定是一个child
// 在某种程度上，final类与抽象类相反，因为抽象类必须被扩展才能使用，而final类不能被扩展
public final class SaberToothedCat extends Animal
{
    // 注意，仍需重写抽象类中的抽象方法
    @Override
    public void makeSound()
    {
        System.out.println("Roar");
    }
}

// Final Methods：final方法
public abstract class Mammal
{
    // 如果用final来修饰变量的话，则该变量的值不能被修改，成为常量，如下：
    protected final int finalFoo = 0;

    // Final Method 语法:
    // <access modifier> final <return type> <function name>(<args>)

    // final方法和final类一样，不能在子类中被重写
    // 因此一定是该方法的最终实现
    public final boolean isWarmBlooded()
    {
        return true;
    }
}

// Enum Type：枚举类型
//
// 枚举类型是一种特殊的数据类型，它使变量成为一组预定义的常量。
// 变量必须等于为其预定义的值之一。 因为它们是常量，所以枚举类型字段的名称是大写字母。
// 在Java编程语言中，使用enum关键字定义枚举类型。
// 例如，您可以将星期几的枚举类型指定为：
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY
}

// 我们可以像这样使用Day
public class EnumTest {
    // enum变量
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("Mondays are bad.");
                break;
            case FRIDAY:
                System.out.println("Fridays are better.");
                break;
            case SATURDAY:
            case SUNDAY:
                System.out.println("Weekends are best.");
                break;
            default:
                System.out.println("Midweek days are so-so.");
                break;
        }
    }

    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs(); // => Mondays are bad.
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs(); // => Midweek days are so-so.
    }
}

// Enum types(枚举类型) 比我们上边展示的要强大的多
// enum正文可以包含各种方法和字段
// 详细的可以看： https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html

// 现在开始学Lambda表达式
//
// Java8版本的新增功能是lambda表达式。
// Lambda在函数式编程语言中更常见，这意味着它们是可以在不属于类的情况下创建的方法，如同它本身就是一个对象一样传递，并按需执行。

// 最后要注意，lambdas必须通过一个功能接口实现
// 功能接口是仅声明了一个抽象方法的接口
// 它可以有任意数量的默认方法
// Lambda表达式可以用作该功能接口的实例
// 满足要求的任何接口都被视为功能接口
// 您可以阅读更多有关上述接口的资料
//
import java.util.Map;
import java.util.HashMap;
import java.util.function.*;
import java.security.SecureRandom;

public class Lambdas {
    public static void main(String[] args) {
        // Lambda 声明语句:
        // <zero or more parameters> -> <expression body or statement block>

        // 我们将在接下来的例子中使用这个hashmap
        Map<String, String> planets = new HashMap<>();
        planets.put("Mercury", "87.969");
        planets.put("Venus", "224.7");
        planets.put("Earth", "365.2564");
        planets.put("Mars", "687");
        planets.put("Jupiter", "4,332.59");
        planets.put("Saturn", "10,759");
        planets.put("Uranus", "30,688.5");
        planets.put("Neptune", "60,182");

        // 下边这个例子是一个没有参数，使用了java.util.function.Supplier中的Supplier功能接口的Lambda
        // 实际的lambda表达式是在numPlanets =之后出现的内容
        Supplier<String> numPlanets = () -> Integer.toString(planets.size());
        System.out.format("Number of Planets: %s\n\n", numPlanets.get());

        // 一下例子是具有一个参数并使用java.util.function.Consumer中的Consumer功能接口的Lambda
        // 这是因为行星是一个Map，它实现了Collection和Iterable
        // 此处使用的forEach（可以在Iterable中找到）将lambda表达式应用于Collection的每个成员
        // forEach的默认实现表现如下：
        /*
            for (T t : this)
                action.accept(t);
        */

        // 实际上lambda表达式是传给forEach的参数
        planets.keySet().forEach((p) -> System.out.format("%s\n", p));

        // 如果你只有一个参数，也可以写成 (注意p周围的括号没了):
        planets.keySet().forEach(p -> System.out.format("%s\n", p));

        // 在以上内容中，我们可以看到planets是一个HashMap，keySet()方法返回一个它的键集，forEach将每个元素应用于lambda表达式:(parameter p) -> System.out.format("%s\n", p)
        // 每次，该元素被称为“消耗”，并且应用lambda体中引用的语句
        // lambda体是在->之后出现的部分

        // 上边的语句如果不用lambda写的话会看起来更加传统
        for (String planet : planets.keySet()) {
            System.out.format("%s\n", planet);
        }

        // 这个例子与上面的不同之处在于使用了不同的forEach实现：在实现Map接口的HashMap类中找到的forEach
        // 这个forEach接受一个BiConsumer，一般来说这是一种奇特的方式，也就是说它处理的是每个Key -> Value对的Set
        // 此默认实现表现如下：
        /*
            for (Map.Entry<K, V> entry : map.entrySet())
                action.accept(entry.getKey(), entry.getValue());
        */

        // 实际的lambda表达式是传给forEach的参数
        String orbits = "%s orbits the Sun in %s Earth days.\n";
        planets.forEach((K, V) -> System.out.format(orbits, K, V));

        // 上边的语句如果不用lambda写的话会看起来更加传统
        for (String planet : planets.keySet()) {
            System.out.format(orbits, planet, planets.get(planet));
        }

        // 或者，如果更严格地遵循默认实现提供的规范
        for (Map.Entry<String, String> planet : planets.entrySet()) {
            System.out.format(orbits, planet.getKey(), planet.getValue());
        }

        // 这些例子仅涵盖了lambda的基本用法
        // 它可能看起来不是非常有用，但请记住，lambda可以创建为一个对象，以后可以作为参数传递给其他方法
    }
}
```