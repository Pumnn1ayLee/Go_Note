# Golang个人总结

##                          SplashNirvana2K

## 写在前面

每种语言都有每种语言的独特的应用领域。



例如

嵌入式领域-----C和汇编是首选

操作系统领域-----C是首选

系统级服务编程领域-----C++是首选

企业级应用程序和Web应用领域-----Java是首选



而Go产生主要有以下原因：

1.当前编程语言对并发的支持不是很好，不能很好的发挥多核CPU的能力。

2.程序规模越来越大，编译速度越来越慢。

3.某些特性的实现不怎么优雅，程序员花费更多的精力来应对语法而不是问题域。



**Go语言就是为了解决并发支持不友好、编译速度慢、编译复杂的。**



本个人总结将以最简单直接的方式记录个人对golang的学习。



以下表格选自书《Go语言核心编程》：

|    特性集合    |      特性项      |                 Go                 |           C            |              Java               |
| :------------: | :--------------: | :--------------------------------: | :--------------------: | :-----------------------------: |
|    基础语法    |  关键字和保留字  |                25个                |        ANSI32个        |            大于48个             |
|                |     控制结构     |        支持顺序、循环、分支        |  支持顺序、循环、分支  |      支持顺序、循环、分支       |
|    类型系统    |    动、静特性    |    静态语言、支持运行时动态类型    |        静态语言        |            静态语言             |
|                |    强、弱特性    |               强类型               |         弱类型         |             强类型              |
|                |   隐式类型推导   |                支持                |           否           |               否                |
|                |     类型安全     |              类型安全              |       非类型安全       |            类型安全             |
|                |  自定义数据类型  |           支持type自定义           |         struct         | 通过类/接口实现自定义类型和行为 |
|      抽象      |       函数       |                支持                |          支持          |              支持               |
|                |   面向对象支持   |        类型组合支持面向对象        | struct内嵌函数指针支持 |             类/接口             |
|                |       接口       |              Duck类型              |     void*间接支持      |            显式声明             |
|                |       多态       |            通过接口支持            |     void*间接支持      |       接口及继承关系支持        |
|     元编程     |     泛型支持     |                 有                 |           无           |               有                |
|                |     反射支持     |                 有                 |           无           |               有                |
| 平台和运行模式 |     编译模式     |          编译成可执行文件          |    编译成可执行文件    |        编译成中间字节码         |
|                |     运行模式     |              直接运行              |        直接运行        |         虚拟机加载运行          |
|                |     内存管理     |          支持自动垃圾回收          |        手动管理        |        支持自动垃圾回收         |
|                |     并发支持     |        协程（语言原生支持）        |  OS线程（库支持协程）  | Java线程（JVM内部映射到OS线程） |
|                |     交叉编译     |                支持                |          支持          |     中间代码无交叉编译必要      |
|                |      跨平台      |                支持                |          支持          |           原生跨平台            |
|   语言软实力   | 标准库和第三方库 |           丰富、发展很快           |         很丰富         |             很丰富              |
|                |       框架       |           丰富、发展很快           |         很丰富         |             很丰富              |
|                |    语法兼容性    |            向前兼容性好            |      向前兼容性好      |          向前兼容性好           |
|                |      影响力      |          社区活跃，Google          |     40多年宝刀未来     |  社区活跃、近几年受Oracle控制   |
|                |     应用领域     | 云计算基础设施软件、中间件、区块链 |      OS及系统软件      |   企业级应用、大数据、移动端    |



**当然，学习笔记怎么可能完全是课本照搬呢？！**

<font size = 16>精简易懂</font>才是重点

### 基础

### 标识符

#### 关键字

Go有**25个关键字**



**引导程序整体结构**的有**8个**关键字：

package  //定义包名

import  //导入包名

const  //常量声明

var  //变量声明

func  //函数定义

defer  //延迟调用

go  //并发语法

return  //函数返回



**声明复合数据结构**的有**4个**关键字：

struct  //定义结构类型

interface  //定义接口

map  //声明或创建map类型

chan  //声明或创建通道类型



**控制程序结构**的有**13个**关键字：

if else  //if else语句

for range break continue  //for循环使用

switch select type case default fallthrough  //switch和select语句

goto  //goto语句跳转



数据类型标识符

数值（16个）

  整型（12个）：byte int int8 int16 int32 int64

​                             uint uint8 uint 16 uint32 uint64 uintprt

  浮点型（2个）： float32 float64

  复数型（2个）： complex64 complex128



字符和字符串型（2个）

string rune



接口型（1个）

error



布尔型（1个）

bool



#### 内置函数标识符

make

new

len

cap

append

copy

delete

panic

recover

close

complex

real

image

Print

Println



#### 常量值标识符



true false

iota

nil



#### 空白标识符

_

### 变量和常量

变量指向的内存可以被修改，常量指向的内存不能被修改。

#### 变量

##### 变量的显式声明

```go
var a int = 1
var a int = 2*3
var a int = b
```

##### 短类型声明

```go
varName := value
```

:= 声明只能出现在函数内

此时Go编译器自动进行数据类型判断

Go支持多个类型变量同时声明并赋值，如：

```go
a,b := 1,"hello"
```

1.变量实际指向地址里存放的值

2.Go语言提供自动内存管理，编译器使用栈逃逸技术能够自动为变量分配空间，可能在栈上也可能在堆上。

3.类型决定了该变量存储的值怎么解析，以及支持哪些操作和运算。

4.Go内部使用统一的命名空间对变量进行管理，每个变量都有一个唯一的名字。

#### 常量

常量使用一个名称来绑定一块内存地址，该内存地址存放的数据类型由定义常量时的类型决定，并且该内存地址里面存放的内容不可以改变。

```go
const (
c0 = iota //c0 == 0
c1 = iota //c1 == 1
c2 = iota //c2 == 2
)

//简写模式
const(
c0 = iota // c0 ==0
c1
c2
)

//分开的const语句,iota每次都从0开始
const x = iota  // x == 0
const y = iota  // y == 0
```

### 字符串

```go
var a = "hello,world"
```

1.字符串是常量，可以通过类似数组的索引访问其字节单元，但不能修改某个值。

```go
var a = "hello world"
b := a[0]
a[1] = 'a' //error
```

2.字符串转换为切片[]byte(s)要慎用，尤其是数据量大的时候（每转换一次都要转换内容）

```go
a := "hello world"
b := []byte(a)
```

3.字符串尾部不包含NULL字符

4.字符串类型底层实现是一个二元的数据结构，一个指向字节数组的起点，另一个是长度

```go
// runtime/string.go

type stringStruct struct{
str unsafe.Pointer  //指向底层字节数组的指针
len int  //字节数组长度
}
```

5.基于字符串创建的切片和原字符串指向相同的底层字符数组，一样不能修改，对于字符串的切片操作返回的字串仍然是string，而非slice,如：

