### 主要特征
1. 自动立即回收
2. 更丰富的内置类型
3. 函数多返回值
4. 错误处理
5. 匿名函数和闭包
6. 类型和接口
7. 并发编程
8. 反射
9. 语言交互性

### 命名规则
1. 首字符可以是任意的Unicode字符或者下划线
2. 剩余字符可以是Unicode字符、下划线、数字
3. 字符长度不限

### 25个关键字
    break        default      func         interface    select
    case         defer        go           map          struct
    chan         else         goto         package      switch
    const        fallthrough     if           range        type
    continue     for          import       return       var

### 37个保留字
    Constants:    true  false  iota  nil

    Types:    int  int8  int16  int32  int64
              uint  uint8  uint16  uint32  uint64  uintptr
              float32  float64  complex128  complex64
              bool  byte  rune  string  error

    Functions:   make  len  cap  new  append  copy  close  delete
                 complex  real  imag
                 panic  recover


### 可见性
1. 声明在函数内部，是函数的本地值，类似private
2. 声明在函数外部，是对当前包可见(包内所有.go文件都可见)的全局值，类似protect
3. 声明在函数外部且首字母大写是所有包可见的全局值,类似public

### 声明方式
    var（声明变量）, const（声明常量）, type（声明类型） ,func（声明函数）

Go的程序是保存在多个.go文件中，文件的第一行就是package XXX声明，用来说明该文件属于哪个包(package)，package声明下来就是import声明，再下来是类型，变量，常量，函数的声明

### 主要目录
    src：源代码文件
    pkg：包文件
    bin：相关bin文件


1: 建立工程文件夹 goproject

2: 在工程文件夹中建立src,pkg,bin文件夹

3: 在GOPATH中添加projiect路径 例 e:/goproject

4: 如工程中有自己的包examplepackage，那在src文件夹下建立以包名命名的文件夹 例 examplepackage

5：在src文件夹下编写主程序代码代码 goproject.go

6：在examplepackage文件夹中编写 examplepackage.go 和 包测试文件 examplepackage_test.go

7：编译调试包

go build examplepackage

go test examplepackage

go install examplepackage

这时在pkg文件夹中可以发现会有一个相应的操作系统文件夹如windows_386z, 在这个文件夹中会有examplepackage文件夹，在该文件中有examplepackage.a文件

8：编译主程序

go build goproject.go

成功后会生成goproject.exe文件

至此一个Go工程编辑成功。

```
1.建立工程文件夹 go
$ pwd
/Users/***/Desktop/go
2: 在工程文件夹中建立src,pkg,bin文件夹
$ ls
bin        conf    pkg        src
3: 在GOPATH中添加projiect路径
$ go env
GOPATH="/Users/liupengjie/Desktop/go"
4: 那在src文件夹下建立以自己的包 example 文件夹
$ cd src/
$ mkdir example
5：在src文件夹下编写主程序代码代码 goproject.go
6：在example文件夹中编写 example.go 和 包测试文件 example_test.go
    example.go 写入如下代码：

    package example

    func add(a, b int) int {
        return a + b
    }

    func sub(a, b int) int {
        return a - b
    }

    example_test.go 写入如下代码：

    package example

    import (
        "testing"
    )

    func TestAdd(t *testing.T) {
        r := add(2, 4)
        if r != 6 {
            t.Fatalf("add(2, 4) error, expect:%d, actual:%d", 6, r)
        }
        t.Logf("test add succ")
    }

7：编译调试包
    $ go build example
    $ go test example
    ok      example    0.013s
    $ go install example

$ ls /Users/***/Desktop/go/pkg/
darwin_amd64
$ ls /Users/***/Desktop/go/pkg/darwin_amd64/
example.a
8：编译主程序
    oproject.go 写入如下代码：
    package main

    import (
        "fmt"
    )

    func main(){
        fmt.Println("go project test")
    }

    $ go build goproject.go
    $ ls
    example        goproject.go    goproject

       成功后会生成goproject文件
    至此一个Go工程编辑成功。

       运行该文件：
    $ ./goproject
    go project test
```

