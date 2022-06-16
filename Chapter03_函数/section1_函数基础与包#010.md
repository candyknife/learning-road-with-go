# 3.1 函数基础与包

## 3.1.1 函数的引入

### 什么是函数

为完成某一功能的程序指令（语句）的集合，称为函数。

### 为什么要使用函数

提高代码的复用性，减少代码的冗余，代码的维护性也提高了。

### 函数的基本语法

    func 函数名(形参列表)(返回值类型列表){
        执行语句...
        return + 返回值列表
    }

例子：

    package main
    
    import "fmt"
    
    //自定义函数：功能：两数相加
    func cal(num1 int, num2 int) int { //如果返回值类型只有一个则可以不写括号
        var sum int = 0
        sum += num1
        sum += num2
        return sum
    }
    
    func main() {
        sum := cal(10, 20)
        fmt.Println(sum)
    }

## 3.1.2 函数的细节讲解

### 函数命名

遵循标识符命名规范：见名知意，**驼峰命名**。

1. 首字母不能是数字。
1. 首字母大写，则该函数可以被本包文件和其他包文件调用（类似于 Public）
1. 首字母小写，则该函数只能被本包文件调用，其他包文件不能调用（类似于 Private）

### 形参列表

个数：0个、1 个、n 个均可。

形参：接受外来数据，仅作值传递。

实参：实际传入的数据，传入的是地址，可通过地址对原值操作。

- 基本数据类型和数组默认都是值传递的，即进行值拷贝。在函数内修改，不会影响到原来的值。
- 以值传递方式的数据类型，如果希望在函数内能修改函数外的变量，可以传入变量的地址 &，函数内以指针的方式操作变量。从效果来看类似于引用传递。

### 返回值（类型）列表

返回 0 个：可以

返回 1 个：可以，且返回值列表的括号可以不写

返回 2 个：可以

如果有返回值不想接收，可用下划线忽略。

### 返回值命名

传统写法：返回值和返回值类型对应，顺序不能错。且返回值在函数内部需要定义。

    //定义一个函数:求两个数的和，差
    func test04(num1 int, num2 int)(int, int){
        result01 := num1 + num2
        result02 := num1 - num2
        return result01, result02
    }

升级写法：对函数返回值命名后，里面顺序无所谓了，顺序不用对应，并且无须定义变量，可以直接赋值。并且返回值时可以直接 return。

    //定义一个函数:求两个数的和,
    func test05(num1 int, num2 int)(sum int, sub int){
        sub = num1 - num2
        sum = num1 + num2
        return
    }

### 内存分析

实参只会把值传递给形参，但函数内对形参的所有操作，不会发生在实参上。

## 3.1.3 Golang 函数的特性

### 不支持重载

Golang 函数不支持重载：一个函数名只能对应一组形参/返回值，一个函数。

### 支持可变参数

Golang 函数支持可变参数。可变参数在形参列表里放在最后。

    package main
    
    import "fmt"
    
    //定义一个函数，函数的参数为：可变参数 ... 参数的数量可变
    //args...int 可以传入任意多个 int 类型的数据
    func test(args ...int) {
        //函数内部处理可变参数的时候，将可变参数当作切片来处理
        //遍历可变参数
        for i := 0; i < len(args); i++ {
            fmt.Println(args[i])
        }
    }
    
    func main() {
        test()
        fmt.Println("...")
        test(3)
        fmt.Println("...")
        test(37, 58, 97, 66)
    }

### 函数可作为变量

Golang 中，函数也是一种数据类型，可以赋值给一个变量，则该变量就是一个函数类型的变量了。可以通过该变量对函数进行调用。

    package main
    
    import "fmt"
    
    func test(num int) {
        fmt.Println(num)
    }
    
    func main() {
        a := test
        fmt.Printf("a 的类型是：%T，test 函数的类型是：%T \n", a, test)
        
        //等价于 test(10)
        a(10) 
    }

输出：

    a 的类型是：func(int)，test 函数的类型是：func(int)
    10

函数既然是一种数据类型，因此在 Golang 中，函数可以作为形参，并且调用（把函数本身当成一种数据类型）

    package main
    
    import "fmt"
    
    func test(num int) {
        fmt.Println(num)
    }
    
    func test2(num1 int, num2 float32, testFunc func(int)) {
        fmt.Println("----test2")
    }
    
    func main() {
        a := test

        //调用test2
        test2(10, 3.19, test)
        test2(10, 3.19, a)
    }

### 支持自定义数据类型

    type [自定义数据类型名] [数据类型]

为了简化数据类型定义，Golang 支持自定义数据类型。可以理解为：起了一个别名。

    package main
    
    import "fmt"
    
    func main() {
        //自定义数据类型：（相当于起别名）
        type myInt int
    
        var num1 myInt = 30
        fmt.Println("num1", num1)
        var num2 int = 20
        //num2 = num1
        //虽然是别名，但 myInt 与 int 仍然是两种数据类型，不能互相赋值。
        //但是可以转换之后再赋值
        num2 = int(num1)
        fmt.Println("num2", num2)
    }

## 3.1.4 包（Package）

### 使用包的原因

我们不可能把所有的函数放在同一个源文件中，有了包可以分门别类地把函数放在不同源文件中。

并且包可以解决同名问题：两个人都想定义一个同名的函数，在同一文件中是不可以定义相同名字的函数的。此时可以用包来区分。

### 包的使用规则

1. package 进行包的声明，建议包的声明与所在的文件夹同名，但也可以不一样
2. main 包是程序的入口包，一般 main 函数会使用这个包
3. 包名是从 $GOPATH/src/ 后开始计算的，使用 / 进行路径分隔
4. 如果有多个包，建议一次性导入
5. 包中的函数需要被外部调用时，需要首字母大写
6. 同一个目录下不能有重复的函数

### 包的一些其他细节

- 一个目录下的同级文件归属于一个包
- 包本质是什么：
  - 程序层面：所有使用相同 package 包名的源文件组成的代码块
  - 源文件层面：一个文件夹
- 可以给包取别名，但起完别名，原包名就不能使用了
  - 使用方式：别名 "包目录"

        import fyne “github/fyne.v2”
