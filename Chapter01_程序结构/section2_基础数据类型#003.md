# #003 基础数据类型

## 3.1 整型

| 整型类型 | 有无符号 | 存储空间 | 范围         |
|----------|----------|--------|--------------|
| int8     | 1        | 1 字节   | -128~127     |
| int16    | 1        | 2 字节   | -32768~32767 |
| int32    | 1        | 4 字节   | -2^31~2^31-1 |
| int64    | 1        | 5 字节   | -2^63~2^63-1 |
| uint8    | 0        | 1 字节   | 0~255        |
| uint16   | 0        | 2 字节   | 0~2^16-1     |
| uint32   | 0        | 4 字节   | 0~2^32-1     |
| uint64   | 0        | 5 字节   | 0~2^64-1     |

- int 根据处理器位数一般等价于 int64
- uint 根据处理器位数一般等价于 uint64
- rune 等价于 int32
- byte 等价于 uint8

> Golang 声明整数类型时默认为 int 型。

    package main

    import (
        "fmt"
        "unsafe"
    )

    func main() {
        //定义一个整数类型
        var num1 int8 = 120
        fmt.Println(num1)

        var num2 uint8 = 200
        fmt.Println(num2)

        num3 := 28
        //Printf 的作用是：格式化地，把 num3 的类型填充到 %T 的位置上
        fmt.Printf("num3的类型是：%T \n", num3)
        fmt.Println("占用存储空间", unsafe.Sizeof(num3), "字节")
    }
> 程序正常运行的前提下遵循保小不保大的原则。

## 3.2 浮点型

| 整型类型 | 存储空间 | 范围                 |
|----------|--------|----------------------|
| float32  | 4 字节   | -3.403E38~3.403E38   |
| float64  | 8 字节   | -1.798E308~1.798E308 |

浮点型的底层存储空间与操作系统无关。

**符号位**+**指数位**+**尾数位**，所以尾数只存了个大概，可能出现精度损失。

    package main

    import (
        "fmt"
        "unsafe"
    )

    func main() {
        //定义浮点类型的数据
        var num1 float32 = 3.14
        fmt.Println(num1)
        //正负浮点数
        var num2 float32 = -3.14
        fmt.Println(num2)
        //科学计数法
        var num3 float32 = 314e-2
        fmt.Println(num3)
        var num4 float32 = 314e+2
        fmt.Println(num4)
        var num5 float64 = 314e+2
        fmt.Println(num5)
        var num6 float64 = 314e+2
        fmt.Println(num6)
        //浮点数可能会有精度的损失，通常建议使用：float64
        var num7 float32 = 256.000000916
        fmt.Println(num7)
        var num8 float64 = 256.000000916
        fmt.Println(num8)

        //golang中浮点的默认类型是float64
        num9 := 3.14
        fmt.Printf("num3的类型是：%T \n", num9)
        fmt.Println("占用存储空间", unsafe.Sizeof(num9), "字节")
    }

## 3.3 字符型

Golang 中没有专门的字符类型，要存储单个字符（字母），一般用 **byte 型**来保存。

Golang 中字符使用 UTF-8 编码，参考 ASCII 码表。

    package main

    import (
        "fmt"
    )

    func main() {
        //定义字符类型的数据
        var c1 byte = 'a'
        fmt.Println(c1)
        //想显示对应字符需要格式化输出
        fmt.Printf("对应的字符为：%c \n", c1)
        var c2 byte = '6'
        fmt.Println(c2)
        var c3 byte = '('
        fmt.Println(c3)
        //字符类型本质上是整数，可以直接参与运算，输出字符时输出的是 ASCII 码值
        //汉字对应的是Unicode码值，byte会溢出，可以用int
        //总结：Golang的字符对应地使用的是UTF-8编码（Unicode是字符集，UTF-8是Unicode其中的一种编码方案）
    }

## 3.4 布尔型

布尔型也称 bool 型，只允许取值 true 和 false。

bool 型占 1 个字节。

bool 型适用于逻辑运算，一般用于程序流程控制。


    package main

    import (
        "fmt"
    )

    func main() {
        //测试bool类型的数值
        var flag01 bool = true
        fmt.Println(flag01)
        var flag02 bool = false
        fmt.Println(flag02)

        var flag03 bool = 5 < 9
        fmt.Println(flag03)
    }

## 3.5 字符串