### 编译问题
    1.系统编译时 go install abc_name时，系统会到GOPATH的src目录中寻找abc_name目录，然后编译其下的go文件；

    2.同一个目录中所有的go文件的package声明必须相同，所以main方法要单独放一个文件，否则在eclipse和liteide中都会报错；
    编译报错如下：（假设test目录中有个main.go 和mymath.go,其中main.go声明package为main，mymath.go声明packag 为test);

        $ go install test

        can't load package: package test: found packages main (main.go) and test (mymath.go) in /home/wanjm/go/src/test

        报错说 不能加载package test（这是命令行的参数），因为发现了两个package，分别时main.go 和 mymath.go;

    3.对于main方法，只能在bin目录下运行 go build path_tomain.go; 可以用-o参数指出输出文件名；

    4.可以添加参数 go build -gcflags "-N -l" ****,可以更好的便于gdb；详细参见 http://golang.org/doc/gdb

    5.gdb全局变量主一点。 如有全局变量 a；则应写为 p 'main.a'；注意但引号不可少；
    赏

## 内置类型和函数
### 值类型
    bool
    int(32 or 64), int8, int16, int32, int64
    uint(32 or 64), uint8(byte), uint16, uint32, uint64
    float32, float64
    string
    complex64, complex128
    array    -- 固定长度的数组

### 引用类型
    slice   -- 序列数组(最常用)
    map     -- 映射
    chan    -- 管道

## 内置函数
Go 语言拥有一些不需要进行导入操作就可以使用的内置函数。它们有时可以针对不同的类型进行操作，例如：len、cap 和 append，或必须用于系统级的操作，例如：panic。因此，它们需要直接获得编译器的支持。

    append          -- 用来追加元素到数组、slice中,返回修改后的数组、slice
    close           -- 主要用来关闭channel
    delete            -- 从map中删除key对应的value
    panic            -- 停止常规的goroutine  （panic和recover：用来做错误处理）
    recover         -- 允许程序定义goroutine的panic动作
    real            -- 返回complex的实部   （complex、real imag：用于创建和操作复数）
    imag            -- 返回complex的虚部
    make            -- 用来分配内存，返回Type本身(只能应用于slice, map, channel)
    new                -- 用来分配内存，主要用来分配值类型，比如int、struct。返回指向Type的指针
    cap                -- capacity是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）
    copy            -- 用于复制和连接slice，返回复制的数目
    len                -- 来求长度，比如string、array、slice、map、channel ，返回长度
    print、println     -- 底层打印函数，在部署环境中建议使用 fmt 包

### 内置接口error
    type error interface { //只要实现了Error()函数，返回值为String的都实现了err接口

            Error()    String

    }

### init函数

go语言中init函数用于包(package)的初始化，该函数是go语言的一个重要特性

    1 init函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等

    2 每个包可以拥有多个init函数

    3 包的每个源文件也可以拥有多个init函数

    4 同一个包中多个init函数的执行顺序go语言没有明确的定义(说明)

    5 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序

    6 init函数不能被其他函数调用，而是在main函数执行之前，自动被调用

### main 函数
    Go语言程序的默认入口函数(主函数)：func main()
    函数体用｛｝一对括号包裹。

    func main(){
        //函数体
    }

### init函数和main函数的异同
    相同点：
        两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用。
    不同点：
        init可以应用于任意包中，且可以重复定义多个。
        main函数只能用于main包中，且只能定义一个。

