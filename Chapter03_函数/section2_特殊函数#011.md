# 3.2 特殊函数

## 3.2.1 匿名函数

Golang 支持匿名函数，如果某个函数只是希望使用一次，可以考虑使用匿名函数。匿名函数可以在其他函数中定义，而普通函数不可以。

### 使用方式

第一种为在定义匿名函数时就直接调用，这种方式匿名函数只能调用一次（最常用）。

    package main
    
    import "fmt"
    
    func main() {
        //定义匿名函数,并直接调用
        result := func(num1 int, num2 int) int {
            return num1 + num2
        }(10, 20)
    
        fmt.Println(result)
    }

第二种使用方式为将匿名函数赋给一个变量（函数变量），再通过该变量来调用匿名函数（用得少）。

    package main
    
    import "fmt"
    
    func main() {
        //定义匿名函数，赋给变量
        result := func(num1 int, num2 int) int {
            return num1 + num2
        }
        //通过变量来调用
        fmt.Println(result(10, 20))
    }

### 如何使匿名函数在整个程序中有效？

将匿名函数赋值给一个全局变量。

### 自执行函数

与匿名函数类似，定义完即自执行。

    func main(){
        func(x, y int){
            fmt.Println(x + y)
        }(10，20)
    }

## 3.2.2 闭包

### 什么是闭包？

闭包就是一个函数和与其相关的引用环境组成的一个整体。

    package main
    
    import "fmt"
    
    //函数功能：求和
    //函数名称：getSum 参数为空
    //getSum 函数返回值为一个函数，这个函数的参数是一个 int 类型的参数，返回值也是 int 类型
    func getSum() func(int) int {
        var sum int = 0
        return func(num int) int {
            sum = sum + num
            return sum
        }
    }
    
    //闭包：返回的匿名函数 + 匿名函数以外的变量 sum
    //感受：匿名函数中引用的那个变量会一直保存在内存中，可以一直使用
    func main() {
        f := getSum()
        fmt.Println(f(1))
        fmt.Println(f(2))
        fmt.Println(f(3))
        fmt.Println(f(4))
    }

### 闭包的本质

闭包本质依旧是一个匿名函数，只是这个函数引入了外界的变量/参数。

***[匿名函数] + [引用的变量/参数] = [闭包]***

### 特点

* 返回的是一个匿名函数，但是这个匿名函数引用到函数外的变量/参数，因此这个匿名函数就和变量/参数形成一个整体，构成闭包。
* 闭包中使用的变量/参数会一直保存在内存中，所以会一直使用。意味着闭包不可滥用。

### 闭包的作用

闭包可以保留上次引用的某个值，传入一次就可以反复使用。

## 3.2.3 defer 关键字

### defer 关键字的作用

函数中程序员经常需要创建资源，为了在函数执行完毕之后，及时释放资源，Golang 的设计者提供 defer 关键字。

    package main
    
    import "fmt"
    
    func main() {
        fmt.Println(add(30, 60))
    }
    
    func add(num1 int, num2 int) int {
        //Golang 中，程序遇到 defer 关键字，不会立即执行 defer 后的语句，而是将 defer 后的语句（包括值）压入栈中，然后继续执行后面的语句
        defer fmt.Println("num1=", num1)
        defer fmt.Println("num2=", num2)
        //栈的特点是 先进后出
        //在函数执行完毕后，从栈中取出语句开始执行，按照先进后出的顺序执行
        var sum int = num1 + num2
        fmt.Println("sum =", sum)
        return sum
    }

遇到 defer 关键字，会将后面的代码语句压入栈中，也会将相关的值同时拷入栈中，不会随着函数后面的变化而变化。

### defer 应用场景

比如想关闭某个使用的资源，在使用的时候直接随手 defer ，因为 defer 有延迟执行的机制（函数执行完毕再执行 defer 压入栈的语句），所以可以用完随手写了关闭，省心省事。
