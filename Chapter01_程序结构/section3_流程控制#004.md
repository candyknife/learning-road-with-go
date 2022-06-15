# #004 流程控制

## 4.1 引入

### 4.1.1 流程控制的作用

流程控制语句是用来控制程序中个语句执行顺序的语句，可以把语句组合成能完成一定功能的小模块。

### 4.1.2 控制语句的分类

控制语句分三类：顺序、选择 和 循环

- 顺序结构：先执行 a ，再执行 b
- 条件判断结构：如果……，则……
- 循环结构：如果……，则再继续……

三种流程控制语句可以表示所有的事情。

## 4.2 分支结构

### 4.2.1 if 分支

#### a. 单分支

----
    if 条件表达式(
        逻辑代码
    )

当条件表达式为 true 时，就会执行包含的逻辑代码。

- 条件表达式左右的括号，Golang 建议不写。
- if 与 条件表达式 中间一定要有空格。
- Golang 中即便逻辑代码只有一行，花括号也必须有。

#### b. 双分支

----
    if 条件表达式 {
        逻辑代码1
    } else {
        逻辑代码2
    }

> 必须严格按照上面格式书写，else 另起一行报错。

#### c. 多分支

----
    if 条件表达式1 {
        逻辑代码1
    } else if 条件表达式2 {
        逻辑代码2
    } else if 条件表达式3 {
        逻辑代码3
    } ... else {
        逻辑代码n
    }

### 4.2.2 switch 分支

----
    switch 表达式 {
        case 值1，值2，……：
            语块1
        case 值1，值2，……：
            语块2
        ……
        default：
            语块n
    }

示例：

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        // 实现功能：根据学生分数，判断学生等级
        var score int = 87
        // default 是来“兜底”的一个分支，其他 case 分支都不走的情况下会走 default 分支
        // default 分支可以放在任意位置上，不一定非要放在最后
        switch score / 10 {
        case 10:
            fmt.Println("您的等级为 A 级！")
        case 9:
            fmt.Println("您的等级为 A 级！")
        case 8:
            fmt.Println("您的等级为 B 级！")
        case 7:
            fmt.Println("您的等级为 C 级！")
        case 6:
            fmt.Println("您的等级为 D 级！")
        case 5:
            fmt.Println("您的等级为 E 级！")
        case 4:
            fmt.Println("您的等级为 E 级！")
        case 3:
            fmt.Println("您的等级为 E 级！")
        case 2:
            fmt.Println("您的等级为 E 级！")
        case 1:
            fmt.Println("您的等级为 E 级！")
        case 0:
            fmt.Println("您的等级为 E 级！")
        default:
            fmt.Println("您的成绩有误!")
        }
    }

1. switch 后面是一个表达式（常值、变量、有返回值的函数都可以）
2. case 后面各个值的数据类型，必须和 switch 的表达式的数据类型一致
3. case 后面可以带多个表达式，使用逗号间隔
4. case 后面的表达式如果是常量值（字面量），则要求不能重复：比如不能出现两个 case 10
5. case 后面不需要带 break ，程序匹配到一个 case 后就会执行相应的代码块，然后退出 switch，如果一个都匹配不到，则执行 default
6. default 语句不是必须的，default 的位置也可以随意
7. switch 后也可以不带表达式，当作 if 分支来使用
8. switch 后也可以直接声明/定义一个变量，分号结束，但 Golang 不推荐
9. switch 穿透，利用 fallthrough 关键字，如果在 case 语句块后增加 fallthrough ，则会继续执行下一个 case ，这也叫 switch 穿透

## 4.3 循环结构

### 4.3.1 for 循环

----
    for 初始表达式；布尔表达式；迭代因子 {
        循环体；
    }

for 循环语句是支持迭代的一种通用结构，是最有效、最灵活的循环结构。

