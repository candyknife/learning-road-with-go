# 4.1 接口

## 4.1.1 定义

接口是一种规范，是一类方法的集合。

在接口 A 中定义一类方法，对于结构体 B，为 B 挂载 A 中的每个方法，就可以为结构体 B 实现 A 接口。

    type Animal interface {
        Run()
        Eat()
    }

    type Dog struct {
        Name string
    }

    func (d Dog)Run(){
        fmt.Println(d.Name, "开始跑")
    }

    func (d Dog)Eat(){
        fmt.Println(d.Name, "开始吃")
    }

## 4.1.2 使用

当为结构体 B 实现了 A 接口之后，就可以通过接口调用结构体 B 的方法。

    func main(){
        var a Animal
        var d := Dog{
            Name = "中华田园犬"，
        }

        a = d

        a.Eat()
        a.Run()
    }

上面这段代码等价于：

    func main(){
        var a Animal
        a := Dog{
            Name = "中华田园犬"，
        }

        a.Eat()
        a.Run()
    }

但是要注意，此时 a 中没有 `a.Name` 这样的结构体变量。

## 4.1.3 实际应用

### 实现泛型

接口可以用来实现**泛型**。下面这段代码中的函数输入为空接口，从而可以接受任意类型的参数。

    func MyFunc(a interface{}){
        fmt.Prinln(a)
    }

    func main(){
        MyFunc("1")
        MyFunc(1)
        MyFunc([]string{"123", "123"})
    }

类似的，当一个函数的输入为指定接口时，所有所有实现了这一接口的结构体都可以作为参数传入。

    func MyFunc(a Animal){
        a.Run()
        a.Eat()
    }

    func main(){
        var d := Dog{
            Name = "中华田园犬"，
        }

        MyFunc(d)
    }

### 解耦合

实际应用时有时不想知道一个结构体变量到底是哪个结构体，但是想让其做的事情是固定的，无论什么地方都想让其做一件事情。就可以如此操作：

    var L Animal

    func MyFunc(a Animal){
        L = a
    }

    func main(){
        d := Dog{
            Name = "中华田园犬"，
        }

        MyFunc(d)
        L.Run()
    }
