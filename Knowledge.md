本知识文章主要参考书籍：《Rust权威指南》。是我本人学习时的大致过程；第一章主要是介绍Rust：后续在补充；第二章主要是举个猜数游戏的Rust小游戏，也是后续在补充！！！

# 第三章
##  第一节 变量与可变性
**一、针对常量的介绍：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1) Rust用**关键字** `const` 来申明常量；<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2) 变量名在定义的时候，**一定是全大写字母，并且单词之间用 `_` 隔开**:  `const BIT1_N_STRINGS`;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3) Rust对常量定义时**一定要声明类型，且类型必须具有 `'static`生存期**，如果没有需要添加此关键字：声明数值型常量 `const BIT: u32 = 12;`，声明字符型常量 `const BIT_STRINGS: &'static str = 'nihao';`
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(4) Rust对**常量的使用是全局的**，无论是在函数内声明还是在任何地方声明，在该脚本的任何地方都可以使用；<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(5) 常量**是不可变的**。

**二、声明一个变量：`let 变量名 : 类型 = 值;`**
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当然这里的 `:类型` 不是必要的，**但是使用关键字let声明所有类型的变量**。

**三、介绍shadowing性质：**
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可以使用相同的名称声明新的变量，新的变量就会shadow（隐藏）之前声明的同名变量。
> 这个就有点类似于别的语言中，覆盖的含义，就python而已：如果在代码的开头第一行，我写了一个语句：`something = 10`，我之后的第十行语句又写了一个：`something = "nihao"`，
那么在第十行之后的语句中，如果使用了`something`这个变量，那么就是使用了 `"nihao"` 这个值。这里也就是这个意思。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Rust中如果使用了shadow，可以重新对变量进行赋值，赋新的类型。
```
// 简单举个例子
fn main(){
    let x = 5;
    let x = x + 1;
    println!("The value of x is {}", x);  // 这里输出结果是6
}
```

## 第二节 数据类型
Rust是静态编译语言，在编译的时候必须知道所有变量的类型；
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — Rust一般能够自动推断出一个变量的具体的类型；
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 但是**如果可能的类型比较多**，比如把String类型转化为证书的parse方法，这里因为数值类型很多：u8、u16、u32等，**这里就必须要指出这个变量的类型**，否则会报错！

**一、标量的数据类型**（整数类型、浮点类型、布尔类型、字符类型）
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1) 整数类型：每种都分i和u，i是有符号整数，u是无符号整数；有以下几种 `u8、i8、u16、i16、u32、i32、u64、i64、u128、i128、usize、isize` 这12种。**rust默认的整数类型是i32**。其中usize和isize的位数分别取决于当前运行rust程序的计算器的架构决定，如果这个计算机是64位，那么usize就相当于u64，isize相当于i64。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — **整数溢出**：这个类似与C语言的数值范围，这里不多介绍。在调试模式也就是check模式下，如果整数发生溢出会导致panic；但是在发布模式下(--release)下，如果发生溢出则不会发生panic，而是会执行“环绕操作”。环绕操作的意思是：比如是u8类型的整数，那么他的数值表示范围是0-255，如果此时的值是256，那么这个变量的值会变成0，257的值会变成1...。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2) 浮点类型：f64(双精度浮点)、f32(单精度浮点); **Rust默认的浮点类型是：f64**; 
```
// 举个例子
fun main(){
    let x = 2.0;    # 类型是f64
    let y: f32 = 3.0; # 类型是f32
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(3) 布尔类型：bool，同其他语言一样

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(4) 字符类型：char，表示值的时候用**单引号**括起来；占用空间是4字节大小，与C语言的几乎相似的处理。

**二、复合数据类型**（元组、数组）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、元组（Tuple）：**长度固定，不可变**，且可**以存放多个不同数据类型的数据**。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 获取元组里面的数据的方法：
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1) 解构法：`let tup = (500, 6.4, "a"); let (a, b, c) = tup;`；
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2) 通过索引并使用 `.` 号：`lex tup = (500, 6.4, "a"); let x = tup.1;`。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、数组：同Tuple有点类似，**长度固定不可变**；但是有一个不同：**只能存放相同类型的数据**。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 数组的用处：**一般是想要再栈上而不是堆上为数据分配内存空间；或者想要保留固定数量的元素时**。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 标准库的Vector：**Vector俗称动态数组；长度可变**；一般用的比较多！！！
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 数组在定义时候的类型表示：`let a: [i32; 5] = [1, 2, 3, 4, 5];` **前面表示数组中元素的数据类型，分号后表示的是元素的个数**。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;申明数组的方式：(需要注意第三点的申明方式，可以用于一些算法题的初始化)
```
1、let a = [1, 2, 3, 4, 5];
2、let a: [i32, 5] = [1, 2, 3, 4, 5];
3、let a = [3, 5];    // 它的打印结果是这样的 [3, 3, 3, 3, 3]
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 访问数组的元素：同C语言是一样的，使用中括号：`a[0]`，数组下标也是从0开始的。


