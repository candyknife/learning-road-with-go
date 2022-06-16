# 3.4 错误处理

## 3.4.1 代码展示

    package main
    
    import "fmt"
    
    func main() {
        test()
        fmt.Println("上面的除法操作执行成功。")
        fmt.Println("正常执行下面逻辑。")
    }
    
    func test() {
        num1 := 10
        num2 := 0
        result := num1 / num2
        fmt.Println(result)
    }

执行结果：

    [Running] go run "e:\Codes\Go\src\code\basic\unit6\demo01\main.go"
    panic: runtime error: integer divide by zero
    
    goroutine 1 [running]:
    main.test()
        e:/Codes/Go/src/code/basic/unit6/demo01/main.go:16 +0x11
    main.main()
        e:/Codes/Go/src/code/basic/unit6/demo01/main.go:8 +0x2d

发现：程序中出现错误/恐慌之后，程序被中断，无法继续执行。

## 3.4.2 错误处理/捕获机制

Golang 中追求代码优雅，引入机制：defer + recover 来处理错误。

    package main
    
    import "fmt"
    
    func main() {
        test()
        fmt.Println("上面的除法操作执行成功。")
        fmt.Println("正常执行下面逻辑。")
    }
    
    func test() {
        //利用 defer + recover 来捕获错误
        defer func() {
            //调用 recover 内置函数，可以捕获错误：
            err := recover()
            //如果没有捕获错误，返回值为零值：nil
            if err != nil {
                fmt.Println("错误已捕获")
                fmt.Println("err 是：", err)
            }
        }()
        num1 := 10
        num2 := 0
        result := num1 / num2
        fmt.Println(result)
    }

执行结果：

    [Running] go run "e:\Codes\Go\src\code\basic\unit6\demo01\main.go"
    错误已捕获
    err 是： runtime error: integer divide by zero
    上面的除法操作执行成功。
    正常执行下面逻辑。

优点：提高程序的健壮性。

## 3.4.3 自定义错误

需要调用 errors 包下的 New 函数：函数返回 error 类型

    package main
    
    import (
        "errors"
        "fmt"
    )
    
    func main() {
        err := test()
        if err != nil {
            fmt.Println("自定义错误：", err)
        }
        fmt.Println("上面的除法操作执行成功。")
        fmt.Println("正常执行下面逻辑。")
    }
    
    func test() (err error) {
        num1 := 10
        num2 := 0
        if num2 == 0 {
            //抛出自定义错误
            return errors.New("除数不能为 0 哦~")
        } else { //如果除数不为 0，那么正常执行就可以了
            result := num1 / num2
            fmt.Println(result)
            //如果没有错误，返回零值：
            return nil
        }
    }

有一种情况：程序出现错误以后，后续代码没有必要执行，想让程序中断，需要调用 builtin 包下的 panic 函数：

    err := test()
    if err != nil {
        fmt.Println("自定义错误：", err)
        panic(err)
    }
