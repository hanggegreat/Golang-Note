### 第一个程序：打印Hello World!

Go 语言的基础组成有以下几个部分：

- 包声明
- 引入包
- 函数
- 变量
- 语句 & 表达式
- 注释

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



### 条件语句

首先在当前目录下简历abc.txt文件

#### if else语句

```go
func main() {
    const filename = "abc.txt"
    // 系统函数ioutil.ReadFile()会返回两个值，文件内容和错误信息
    contents, err := ioutil.ReadFile(filename)
    
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Printf("%s\n")
    }
}

// 当abc.txt未创建时，会打印错误信息：open aabc.txt: The system cannot find the file specified.
// 当abc.txt创建后，会打印出文件内容信息

// 上面的条件语句也可修改为如下格式
func main() {
    const filename = "abc.txt"
    if contents, err := ioutil.ReadFile(filename); err != nil {
        fmt.Println(err)
    } else {
        fmt.Printf("%s\n")
    }
}

// 执行结果同上，但是需要注意，此时contents, err只能在条件语句中使用
```

#### switch语句

```go
// 实现一个输入分数，返回对应等级的函数
// 不同于其他语言的switch语句，golang中的case中不需要写break，会自动跳出
func grade(score int) string {
    g := ""

    switch {
    case score < 0 || score > 100:
        panic(fmt.Springf("Wrong score: %d", score)) // panic用于报错，会强行终止程序
    case score < 60:
        g = "F"
    case score < 70:
		g = "D"
	case score < 80:
		g = "C"
	case score < 90:
		g = "B"
	default:
		g = "A"
	}
	
    return g
}
```



### 循环语句

**值得注意的是，golang中只有for语句，没有while语句**

```go
// 实现一个将10进制int类型数据转换为二进制格式字符串的函数
func convertToBin(n int) string {
	if n == 0 {
		return "0"
	}

	res := ""
	for ; n > 0; n >>= 1 {
        // 因为 n&1是 int类型，需要调用strconv.Itoa()函数，强制转换为string类型
		res = strconv.Itoa(n&1) + res
	}
	return res
}

// 读取文件
func printFile(filename string) {
	file, err := os.Open(filename)
	if err != nil {
		panic(err)
	}

	scanner := bufio.NewScanner(file)

    // 省略初始条件
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}
}

// 死循环
func forever() {
	for {
		fmt.Println("abc")
	}
}
```



### 函数

**golang中的函数可以有多个返回值，返回值也可以起名字**

```go
// 带余除法
func divfunc div(a, b int) (q, r int) {
	return a / b, a % b
}

// 运算操作
func eval(a, b int, op string) (int, error) {
	switch op {
	case "+":
		return a + b, nil
	case "-":
		return a - b, nil
	case "*":
		return a * b, nil
	case "/":
		return a / b, nil
	default:
		return 0, fmt.Errorf(
			"unsupported operation: %s", op)
	}
}
```

**golang中函数也可以作为函数的参数**

```go
// 定义一个函数apply，第一个参数为一个函数func，后面两个参数为int类型整数
// reflect.ValueOf(op).Pointer()语句可以获取func函数的指针
// 接着可以通过p获取函数func的其他信息，runtime.FuncForPC(p).Name()获取函数名称
func apply(op func(int, int) int, a, b int) int {
	p := reflect.ValueOf(op).Pointer()
	opName := runtime.FuncForPC(p).Name()
	fmt.Printf("Calling function %s with args (%d, %d)\n", opName, a, b)
	return op(a, b)
}

func pow(a, b int) int {
	return int(math.Pow(float64(a), float64(b)))
}

func main {
    // 传入匿名函数进行apply函数的调用
    fmt.Println(apply(
    func(a, b int) int {
        return int(math.Pow(float64(a), float64(b)))
    }, 3, 4))
    
    // 也可以传入定义过的函数作为参数进行调用
    fmt.Println(apply(pow, 3, 4))
}
```

**golang中支持可变长参数列表**

```go
// 计算所有整形数据的和
func sum(numbers ...int) int {
	res := 0
	for i:= range numbers {
		res += numbers[i]
	}
	return res
}
```



### 指针

**和c语言一样，golang中也有指针的概念，不同的是，golang中的指针要比c语言中的方便很多，因为其不能进行运算**

```go
// 指针的定义和使用
a := 2
var pa *int = &a
*pa = 3
fmt.Println(a)

// 此时打印结果为3，说明确实通过指针将a的值修改了
```

**值得注意的是，在golang中，函数传参都属于值传递，没有引用传递的概念**

```go
// 这种方式显然不能修改实际传入的参数的值
func swap(a, b int) {
    a, b = b, a
}

// 要想修改，则可以通过指针进行操作
func swap(pa, pb *int) {
    *pa, *pb = *pb, *pa
}
```



