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
### 练习 array/tset.go
``` go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

// 求元素和
func sumArr(a [10]int) int {
    var sum int = 0
    for i := 0; i < len(a); i++ {
        sum += a[i]
    }
    return sum
}

func main() {
    // 若想做一个真正的随机数，要种子
    // seed()种子默认是1
    //rand.Seed(1)
    rand.Seed(time.Now().Unix())

    var b [10]int
    for i := 0; i < len(b); i++ {
        // 产生一个0到1000随机数
        b[i] = rand.Intn(1000)
    }
    sum := sumArr(b)
    fmt.Printf("sum=%d\n", sum)
}
```







