# 3.3 常用函数

## 3.3.1 字符串相关函数

【1】统计字符串的长度，按字节进行统计：len(str)

* 使用内置函数不用导包，直接用就行。

【2】字符串遍历：r:=[]rune(str)（切片）

【3】字符串转整数：n, err := strconv.Atoi("66")

【4】整数转字符串：str = strconv.Itoa(6887)

【5】查找子串是否在指定的字符串中：strings.Contains("javaandgolang", "go")

【6】统计一个字符串有几个指定的子串：strings.Count("javaandgolang", "a")

【7】不区分大小写的字符串比较：fmt.Println(strings.EqualFold("go", "Go"))

* 区分大小写用 ==

【8】返回子串再字符串第一次出现的索引值，如果没有返回 -1：strings.Index("javaandgolang", "ga")

【9】字符串的替换：strings.Replace("goandjavagogo", "go", "golang", n)

* n 可以指定你希望替换几个,如果 n = -1 表示全部替换，替换两个 n 就是 2

【10】按照指定字符为分割标识，将一个学符串拆分成字符串数组：strings.Split("go-python-java","-")

【11】将字符串的字母进行大小写的转换：strings.ToLower("Go")// go 以及 strings.ToUpper"go")//Go

【12】将字符串左右两边的空格去掉：strings.TrimSpace("go and java ")

【13】将字符串左右两边指定的字符去掉：strings.Trim("~golang~ ",”~")

【14】将字符串左边指定的字符去掉：strings.TrimLeft("~golang~ ","~")

【15】将字符串右边指定的字符去掉：strings.TrimRight("~golang~ ","~")

【16】判断字符串是否以指定字符串开头：strings.HasPrefix("http://java.sun.com/jsp/jstl/fmt", "http")

## 3.3.2 日期与时间相关函数

## 3.3.3 内置函数

### 什么是内置函数、内建函数

Golang 设计者为了编程方便，提供了一些函数，这些函数不用导包也可以直接使用，我们称为 Golang 的内置函数/内建函数。

### 内置函数存放的位置

在 builtin 包下，使用内置函数时直接用就行。

### 常用内置函数

* len 函数：统计字符串的长度，按字节进行统计。
* new 函数：分配内存，主要用来分配值类型（int 系列，float 系列，bool，string、数组和结构体 struct）
* make 函数：分配内存，主要用来分配引用类型（指针、slice 切片、map、管道 chan、interface 等）
