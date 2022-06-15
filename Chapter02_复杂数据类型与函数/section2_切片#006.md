# 2.2 切片（Slice）

## 2.2.1 切片的引入

切片 slice 是 Golang 中一种特有的数据类型。

数组有特定的用处，但是却有一些呆板（数组长度固定不可变），所以在 Golang 的代码里并不是特别常见。相对地，切片却是随处可见的。切片是一种建立在数组类型之上的抽象，它更便捷，能力也更强大。

切片 slice 是对数组一个连续片段的引用，所以切片是一个**引用类型**。这个片段可以是整个数组，或者是由起始与终止索引标识的一些项的子集。需要注意的是，终止索引标识的项不包括在切片内。切片提供了一个相关数组的动态窗口。

    package main
    
    import "fmt"
    
    func main() {
        //定义数组
        var intarr [6]int = [6]int{3, 6, 9, 1, 4, 7}
        //切片构建在数组之上
        //定义一个切片，名为slice，[]动态变化的数组长度不写，int类型，intarr为原数组
        //[1:3]切片 - 切出的一段片段 - 索引：从1开始，到3结束（不包含3） - [1,3)
        //var slice []int = intarr[1:3]
        slice := intarr[1:3]
        //输出数组
        fmt.Println("antarr:", intarr)
        //输出切片
        fmt.Println("slice:", slice)
        //切片元素个数
        fmt.Println("slice 的元素个数：", len(slice))
        //切片容量
        fmt.Println("slice 的容量：", cap(slice))
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo01\main.go"
    antarr: [3 6 9 1 4 7]
    slice: [6 9]
    slice 的元素个数： 2
    slice 的容量： 5

## 2.2.2 切片的内存分析

切片是一个**结构体**，有 3 个字段的数据结构：

- 指向底层数组的**指针**（数组起始索引的地址）
- 切片长度
- 切片容量

例子：

    ...
    slice[1] = 16
    //输出数组
    fmt.Println("antarr:", intarr)
    //输出切片
    fmt.Println("slice:", slice)

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo01\main.go"
    antarr: [3 6 9 1 4 7]
    slice: [6 9]
    antarr: [3 6 16 1 4 7]
    slice: [6 16]

可以看到是直接可以改变原数组的值的。

## 2.2.3 切片的定义

方式 1 ：定义一个切片，让切片去引用一个已经创建好的数组。

方式 2 ：通过 make 内置函数来创建切片。

- 基本语法: var [切片名] type = make([]type, len,[cap])
- 实际上 make 函数在底层创建了一个数组，对外不可见，所以不可以直接操作数组，只能通过 slice 间接访问。

方式 3 ：定一个切片，直接就指定具体数组，使用原理类似make的方式。

    package main
    
    import "fmt"
    
    func main() {
        //方式 1
        var intarr [6]int = [6]int{3, 6, 9, 1, 4, 7}
        slice1 := intarr[1:3]
        fmt.Println("slice1:", slice1)
        fmt.Println("slice1 的元素个数：", len(slice1))
        fmt.Println("slice1 的容量：", cap(slice1))
        fmt.Println("--------------------")
        //方式 2
        slice2 := make([]int, 4, 20)
        fmt.Println("slice2:", slice2)
        fmt.Println("slice2 的元素个数：", len(slice2))
        fmt.Println("slice2 的容量：", cap(slice2))
        fmt.Println("--------------------")
        //方式 3
        slice3 := []int{2, 5, 8}
        fmt.Println("slice3:", slice3)
        fmt.Println("slice3 的元素个数：", len(slice3))
        fmt.Println("slice3 的容量：", cap(slice3))
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo02\main.go"
    slice1: [6 9]
    slice1 的元素个数： 2
    slice1 的容量： 5
    --------------------
    slice2: [0 0 0 0]
    slice2 的元素个数： 4
    slice2 的容量： 20
    --------------------
    slice3: [2 5 8]
    slice3 的元素个数： 3
    slice3 的容量： 3

## 2.2.4 切片的遍历

方式 1 ：for 循环

方式 2 ：for-range 结构

    package main
    
    import "fmt"
    
    func main() {
        slice := []int{2, 5, 8, 13, 17, 26, 87}
        // fmt.Println("slice3:", slice3)
        // fmt.Println("slice3 的元素个数：", len(slice3))
        // fmt.Println("slice3 的容量：", cap(slice3))
        //方式 1
        for i := 0; i < len(slice); i++ {
            fmt.Printf("slice[%v] = %v\t", i, slice[i])
        }
        fmt.Println("\n--------------------")
        //方式 2
        for i, v := range slice {
            fmt.Printf("slice[%v] = %v\t", i, v)
        }
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo03\main.go"
    slice[0] = 2	slice[1] = 5	slice[2] = 8	slice[3] = 13	slice[4] = 17	slice[5] = 26	slice[6] = 87	
    --------------------
    slice[0] = 2	slice[1] = 5	slice[2] = 8	slice[3] = 13	slice[4] = 17	slice[5] = 26	slice[6] = 87	

## 2.2.5 切片的注意事项

切片定义后无法直接使用，必须让其引用到一个数组上去，或者 make 一个空间供切片使用。

切片使用下标不能越界。

简写方式：

- var slice = arr[0:end] -> var slice = arr[:end]
- var slice = arr[start:len(arr)] -> var slice = arr[start:]
- var slice = arr[0:len(arr)] -> var slice = arr[:]

切片可以继续切片。

切片可以动态增长，且还可以通过 append 把切片追加给切片。

    package main
    
    import "fmt"
    
    func main() {
        var intarr [6]int = [6]int{3, 6, 9, 1, 4, 7}
        slice := intarr[1:3]
        fmt.Println(slice)
        fmt.Println("----------")
        slice2 := append(slice, 88, 50)
        fmt.Println("slice2:", slice2)
        fmt.Println("slice:", slice)
        fmt.Println("----------")
        // 底层原理
        // 1. 底层追加元素的时候对数组进行扩容，老数组扩容为新数组
        // 2. 创建一个新数组，将老数组中的 4、7、3 复制到新数组中，并在新数组中追加 88、50
        // 3. slice2 底层数组指向的是新数组
        // 4. 我们可以直接将新数组的切片赋给原来的切片
        // 5. 底层的新数组仍然不能直接维护，只能通过切片间接操作
        slice = append(slice, 88, 50)
        fmt.Println("slice:", slice)
        fmt.Println("----------")
        slice3 := append(slice, slice2...)
        fmt.Println("slice3:", slice3)
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo04\main.go"
    [6 9]
    ----------
    slice2: [6 9 88 50]
    slice: [6 9]
    ----------
    slice: [6 9 88 50]
    ----------
    slice3: [6 9 88 50 6 9 88 50]

切片的拷贝

    package main
    
    import "fmt"
    
    func main() {
        var a []int = []int{3, 6, 9, 1, 4, 7}
        var b []int = make([]int, 10)
        //将 a 中对应数组中元素值复制到 b 中对应的数组中
        copy(b, a)
        fmt.Println(b)
    }

输出:

    [Running] go run "e:\Codes\Go\src\code\basic\unit8\demo05\main.go"
    [3 6 9 1 4 7 0 0 0 0]
