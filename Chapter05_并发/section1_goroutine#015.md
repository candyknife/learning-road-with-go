# 5.1 goroutine

## 5.1.1 定义与使用

在调用一个方法的前面加上`go`就是`goroutine`，它会让方法异步执行，相当于协程。

如果没有主程序，仅有异步执行的程序，那就会跳过执行。

    func ARun(){
        fmt.Prinln("A 正在运行")
    }

    func main(){
        go ARun()
    }

上面这段代码不会有任何输出。

    func ARun(){
        fmt.Prinln("A 正在运行")
    }

    func main(){
        go ARun()
        time.Sleep(1*time.Second)
    }

此时才会有打印输出。想要观察到底何时进行的异步执行，可以运行如下代码。

    func ARun(){
        fmt.Prinln("A 正在运行")
    }

    func main(){
        go ARun()
        i := 0
        for i < 10 {
            i++
            fmt.Println(i)
        }
    }

## 5.1.2 协程管理器

利用协程管理器可以不用每次都写`time.Sleep`。

使用的流程为：

- 声明 `sync.WaitGroup`
- wg.Add()
- wg.Done()
- wg.Wait()

下面给出例子：

    func ARun(*wg sync.WaitGroup){
        fmt.Prinln("A 正在运行")
        wg.Done()
    }

    func main(){
        var wg sync.WaitGroup
        wg.Add(1)
        go ARun(&wg)
        wg.Wait()
    }