```go
a := "hello world"
b := a[0:4]
c := a[1:]
d := a[:4]
```

6.字符串和切片的转换，字符串可以转换为字节数组，也可以转换为Unicode的字数组：

```go
a := "hello，世界"
b := []byte(a)
c := []rune(a)
```

### rune类型

rune表示Unicode编码的字符，在Go内部是int32类型的别名，占用4个字节。

Go语言默认的字符编码是UTF-8类型。

### 指针

1.在赋值语句中，*T出现在“=”左边表示指针声明，*T出现在在“=”右边表示取指针指向的值。

```go
var a = 11
p := &a //*p和a的值都为11
```

2.结构体指针访问结构体字段仍然使用"."点操作符

```go
type User struct{
name string
age int
}
andes := User{
name : "andes",
age : 18,
}
p := &andes
fmt.Println(p.name)
```

3.Go不支持指针运算。

Go由于支持垃圾回收，如果支持指针运算，则会给垃圾回收带来很多不便（点名C和C++)。

```go
a := 1234
p := &a
p++ //不允许
```

4.函数中允许返回局部变量的地址。

Go编译器使用“栈逃逸”机制将这种局部变量的空间分配在堆上

```go
func sum(a,b int) *int{
sum := a+b
return &sum  //允许，sum会分配在堆heap上
}
```

### 数组

数组的类型名是[n]elementType，其中n是数组长度，elementType是数组元素类型。

#### **数组的初始化**

```go
a := [3]int{1,2,3} //指定长度和初始化字面量
```

```go
a := [...]int{1,2,3} //不指定长度，但是由后面的初始化列表数量来确定其长度
```

```go
a := [3]int{1:1,2:3} //指定总长度，并通过索引值进行初始化，没有初始化元素时使用类型默认值
```

```go
a := [...]int{1:1,2:3} //不指定总长度，，并通过索引值进行初始化，没有初始化元素时使用类型默认值
```

##### 数组的特点

1.创建完长度后就固定，不可以再追加元素

2.数值为值类型，数组赋值或作为函数参数都是值拷贝

3.[10]int和[20]int表示不同的类型

4.可以根据数组创建切片

### 切片

一种变长数组

数据结构中有指向数组的指针

是一种引用类型

Slice依托数组实现，底层数组对用户屏蔽，在底层数组容量不足时可以实现自动重分配并生成新的Slice。

#### Slice的数据结构

```go
type slice struct{
  array unsafe.Pointer //指针指向底层数组
  len int  //切片长度
  cap int  //底层数组容量
}
```

#### 切片的创建

##### 使用make创建Slice

```go
slice := make([]int,5,10)
```

