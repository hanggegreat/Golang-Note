### 第一个程序：打印Hello World!

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World!")
}
```



### 变量定义

```go
// 1.指定类型的方式
var a int = 6
var b int // golang中变量声明时若未指定变量的值，则为默认值
var s string // 默认为 ""
var x, y, z int = 1, 2, 3 // 指定类型后，多个变量一起声明，其类型都为指定的类型
// 也可以把多个变量放在"()"中一起声明
var (
    aa = 3
    ss = "kkk"
    bb = true
)


// 2.不指定类型的方式
var a = 6
var s = "abc"
var x, y, z = 100, 9.3, "sss"  // 多个变量可以一起声明，并且类型也可不同

// 3.甚至可以不适用var，只需在声明变量的时候使用 ":=" 即可，编译器会自动推断类型（包内变量不能这么声明）
a, b, c, d := 3, 4, true, "abc"
```



### 内建变量类型

- bool、string
- (u)int（长度由操作系统决定）, (u)int8, (u)int16, (u)int32, (u)int64, uintptr（指针类型）
- byte、rune（长度为4字节的字符类型）
- float32、float64、complex64（实部虚部各32位的复数）、complex128（实部虚部各64位的复数）

**golang中只有强制类型转换**

```go
// 求直角三角形斜边长度
func triangle() {
	a, b := 3, 4
    // a*a + b*b 为int类型，math.Sqrt()参数类型为float64，需要通过float64(xxx)进行强制类型转换
    // c类型为int，math.Sqrt()返回值为float64,需要通过int(xxx)进行强制类型转换
	var c int = int(math.Sqrt(float64(a*a + b*b)))
	fmt.Println(c)
}
```



### 常量类型

```go
// golang中常量用const定义，其数值可以作为任意变量使用
// golang中常量不需要用全大写表示
// 有如下几种定义方式
const filename = "abc.txt"

const (
    aa = 3
    ss = "kkk"
    bb = true
)
// 因为a, b为常量，所以可以把它们看作文本，当调用math.Sqrt(a*a + b*b)时，可以视为float类型
const a, b = 3, 4
var c int = int(math.Sqrt(a*a + b*b))
```



### 枚举类型

```go
// 使用常量定义枚举类型
func enum() {
    const (
        cpp = 0
        java = 1
        python = 2
        golang = 3
    )
    
    fmt.Println(cpp, java, python, golang)
}

// 打印结果为 0， 1， 2， 3


// 可以使用自增值iota简化
func enum() {
    // _表示占位符，此处没有变量
    const (
        cpp = iota
        _
        javascript
        python
        golang
    )
    
    fmt.Println(cpp, javascript, python, golang)
}

// 打印结果为 0， 2， 3， 4

// 使用iota打印b, kb, mb, gb, tb, pb大小
func enum() {
    // _表示占位符，此处没有变量
    const (
        b = 1 << (10 * iota)
        kb
        mb
        gb
        tb
        pb
    )
    
    fmt.Println(b, kb, mb, gb, tb, pb)
}

// 打印结果为 1 1024 1048576 1073741824 1099511627776 1125899906842624 1152921504606846976

```

