# 第3章  操作符

---
### 3.4 赋值
- 值传递&应用传递

### 3.5 算术操作符
- 随机数生成器(Random)对于特定的种子值总是产生相同的随机数序列。
- `x = a * -b`等同于`x = a * (-b)`

### 3.6 自动递增和递减
- `System.out.print(a++)`先输出a，再进行自加运算。

### 3.9 直接常量
``` java
/**
 * 3.9 直接常量
 * 
 * @author wang
 *
 */
public class Literals {

    public static void main(String[] args) {
        int i1 = 0x2f; // Hexadecimal (lowercase)
        System.out.println("i1= " + i1 + " ; binary string= " + Integer.toBinaryString(i1));
        int i2 = 0X2f; // Hexadecimal (uppercase)
        System.out.println("i2= " + i2 + " ; binary string= " + Integer.toBinaryString(i2));
        int i3 = 0177; // Octal (leading zero)
        System.out.println("i3= " + i3 + " ; binary string= " + Integer.toBinaryString(i3));

        char c = 0xffff; // max char hex value
        System.out.println("c= " + c + " ; binary string= " + Integer.toBinaryString(c));
        byte b = 0x7f; // max byte hex value
        System.out.println("b= " + b + " ; binary string= " + Integer.toBinaryString(b));
        short s = 0x7fff; // max short hex value
        System.out.println("s= " + s + " ; binary string= " + Integer.toBinaryString(s));

        long n1 = 200L; // long suffix
        long n2 = 200l; // long suffix (but can be confusing)
        long n3 = 200;
        // float f0 = 1.1; // cannot convert from double to float
        float f1 = 1;
        float f2 = 1F; // float suffix
        float f3 = 1f; // float suffix
        double d0 = 1.111;
        double d1 = 1;
        double d2 = 1d; // double suffix
        double d3 = 1D; // double suffix
    }

    /*
     * output: 
     * i1= 47 ; binary string= 101111 
     * i2= 47 ; binary string= 101111 
     * i3= 127 ; binary string= 1111111 
     * c= ￿ ; binary string= 1111111111111111 
     * b= 127 ; binary string= 1111111 
     * s= 32767 ; binary string= 111111111111111
     */
}
```

### 3.10 按位操作符
- 按位操作符用来操作整数基本数据类型中的单个“比特”（bit），即二进制位。
- 按位“与”（&）
- 按位“或”（｜）
- 按位“非”（～）：一元操作符
- 按位“异或”（^）：相同为0，不同为1。

### 3.11 移位操作符
- 移位操作符可以用来处理整数类型。
- 左移位操作符（<<）：能按照操作符右侧指定的位数将操作符左边的操作数向左移动（在低位补0）
- “有符号”右移操作符（>>）：符号为正时，在高位插入0，反之插入1
- “无符号”右移位操作符（>>>）：无论正负，都插入0