![2d753d620467ea531ba7f769a948b95](https://github.com/Pumnn1ayLee/Go_Note/blob/main/images/image-20230705002305381.png)

该Slice长度为5，即可以使用下标slice[0] ~ slice[4]来操作里面的元素，capacity为10，表示后续向 slice添加新的元素时可以不必重新分配内存，直接使用预留内存即可。

##### 使用数组创建Slice

```go
slice := array[5:7]
```

使用数组来创建Slice时，Slice将与原数组共用一部分内存

![](images\image-20230705002305381.png)

切片从数组array[5]开始，到数组array[7]结束（不含array[7]），即切片长度为2，数组后面的内容都作为切 片的预留内存，即capacity为5

数组和切片操作可能作用于同一块内存

#### Slice扩容

使用append向Slice追加元素时，如果Slice空间不足，将会触发Slice扩容

扩容实际上重新分配一块更大的内 存，将原Slice数据拷贝进新Slice，然后返回新Slice，扩容后再将数据追加进去。

例如，当向一个capacity为5，且length也为5的Slice再次追加1个元素时，就会发生扩容，如下图所示：

![](https://github.com/Pumnn1ayLee/Go_Note/blob/main/images/image-20230705002658257.png)

扩容操作只关心容量，会把原Slice数据拷贝到新Slice，追加数据由append在扩容结束后完成

上图可见，扩容后 新的Slice长度仍然是5，但容量由5提升到了10，原Slice的数据也都拷贝到了新Slice指向的数组中

**扩容容量选择遵循以下规则：**

1.如果原Slice容量小于1024，则新Slice容量将扩大为原来的2倍；

2.如果原Slice容量大于等于1024，则新Slice容量将扩大为原来的1.25倍；

使用append()向Slice添加一个元素的实现步骤如下： 

1. 假如Slice容量够用，则将新元素追加进去，Slice.len++，返回原Slice 
2. 原Slice容量不够，则将Slice先扩容，扩容后得到新Slice 
3. 将新元素追加进新Slice，Slice.len++，返回新的Slice

#### 切片支持的操作

len()返回切片长度

cap()返回切片底层数组容量

append()对切片追加元素

copy()用于复制一个切片

#### 字符串和切片的相关转换

```go
str := "hello，世界!"
a := []byte(str) //将字符串转换为[]byte类型切片
b := []rune(st) //将字符串转换为[]rune类型切片
```

### map字典

#### map的创建

##### 字面量创建

```go
ma := map[string]int{"a" : 1,"b" : 2}
fmt.Println(ma["a"])
fmt.Println(ma["b"])
```

##### make函数创建

```go
ma := make(map[string]int,len) //map容量使用给定的len值
```

#### map支持的操作

访问格式：mapName[key]

可以使用range遍历一个map类型变量，但不保证每次迭代元素的顺序

删除map中某个键值：delete(mapName,key)

可以使用len()函数返回map中的键值对数量

```go
ma := make(map[int]string)
ma[1] = "tom"
ma[1] = "pony"
ma[2] = "jaky"
ma[3] = "andes"
delete(ma,3)

fmt.Println(len(ma))
for key,value := range ma{
fmt.Println("key is:",key,"value is:",value)
}
```

#### 注意

1.Go内置的map不是并发安全的，并发安全的map可以使用标准包sync中的map

2.不要直接修改map value内某个元素的值，如果想修改map的某个键值，则必须整体赋值。

```go
type User struct{
    name string
    age int
}
ma := make(map[int]User)
andes := User{
    name : "andes",
    age : 18,
}

ma[1] = andes
//ma[1].age = 19 //error,不能通过map引用直接修改
andes.age = 19

ma[1] = andes //必须整体替换value
fmt.Printf("%v\n",ma)
```

### 结构Struct

1.struct结构中的类型可以是任意类型

2.struct的存储空间是连续的

struct有两种形式：1.struct类型字面量 2.使用type声明的自定义struct类型

#### struct类型字面量

格式：

```go
struct {
    FeildName FeildType
    FeildName FeildType
    FeildName FeildType
}
```

#### 自定义struct类型*

一般用这个

```go
type TypeName struct{
    FeildName FeildType
    FeildName FeildType
    FeildName FeildType
}
```

#### struct类型变量的初始化

```go
type Person struct{
    Name string
    Age int
}
type Student struct{
    *Person
    Number int
}

//推荐使用这种使用Feild名字的初始化方式
p := &Person{
    Name : "tata",
    Age : 12,
}

s := Student{
    Person : p,
    Number : 120,
}
```

### 控制结构

#### if else语句

```go
if x := f(); x < y{ //初始化语句中的声明变量x
    return x
} else if x > z{  //x在else if 里面一样可以被访问
    return z
} else {
    return y
}
```

#### 推荐

```go
err , file := os.Open("xxxx")
if err != nil{
    return nil,err
}
defer file.Close()
```

#### switch语句

1.switch后面可带一个可选的简单的初始化语句

2.switch后面的表达式是可选的，若没有表达式，则case是一个布尔表达式

3.switch支持相等比较运算的类型变量。

4.通过fallthough语句来强制执行case子句（不再判断下一个case子句的条件是否满足

5.支持default

6.switch和.(type)结合可以进行类型的查询

### 循环结构

#### for语句

```go
for init; condition;post{}
```

```go
for condition{}
```

```go
for {}
```

对数组、切片、字符串、map和通道的访问

```go
//访问map
for key,value := range map{}
for key := range map{}
```

```go
//访问数组
for index, value := range arry{}
for index := range arry{}
for _,value := range arry{}
```

```go
//访问切片
for index,value := range slice{}
for index := range slice{}
for _,value := range slice{}
```

```go
//访问通道
for value := range channel{}
```

## 函数

### 函数定义

```go
func funcName(param-list)(result-list){
    function-body
}
```

### 函数特点

1.函数可以没有输入参数，也可以没有返回值

```go
func A(){
    //do something
}
func A()int{
    return 1
}
```

2.多个相邻的相同类型参数可以使用简写模式

```go
func add(a,b int)int{
return a + b
}
```

3.支持有名的返回值

```go
func add(a,b int)(sum int){
sum = a + b
return // return sum 的简写模式
    // sum := a + b
    // return sum  需要显式调用return sum
}
```

4.不支持默认值参数

5.不支持函数重载

6.不支持命名函数的嵌套定义，支持嵌套匿名函数

```go
func add(a,b int)(sum int){
    a := func(x,y int) int {
        return x + y
    }
    return a(a,b)
}
```

### 多值返回

```go
func swap(a,b int)(int,int){
    return b,a
}
```

.如果多值返回值有错误类型，则一般将错误类型err作为最后一个返回值。

### 实参到形参的传递

Go函数实参到形参的传递永远是值拷贝，有时函数调用后实参指向的值发生了变化。那是因为参数传递的是指针值的拷贝。

```go
package main

import "fmt"

func chvalue(a int) int {
    a = a + 1
    return a
}

func chpointer(a *int){
    *a = *a + 1
    return
}

func main(){
    a := 10
    chvalue(a) //实参传递给形参是值传递
    fmt.Println(a)
    
    chpointer(&a) //仍然是值拷贝，只不过复制的是a的地址值
    fmt.Println(a)
}
```

### 不定参数

Go函数支持不定数目的形式参数。

使用param ...type的语法格式。

1.所有的不定参数类型必须是相同的

2.不定参数必须是函数的最后一个参数

3.不定参数名在函数体内相当于切片，切片操作同样适应不定参数

```go
for sum(arr ...int)(sum int){
    for _,v := range arr{
        sum += v
    }
    return
}
```

4.切片可以作为参数传递给不定参数，切片名后要加上"..."

```go
func sum(arr ...int)(sum int){
    for _,v := range arr{
        sum += v
    }
    return
}
func main(){
    slice := []int{1,2,3,4}
    array := [...]int{1,2,3,4}
    sum(slice...)
}
```

5.形参为不定参数的函数和形参为切片的函数类型不相同

```go
func suma(arr ...int)(sum int){
    for v := range arr{
        sum += v
    }
    return
}

for sumb(arr []int)(sum int){
    for v := range arr{
        sum += v
    }
    return
}

//类型不一样
fmt.Printf("%T\n",suma) //func(...int) int
fmt.Printf("%T\n",sumb) //func([]int) int
```

### 有名函数

有名函数可以直接调用和直接赋值给变量

```go
package main

func sum(a,b int) int {
    return a + b
}

func main(){
    sum(3,4) //直接调用
    f := sum //直接赋值给变量
    f(1,2)
}
```

### 匿名函数

匿名函数可以看作函数字面量

可以直接赋值给函数变量

可以当作实参

可以作为返回值

也可以直接被调用

```go
package main

import "fmt"

//直接赋值函数变量
var sum = func(a,b int)int {
    return a + b
}

func doinput(f func(int,int) ,a,b int)int{
    return f(a,b)
}

//作为返回值
func wrap(op string) func (int,int) int{
    switch op{
        case "add":
        return func(a,b int) int{
            return a + b
        }
    case "sub" :
        return func(a , b int) int{
            return a + b
        }
        default:
        return nil
    }
}

func main(){
    //匿名函数直接被调用
    defer func{
        if err := recover() ; err != nil{
            fmt.Println(err)
        }
    }()
    
    sum(1,2)
    
    //匿名函数作实参
    doinput(func(x,y int)int{
        return x + y
    }1,2)
    
    opFunc := wrap("add")
    re := opFunc(2,3)
    
    fmt.Printf("%d\n",re)
}
```

### defer

延迟调用，可以注册多个延迟调用，这些调用以先进后厨的顺序在函数返回前被执行。

defer后面必须是函数或方法的调用

defer函数的实参在注册时通过值拷贝传递进去

defer必须先注册后才能执行

当主动调用os.Exit(int)退出进程时，defer将不再被执行



如果defer位置不当，可能导致panic

defer会推迟资源的释放，尽量不要放到循环语句里面

defer中最好不要对有名返回值参数进行操作



### 闭包

闭包是由函数及其相关引用环境组合而成的实体，一般通过在匿名函数中引用外部函数的局部变量或包全局变量构成

闭包 = 函数 + 引用环境

```go
package main

import "fmt"

func main() {
    // 外部函数返回一个闭包函数
    greeting := getGreeting("John")
    
    // 调用闭包函数
    fmt.Println(greeting()) // 输出: Hello, John!
}

// 外部函数，接收一个名字参数，返回一个闭包函数
func getGreeting(name string) func() string {
    // 内部函数，返回一个拼接了名字的问候语字符串
    return func() string {
        return "Hello, " + name + "!"
    }
}

```

#### 闭包的作用（有点长）

闭包在编程中有几个重要的作用：

1. **保留状态：** 闭包可以捕获并保留其创建时的上下文状态。这意味着闭包函数可以访问并修改其外部函数的变量，即使外部函数已经返回，闭包仍然可以继续使用这些变量。这对于需要维护状态或记住先前状态的情况非常有用。
2. **封装数据：** 闭包允许将数据和相关操作封装在一个函数内部。通过将数据存储在闭包的上下文中，可以隐藏数据并仅通过闭包函数来访问和操作数据。这有助于实现信息隐藏和封装的概念，提高代码的可维护性和安全性。
3. **实现函数工厂：** 闭包函数可以用作函数工厂，动态创建并返回其他函数。通过为闭包提供不同的参数，可以生成具有不同行为或配置的函数。这种模式在编写可重用的代码和实现策略模式时非常有用。
4. **实现回调：** 闭包函数可以作为回调函数传递给其他函数。回调函数是在特定事件发生或特定条件满足时被调用的函数。通过将闭包作为回调函数传递，可以在回调函数中访问外部函数的变量和状态，从而实现更灵活的回调机制。

### panic和recover

简单来说，panic用来主动抛出错误，recover用来捕获panic抛出的错误

发生panic后，程序会从调用panic的函数位置或发生panic的地方立即返回，逐层向上执行函数的defer语句，然后逐层打印函数调用堆栈，直到被recover捕获或执行到函数最外层后退出

```go
panic(i interface{})
recover()interface{}
```

```go
//如下场景会捕获成功
defer func(){
    println("defer inner")
    recover()
}()

func except(){
    recover()
}

func test(){
    defer except()
    panic("test")
}
```

#### panic使用场景

1.程序遇到了无法正常执行下去的错误，主动调用panic函数结束程序运行

2.在调试程序时，通过主动调用panic实现快速退出，panic打印出的堆栈能够更快地定位错误

为了保证程序的健壮性，需要主动在程序的分支流程上使用recover()拦截运行时的错误

## 类型

### 命令类型

通过标识符表示

### 未命名类型

由预声明类型、关键字和操作符组合而成。

又称为类型字面量，数组、切片、通道、指针、函数字面量、结构和接口都属于类型字面量，也都是未命名类型

```go
package main

import "fmt"

//使用type声明的是命名类型
type person struct{
    name string
    age int
}

func main(){
    //使用struct字面量声明的是未命名类型
    a := struct{
        name string
        age int
    }{"ands",18} //不建议这么用
    
    fmt.Printf("%T\n" , a) //struct {name string;age int}
    
    fmt.Printf("%v\n" , a) //{andes 18}
    
    b := Person{"Tom",21}
    fmt.Printf("%T\n" , b) //main.Person
    fmt.Printf("%v\n" ,b) //{Tom 21}
}
```

### 类型相同

两个命名类型是否相同，参考如下：

1.两个类型声明的语句完全相同

2.命名类型和未命名类型永远不相同

3.两个未命名类型相同的条件是它们的类型声明字面量结构相同，并且内部元素的类型相同

4.通过类型别名语句声明的两个类型相同

#### 类型别名

Go1.9引入了类型别名语法

```go
type T1 = T2  //T1的类型完全和T2一样
```

引入别名的原因：

1.为了解决旧包迁移兼容问题

2.Go的按包隔离机制不太精细，有时需要将大包划分为几个小包进行开发，但需要在大包里面暴露全部的类型给使用者

3.解决新旧类型迁移问题

### 类型赋值

```go
//a是类型为T1的变量，或者a本身就是一个字面常量或nil
//如果如下语句可以执行，称为类型T1可以赋值给类型T2
var b T2 = a
```

a可以赋值给变量b必须满足：

1.T1和T2类型相同

2.T1和T2具有相同的底层类型，且T1和T2里面至少有一个是未命名类型

3.T2是接口类型，T1是具体类型

4.T1和T2都是通道类型，拥有相同的元素类型，T1和T2里面至少有一个是未命名类型

5.a是nil，T2是pointer、function、slice、map、channel、interface中的一个

6.a是一个字面常量值，可以用来表示类型T的值

重要

```go
package main

import (
    "fmt"
)

type Map map[string]string

func (m Map) Print() {
    for _,key := range m {
        fmt.Println(key)
    }
}

type iMap Map

//只要底层类型是slice、map等支持range的类型字母量，新类型仍然可以使用range替代

func (m iMap) Print(){
    for _, key := range m{
        fmt.Println(key)
    }
}

type silce []int

func (s slice) Print(){
    for _, v := range s{
        fmt.Println(v)
    }
}

func main(){
    mp := make(map[string]string,10)
    mp["hi"] = "tata"
    
    //mp和ma有相同的底层类型map[string]string，且mp为未命名类型，可以赋值
    var ma Map = mp
    
    //im和ma虽有同样的底层类型map[string]string,但它们中没有一个是未命名类型，不能赋值
    var im iMap = ma //error
    
    ma.Print()
    im.Print()
    
    //Map实现了Print(),所以其可以赋值给接口类型变量
    var i interface{
        Print()
    } = ma
    
    i.Print()
    
    s1 := []int{1,2,3}
    var s2 slice
    s2 = s1
    s2.Print()
}
```



### 类型强制转换

强制类型转换的语法格式

```go
var a T = (T) (b)
```

Go是强类型语言，如果不满足自动转换的条件，则必须进行强制类型转换。

任意两个不相干的类型如果进行强制转换，则必须符合一定的规则

例如非常量类型变量x可以强制转化并传递给类型T，需满足：

1.x可以直接赋值给T类型变量

2.x的类型和T具有相同的底层类型

```go
package main

import(
    "fmt"
)

type Map map[string]string

func (m Map) Print(){
    for _, key := range m{
        fmt.Println(key)
    }
}

type iMap Map

//只要底层类型是slice、map等支持range的类型字面量，新类型仍然可以使用range迭代

func (m iMap) Print(){
    for _, key := range m{
        fmt.Println(key)
    }
}

func main(){
    mp := make(map[string]string,10)
    mp["hi"] = "tata"
    
    //mp和ma具有相同底层类型map[string]string,并且是未命名类型
    var ma Map = mp
    
    //im与ma虽然有相同的底层类型，但是二者没有一个是字面量类型，不能直接赋值，可以强制进行类型转换
    var im iMap = ma //error
    
    var im iMap = (iMap)(ma)
   
    ma.Print()
    im.Print()
}
```

3.x和T都是未命名的指针类型，并且指针指向的类型具有相同的底层类型

4.x和T都是整型，或者都是浮点型

5.x和T都是复数类型

6.x是整数型或[]byte类型的值，T是string类型

7.x是一个字符串，T是[]type或[]rune



常见字符串和切片之间的转换：

```go
s := "hello，世界"
var a []byte
a = []byte(s)

var b string
b = string(a)

var c []rune
c = []rune(s)
```

### 自定义类型

#### 用户自定义类型

语法是

```go
type newtype oldtype
```

oldtype可以是自定义类型、预声明类型、未命名类型的任意一种

newtype是新类型的标识符，与oldtype具有相同的底层类型，并且都继承了底层类型的操作集合（这里的操作不是方法，例如底层类型是map,支持range迭代访问，则新类型也可以使用range迭代访问）

newtype和oldtype是两个完全不同的类型，newtype不会继承oldtype的方法

```go
type INT int  //INT是一个使用预声明类型声明的自定义类型
type Map map[string]string //类型字面量声明的

type myMap Map //myMap是自定义类型Map声明的自定义类型
```

#### 自定义struct类型

struct类型是Go语言自定义类型的普遍形式，是Go语言类型扩展的基石，也是Go语言面向对象承载的基础

struct初始化（只写推荐写法）

```go
type Person struct{
    name string
    age int
}

//推荐初始化,以下都是
a := Person{name:"andes",age:18}

b := Person{
    name:"andes",
    age:18,
}

c:= Person{
    name : "andes",
    age : 18}
```

使用构造函数进行初始化

```go
//标准库中errors的New函数示例
//$ {GOROOT}/src/errors/errors.go
func New(text string) error {
    return &errorString{test}
}

type errorString struct{
    s string
}
```

#### 结构字段的特点

1.结构的字段可以是任意类型、基本类型、接口类型、指针类型、函数类型

2.结构字段的类型名必须唯一

3.struct字段类型可以是普通类型，也可以是指针

4.支持内嵌自身的指针

```go
//标准库container/list

type Element struct{
    //指向自身类型的指针
    next,prev *Element
    list *List
    Value interface{}
}
```

#### 匿名字段

字段只给出字段类型，没有字段名

被匿名嵌入的字段必须是命名类型或者命名类型的指针

类型字面量不能作为匿名字段使用

匿名字段的字段名默认就是类型名，如果匿名字段是指针类型，则默认的字段名则是指针指向的类型名

```go
//标准库${GOROOT}/src/os/type.go
type File struct{
    *file //os specific
}
```

### 方法

为命名类型定义方法的语法格式如下：

```go
//类型方法的接收者是值类型
func (t TypeName)MethodName(ParamList)(Returnlist){
    //method body
}
//类型方法的接收者是指针
func (t *TypeName)MethodName(ParamList)(Returnlist){
    //method body
}
```

Go语言的类型方法本质上就是一个函数，没有使用隐式的指针，简单明了。

将类型方法改写成常规函数：

```go
//类型方法接收者是值类型
func TypeName_MethodName(t TypeName,otherParamList)(Returnlist){
    //method body
}
//类型方法接收者是指针类型
func TypeName_MethodName(t *TypeName,otherParamList)(Returnlist){
    //method body
}
```

```go
//示例
type SliceInt []int
func (s SliceInt) Sum() int{
    sun := 0
    for _ , i := range s{
        sum += i
    }
    return sum
}
//与上面的方法等价
func SliceInt_Sum(s SliceInt) int{
    sum := 0
    for _ , i := range s{
        sum += i
    }
    return sum
}

var s SliceInt = []int{1,2,3,4}
s.Sum()
Slice_Sum(s)
```

#### 方法调用

一般调用方式

```go
TypeInstanceName.MethodName(ParamList)
```

TypeInstanceName:类型实例名或者指向实例的指针变量名

MethodName:类型方法名

ParamList:方法实参

```go
type T struct{
    a int
}

func (t T) Get() int{
    return t.a
}

func (t *T) Set(i int){
    t.a = i
}

var t = &T{}

t.Set(2)

t.Get()
```

#### 方法值

变量x的静态类型是T，M是类型T的一个方法，x.T被称为方法值

方法值是一个函数类型变量，可以赋值给其他变量，并可以像普通的函数名一样使用

```go
f := x.M
f(args...)

//等价于
x.M(args...)
```

示例：

```go
type T struct{
    a int
}

func (t T) Get() int{
    return t.a
}

func (t *T) Set(i int){
    t.a = i
}

func (t *T) Print(){
    fmt.Printf("%p,%v,%d \n",t,t,t.a)
}

var t = &T{}

f := t.Set

f(2)
t.Print()

f(3)
t.Print()
```

#### 方法表达式

方法表达式相当于提供一种语法将类型方法调用显式地转换为函数调用，接收者必须显式地传递出去。

拿这个例子做解释

```go
type T struct{
    a int
}

func (t T) Get() int{
    return t.a
}

func (t *T) Set(i int){
    t.a = i
}

func (t *T) Print(){
    fmt.Printf("%p,%v,%d \n",t,t,t.a)
}

//以下方法表达式调用都是等价的
t := T{
    a : 1,
}

//普通方法调用
t.Get(t)

//方法表达式调用
(T).Get(t)

//方法表达式调用
f1 := T.Get; f1(t)

//方法表达式调用
f2 := (T).Get; f2(t)


(*T).Set(&t,1)
f3 := (*T).Set(); f3(&t,1)
```

表达式T.Get()和(*T).Set被称为方法表达式

方法表达式可以看作函数名，只不过函数的首个参数需要是接收者的实例或者指针

T.Get的函数签名是func(t T) int

(*T).Set的函数签名是func(t *T, i int)

如果写反了在方法表达式中编译器不会自动转换

#### 方法集

命名类型方法接收者有两种类型，一种是值类型，另一种是指针类型

无论接收者是什么类型，方法和函数的实参传递都是值拷贝，传的是副本

```go
package main

import "fmt"

type Int int

func (a Int) Max(b Int) Int{
    if a >= b{
        return a
    }else{
        return b
    }
}

func (i *Int) Set(a Int){
    *i = a
}

func (i Int) Print(){
    fmt.Printf("value=%d\n",i)
}

func main(){
    var a Int = 10
    var b Int = 20
    
    c := a.Max(b)
    c.Print()
    (&c).Print()
    
    a.Set(20)
    a.Print()
    
    (&a).Set(30)
    a.Print()
}
```

定义了新类型Int,其底层类型是int

Int虽然不能继承int的方法，但底层类型支持的操作（算术运算和赋值运算）可以被上层类型继承，这是Go的一大特点

上述例子：

接收者是Int类型的方法集合:

```go
func (i Int) Print()
func (a Int) Max(b Int) Int
```

接收者是*Int类型的方法集合：

```go
func (i *Int) Set(a Int)
```

将接收者的值类型T的方法的集合记录为S，将接收者为指针类型**T*的方法的集合统称为*S：

1.T类型的方法集是S

2.**T*类型的方法集是S和*S

注意：示例中，无论值类型变量还是指针类型变量，都可以调用类型的所有方法，那是因为编译器在编译期间能够识别出这种调用关系，做了自动的转换

### 组合 Go的“继承”

#### 组合

命名结构类型可以嵌套其他的命名类型的字段，外层的结构类型是可以调用嵌入字段类型的方法，这种调用既可以是显式的调用，也可以是隐式的调用。

Go语言没有继承的语义，所以我在继承上打了双引号

结构和字段之间是"has a"的关系，而不是"is a"的关系

没有父子的概念，仅仅是整体和局部的概念

称这种嵌套的结构和字段的关系为**组合**

#### 内嵌字段

由于struct可以嵌套其他struct字段，所以组合也可以分层次扩展

struct类型中的字段称为“内嵌字段”

##### 内嵌字段的初始化和访问

struct的字段访问使用点操作符"."，struct的字段可以嵌套很多层，只要内嵌的字段是唯一的即可

```go
package main

type X struct{
    a int
}

type Y struct{
    X
    b int
}

type Z struct{
    Y
    c int
}

func main(){
    x := X{
        a : 1,
    }
    
    y := Y{
        X : x,
        b : 2,
    }
    
    z := Z{
        Y : y,
        c : 3,
    }
    
    //z.a z.Y.a z.Y.X.a 三者等价  z.a z.Y.a是z.Y.X.a的简写
    println(z.a, z.Y.a, z.Y.X.a)  //1 1 1
    
    z = Z{}
    z.a = 2
    println(z.a, z.Y.a, z.Y.X.z)  //2 2 2
}
```

##### 内嵌字段的方法调用

也使用点操作符

不同嵌套层次的字段可以有相同的方法

外层变量调用内嵌字段方法时也可以像嵌套字段访问一样使用简化模式

如果外层字段和内层字段有相同方法，则使用简化模式访问外层的方法会覆盖内层的方法（Go编译器优先从外向内逐层查找方法，同名方法中外层的方法能够覆盖内层的方法

```go
package main

import "fmt"

type X struct{
    a int
}

type Y struct{
    X
    b int
}

type Z struct{
    Y
    c int
}

func (x X) Print(){
    fmt.Printf("In X, a=%d\n",x.a)
}

func (x X) XPrint(){
    fmt.Printf("In X, a=%d\n",x.a)
}

func (y Y) Print(){
    fmt.Printf("In Y, b=%d\n",y.b)
}

func (z Z) Print(){
    fmt.Printf("In Z, c=%d\n",z.c)
    
    //显式的完全路径调用内嵌字段的方法
    z.Y.Print()
    z.Y.X.Print()
}

func main(){
    x := X{a : 1}
    
    y := Y{
        X : x,
        b : 2,
    }
    
    z := Z{
        Y : y,
        c : 3,
    }
    
    //从外向内查找，首先查找的是Z的Print()方法
    z.Print()
    
    //最后找到的是X的XPrint()方法
    z.XPrint()
    z.Y.XPrint()
}
```

不推荐内嵌多个同名的字段

但是不反对struct定义和内嵌字段同名方法的用法，因为这提供了一种编程技术，使得struct能够重写内嵌字段的方法，提供面向对象编程中子类覆盖父类同名方法的功能

#### 组合的方法集

与前文所说的方法集道理类似

```go
package main

type X struct{
    a int
}

type Y struct{
    X
}

type Z struct{
    *X
}

func (x X) Get() int {
    return x.a
}

func (x *X) Set(i int){
    x.a = i
}

func main(){
    x := X{a : 1}
    
    y := Y{
        X : x,
    }
    
    Println(y.Get())  //1
    
    //此处编译器做了自动转换
    y.Set(2)
    Println(y.Get()) //2
    
    //为了不让编译器做自动转换，使用方法表达式调用方式
    //Y内嵌字段X,所以type Y的方法集是Get,type *Y的方法集是Set和Get 
    (*Y).Set(&y,3)
    
    //type Y的方法集合并没有Set方法
    //Y.Set(y,3)
    
    Println(y.Get())
    
    z := Z{
        X : &x,
    }
    
    //按照嵌套字段的方法集规则
    //Z 内嵌字段*X,所以type Z和type *Z方法集都包含类型X定义的方法Get和Set
    
    //为了不让编译器做自动转换，仍然使用方法表达式调用方式
    Z.Set(z, 4)
    Println(z.Get()) //4
    
    (*Z).Set(&z,5)
    Println(z.Get())  //5
}
```

值得注意的是，编译器的自动转换仅适用于直接通过类型实例调用方法时才有效，类型实例传递给接口时，编译器不会进行自动转换，而是会进行严格的方法集校验

## 接口

接口是一个编程规约，也是一组方法签名的集合

Go的接口是非侵入式的设计，也就是说，一个具体类型实现接口不需要在语法上显式地声明，只要具体类型的方法集是接口方法的超集，就代表实现了接口，编译器在编译时会进行方法集的校验

接口是没有具体实现逻辑的，也不能定义字段



接口变量只有值和实例的概念，所以接口类型变量仍然称为接口变量，接口内部存放的具体类型变量被称为接口指向的“实例”



接口只有声明没有实现

### 空接口

空接口：

```go
interface{}
```

由于空接口的方法集为空，所以任意类型都被认为实现了空接口，任意类型的实例都可以赋值或传递给空接口，包括非命名类型的实例

空接口是反射实现的基础，反射库就是将相关具体类型转换并赋值给空接口后才去处理

### 接口声明

常用使用接口命名类型type关键字声明

```go
type InterfaceName interface{
    MethodSignature1
    MethodSignature2
}
```

使用接口字面量类型的声明很少

```go
interface{
   MethodSignature1
   MethodSignature2
}
```

接口支持嵌入匿名接口字段，即一个接口定义里面可以包括其他接口，编译器会自动进行展开处理

```go
type Reader interface{
    Read(p []byte) (n int, err error)
}

type Writer interface{
    Write(p []byte) (n int,err error)
}

//以下三种声明均等价，最终展开模式都是第三种格式
type ReadWriter interface{
    Reader
    Writer
}

type ReadWriter interface{
    Reader
    Writer(p []byte) (n int,err error)
}

type ReadWriter interface{
    Reader(p []byte) (n int, err error)
    Writer(p []byte) (n int,err error)
}
```

#### 方法声明

```go
//方法声明 = 方法名 + 方法签名

MethodName (InputTypeList)OutputTypeList
```

声明新接口类型的特点

1.接口命名一般以"er"结尾

2.接口定义的内部方法声明不需要func引导

3.在接口定义中，只有方法声明没有方法实现

### 接口方法调用

直接调用未初始化的接口变量方法会产生panic

```go
package main

type Printer interface{
    Print()
}

type S struct{}

func (s S) Print(){
    println("print")
}

func main(){
    var i Printer
    
    //没有初始化的接口调用其方法会产生panic
    // i.Print()
    
    //需要初始化
    i = S{}
    i.Print()
}
```

### 接口的动态类型和静态类型

动态类型：接口绑定的具体实例的类型

静态类型：接口被定义时，其类型就是静态类型

接口的动态类型随绑定的不同类型实例而发生变化

静态类型的本质特征就是接口的方法签名集合

### 类型断言

语法形式如下

```go
i.(TypeName)  // i必须是接口变量，TypeName可以是接口类型名，也可以是具体类型名
```

1.如果TypeName是一个具体类型名，则判断i绑定的实例类型是否就是具体类型TypeName

2.如果TypeName是一个接口类型名，则判断i绑定的实例类型是否同时实现了TypeName接口

接口断言还有直接赋值模式，如下：

```go
o := i.(TypeName)
```

一个例子：

```go
package main

import "fmt"

type Inter interface{
    Ping()
    Pong()
}

type Anter interface{
    Inter
    String()
}

type St struct{
    Name string
}

func (St) Ping(){
    println("ping")
}

func (*St) Pang(){
    println("pang")
}

func main(){
    st := &St{"andes"}
    var i interface{} = st
    
    //判断i绑定的实例是否实现了接口类型Inter
    o := i.(Inter)
    o.Ping()
    o.Pang()
    
    //i没有实现接口Anter
    //p := i.(Anter)
    //p.String()
    
    //判断i绑定的实例是否就是具体类型St
    s := i.(*St)
    fmt.Printf("%s",s.Name)
    
    if o,of := i.(TypeName) ; ok{
        
    }
}
```

### 类型查询

接口类型查询语法格式如下：

```go
switch v := i.(type){ 
    case type1:
    xxxx
    case type2:
    xxxx
    default:
    xxxx
}
```

### 接口的优点

1.解耦：复杂系统进行垂直和水平的分割是常用的设计手段，层与层之间使用接口进行抽象和解耦是一种好的编程策略

2.实现泛型：使用空接口作为函数或方法参数能够使用在需要泛型的场景中

### 接口使用场景

主要使用在：

1.作为结构内嵌字段

2.作为函数或方法的形参

3.作为函数或方法的返回值

4.作为其他接口定义的嵌入字段

### 接口的内部实现

非空接口的数据结构是iface,代码位于src/runtime/runtime2.go

```go
type iface struct{
    tab *itab  //itab存放类型及方法指针信息
    data unsafe.Pointer  //数据信息
}
```

```go
type itab struct{
    inter *interfacetype //接口自身的静态类型
    _type *_type //_type就是接口存放的具体实例的类型（动态类型）
    //hash存放具体类型的Hash值
    hash uint32
    _ [4]byte
    fun [1]uintptr
}
```

空接口的数据结构是eface

```go
type eface struct{
    _type *_type
    data unsafe.Pointer
}
```

## 并发

现有的软件对并发支持不是很好，Go语言就是在这个背景下诞生的

### 并发和并行

并行：程序在任意时刻都是同时运行的，是在任一粒度的时间内具备同时执行的能力，最简单的并行就是多机，但由于是共享内存，以及线程间的同步，不可能完全做到并行

并发：程序在单位时间内是同时运行的，在规定的时间内多个请求都得到执行和处理，强调的是给外界的感觉，实际上内部可能还是分时操作的

并行是硬件和操作系统开发者重点考虑的问题

在当前的计算机体系下：并行具有瞬时性，并发具有过程性；并发在于结构，并行在于执行

一个应用程序具备好的并发结构，操作系统才能更好地利用硬件并行执行，同时避免阻塞等待，合理地进行调度，提升CPU利用率

### goroutine

Go语言的并发执行体称为goroutine,英文翻译是例程

通过go+匿名函数启动gorouinte

```go
package main

import(
    "runtime"
    "time"
)

func main(){
    go func(){
        sum := 0
        for i :=0;i < 1000; i++{
            sum += i
        }
        println(sum)
        time.Sleep(1 * time.Second)
    }()
    
    //NumGoroutine可以返回当前程序的goroutine数目
    println("NumGoroutine=", runtime.NumGoroutine())
    
    //main goroutine故意"sleep"5秒，防止提前退出
    time.Sleep(5 * time.Second)
}

//NumGoroutine = 2
```

通过go + 有名函数启动goroutine

```go
package main

import(
    "runtime"
    "time"
)

func sum(){
      sum := 0
      for i :=0;i < 1000; i++{
          sum += i
      }
      println(sum)
      time.Sleep(1 * time.Second)
}
func main(){
    go sum()
    //NumGoroutine可以返回当前程序的goroutine数目
    println("NumGoroutine=", runtime.NumGoroutine())
    
    //main goroutine故意"sleep"5秒，防止提前退出
    time.Sleep(5 * time.Second)
}
```

### goroutine特性

go的执行是非阻塞的，不会等待

go后面的函数的返回值会被忽略

调度器不能保证多个goroutine的执行次序

没有父子goroutine的概念，所有的goroutine是平等地被调度和执行的

Go程序在执行时会单独为main函数创建一个goroutine，遇到其他go关键字时再去创建其他的goroutine

Go没有暴露goroutine id给用户，所以不能在一个goroutine里面显式地操作另一个goroutine，不过runtime包提供了一些函数访问和设置goroutine相关信息

#### 一些关于goroutine的API

##### func GOMAXPROCS(n int) int

用来设置或查询可以并发执行的goroutine数目，当n大于1表示设置GOMAXPROCS值，否则表示查询当前GOMAXPROCS值

##### func Goexit()

结束当前goroutine的运行，Goexit在结束当前goroutine运行之前会调用当前goroutine已经注册的defer。

Goexit并不会产生panic，所以recover返回都为nil

##### func Gosched()

放弃当前调度执行机会，将当前goroutine放到队列中等待下次被调度

### Chan通道

通道

goroutine之间通信和同步的重要组件

“不要通过共享内存来通信，要通过通信来共享内存

Go语言提供一个内置函数make来创建通道

```go
//创建一个无缓冲的通道，通道存放元素类型为datatype
make(chan datatype)

//创建一个有10个缓冲的通道，通道存放元素的类型为datatype
make(chan datatype,10)
```

无缓冲通道：len和cap都是0，可以用于通信，也可以用于两个goroutine的同步

有缓冲通道：len代表没有被读取的元素数，cap代表整个通道的容量

有了通道后，可以使用无缓冲的通道来实现goroutines之间的同步等待：

```go
package main

import(
    "runtime"
)

func main(){
    c := make(chan struct{})
    go func(i chan struct{}){
        sum := 0
        for i := 0; i < 10000; i++{
            sum += i
        }
        println(sum)
        //写通道
        c <- struct{}{}
    }(c)
    
    println("NumGoroutine=",runtime.NumGoroutine())
    //读通道，通过通道进行同步等待
    <-c
}
```

goroutine运行结束后退出，写到缓冲通道中的数据不会消失，它可以缓冲和适配两个goroutine处理速率不一致的情况，缓冲通道和消息队列类似，有削峰和增大吞吐量的功能

```go
package main

import(
    "runtime"
)

func main(){
    c := make(chan struct{})
    ci := make(chan int,100)
    go func(i chan struct{},j chan int){
        for i := 0; i < 10; i++{
            ci <- i
        }
        close(ci)
        //写通道
        c <- struct{}{}
    }(c,ci)
    
    println("NumGoroutine=",runtime.NumGoroutine())
    //读通道c，通过通道进行同步等待
    <-c
    
    //此时ci通道已经关闭，匿名函数启动的goroutine已经退出
    println("NumGoroutine=",runtime.NumGoroutine())
    
    //但通道ci仍可以继续读取
    for v := range ci{
        println(v)
    }
}
```

操作不同状态的chan会引发三种行为

**panic**

1.向已经关闭的通道写数据会panic（最好就是由写入者去关闭通道）

2.重复关闭的通道会panic

**阻塞**

1.向未初始化的通道写数据或读数据都会导致当前goroutine永久阻塞

2.向缓冲区已满的通道写入数据会导致goroutine阻塞

3.通道中没有数据，读取该通道会导致goroutine阻塞

**非阻塞**

1.读取已经关闭的通道不会引发阻塞，而是立即返回通道元素类型的零值，可以使用comma，ok语法判断通道是否关闭

2.向有缓冲且没有满的通道读/写不会引起阻塞

### WaitGroup

sync包提供多个goroutine同步的机制，主要通过WaitGroup实现的

```go
type WaitGroup struct{
    //contains filtered or unexported fields
}

//添加等待信号
func (wg *WaitGroup) Add(delta int)

//释放等待信号
func (wg *WaitGroup) Done()

//等待
func (wg *WaitGroup) Wait()
```

WaitGroup用来等待多个goroutine完成，main goroutine调用Add设置需要等待goroutine的数目，每一个goroutine结束时调用Done()，Wait()被main用来等待所有goroutine完成

看个例子如何用sync.WaitGroup完成多个goroutine之间的协同工作

```go
package main

import(
    "net/http"
    "sync"
)

var wg sync.WaitGroup
var urls = []string{
    "http://www.golang.org/"
    "http://www.google.com/"
    "http://www.qq.com"
}

func main(){
    for _, url := range urls{
        //每一个URL启动一个goroutine,同时给wg加1
        wg.Add(1)
        
        go func(url string){
            //当前goroutine结束后给wg计数减一，wg.Done() = wg.Add(-1)
            defer wg.Done()
            
            //发送HTTP get请求并打印HTTP返回码
            resp, err := http.Get(url)
            if err == nil{
                println(resp.Status)
            }
        }(url)
    }
    //等待所有请求结束
    wg.Wait()
}
```

### Select

selct是类UNIX系统提供的一个多路复用系统API，select关键字用于多路监听多个通道

当监听的通道没有状态是可读或可写的，select为阻塞

只要监听的通道中有一个状态是可读或可写的，则select就不会阻塞，而是进入处理就绪通道的分支流程

如果监听的通道有多个可读或可写的状态，则select随机选取一个处理

```go
package main

func main(){
    ch := make(chan int ,1)
    go func(chan int){
        for{
            select {
                //0or1写入随机
            case ch <- 0:
            case ch <- 1:
            }
        }
    }(ch)
    for i := 0;i < 10; i++{
        println(<-ch)
    }
}
```

### 扇入和扇出

扇入：多路通道聚合到一条通道中处理，Go最简单的扇入就是使用select聚合多条通道服务

扇出：一条通道发散到多条通道中处理

### 通知退出机制

读取已经关闭的通道不会引起阻塞，也不会导致panic，而是立即返回该通道存储类型的零值，关闭select监听的某个通道能使select立即感知这种通知，然后进行相应的处理，这就是所谓的退出通知机制

随机数生成器演示退出通知机制

```go
package main

import (
    "fmt"
    "math/rand"
    "runtime"
)

func GenerateIntA(done chan struct()) chan int{
    ch := make(chan int)
    go func{
        Lable:
        for{
            select {
                case ch <- rand.Int():
                //增加一路监听，对退出通知信号done的监听
                case <- done:
                break Lable
            }
        }
        //收到通知后关闭通道ch
        close(ch)
    }()
    return ch
}

func main(){
    done := make(chan struct{})
    ch := GenerateIntA(done)
    
    fmt.Println(<-ch)
    fmt.Println(<-ch)
    
    //发送通知，告诉生产者停止生产
    close(done)
    
    fmt.Println(<-ch)
    fmt.Println(<-ch)
    
    //此时生产者已经退出
    Println("NumGoroutine=", runtime.NumGoroutine())
}
```

### Goroutine调度模型

**G(Goroutine)**

G是Go运行时对goroutine的抽象描述，G中存放并发执行的代码入口地址、上下文、运行环境（关联的P和M）、运行栈等执行相关的元信息

G的新建、休眠、恢复、停止都受到go运行时的管理

**M(Machine)**

M代表OS内核线程，是操作系统层面调度和执行的实体。

M仅负责执行，M不停地被唤醒和创建，然后执行

**P(Processor)**

P代表M运行G所需要的资源，是对资源的一种抽象和管理，P不是一段代码实体，而是一个管理的数据结构

P主要是降低M管理调度G的复杂性，增加一个间接的控制层数据结构

## 反射

本章不会详细说明反射（以后再更。。）

反射就是程序能够在运行时动态地查看自己的状态，并且允许修改自身的行为

Go语言反射的基础是接口和类型系统，是编译器和运行时把类型信息以合适的数据结构保存在可执行程序中

### 反射API

#### 从实例到Value

```go
func ValueOf(i interface{}) Value
```

#### 从实例到Type

```go
func TypeOf(i interface{}) Type
```

#### 从Type到Value

```go
func New(typ Type) Value
```

```go
func Zero(typ Type) Value
```

如果知道类型值的底层存放地址

```go
func NewAt(typ Type, p unsafe.Pointer) Value
```

#### 从Value到Type

```go
func (v Value) Type() Type
```

#### 从Value到实例

```go
func (v Value) interface() (i interface{})
```

#### 从Value的指针到值

```go
func (v Value) Elem() Value
```

```go
//不会引起panic
func Indirect(v Value) Value
```

#### Type指针和值的相互转换

1.指针类型Type到值类型Type

```go
t.Elem() Type
```

2.值类型Type到指针类型Type

```go
func PtrTo(t Type) Type
```

#### Value值的可修改性

涉及以下两种方法

```go
//通过CanSet判断能否修改
func (v Value) CanSet() bool
```

```go
//通过Set进行修改
func (v Value) Set(x Value)
```

### 反射三定律

1.反射可以从接口值得到反射对象

2.反射可以从反射对象获得接口值

3.若要修改一个反射对象，则其值必须可以修改
