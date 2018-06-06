#



## 基础

### 包、变量和函数

#### 包
每个 Go 程序都是由包组成的。

程序运行的入口是包 `main`。

这个程序使用并导入了包 "fmt" 和 `"math/rand"`。

按照惯例，包名与导入路径的最后一个目录一致。例如，`"math/rand"` 包由 package rand 语句开始。

#### 导入
这个代码用圆括号组合了导入，这是“打包”导入语句。

同样可以编写多个导入语句，例如：

import "fmt"
import "math"
不过使用打包的导入语句是更好的形式。

#### 导出名
在导入了一个包之后，就可以用其导出的名称来调用它。

在 Go 中，首字母大写的名称是被导出的。

Foo 和 FOO 都是被导出的名称。名称 foo 是不会被导出的。

执行代码。然后将 math.pi 改名为 math.Pi 再试着执行一下。

#### 函数
函数可以没有参数或接受多个参数。

在这个例子中，`add` 接受两个 int 类型的参数。

注意类型在变量名 _之后_。

（参考 [这篇关于](https://blog.golang.org/gos-declaration-syntax) Go 语法定义的文章了解类型以这种形式出现的原因。）

#### 函数（续）
当两个或多个连续的函数命名参数是同一类型，则除了最后一个类型之外，其他都可以省略。

在这个例子中 ，

x int, y int
被缩写为

x, y int
x, y int

#### 多值返回
``` go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}

```

函数可以返回任意数量的返回值。

swap 函数返回了两个字符串。

#### 命名返回值
``` go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

Go 的返回值可以被命名，并且像变量那样使用。

返回值的名称应当具有一定的意义，可以作为文档使用。

没有参数的 return 语句返回结果的当前值。也就是`直接`返回。

直接返回语句仅应当用在像下面这样的短函数中。在长的函数中它们会影响代码的可读性。

#### 变量
``` go
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}

```

var 语句定义了一个变量的列表；跟函数的参数列表一样，类型在后面。

就像在这个例子中看到的一样，`var` 语句可以定义在包或函数级别。

#### 初始化变量
``` go
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```
变量定义可以包含初始值，每个变量对应一个。

如果初始化是使用表达式，则可以省略类型；变量从初始值中获得类型。

#### 短声明变量
``` go
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}

```

在函数中，`:=` 简洁赋值语句在明确类型的地方，可以用于替代 var 定义。

函数外的每个语句都必须以关键字开始（`var`、`func`、等等），`:=` 结构不能使用在函数外。

#### 基本类型
``` go
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	const f = "%T(%v)\n"
	fmt.Printf(f, ToBe, ToBe)
	fmt.Printf(f, MaxInt, MaxInt)
	fmt.Printf(f, z, z)
}

```

Go 的基本类型有Basic types

bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 代表一个Unicode码

float32 float64

complex64 complex128
这个例子演示了具有不同类型的变量。 同时与导入语句一样，变量的定义“打包”在一个语法块中。

#### 零值
``` go
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s)
}

```
变量在定义时没有明确的初始化时会赋值为_零值_。

零值是：

数值类型为 `0`，
布尔类型为 `false`，
字符串为 `""`（空字符串）。

#### 类型转换
``` go
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z int = int(f)
	fmt.Println(x, y, z)
}

```
表达式 T(v) 将值 v 转换为类型 `T`。

一些关于数值的转换：

var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
或者，更加简单的形式：

i := 42
f := float64(i)
u := uint(f)
与 C 不同的是 Go 的在不同类型之间的项目赋值时需要显式转换。 试着移除例子中 float64 或 int 的转换看看会发生什么。

#### 类型推导
``` go
package main

import "fmt"

func main() {
	v := 42 // change me!
	fmt.Printf("v is of type %T\n", v)
}

```
在定义一个变量但不指定其类型时（使用没有类型的 var 或 := 语句）， 变量的类型由右值推导得出。

当右值定义了类型时，新变量的类型与其相同：

var i int
j := i // j 也是一个 int
但是当右边包含了未指名类型的数字常量时，新的变量就可能是 int 、 float64 或 `complex128`。 这取决于常量的精度：

i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
尝试修改演示代码中 v 的初始值，并观察这是如何影响其类型的。

#### 常量
``` go
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```
常量的定义与变量类似，只不过使用 const 关键字。

常量可以是字符、字符串、布尔或数字类型的值。

常量不能使用 := 语法定义。


#### 数值常量
``` go
package main

import "fmt"

const (
	Big   = 1 << 100
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}

```
数值常量是高精度的 _值_。

一个未指定类型的常量由上下文来决定其类型。

也尝试一下输出 needInt(Big) 吧。

### 流程控制语句：for、if、else 和 switch

#### for
``` go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

```
Go 只有一种循环结构——`for` 循环。

基本的 for 循环除了没有了 `( )` 之外（甚至强制不能使用它们），看起来跟 C 或者 Java 中做的一样，而 `{ }` 是必须的。

#### for（续）
``` go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```
跟 C 或者 Java 中一样，可以让前置、后置语句为空。

#### for 是 Go 的 “while”
``` go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

```
基于此可以省略分号：C 的 while 在 Go 中叫做 `for`。

#### 死循环
``` go
package main

func main() {
	for {
	}
}
```
如果省略了循环条件，循环就不会结束，因此可以用更简洁地形式表达死循环。

#### if
``` go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}

```
if 语句除了没有了 `( )` 之外（甚至强制不能使用它们），看起来跟 C 或者 Java 中的一样，而 `{ }` 是必须的。

（耳熟吗？）

#### if 的便捷语句
``` go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```
跟 for 一样，`if` 语句可以在条件之前执行一个简单的语句。

由这个语句定义的变量的作用域仅在 if 范围之内。

（在最后的 return 语句处使用 v 看看。）

#### if 和 else
``` go
import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// 这里开始就不能使用 v 了
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```
在 if 的便捷语句定义的变量同样可以在任何对应的 else 块中使用。

#### 练习：循环和函数
``` go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
}

func main() {
	fmt.Println(Sqrt(2))
}
```

作为练习函数和循环的简单途径，用牛顿法实现开方函数。

在这个例子中，牛顿法是通过选择一个初始点 z 然后重复这一过程求 Sqrt(x) 的近似值：


为了做到这个，只需要重复计算 10 次，并且观察不同的值（1，2，3，……）是如何逐步逼近结果的。 然后，修改循环条件，使得当值停止改变（或改变非常小）的时候退出循环。观察迭代次数是否变化。结果与 [[http://golang.org/pkg/math/#Sqrt][math.Sqrt] 接近吗？

提示：定义并初始化一个浮点值，向其提供一个浮点语法或使用转换：

z := float64(1)
z := 1.0

#### 

#### 

#### 

#### 

#### 

#### 

#### 

#### 


### 复杂类型：struct、slice 和 map

## 方法和接口

### 方法和接口

## 并发

### 并发