# 泛型基础

## 什么是泛型
java泛型是JDK1.5版本后引入的。泛型让编程人员能够使用类型抽象，通常用于集合里面  
泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型
接口、泛型方法。  
使用泛型前，存入集合的元素可以是任何类型的，当从集合中取出时，所有的元素都是Object类型，需要进行向下的强制类型转换，转换到特定的
类型。而强制类型转换容易引起运行时错误。

两数相加，可以使用重载的方式实现，也可以使用泛型的范式实现
```java
/**
 * 使用重载的方式实现两数相加
 * 优点是可以返回指定类型，缺点是需要书写过多多余代码
 */
class Add {
    public static double add(double a, double b) {
        return a + b;
    }
    public static int add(int a, int b) {
        return a + b;
    }
}

/**
 * 使用泛型的方式实现两数相加
 * 优点是只需要写一个方法就可以实现多种调用
 * 缺点是返回类型值固定为double类型，(可通过调用时强制转换输出为整型数据)
 */
class AddG {
    /**
    * T extends Number表明参数类型必须为Number的子类
    * @param a
    * @param b
    * @param <T>
    * @return 
    */
    public static  <T extends Number>double add(T a, T b) {
        return a.doubleValue() + b.doubleValue();
    }
}

/**
* 调用类
*/
public class Main {
    public static void main(String[] args) {
        //使用重载类调用方法，参数为int类型时，返回int类型，以下结果为2
        Add.add(1, 1);
        //参数为double时，返回double类型，结果为2.0
        Add.add(1.0, 1.0);
        
        //使用泛型类调用方法，参数必须为(Number的子类)，返回值为2.0
        AddG.add(1, 1);
        AddG.add(1.0, 1.0);
        //将结果强制转换成int类型
        (int)AddG.add(1, 1);
    }
} 

```
