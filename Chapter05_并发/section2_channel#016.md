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

## 5.2.3