### 执行顺序
    对同一个go文件的init()调用顺序是从上到下的。

    对同一个package中不同文件是按文件名字符串比较“从小到大”顺序调用各文件中的init()函数。

    对于不同的package，如果不相互依赖的话，按照main包中"先import的后调用"的顺序调用其包中的init()，如果package存在依赖，则先调用最早被依赖的package中的init()，最后调用main函数。

    如果init函数中使用了println()或者print()你会发现在执行过程中这两个不会按照你想象中的顺序执行。这两个函数官方只推荐在测试环境中使用，对于正式环境不要使用。

### 命令
    go env用于打印Go语言的环境信息。

    go run命令可以编译并运行命令源码文件。

    go get可以根据要求和实际情况从互联网上下载或更新指定的代码包及其依赖包，并对它们进行编译和安装。

    go build命令用于编译我们指定的源码文件或代码包以及它们的依赖包。

    go install用于编译并安装指定的代码包及它们的依赖包。

    go clean命令会删除掉执行其它命令时产生的一些文件和目录。

    go doc命令可以打印附于Go语言程序实体上的文档。我们可以通过把程序实体的标识符作为该命令的参数来达到查看其文档的目的。

    go test命令用于对Go语言编写的程序进行测试。

    go list命令的作用是列出指定的代码包的信息。

    go fix会把指定代码包的所有Go语言源码文件中的旧版本代码修正为新版本的代码。

    go vet是一个用于检查Go语言源码中静态错误的简单工具。

    go tool pprof命令来交互式的访问概要文件的内容。


### 运算符
#### Go语言内置运算符
    算术运算符
    关系运算符
    逻辑运算符
    位运算符
    赋值运算符

### 下划线
在Golang里，import的作用是导入其他package。






## 数组Array
    1. 数组：是同一种数据类型的固定长度的序列。
    2. 数组定义：var a [len]int，比如：var a [5]int，数组长度必须是常量，且是类型的组成部分。一旦定义，长度不能变。
    3. 长度是数组类型的一部分，因此，var a[5] int和var a[10]int是不同的类型。
    4. 数组可以通过下标进行访问，下标是从0开始，最后一个元素下标是：len-1
    for i := 0; i < len(a); i++ {
    }
    for index, v := range a {
    }
    5. 访问越界，如果下标在数组合法范围之外，则触发访问越界，会panic
    6. 数组是值类型，赋值和传参会复制整个数组，而不是指针。因此改变副本的值，不会改变本身的值。
    7.支持 "=="、"!=" 操作符，因为内存总是被初始化过的。
    8.指针数组 [n]*T，数组指针 *[n]T。

### 数组初始化
#### 一维数组：
    全局：
    var arr0 [5]int = [5]int{1, 2, 3}
    var arr1 = [5]int{1, 2, 3, 4, 5}
    var arr2 = [...]int{1, 2, 3, 4, 5, 6}
    var str = [5]string{3: "hello world", 4: "tom"}
    局部：
    a := [3]int{1, 2}           // 未初始化元素值为 0。
    b := [...]int{1, 2, 3, 4}   // 通过初始化值确定数组长度。
    c := [5]int{2: 100, 4: 200} // 使用索引号初始化元素。
    d := [...]struct {
        name string
        age  uint8
    }{
        {"user1", 10}, // 可省略元素类型。
        {"user2", 20}, // 别忘了最后一行的逗号。
    }

代码
``` go
package main

import (
    "fmt"
)

var arr0 [5]int = [5]int{1, 2, 3}
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 4, 5, 6}
var str = [5]string{3: "hello world", 4: "tom"}

func main() {
    a := [3]int{1, 2}           // 未初始化元素值为 0。
    b := [...]int{1, 2, 3, 4}   // 通过初始化值确定数组长度。
    c := [5]int{2: 100, 4: 200} // 使用引号初始化元素。
    d := [...]struct {
        name string
        age  uint8
    }{
        {"user1", 10}, // 可省略元素类型。
        {"user2", 20}, // 别忘了最后一行的逗号。
    }
    fmt.Println(arr0, arr1, arr2, str)
    fmt.Println(a, b, c, d)
}
```

