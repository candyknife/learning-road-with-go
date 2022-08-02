# 5.2 channel

## 5.2.1 定义

channel 是 goroutine 中不同线程之间通讯的桥梁。

## 5.2.2 分类

channel 可分为 5 种：

- 可读可取：`c:=make(chan int)`
- 可读：`var readChan <- chan int = c`
- 可取：`var setChan chan <- int = c`
- 有缓冲：`c:=make(chan int, 5)`
- 无缓冲：`c:=make(chan int)`

### 有无缓冲

有缓冲的 channel，先存再取：

    func main(){
        c1 := make(chan int, 1)

        c1 <- 1

        fmt.Println(<- c1)
    }

此时若无缓冲区，则会死锁。因为无缓冲区时先取，发现没有（发生阻塞）才会回过头往里存，但顺序结构不允许。想要顺利执行可以利用匿名函数。

    func main(){
        c1 := make(chan int)

        go func(){
            c1 <- 1
        }()

        fmt.Println(<- c1)
    }

### 可读可取

`readc`仅仅可读，`setc`仅仅可写。

    func main(){
        c1 := make(chan int, 1)
        var = readc <-chan int = c1
        var = setc chan<- int = c1
        setc <- 1

        fmt.Println(<- readc)
    }

## 5.2.3

### 关闭

channel 用完（不需要再存数据时）即可关闭，关闭后无法再往里存数据，但剩下的数据仍可读取。例如下面代码不会报错。

    func main(){
        c1 := make(chan int, 1)

        c1 <- 1
        c1 <- 2
        c1 <- 3

        close(c1)

        fmt.Println(<- c1)
    }

但关闭后再存会报错：

    func main(){
        c1 := make(chan int, 1)

        c1 <- 1
        c1 <- 2
        c1 <- 3

        close(c1)

        c1 <- 4
    }

### 遍历 channel

想要通过`for range`的方式遍历 channel 的内容，必须先关闭 channel。

    func main(){
        c1 := make(chan int, 1)

        c1 <- 1
        c1 <- 2
        c1 <- 3

        close(c1)

        for v := range c1:{
            fmt.Println(v)
        }
    }

### case 相关用法

以下代码执行结果随机。

    func main(){
        ch1 := make(chan int, 1)
        ch2 := make(chan int, 1)
        ch3 := make(chan int, 1)

        ch1 <- 1
        ch2 <- 2
        ch3 <- 3

        select{
            case <-ch1:
            fmt.Print1n("ch1")
            case <-ch2:
            fmt.Println("ch2")
            case <-ch3:
            fmt.Println("ch3")
            default:
            fmt.Println("都不满足")
        }
    }