### 数组

```go
func main() {
    // 数组定义有如下几种方式
	var arr1 [5]int // 默认每个元素都是零值
    arr2 := [3]int{1, 3, 5} // 用 := 声明时，必须要赋值
	arr3 := [...]int{2, 4, 6, 8, 10}// 可以用[...]省略数组长度
	var grid [4][5]int // 二维数组

	fmt.Println(arr1, arr2, arr3, grid)

    // 对数组进行遍历，使用 range arr3，可以得到两个值 i, v 分别表示下标和对应位置元素值
	for i, v := range arr3 {
		fmt.Println(i, v)
	}
}

// 打印结果为：
// [0 0 0 0 0] [1 3 5] [2 4 6 8 10] [[0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0]]
// 0 2
// 1 4
// 2 6
// 3 8
// 4 10
```

**值得一提的是，不同于其他语言，在golang中，数组作为参数时，其传递的也是值，并非引用**

```go
func main() {
	arr := [...]int{1, 2, 3, 4, 5}
	update(arr)

	fmt.Println(arr)
}

func update(arr [5]int) {
	arr[0] = 100
}

// 打印结果为 1， 2， 3， 4， 5 证明update函数并没有修改实参的值，故数组作为参数也是值传递
```

- 在golang中[10]int 和 [20]int是两种不同的类型

- 调用 update(arr [5]int)会拷贝数组
- 在golang中一般不直接使用数组，而是使用切片



### 切片

**golang中切片本身是没有值的，它是对底层数组的一个view，即一个视图**

```go
arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7}
s1 = arr[2:6] // 数组下标[2, 6)的元素的视图，即 {2, 3, 4, 5, 6}
s2 = arr[:6]// 数组下标[0, 6)的元素的视图，即 {2, 3, 4, 5, 6}
s3 = arr[2:]// 数组下标[2, 7)的元素的视图，即 {2, 3, 4, 5, 6, 7}
s4 = arr[:]// 数组的全部元素的一个视图，即 {1, 2, 3, 4, 5, 6, 7}

s2 = s2[:3]// 对切片进行reslice，即 {2, 3, 4}
s2 = s2[1:]// 再进行reslice，即 {3, 4}

// 切片作为函数参数传递时，不会进行拷贝，而是作为底层数组的一个view传入，所以在函数内部修改切片会改变数组元素的值
func main() {
	arr := [...]int{1, 2, 3, 4, 5}
    update(arr[:])

	fmt.Println(arr)
}

func update(arr []int) {
	arr[0] = 100
}
// 打印结果为 100, 2, 3, 4, 5，证明以上说法是对的
```

#### 切片的扩展

![https://img-blog.csdnimg.cn/2019042514391284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg5Njk5OA==,size_16,color_FFFFFF,t_70]()

![https://img-blog.csdnimg.cn/20190425143930357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg5Njk5OA==,size_16,color_FFFFFF,t_70]()

- arr为{0, 1, 2, 3, 4, 5, 6}, s1为{2, 3, 4, 5}, s2为{5, 6}
- slice可以向后扩展，但是不能向前扩展
- s[i]的i不可以超越len(s)，向后扩展不可以超过cap(s)

#### 切片的其他操作

**append() 和 copy() 函数**

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

```go
func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}

/**
	打印结果为
	len=0 cap=0 slice=[]
    len=1 cap=1 slice=[0]
    len=2 cap=2 slice=[0 1]
    len=5 cap=6 slice=[0 1 2 3 4]
    len=5 cap=12 slice=[0 1 2 3 4]
*/
```

### Map

golang中的Map是通过hash表实现的，可以使用内建函数 make 也可以使用 map 关键字来定义 Map，如果不初始化 map，那么就会创建一个 nil map。

```go
func main() {
    var m map[string]string // nil
    m2 := make(map[string]string) // empty map
    m3 := map[string]string {
        "France": "Paris",
        "Italy": "罗马",
        "Japan": "东京",
        "India": "新德里"
    }

    // 使用range进行遍历
    for k, v := range countryCapitalMap {
        fmt.Println(k, "首都是", v)
    }

    // 查看元素在集合中是否存在
    capital, ok := countryCapitalMap[ "美国" ]
    if (ok) {
        fmt.Println("美国的首都是", capital)
    } else {
        fmt.Println("美国的首都不存在")
    }
    
    // 打印结果为“美国的首都不存在”
}
```

- map使用了hash表，所以键值必须可以比较相等
- 除了slice，map，function以外的内建类型都可以作为key
- Struct类型不包含slice，map，function类型字段的话，也可以作为key