结果：
[1 2 3 0 0] [1 2 3 4 5] [1 2 3 4 5 6] [   hello world tom]
[1 2 0] [1 2 3 4] [0 0 100 0 200] [{user1 10} {user2 20}]


#### 多维数组
    全局：
    var arr0 [5][3]int
    var arr1 [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}
    局部：
    a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    b := [...][2]int{{1, 1}, {2, 2}, {3, 3}} // 第 2 纬度不能用 "..."。
代码：
``` go
package main

import (
    "fmt"
)

var arr0 [5][3]int
var arr1 [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

func main() {
    a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    b := [...][2]int{{1, 1}, {2, 2}, {3, 3}} // 第 2 纬度不能用 "..."。
    fmt.Println(arr0, arr1)
    fmt.Println(a, b)
}
```

结果：
    [[0 0 0] [0 0 0] [0 0 0] [0 0 0] [0 0 0]] [[1 2 3] [7 8 9]]
    [[1 2 3] [4 5 6]] [[1 1] [2 2] [3 3]]


**值拷贝行为会造成性能问题，通常会建议使用 slice，或数组指针**
代码：
``` go
package main

import (
    "fmt"
)

func test(x [2]int) {
    fmt.Printf("x: %p\n", &x)
    x[1] = 1000
}

func main() {
    a := [2]int{}
    fmt.Printf("a: %p\n", &a)

    test(a)
    fmt.Println(a)
}
```

输出结果：
    a: 0xc42007c010
    x: 0xc42007c030
    [0 0]


内置函数len和cap均用于返回数组长度（元素数量）
代码：
``` go
package main

func main() {
    a := [2]int{}
    println(len(a), cap(a))
}
```
结果：
2 2

多维数组遍历：
代码：
``` go
package main

import (
    "fmt"
)

func main() {

    var f [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

    for k1, v1 := range f {
        for k2, v2 := range v1 {
            fmt.Printf("(%d,%d)=%d ", k1, k2, v2)
        }
        fmt.Println()
    }
}
```
输出结果：
    (0,0)=1 (0,1)=2 (0,2)=3
    (1,0)=7 (1,1)=8 (1,2)=9


#### 数组的拷贝和传参
代码：
package main
``` go
import "fmt"

func printArr(arr *[5]int) {
    arr[0] = 10
    for i, v := range arr {
        fmt.Println(i, v)
    }
}

func main() {
    var arr1 [5]int
    printArr(&arr1)
    fmt.Println(arr1)
    arr2 := [...]int{2, 4, 6, 8, 10}
    printArr(&arr2)
    fmt.Println(arr2)
}
```
### 练习
1.求数组所有元素之和 array/tset_sum.go
2.找出数组中和为给定值的两个元素的下标，例如数组[1,3,5,8,7]，找出两个元素之和等于8的下标分别是（0，4）和（1，2） srray/test_two_sum.go



### 切片 Slice

slice 并不是数组或数组指针。它通过内部指针和相关属性引用数组片段，以实现变长方案。
        1. 切片：切片是数组的一个引用，因此切片是引用类型。但自身是结构体，值拷贝传递。
        2. 切片的长度可以改变，因此，切片是一个可变的数组。
        3. 切片遍历方式和数组一样，可以用len()求长度。表示可用元素数量，读写操作不能超过该限制。
        4. cap可以求出slice最大扩张容量，不能超出数组限制。0 <= len(slice) <= len(array)，其中array是slice引用的数组。
        5. 切片的定义：var 变量名 []类型，比如 var str []string  var arr []int。
        6. 如果 slice == nil，那么 len、cap 结果都等于 0。

