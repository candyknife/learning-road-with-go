# 2.5 指针

## 2.5.1 两个重要符号

指针就是内存地址，重点为 & 符号（取地址符） 与 * 符号（指针）。

    var [指针变量名p] *[所指变量类型T] = &[所指变量a]

上面表达式含义为定义一个指向 T 类型的指针 p，并将 T 类型的一个变量 a 的地址赋给它。

    package main

    import (
        "fmt"
    )

    func main() {
        var age int = 18

        //&符号+变量，就可与获取这个变量的内存地址
        fmt.Println(&age)

        //定义一个指针变量：
        //var ptr:声明一个叫ptr的变量
        //*int:这是一个指向int类型变量地址的指针
        //&age:int型变量age的地址
        var ptr *int = &age
        fmt.Println(ptr)
        fmt.Println("ptr本身的内存地址为", &ptr)
        fmt.Println("ptr这个地址存放的数值为", *ptr)
    }

更简便地，可以通过 := 符号隐式定义指针。

    package main

    import "fmt"

    func main() {
        var age int = 18

        //隐式定义
        ptr := &age
        fmt.Println(ptr)
        fmt.Println("ptr本身的内存地址为", &ptr)
        fmt.Println("ptr这个地址存放的数值为", *ptr)
    }

## 3.2 指针的 4 个细节

* 可以直接通过指针改变指向值：*num = 10
* 指针变量接收的一定是地址值。
* 指针变量的地址必须匹配。
* 基本数据类型（又叫值类型），都有对应的指针类型，形式为 *数据类型 ，比如 int 对应的指针为*int ，float32 对应的指针类型是 *float32 。以此类推。

## 3.3 数组指针与指针数组

数组指针仅记录数组的地址。指针数组为存放有许多指针的数组。

    package main
    
    import "fmt"
    
    func main(){
        //数组指针
        var arr [3]string
        arr = [3]string{"1,2,3"}
        var arrP *[3]string
        arrP = &arr
        fmt.Println(arr, arrP)
        
        //指针数组
        var arrpp [3]*string
        var str1 string = "str1"
        var str2 string = "str2"
        var str3 string = "str3"
        arrpp = [3]string{&str1, &str2, &str3}
        *arrpp[1] = “555"
        
        fmt.Println(str3)
    }

## 3.4 指针传参

接受指针作为函数的参数时，函数可以修改传入的变量；否则将只进行值的传递。

    package main
    
    import "fmt"
    
    func main(){
        var str1 ="123"
        fmt.Println(str1)
        noPointFun(str1)
        fmt.Println(str1)
        pointFun(str1)
        fmt.Println(str1)
    }
    
    func pointFun(p1 *string){
        *p1 = "789"
    }
    
    func noPointFun(p1 string){
        p1 = "456"
    }
