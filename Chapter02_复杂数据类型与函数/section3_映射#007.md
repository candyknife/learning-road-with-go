# 2.3 映射（Map）

## 2.3.1 map 的引入

映射 map 是 Golang 内置的一种类型，它将键值对相关联，我们可以通过键 key 来获取对应的值 value。

通俗说就是一对匹配的信息：**键 - 值**

基本语法：

    var map 变量名 map[keytype]valuetype

key、value 的类型：bool、数字、string、指针、channel，还可以是前面几个类型的接口、结构体、数组。

key 通常为 int、string 类型，value 通常为数字（整数、浮点）、string、map、结构体。

key：不可以为 slice、map、function。

    package main

    import "fmt"

    func main() {
        var a map[int]string
        //只声明 map 内存是不会自动分配的
        //必须通过 make 函数进行初始化，才会分配空间
        a = make(map[int]string, 10)
        a[20095452] = "张三"
        a[20095387] = "李四"
        a[20097291] = "王五"
        fmt.Println(a)
    }

输出：

    [Running] go run "e:\Codes\Go\src\code\basic\unit9\demo01\main.go"
    map[20095387:李四 20095452:张三 20097291:王五]

1. map 集合在使用前一定要 make。
2. map 的 key-value 存储是无序的。
3. key 不可以重复，如果重复，后一个 value 会替换前一个 value。
4. value 可以重复。
5. make(type, size) 中的 size 可省略，默认分配一个元素的空间。

## 2.3.2 map 的创建

    package main
    
    import "fmt"
    
    func main() {
        //方式 1
        var a map[int]string
        //只声明 map 内存是不会自动分配的
        //必须通过 make 函数进行初始化，才会分配空间
        a = make(map[int]string, 10)
        a[20095452] = "张三"
        a[20095387] = "李四"
        a[20097291] = "王五"
        fmt.Println(a)
    
    
        //方式 2
        b := make(map[int]string)
        b[20095452] = "张三"
        b[20095387] = "李四"
        fmt.Println(b)
    
    
        //方式 3
        c := map[int]string{
            20095452: "张三",
            20095387: "李四",
        }
        fmt.Println(c)
    }

## 2.3.3 map 的操作

* 增加：map["key"]= value （key 未有）
* 删除：delete(map, "key") - 如果 key 存在，删除该 key-value，如果 key 不存在，则不操作，也不报错
* 清空：
  * 要删除 map 的所有 key，没有专门的办法可以一次性删除，只能遍历然后逐个删除
  * map = make(...)，make 一个新的，让原来的成为垃圾被 gc 回收
* 查找：value, bool = map[key] - value 为返回的 value，bool 为是否返回，要么 true 要么 false
* 修改：map["key"]= value （key 已有）
* 获取长度：len 函数
* 遍历：for-range
  * 两层 map 的遍历
