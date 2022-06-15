# 2.1 数组

## 2.1.1 数组的定义

数组是一个由固定长度的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。因为数组的长度是固定的，因此在Go语言中很少直接使用数组。和数组对应的类型是 Slice（切片），它是可以增长和收缩的动态序列，slice 功能也更灵活，但是要理解 slice 工作原理的话需要先理解数组。

    package main
    
    import (
        "fmt"
    )
    
    func main() {
        //给出五个学生的成绩，求出成绩的总和、平均数
        var scores [5]int
        scores[0] = 95
        scores[1] = 91
        scores[2] = 39
        scores[3] = 60
        scores[4] = 21
        //求和
        sum := 0
        for i := 0; i < len(scores); i++ {
            sum += scores[i]
        }
        //平均数
        avg := sum / 5
        //输出
        fmt.Printf("成绩的总和为：%v；平均分数为：%v", sum, avg)
    }

## 2.1.2 数组的初始化

    package main
    
    import "fmt"
    
    func main() {
        //1
        var arr1 [3]int = [3]int{3, 6, 9}
        fmt.Println(arr1)
        //2
        var arr2 = [3]int{3, 6, 9}
        fmt.Println(arr2)
        //3
        var arr3 = [...]int{4, 5, 6, 7}
        fmt.Println(arr3)
        //4
        var arr4 = [...]int{2: 66, 0: 33, 1: 99, 3: 88}
        fmt.Println(arr4)
    }

## 2.1.3 数组的内存分析

    package main
    
    import "fmt"
    
    func main() {
        var arr [3]int16
        //获取长度
        fmt.Println(len(arr))
        //打印数组
        fmt.Println(arr)
        //获取
        for i := 0; i < len(arr); i++ {
            fmt.Printf(":arr[%d]的地址为：%p \n", i, &arr[i])
        }
    }

运行结果：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo02\main.go"
    3
    [0 0 0]
    :arr[0]的地址为：0xc000014088 
    :arr[1]的地址为：0xc00001408a 
    :arr[2]的地址为：0xc00001408c 

## 2.1.4 数组的遍历

### 1) 普通 for 循环

    package main
    
    import "fmt"
    
    func main() {
        var scores [5]int
        //录入成绩
        for i := 0; i < len(scores); i++ {
            fmt.Printf("请录入第 %d 个学生的成绩：", i+1)
            fmt.Scanln(&scores[i])
        }
        //求和
        sum := 0
        for i := 0; i < len(scores); i++ {
            sum += scores[i]
        }
        //平均数
        avg := sum / 5
        //输出
        fmt.Printf("成绩的总和为：%v；平均分数为：%v", sum, avg)
    }

### 2) 键值循环

键值循环 for range 结构是 Golang 特有的一种迭代结构，在许多情况下都非常有用，它可以遍历数组、切片、字符串、map 以及通道。

    package main
    
    import "fmt"
    
    func main() {
        var scores [5]int
        //录入成绩
        for i := 0; i < len(scores); i++ {
            fmt.Printf("请录入第 %d 个学生的成绩：", i+1)
            fmt.Scanln(&scores[i])
        }
        //遍历方式 1：普通 for 循环
        for i := 0; i < len(scores); i++ {
            fmt.Printf("第 %d 个学生的成绩为：%d\n", i+1, scores[i])
        }
        //遍历方式 2：for-range 循环
        for key, value := range scores {
            fmt.Printf("第 %d 个学生的成绩为：%d\n", key+1, value)
        }
    }

## 2.1.5 数组的注意事项

数组长度是数组类型的一部分。

    func main() {
        var arr = [3]int{3, 6, 9}
        fmt.Println(arr)
        fmt.Printf("数组的类型为：%T", arr)
    }

输出为：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo05\main.go"
    [3 6 9]
    数组的类型为：[3]int

Golang 中数组属于**值类型**，在默认情况下值传递，因此会进行值拷贝。

    package main
    
    import "fmt"
    
    func main() {
        var arr = [3]int{3, 6, 9}
        test(arr)
        fmt.Println(arr)
    }
    
    func test(arr [3]int) {
        arr[0] = 7
    }

输出为：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo05\main.go"
    [3 6 9]

如果函数想修改传递进来的数组，可以使用引用传递（指针方式）

    func main() {
        var arr = [3]int{3, 6, 9}
        test(&arr)
        fmt.Println(arr)
    }
    
    func test(arr *[3]int) {
        (*arr)[0] = 7
    }

输出为

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo05\main.go"
    [7 6 9]

## 2.1.6 二维数组

### 1) 二维数组的定义与赋值

    func main() {
        var arr [2][3]int = [2][3]int{{1, 2, 3}, {4, 5, 6}}
        fmt.Println(arr)
        fmt.Printf("数组的类型为：%T\n", arr)
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo06\main.go"
    [[1 2 3] [4 5 6]]
    数组的类型为：[2][3]int

### 2) 二维数组的内存分析

    func main() {
        var arr [2][3]int
        fmt.Println(arr)
        fmt.Printf("数组的类型为：%T\n", arr)
    
        for i := 0; i < 2; i++ {
            for j := 0; j < 3; j++ {
                fmt.Printf("Index [%d][%d] value %d Address %p\n", i, j, arr[i][j], &arr[i][j])
            }
        }
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo06\main.go"
    [[0 0 0] [0 0 0]]
    数组的类型为：[2][3]int
    Index [0][0] value 0 Address 0xc00000c3c0
    Index [0][1] value 0 Address 0xc00000c3c8
    Index [0][2] value 0 Address 0xc00000c3d0
    Index [1][0] value 0 Address 0xc00000c3d8
    Index [1][1] value 0 Address 0xc00000c3e0
    Index [1][2] value 0 Address 0xc00000c3e8

### 3) 二维数组的遍历

    package main
    
    import "fmt"
    
    func main() {
        var arr = [3][3]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
        fmt.Println(arr)
        fmt.Println("--------------------")
        
        //1.for 循环
        for i := 0; i < len(arr); i++ {
            for j := 0; j < len(arr[i]); j++ {
                fmt.Printf("arr[%d][%d] = %d ;", i, j, arr[i][j])
            }
            fmt.Println()
        }
        fmt.Println("--------------------")
        
        //2.for range 循环
        for key, value := range arr {
            for k, v := range value {
                fmt.Printf("arr[%v][%v] = %v ;", key, k, v)
            }
            fmt.Println()
        }
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit7\demo07\main.go"
    [[1 2 3] [4 5 6] [7 8 9]]
    --------------------
    arr[0][0] = 1 ;arr[0][1] = 2 ;arr[0][2] = 3 ;
    arr[1][0] = 4 ;arr[1][1] = 5 ;arr[1][2] = 6 ;
    arr[2][0] = 7 ;arr[2][1] = 8 ;arr[2][2] = 9 ;
    --------------------
    arr[0][0] = 1 ;arr[0][1] = 2 ;arr[0][2] = 3 ;
    arr[1][0] = 4 ;arr[1][1] = 5 ;arr[1][2] = 6 ;
    arr[2][0] = 7 ;arr[2][1] = 8 ;arr[2][2] = 9 ;