#### 创建切片
``` go
package main

import "fmt"

func main() {
   //1.声明切片
   var s1 []int
   if s1 == nil {
      fmt.Println("是空")
   } else {
      fmt.Println("不是空")
   }
   // 2.:=
   s2 := []int{}
   // 3.make()
   var s3 []int = make([]int, 0)
   fmt.Println(s1, s2, s3)
   // 4.初始化赋值
   var s4 []int = make([]int, 0, 0)
   fmt.Println(s4)
   s5 := []int{1, 2, 3}
   fmt.Println(s5)
   // 5.从数组切片
   arr := [5]int{1, 2, 3, 4, 5}
   var s6 []int
   // 前包后不包
   s6 = arr[1:4]
   fmt.Println(s6)
}
```
#### 切片初始化
``` go
    全局：
    var arr = [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    var slice0 []int = arr[start:end]
    var slice1 []int = arr[:end]
    var slice2 []int = arr[start:]
    var slice3 []int = arr[:]
    var slice4 = arr[:len(arr)-1]      //去掉切片的最后一个元素
    局部：
    arr2 := [...]int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    slice5 := arr[start:end]
    slice6 := arr[:end]
    slice7 := arr[start:]
    slice8 := arr[:]
    slice9 := arr[:len(arr)-1] //去掉切片的最后一个元素
```
``` go
package main

import (
    "fmt"
)

var arr = [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
var slice0 []int = arr[2:8]
var slice1 []int = arr[0:6]        //可以简写为 var slice []int = arr[:end]
var slice2 []int = arr[5:10]       //可以简写为 var slice[]int = arr[start:]
var slice3 []int = arr[0:len(arr)] //var slice []int = arr[:]
var slice4 = arr[:len(arr)-1]      //去掉切片的最后一个元素
func main() {
    fmt.Printf("全局变量：arr %v\n", arr)
    fmt.Printf("全局变量：slice0 %v\n", slice0)
    fmt.Printf("全局变量：slice1 %v\n", slice1)
    fmt.Printf("全局变量：slice2 %v\n", slice2)
    fmt.Printf("全局变量：slice3 %v\n", slice3)
    fmt.Printf("全局变量：slice4 %v\n", slice4)
    fmt.Printf("-----------------------------------\n")
    arr2 := [...]int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    slice5 := arr[2:8]
    slice6 := arr[0:6]         //可以简写为 slice := arr[:end]
    slice7 := arr[5:10]        //可以简写为 slice := arr[start:]
    slice8 := arr[0:len(arr)]  //slice := arr[:]
    slice9 := arr[:len(arr)-1] //去掉切片的最后一个元素
    fmt.Printf("局部变量： arr2 %v\n", arr2)
    fmt.Printf("局部变量： slice5 %v\n", slice5)
    fmt.Printf("局部变量： slice6 %v\n", slice6)
    fmt.Printf("局部变量： slice7 %v\n", slice7)
    fmt.Printf("局部变量： slice8 %v\n", slice8)
    fmt.Printf("局部变量： slice9 %v\n", slice9)
}
```
输出结果：
```
    全局变量：arr [0 1 2 3 4 5 6 7 8 9]
    全局变量：slice0 [2 3 4 5 6 7]
    全局变量：slice1 [0 1 2 3 4 5]
    全局变量：slice2 [5 6 7 8 9]
    全局变量：slice3 [0 1 2 3 4 5 6 7 8 9]
    全局变量：slice4 [0 1 2 3 4 5 6 7 8]
    -----------------------------------
    局部变量： arr2 [9 8 7 6 5 4 3 2 1 0]
    局部变量： slice5 [2 3 4 5 6 7]
    局部变量： slice6 [0 1 2 3 4 5]
    局部变量： slice7 [5 6 7 8 9]
    局部变量： slice8 [0 1 2 3 4 5 6 7 8 9]
    局部变量： slice9 [0 1 2 3 4 5 6 7 8]
```

