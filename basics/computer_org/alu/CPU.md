# CPU

## 指令集
> ARM处理器与Intel处理器的对比.   
> 参考 [三种移动处理器(ARM, Intel和MIPS)之间的主要区别](http://www.vaikan.com/arm-vs-x86-key-differences-explained/)

1. ARM处理器使用精简指令集（RISC）
    - 通俗解释：精简指令集规模较小, 更接近原子操作
    - **原子操作**：是指每条指令的工作大都可以由处理器在一个操作内完成, 例如对两个寄存器做加法. 
1. Intel处理器使用复杂指令集（CISC)
    - 通俗解释：复杂指令集规模较大, 更加复杂
    - **复杂指令集**的指令描述某个意图. 但是处理器必须执行3或4个更简单的指令才能实现这个意图
    - 示例： 命令一个复杂指令集处理器对2个数求和, 并把结果存入主内存中. 
        - 为了完成这个命令, 处理器首先从地址1中取得第一个数（操作1）, 然后从地址2中取得另一个数（操作2）, 然后求和（操作3）
1. 所有的现代处理器都使用一种所谓微指令的概念, 这是一个处理器内部的指令集合, 用来描述处理器可以做的原子操作. 复杂指令集处理器实际上执行了3条微指令. 对精简指令集处理器而言, 其指令跟其微指令十分接近. 而复杂指令集处理器的指令需要先被转换成一些更精简的微指令（就像前面的复杂指令集处理器做加法的例子中那样）. 也就是说精简指令集处理器中的解码器（负责告诉处理器到底要干些什么的东东）要简单得多, 而简洁意味着高效和低功耗. 
2. ARM处理器的强项是设计低功耗处理器, Intel的强项是设计超高性能的台式机和服务器处理器. 
3. Intel 64位架构
    - Intel并没有开发64位版本的x86指令集. 这个64位的指令集, 名为x86-64（有时简称为x64）, 实际上是AMD设计开发的. 故事是这样的：Intel想搞64位计算, 它知道如果从自己的32位 x86架构进化出的64位架构的话, 新架构效率会很低. 于是它搞了一个新64位处理器项目名为IA64. 由此制造出了Itanium系列处理器. 同时AMD知道自己造不出能与IA64兼容的处理器, 于是它把x86扩展一下, 加入了64位寻址和64位寄存器. 最终出来的架构, 人称AMD64, 成为了64位版本的x86处理器的标准. 
    - IA64项目并不算得上成功, 现如今基本被放弃了. Intel最终采用了AMD64. Intel当前给出的移动方案, 是采用了AMD开发的64位指令集（有些许差别）的64位处理器. 
3. ARM 64位架构
    - 了解移动设备对64位计算的需求后, ARM于2011年发布了ARMv8 64位架构, 这是为了下一代ARM指令集架构工作若干年后的结晶. 为了基于原有的原则和指令集, 开发一个简明的64位架构, ARMv8使用了两种执行模式, AArch32和AArch64. 
    - AArch32运行32位代码, AArch64运行64位代码. ARM设计的巧妙之处, 是处理器在运行中可以无缝地在两种模式间切换. 这意味着64位指令的解码器是全新设计的, 不用兼顾32位指令, 而处理器依然可以向后兼容. 
4. [异构计算](https://community.arm.com/cn/b/blog/posts/big-little-help-you-save-your-power-2013)
    - ARM的 **big.LITTLE架构** 是一项Intel一时无法复制的创新. **在big.LITTLE架构里, 处理器可以是不同类型的**. 传统的双核或者四核处理器中包含同样的2个核或者4个核. 一个双核Atom处理器中有两个一模一样的核, 提供一样的性能, 拥有相同的功耗. ARM通过big.LITTLE向移动设备推出了异构计算. 这意味着处理器中的核可以有不同的性能和功耗. 当设备正常运行时, 使用低功耗核, 而当你运行一款复杂的游戏是, 使用的是高性能的核. **简单来讲, 就是： 采用big.LITTLE架构的处理器可以同时拥有Cortex-A53和Cortex-A57核**
    - **流水线**： 设计处理器的时候, 要考虑大量的技术设计的采用与否, 这些技术设计决定了处理器的性能以及功耗. 在一条指令被解码并准备执行时, Intel和ARM的处理器都使用流水线. 就是说解码的过程是并行的. 第一步从内存中读取指令, 第二步检查和解码指令, 第三步执行指令, 周而复始. 流水线的好处在于, 当前指令在第二步的时候, 下一条指令已经处于第一步. 当前指令在第三步中执行的时候, 下一条指令正处于第二步, 而下下条指令处于第一步中, 如此循环. 