for 循环在第一次循环之前要进行初始化，即执行初始表达式；随后，对布尔表达式进行判定，若判定结果为 true 则执行循环体，否则终止循环；在每一次循环的时候，都要进行某种迭代。

示例：

----
    package main

    import "fmt"
    
    func main() {
        //求和
        var sum int
        for i := 1; i < 6; i++ {
            sum += i
        }
        fmt.Println(sum)
    
        //注意：for 的初始表达式不能用 var 定义变量，要用 :=
        //注意：for 循环只是写代码时更简洁了，执行层面上没有变化
        //当while用
        j := 1
        for j < 6 {
            sum += j
            j++
        }
        fmt.Println(sum)
    
        //死循环
        for {
            fmt.Println("Hello World!")
        }
    }

### 4.3.2 for range 结构

----
    for key, val := range roll {
        ...
    }

这是 Golang 特有的一种结构，许多情况下都特备有用。for range 可以遍历数组、切片、字符串、map 及通道，for range 语法上类似于其他语言中的 foreach 语句。

示例：

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        // 定义一个字符串
        var str string = "hello golang你好"
        
        // 方式1：for循环，按字节进行遍历
        for i := 0; i < len(str); i++ {
            fmt.Printf("%c", str[i])
        }
        
        // 方式2：for range
        // 对 str 进行遍历，遍历的结果的索引值被 i 接收，每个结果的具体数值被 value 接收
        // for range 按照字符进行遍历
        for i, value := range str {
            fmt.Printf("索引为：%d，具体的值是：%c \n", i, value)
        }
    }

## 4.4 流程控制中的关键字

### 4.4.1 break

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        // 功能：求1-100的和，当和第一次超过300的时候，停止程序
        var sum int = 0
        for i := 0; i < 100; i++ {
            sum += i
            fmt.Println(sum)
            if sum > 300 {
                //停止正在进行的这个循环
                break
            }
        }
    }

1. switch 分支中，每个 case 分支实际上由 break 终止，只是 Golang 中可以省略。
2. 可以用 break 终止循环，但只能结束 break 所在的那一层循环。
3. 想用 break 终止任意循环，可以使用标签。

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        label:
        for i := 1; i < 5; i++ {
            for j := 1; j < 5; j++ {
                fmt.Printf("i = %v, j = %v \n", i, j)
                if i == 3 && j == 3 {
                    break label
                }
            }
        }
    }

### 4.4.2 continue

continue 的作用：结束本次循环，继续下一次循环。

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        //功能：输出1-100中被6整除的数
        for i := 1; i <= 100; i++ {
            if i%6 == 0 {
                fmt.Println(i)
            }
        }
    
        //continue 结束本次循环，继续下一次循环
        for i := 1; i <= 100; i++ {
            if i%6 != 0 {
                continue
            }
            fmt.Println(i)
        }
    }

- continue 结束与继续的也只是 continue 所在的那一层循环。
- 也可以使用 **标签** 来控制结束与继续的循环层级。

### 4.4.3 goto

goto 语句可以无条件地转移到程序中指定的行。

goto 语句通常与条件语句配合使用。可用来实现条件转移。

> [!WARNING]
> Golang 中一般不建议使用 goto 语句，以免造成程序流的混乱。

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        fmt.Println("Hello Golang1")
        fmt.Println("Hello Golang2")
        fmt.Println("Hello Golang3")
        if true {
            goto label
        }
        fmt.Println("Hello Golang4")
        fmt.Println("Hello Golang5")
        fmt.Println("Hello Golang6")
    label:
        fmt.Println("Hello Golang7")
        fmt.Println("Hello Golang8")
        fmt.Println("Hello Golang9")
    }

### 4.4.4 return

return 可以直接终止当前函数。

----
    package main
    
    import (
        "fmt"
    )
    
    func main() {
        for i := 0; i < 100; i++ {
            fmt.Println(i)
            if i == 14 {
                return
            }
        }
        fmt.Println("Hello Golang")
    }