通过make创建切片：
``` go
    var slice []type = make([]type, len)
    slice  := make([]type, len)
    slice  := make([]type, len, cap)
```
``` go
package main

import (
    "fmt"
)

var slice0 []int = make([]int, 10)
var slice1 = make([]int, 10)
var slice2 = make([]int, 10, 10)

func main() {
    fmt.Printf("make全局slice0 ：%v\n", slice0)
    fmt.Printf("make全局slice1 ：%v\n", slice1)
    fmt.Printf("make全局slice2 ：%v\n", slice2)
    fmt.Println("--------------------------------------")
    slice3 := make([]int, 10)
    slice4 := make([]int, 10)
    slice5 := make([]int, 10, 10)
    fmt.Printf("make局部slice3 ：%v\n", slice3)
    fmt.Printf("make局部slice4 ：%v\n", slice4)
    fmt.Printf("make局部slice5 ：%v\n", slice5)
}
```
输出结果：
```
    make全局slice0 ：[0 0 0 0 0 0 0 0 0 0]
    make全局slice1 ：[0 0 0 0 0 0 0 0 0 0]
    make全局slice2 ：[0 0 0 0 0 0 0 0 0 0]
    --------------------------------------
    make局部slice3 ：[0 0 0 0 0 0 0 0 0 0]
    make局部slice4 ：[0 0 0 0 0 0 0 0 0 0]
    make局部slice5 ：[0 0 0 0 0 0 0 0 0 0]
```
读写操作实际目标是底层数组，只需注意索引号的差别。
``` go
package main

import (
    "fmt"
)

func main() {
    data := [...]int{0, 1, 2, 3, 4, 5}

    s := data[2:4]
    s[0] += 100
    s[1] += 200

    fmt.Println(s)         //    [102 203]
    fmt.Println(data)       //    [0 1 102 203 4 5]
}
```
可直接创建 slice 对象，自动分配底层数组。
``` go
package main

import "fmt"

func main() {
    s1 := []int{0, 1, 2, 3, 8: 100} // 通过初始化表达式构造，可使用索引号。
    fmt.Println(s1, len(s1), cap(s1))       //[0 1 2 3 0 0 0 0 100] 9 9

    s2 := make([]int, 6, 8) // 使用 make 创建，指定 len 和 cap 值。
    fmt.Println(s2, len(s2), cap(s2))       //[0 0 0 0 0 0] 6 8

    s3 := make([]int, 6) // 省略 cap，相当于 cap = len。
    fmt.Println(s3, len(s3), cap(s3))       //[0 0 0 0 0 0] 6 6
}
```


## IO操作
### 输入输出底层原理
终端其实是一个文件，相关实例如下：
    os.Stdin：标准输入的文件实例，类型为*File
    os.Stdout：标准输出的文件实例，类型为*File
    os.Stderr：标准错误输出的文件实例，类型为*File

``` go
package main

import "os"

func main() {
    var buf [16]byte
    os.Stdin.Read(buf[:])
    os.Stdin.WriteString(string(buf[:]))
}
```
### 文件操作相关api
``` go
func Create(name string) (file *File, err Error)
根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666
func NewFile(fd uintptr, name string) *File
根据文件描述符创建相应的文件，返回一个文件对象
func Open(name string) (file *File, err Error)
只读方式打开一个名称为name的文件
func OpenFile(name string, flag int, perm uint32) (file *File, err Error)
打开名称为name的文件，flag是打开的方式，只读、读写等，perm是权限
func (file *File) Write(b []byte) (n int, err Error)
写入byte类型的信息到文件
func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
在指定位置开始写入byte类型的信息
func (file *File) WriteString(s string) (ret int, err Error)
写入string信息到文件
func (file *File) Read(b []byte) (n int, err Error)
读取数据到b中
func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
从off开始读取数据到b中
func Remove(name string) Error
删除文件名为name的文件
```
### 打开和关闭文件
os.Open()函数能够打开一个文件，返回一个*File和一个err。对得到的文件实例调用close()方法能够关闭文件。
``` go
package main

import (
    "fmt"
    "os"
)

func main() {
    // 只读方式打开当前目录下的main.go文件
    file, err := os.Open("./main.go")
    if err != nil {
        fmt.Println("open file failed!, err:", err)
        return
    }
    // 关闭文件
    file.Close()
}
```
### 写文件
``` go
package main

import (
    "fmt"
    "os"
)

func main() {
    // 新建文件
    file, err := os.Create("./xxx.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer file.Close()
    for i := 0; i < 5; i++ {
        file.WriteString("ab\n")
        file.Write([]byte("cd\n"))
    }
}
```

