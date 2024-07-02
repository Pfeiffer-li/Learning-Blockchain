本知识文章主要参考书籍：Rust权威指南。是我本人学习时的大致过程；第一章主要是介绍Rust：后续在补充；第二章主要是举个猜数游戏的Rust小游戏，也是后续在补充！！！

# 第三章 —— 变量与可变性
1、针对常量的介绍：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1) Rust用**关键字** `const` 来申明常量；<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2) 变量名在定义的时候，**一定是全大写字母，并且单词之间用 `_` 隔开**:  `const BIT1_N_STRINGS`;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3) Rust对常量定义时**一定要声明类型，且类型必须具有 `'static`生存期**，如果没有需要添加此关键字：声明数值型常量 `const BIT: u32 = 12;`，声明字符型常量 `const BIT_STRINGS: &'static str = 'nihao';`
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(4) Rust对**常量的使用是全局的**，无论是在函数内声明还是在任何地方声明，在该脚本的任何地方都可以使用；<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(5) 常量**是不可变的**。

2、声明一个变量：**`let 变量名 : 类型 = 值;`**
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当然这里的 `:类型` 不是必要的，**但是使用关键字let声明所有类型的变量**。

3、介绍shadowing性质：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可以使用相同的名称声明新的变量，新的变量就会shadow（隐藏）之前声明的同名变量。
> 这个就有点类似于别的语言中，覆盖的含义，就python而已：如果在代码的开头第一行，我写了一个语句：`something = 10`，我之后的第十行语句又写了一个：`something = "nihao"`，
那么在第十行之后的语句中，如果使用了`something`这个变量，那么就是使用了 `"nihao"` 这个值。这里也就是这个意思。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Rust中如果使用了shadow，可以重新对变量进行赋值，赋新的类型。
```
fn main(){
    let x = 5;
    let x = x + 1;

    println!("The value of x is {}", x);  // 这里输出结果是6
}
```
