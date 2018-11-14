let/var/const:
- const/let 是 es6 中的语法, 是为了弥补 var 作用域方面的问题而提出的
- 使用var声明的变量, 其作用域为该语句所在的函数内, 且存在变量提升现象.
    - 变量提升是指在 JavaScript 中, 函数及变量的声明都将被提升到函数的最顶部, 即 变量可以先使用后声明
- 使用let声明的变量, 其作用域为该语句所在的代码块内, 不存在变量提升
- 使用const声明的是常量, 在后面出现的代码中不能再修改该常量的值。
- 参考 http://es6.ruanyifeng.com/#docs/let

以 for 循环举例,
```js
for (let i = 0; i < 10; i++) {
  // ...
}
// 报错, 变量i不在作用域内
console.log(i);

for (var i = 0; i < 10; i++) {
  // ...
}
// 正常执行, 打印 10. 使用var定义的变量存在变量提升现象, 即 `var i` 会被提升到函数的最顶部, 在整个函数作用域内都有效
// 即等同于: var i=0;for(i=0;i<10;i++){}
console.log(i);
```