### 读文件
``` go
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    // 打开文件
    file, err := os.Open("./xxx.txt")
    if err != nil {
        fmt.Println("open file err :", err)
        return
    }
    defer file.Close()
    // 定义接收文件读取的字节数组
    var buf [128]byte
    var content []byte
    for {
        n, err := file.Read(buf[:])
        if err == io.EOF {
            // 读取结束
            break
        }
        if err != nil {
            fmt.Println("read file err ", err)
            return
        }
        content = append(content, buf[:n]...)
    }
    fmt.Println(string(content))
}
```
### 拷贝文件
``` go
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    // 打开源文件
    srcFile, err := os.Open("./xxx.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    // 创建新文件
    dstFile, err2 := os.Create("./abc2.txt")
    if err2 != nil {
        fmt.Println(err2)
        return
    }
    // 缓冲读取
    buf := make([]byte, 1024)
    for {
        // 从源文件读数据
        n, err := srcFile.Read(buf)
        if err == io.EOF {
            fmt.Println("读取完毕")
            break
        }
        if err != nil {
            fmt.Println(err)
            break
        }
        //写出去
        dstFile.Write(buf[:n])
    }
    srcFile.Close()
    dstFile.Close()
}
```
### bufio
* bufio包实现了带缓冲区的读写，是对文件读写的封装
* bufio缓冲写数据

| 模式  | 含义  |
|  ----  | ----  |
| os.O_WRONLY  | 只写 |
| os.O_CREATE  | 创建文件 |
| os.O_RDONLY  | 只读 |
| os.O_RDWR  | 读写 |
| os.O_TRUNC  | 清空 |
| os.O_APPEND  | 追加 |

#### bufio读数据
``` go
package main

import (
    "bufio"
    "fmt"
    "io"
    "os"
)

func wr() {
    // 参数2：打开模式，所有模式d都在上面
    // 参数3是权限控制
    // w写 r读 x执行   w  2   r  4   x  1
    file, err := os.OpenFile("./xxx.txt", os.O_CREATE|os.O_WRONLY, 0666)
    if err != nil {
        return
    }
    defer file.Close()
    // 获取writer对象
    writer := bufio.NewWriter(file)
    for i := 0; i < 10; i++ {
        writer.WriteString("hello\n")
    }
    // 刷新缓冲区，强制写出
    writer.Flush()
}

func re() {
    file, err := os.Open("./xxx.txt")
    if err != nil {
        return
    }
    defer file.Close()
    reader := bufio.NewReader(file)
    for {
        line, _, err := reader.ReadLine()
        if err == io.EOF {
            break
        }
        if err != nil {
            return
        }
        fmt.Println(string(line))
    }

}

func main() {
    re()
}
```
#### ioutil工具包
* 工具包写文件
* 工具包读取文件
``` go
package main

import (
   "fmt"
   "io/ioutil"
)

func wr() {
   err := ioutil.WriteFile("./yyy.txt", []byte("www.5lmh.com"), 0666)
   if err != nil {
      fmt.Println(err)
      return
   }
}

func re() {
   content, err := ioutil.ReadFile("./yyy.txt")
   if err != nil {
      fmt.Println(err)
      return
   }
   fmt.Println(string(content))
}

func main() {
   re()
}
```