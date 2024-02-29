# 编程习惯
需要考虑和 /home/wzs/go/src/github.com/everywan/note/standard/design_pattern/coding_guidelines.md 合并

## 编程习惯
> No BB, show me the code
1. 过早的优化时万恶之源：premature optimization is the root of all evil
    - 先有质量的实现需求, 写够testcase, 然后做profile去找到性能的瓶颈, 这个时候才优化
        - 或者说: 让正确的程序更快, 要比让快速的程序正确容易得多
        - 或者说: 先解决问题, 再考虑优化
    - 不要浪费时间做那些根本不重要的优化
    - [知乎讨论](https://www.zhihu.com/question/24282796/answer/27279410)
2. 如无必要, 勿增实体
3. 计算机领域的任何问题都可以通过增加一个中间件来解决
4. 类默认继承 object
5. 区间左开右闭
6. 文件头规范：
    - 作者 / 创建时间 / 修改人 / 修改时间 / 对应系统模块 / 描述
    - 参数注释
    - TRANSACTION事务注释
7. 每个实体表都必须包含 CreateDate、CreateBy、ModifyDate、ModifyBy四个字段
8. 不要看到除了问题就瞎改, 看异常, 去分析
9. 定义类型尽量精确, 可以使用const的情况下尽量使用const
10. 无需改变形参值推荐将其声明为常量引用
11. 尽量不用指针. 使用引用类型的形参代替指针
12. 约定优于配置.

### 不知道怎么划分
1. 应当等所有操作完成后才执行 更新数据库等关键操作

### 断言和异常
1. 使用断言保证程序逻辑的正常执行, 使用异常保证程序健壮性: 不崩溃并且可以正确返回错误消息
2. 调用外部函数 使用断言保证 数据/逻辑 正常, 调用内部函数 使用异常保证执行成功
3. 无法处理的异常不要捕获, 抛到上层再处理
4. 只捕获特定的异常
5. 保证异常处理部分代码的正确性

## 编程术语
1. 上下文: 每一段程序都有很多外部变量. 一旦你的一段程序有了外部变量, 这段程序就不完整, 不能独立运行. 你为了使他们运行, 就要给所有的外部变量一个一个写一些值进去. 这些值的集合就叫上下文. 
    - 举例: lambda表达式中, `[写在这里的就是上下文](int a, int b){ ... }`
    - 参考: [vczh的回答](https://www.zhihu.com/question/26387327/answer/32611575)
2. 僵尸进程: 僵尸进程是指进程完成执行(通过exit系统调用, 或运行时发生致命错误或收到终止信号所致)但在操作系统的进程表中仍然有一个表项(进程控制块PCB), 处于"终止状态"的进程.

### 事件
> 参考 [事件-维基百科](https://zh.wikipedia.org/wiki/事件驅動程式設計)
1. 理解事件
    - 事件是一种 订阅/消费 模型; 分为 事件发布者和事件订阅者; 事件发布者发布事件后, 事件订阅者接收到消息开始执行
    - 事件的理解可以参考 C# 中的 **委托和事件**
2. 事件示例: 需求: 当用户点击鼠标时, 执行相关逻辑
    - 传统方式: 使用一个线程循环监听鼠标动作, 当用户点击鼠标时, 调用相关函数执行
    - 使用事件: 给鼠标点击绑定一个事件, 当用户点击鼠标时触发事件, 然后发送消息, 订阅事件者接收消息后开始执行
        - 优点1: 事件发布者和接受者都不需要一直运行, 只需要接收系统的调度即可
        - 优点2: 程序逻辑只需要订阅事件即可, 不需要更改事件发布者的逻辑, 减少代码耦合

## 代码规范
1. 代码每行长度不超过80. 原因是在某些条件下(如屏幕太小,
  或者github查看代码), 在vim中查看代码时, 如果不设置 自动换行, 那么超过80个字符可能导致代码折叠.
2. 文件命名尽量使用小写字符. 原因是命令行输入时会更快捷(大写需要转换)
3. 连字符/下划线的使用 参考 [a](/standard/design_pattern/coding_guidelines.md)
    
## 编程命名规范
1. web服务路由命名: `/version/business/funcname`
    - 函数名称使用全小写+下划线分割: `get_order_info_by_userid`
    
### 命名习惯
类型  |命名法
|:--:|:--:|
namespace   |Pascal
class   |Pascal
interface   |Pascal
method  |Pascal
enum    |Pascal
delegate    |Pascal
Locals(局部变量)    |Camel
1. 依赖条件使用 By+条件
2. 把相似的内容放在一起, 比如数据成员、属性、方法、事件等, 并适当的使用#region…#endregion
3. 方法的命名: 一般命名为动宾短语, 如:
    - ShowDialog（）
    - CreateFile（）
4. 接口的名称加前缀 `I`
5. 成员变量前加前缀 `_`
6. Util: 被理解为只有静态方法并且是无状态的。你不会创建这样一个类的实例。helper 可以是实用程序类，也可以是有状态的或需要创建实例。
    - https://stackoverflow.com/questions/12192050/what-are-the-differences-between-helper-and-utility-classes

## 各语言规范
### markdown
1. 使用 `##` 作为目录/业内锚点,而不是使用 html节点
2. 参考 `linux命令介绍`, 多节独立内容,如果是复杂的就单独放到一个标题中,如果是简单的就放到 基础知识/高级进阶/意料之外/ETC 等聚合章节中
    - 章节 是指 `##/###` 这样 由标题分开的部分(同一页面中)
    - 如 `文本分析-AWK`, 标题格式为: `[简短介绍]_CMD`. 如果没有合适的简短介绍,可以不写,只写命令. (如果使用 `_` 分割会出现无法通过目录跳转的情况)
    - 如 `文件传输`, 如果一个标题下是一类命令的对比介绍, 那么可以不写命令

## 变量/函数命名建议
1. Bootstrap: 启动程序预配置, 结构体. 用于加载配置文件, 生成server.

## 如何写博客

先写主要部分, 次要部分想到哪写到哪. 然后逐渐补充次要部分. 不要在开始就想从头到尾完全写好. 借鉴迭代的思想.

否则真的很容易写着写着就没有精力和新起写主要内容了, 都用来写特殊情况/其他情况等次要内容了..