字符串就是一串固定长度的字符连接起来的字符序列。

    package main

    import (
        "fmt"
    )

    func main() {
        //1.定义一个字符串
        var s1 string = "你好全面拥抱Golang"
        fmt.Println(s1)

        //2.字符串是不可变的：指的是字符串一旦定义好，其中字符的值不能改变
        var s2 string = "abc"
        fmt.Println(s2)
        s2 = "def"
        fmt.Println(s2)
        //s2[0] = 't'
        fmt.Println(s2)

        //3.字符串的表现形式
        //(1)如果字符串中没有特殊字符，用双引号
        //var s3 string = "ssss"
        //(2)如果字符串中有特殊字符，用反引号
        var s4 string = `
        package main

        import (
        "fmt"
        )

        func main() {
        //测试bool类型的数值
        var flag01 bool = true
        fmt.Println(flag01)
        var flag02 bool = false
        fmt.Println(flag02)

        var flag03 bool = 5 < 9
        fmt.Println(flag03)
        }`
        fmt.Println(s4)

        //4.字符串的拼接
        var s5 string = "abc" + "def"
        fmt.Println(s5)
        s5 += "ghij"
        fmt.Println(s5)
        //换行拼接，+保留在末尾
        var s6 string = "abc" +
            "def"
        fmt.Println(s6)
    }

## 3.6 基本数据类型的默认值

- 整型：0
- 浮点：0
- 布尔：false
- 字符串：""

## 3.7 基本数据类型之间的转换

Golang 在不同类型的变量赋值时需要**显式转换**，并且只有显式转换（强制转换）。

表达式 T(v) 将值 v 转换为类型 T。

    package main
    import (
        "fmt"
    )

    func main() {
        //进行类型转换
        var n1 int = 100
        //此处无法自动转换，必须显式转换
        //var n2 float32 = n1
        var n2 float32 = float32(n1)
        fmt.Println(n1)
        fmt.Println(n2)
        //n1的类型保持int不变，只是100这个值转换成了float32并赋给了n2

        fmt.Println("-----------------------------------------")
        //将int64转换为int8时，编译不会报错，但数据会溢出
        var n3 int64 = 888888
        var n4 int8 = int8(n3)
        fmt.Println(n3)
        fmt.Println(n4)

        fmt.Println("-----------------------------------------")
        //赋值时左右的数值类型一定要匹配
        var n5 int32 = 12
        var n6 int64 = int64(n5) + 30
        fmt.Println(n6)

        fmt.Println("-----------------------------------------")
        var n7 int64 = 12
        //编译通过，但数值可能溢出
        var n8 int8 = int8(n7) + 127
        //128已经超出int8范围，编译无法通过
        //var n9 int8 = int8(n7) + 128
        fmt.Println(n8)
        //fmt.Println(n9)
    }

## 3.8 基本数据类型转换为String

方式 1：**fmt.Sprinf("%参数", 表达式)**。

方式 2：使用 **strconv 包**的函数。

    package main
    import (
        "fmt"
        "strconv"
    )

    func main() {
        //方式1
        var n1 int = 19
        var n2 float32 = 4.78
        var n3 bool = false
        var n4 byte = 'a'

        var s1 string = fmt.Sprintf("%d", n1)
        fmt.Printf("s1对应的类型是：%T, s1 = %q \n", s1, s1)
        var s2 string = fmt.Sprintf("%f", n2)
        fmt.Printf("s2对应的类型是：%T, s2 = %q \n", s2, s2)
        var s3 string = fmt.Sprintf("%t", n3)
        fmt.Printf("s3对应的类型是：%T, s3 = %q \n", s3, s3)
        var s4 string = fmt.Sprintf("%d", n4)
        fmt.Printf("s4对应的类型是：%T, s4 = %q \n", s4, s4)

        //方式2，更麻烦，不常用
        s1 = strconv.FormatInt(int64(n1), 10) //参数：第一个参数必须为int64类型，第二个参数指定字面进制
        fmt.Printf("s1对应的类型是：%T, s1 = %q \n", s1, s1)
        s2 = strconv.FormatFloat(float64(n2), 'f', 9, 64)
        fmt.Printf("s2对应的类型是：%T, s2 = %q \n", s2, s2)
    }

## 3.9 String转换为基本数据类型

使用 **strconv 包**的函数。

    package main

    import (
        "fmt"
        "strconv"
    )

    func main() {
        //string-->bool
        var s1 string = "true"
        var b bool
        //ParseBool的返回值有两个(value bool, err error)
        //我们只关注返回的Bool值，err可以用_直接忽略
        b, _ = strconv.ParseBool(s1)
        fmt.Printf("b的类型是：%T, b = %v \n", b, b)

        //string-->int64
        var s2 string = "19"
        var num1 int64
        num1, _ = strconv.ParseInt(s2, 10, 64)
        fmt.Printf("num1的类型是：%T, b = %v \n", num1, num1)

        //string-->float64
        var s3 string = "3.14"
        var num2 float64
        num2, _ = strconv.ParseFloat(s3, 64)
        fmt.Printf("num2的类型是：%T, b = %v \n", num2, num2)

        //注意：需要保证被转的字符串与目标类型匹配（有效），否则将会赋予目标变量默认值
    }
