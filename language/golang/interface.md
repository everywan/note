# interface
[参考: 深入理解 Go Interface](http://legendtkl.com/2017/06/12/understanding-golang-interface/)

Go/interface 源码暂未阅读, 先不写

在Golang中
- 泛型编程: interface是一种抽象类型(相对而言, int/string 等都是具体类型).
- 接口编程: interface是一组抽象方法的组合, 不关心属性, 只关心行为(方法).
    - [鸭子类型: 当看到一只鸟走起来像鸭子,游泳起来像鸭子,叫起来也像鸭子, 那么这只鸟就可以被称为鸭子. 既只关注对象的行为, 而不关注对象所属的类型](https://zh.wikipedia.org/wiki/鸭子类型)

## 数据结构
阅读如下示例, 猜测如下两种方式输出是否相同
```Go
func main() {
	type tt interface{}

  // 方式一
	test := func() interface{} {
		var a *tt = nil
		return a
	}
	fmt.Println(test() == nil)
	fmt.Println(reflect.TypeOf(test()))
	fmt.Println(reflect.ValueOf(test()))

	fmt.Println("-----------------")

  // 方式二
	var a *tt = nil
	fmt.Println(a == nil)
	fmt.Println(reflect.TypeOf(a))
	fmt.Println(reflect.ValueOf(a))
}
```

相关知识如下

error 是 interface{} 类型的重命名
```Go
// 参考 $GOROOT/src/go/src/builtin/builtin.go
type error interface {
	Error() string
}
```

interface{} 由 Type/Value 组成, 类似于 `(T, V)` 结构.

`interface{} == nil` 当且仅当 `(T=nil, V is unset)`. _An interface value is nil only if the V and T are both unset, (T=nil, V is not set)._ [摘抄自 Go文档 nil err](https://golang.org/doc/faq#nil_error)

----

答案: 对于方式一, `test() != nil`. 对于方式二, `a == nil`.
1. 对于方式一, test函数将返回值包装为 interface{} 类型, 所以走 `interface{}==nil` 的判断逻辑.
2. 对于方式二, a 为指针类型, 指针零值本身就是nil. 容易犯错的点就是在这里将指针类型当成interface{}类型处理, 从而犯错.


## 泛型
判断 interface 类型
```Go
func do(v interface{}) {
    n, ok := v.(int)
    if !ok {
        // 断言失败处理
    }
}
```

[泛型编程](https://github.com/everywan/note/blob/master/language/java/summary.md#%E6%B3%9B%E5%9E%8B): 泛型类/方法/接口, 既 T.

interface 实现泛型编程
1. 参数为 interface 类型, 在函数内判断 interface 的类型, 然后调用相应的方法. (只能是内置类型,或者双方有约定的类型)
2. 定义 interface 接口, 然后所有该方法依赖的方法都定义到接口里, 要求传入的参数必须实现该接口

## 接口
[interface 简介](/language/base/interface.md)

- **实现interface接口时, 必须保持方法接收者与接口定义的类型相同**. [详细参见: Go-结构体](/language/golang/struct.md#接口继承)
- **接口类型无法被实例化, 但是接口可以使用接口声明一个空指针, 然后被绑定到实现该接口的类上**
- go语言的接口是非侵入式的
