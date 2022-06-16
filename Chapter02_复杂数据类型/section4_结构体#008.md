# 2.4 结构体

## 2.4.1 结构体的定义

结构体是可以储存一个或多个不同数据类型的数据类型。

    package main
    
    import "fmt"
    
    //定义老师结构体，将老师中的各个属性，统一放入结构体中管理
    type Teacher struct {
        Name   string
        Age    int
        School string
    }
    
    func main() {
        //创建老师结构体的实例、对象、变量
        var t1 Teacher
        fmt.Println(t1) //再未赋值时，默认值{0}
        t1.Age = 45
        t1.Name = "马氏"
        t1.School = "清华大学"
        fmt.Println(t1)
    }

## 2.4.2 结构体实例的创建

    package main
    
    import "fmt"
    
    //定义老师结构体，将老师中的各个属性，统一放入结构体中管理
    type Teacher struct {
        Name   string
        Age    int
        School string
    }
    
    func main() {
        //方式 1
        var t1 Teacher
        t1.Name = "马氏"
        t1.Age = 45
        t1.School = "清华大学"
        fmt.Println(t1)
    
        //方式 2
        var t2 Teacher = Teacher{"赵姗姗", 31, "黑龙江大学"}
        fmt.Println(t2)
    
        //方式 3
        var t3 *Teacher = new(Teacher)
        // (*t3).Name = "aaa"
        // (*t3).Age = 33
        // (*t3).School = "bbb"
        //Golang 也提供了简化赋值的方式，由编译器自动转化
        t3.Name = "aaa"
        t3.Age = 33
        t3.School = "bbb"
        fmt.Println(*t3)
    
        //方式 4
        var t4 *Teacher = &Teacher{"ccc", 55, "ddd"}
        fmt.Println(*t4)
    }

## 2.4.3 结构体的转换

* 结构体是用户单独定义的类型，和其他类型进行转换时需要有完全相同的字段（名字、个数和类型）
* 结构体进行 type 重新定义（相当于取别名），Golang 认为是新的数据类型，但相互之间可以强转

## 2.4.4 结构体与面向对象

* Golang 也支持面向对象编程（OOP），但是和传统的面向对象编程有区别，并不是纯粹的面向对象语言。所以我们说 Golang 支持面向对象编程特性更准确。
* Golang 没有类（class），但 Golang 中的结构体（struct）和其他编程语言的类有同等的地位，可以理解为 Golang 是基于 struct 来实现 OOP 特性的。
* Golang 面向对象编程非常简洁，去掉了传统 OOP 语言的方法重载、构造函数与析构函数、隐藏的 this 指针等。
* Golang 仍然有面向对象编程的继承、封装、多态的特性，只是实现方式和其他 OOP 语言不一样，比如继承：Golang 没有 extends 关键字，继承是通过匿名字段来实现的。

## 2.4.5 为结构体挂载方法（前瞻）

    package main
    
    import "fmt"
    
    type Dog struct{
        Name string
        Sex bool
        Species string
    }
    
    func (dog *Dog)Eat(Food string)(result bool){
        fmt.Println("Dog %v have eaten %v!", dog.Name, Food)
        return True
    }
    
    func main(){
        dog := Dog{"33", True, "中华田园犬"}
        dog.Eat("meat")
    }
