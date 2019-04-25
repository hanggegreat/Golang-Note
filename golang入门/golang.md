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

