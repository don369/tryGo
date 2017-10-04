### Go程序
Go程序基础是包(package)，又一个又一个的包组成，程序的运行入口是包main。一个程序必须而且只能有一个mian；

```go
package main //声明mian包

//导入其他的包(package)
import (
    ...
)

func main(){
    ...
}
```

### 包(package)
包是Go程序的组成基础单位；每个Go源文件开头必须有package声明，其作用是被其他源文件导入的默认标识符,包的名称为当前文件的的文件夹名称，如util/md5.go 那么md5.go中的package的名称为util

```go
package util
```
包(package)中的首字母大写的名称都会被导出
```go
package util

var md5 string //不会导出

//会被导出
func GetMD5(str string) string{
    ...
}
```

使用import 导入已定义的包(package),import 导入有两种方式:

```go
package main
//第一种方式，单个导入
import "math"
//第二种方式为多个导入
import (
    "fmt"
    "./util"
)
//导入后就可以调用对应包(package)中定义的函数或数据
func main(){
    fmt.PrintLn(util.GetMD5("md5string"))
}
```

###数据类型 
go得数据类型分为两大类，一种是基本类型，一中是复杂类型。

基本类型主要分为三大类：字符串(string)，布尔值(boolean)，数值类型。
数值类型细分为：整型：

```go
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
PS：其中uint8同表字节(byte)，byte为uint8的别名
而int32代表一个Unicode码，rune为int32 的别名
```

浮点型：

```go
float32 float64
```

以及更为复杂的complex:

```go
complex64 complex128
```

复杂类型为：struct(类似C的结构体)，slice(类似python的list)，map(类似ruby的hash)

### 变量与定义
Go的变量定义规则为：

单个变量的定义：

`var 变量名称 v [= value]`

多个变量的定义：

`var 变量名称, 变量名称1, ... [数据类型] [= value,value1,...]`

省略数据类型的定义：

`var 变量名称[, 变量名称1, ...]  = value[,value1,...]`


```go
var i int //声明一个int类型的变量
var pi float32 = 3.14 //声明一个float32类型的变量并赋值
var j, k int = 1, 2 //声明多个变量以及赋值
var hello = "你好" //可以不声明类型，但必须赋值
```

### 短声明变量

在以下的条件的情况下可以使用 `:=` 简洁的运算符来代替 `var` 定义变量：
1. 必须在函数体内
2. 明确数据类型

短声明变量的定义规则:

` 变量名称[, 变量名称1, ...] := value[,value1,...] `

### 常量
常量与变量类型，区别在于常量的值是不可以更改，以及不能使用 `:=` 定义。

常量的定义规则:

`const 常量名称[, 常量名称1, ...] [数据类型] = value[,value1,...]`

或
```
const (
    常量名称 [数据类型] = value
    ...
)
```
例子：
```go
const Pi = 3.14
const Hello string = "你好"
const MIN, MAX = 0, 100
MIN = -1 //err, 常量的值是不可以更改,重新赋值会报错。
```

### 数组 和 Slice
数组是一组有`固定长度`的集合，Slice是一组没有固定长度的集合，Slice类似于python的list;

数组语法：`var a [n]T = [n]T{...} //n >= 0`

Slice语法: `var a []T = []T{...} `

数组在Go极少用到，主要是用Slice，Slice用法与python的list一样简单易懂，详细在此不表，另开文章再写。

### map
Go中的map是一种 `键(key)-值(value)`的集合，类似python的dict

map必须由make创建：
```go
var dict map[string] string = make(map[string] string)
dict["hello"] = "你好"

```

### 结构体 struct
结构体(struct)就是一个或多个变量的集合。类似于javascript的Object;

结构体的定义以及调用:
```go
type User struct{
    id int
    name string
}

var zhang_sang User

zhang_sang.id = 12333
zhang_sang.name = "张三"

//or
var li_si = User{23333, "李四"}
//or

var xiao_wu = User{ name:"小五" }
```
### 函数

在Go中，函数是第一公民，函数可以有零个或多个参数，函数的语法为:
```go
func 函数名称([参数1 数据类型, 参数2 数据类型,...]) [返回值的数据类型] {
    ....
    [return 返回值]
}
```
如果函数中没有返回值，则返回值的数据类型不用写，如果有返回值，则需写上。返回值可以有多个，如:
```go

func return_one_two() (int, int){
    return 1,2
}

var i, j = return_one_two()

```

### 流程控制语句

#### for
Go的循环语句只有for，没有while，for语句分为三部分：
```go
for 初始化;条件判断;后置语句 {
    //初始化:循环执行前执行一次,之后就进入条件判断，在本次循环中不会再执行
    //条件判断: 如果条件为true则执行块({...})中的代码，结束后进入后置语句;如果为false则结束本次循环
    //后置语句:后置语句执行之后就进入条件判断，直到条件判断为false止。
    ...
}
```

for的`初始化`和`后置语句`可以省略，如果省略`初始化`和`后置语句`那么for用法和while的用发是一致的；
```go
var i = 0

for i >= 10000 {
    fmt.Println("i: ", i)
    i++
}

```
如果连 `条件判断` 则为死循环:

`for {}` 等同于 `for true {}`

#### if 和 else 以及 else if
if和其他语言没有大区别，和for一样，省略了括号，
```go
if 条件判断 {
    ...
}[else if {

}else{

}]
```
和其他语言不同的是 `else` 和 `else if` 必须接在 `}` 的后面:
```go
if isTure {
    ...
}
else {//err
    ...
}
```
#### switch , case, default, fallthrough
Go的 switch 使用灵活。可以接受任意形式的表达式；也不用通过break跳出分支；
```go
switch param {
    case value1:
        ...
    case value2:
        ...
    default:
        ...
}
```
如果希望执行完当前分支后继续执行下一个分支可以在当前分支加上`fallthrough`
```go
    case 1:
        ...
        fallthrough
    case 2:
        ...
```

### defer 延时
被`defer` 标记的函数会延时直到上层函数体执行完毕后再执行:

```go
func main() {
    defer fmt.Println("这是测试defer的延时执行")
    fmt.Println("你好")
    //结果：
    //你好
    //这是测试defer的延时执行
}
```
如果一个函数体内有多个defer，则同栈一样后加载先执行。
`注意`: 延时时参数是不会延时调用

```go
func main() {
    str := "这是测试defer的延时执行"
    defer fmt.Println(str)
    str = "Go"
    fmt.Println("你好")
    //结果：
    //你好
    //这是测试defer的延时执行
}
```

### Go没有class，但是依然可以面向对象

