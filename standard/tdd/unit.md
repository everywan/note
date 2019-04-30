# unit test

Convey:

Stub:

Mock: 

stub/mock 都是为了解决外部依赖, 一般而言, mock 和 stub 是指单元测试种的两种思想. 都是为了解决外部依赖的问题.
stub 重写方法从而伪造方法, 返回预设的值, 从而阻断对原方法的调用.
mock 伪造对象从而伪造方法, 可以修改其中的逻辑.

参考链接
1. https://www.jianshu.com/p/e3b2b1194830
2. https://www.jianshu.com/p/70a93a9ed186
3. https://www.jianshu.com/p/598a11bbdafb
4. https://ruby-china.org/topics/10977
5. https://www.jianshu.com/p/2f675d5e334e

单元测试并不是为了避免所有bug, 而是为了减少所测函数在逻辑上的bug. 如给定不同的输入是否返回预期的值.
单元测试应完全或者尽量减少外部依赖对代码的影响.

我们可以分析下, 什么样的程序不需要单元测试, 什么样的程序需要单元测试.
1. 简单的程序

频繁变化的代码不适合写单元测试, 随便写着玩玩, 或者简单的代码也不需要单元测试.
1. 我认为写单元测试是为了实现一个 稳定/复杂/优质 的程序. 代码频繁变化说明开始开发时没有明确需求, 此时应考虑是不是设计上除了问题.  但没有明确需求的程序, 明显不稳定, 如果是随便写写, 是不需要单元测试的.
2. 得不偿失: 频繁的变更代码会导致频繁的更改单元测试, 降低开发效率和热情, 不值得写.