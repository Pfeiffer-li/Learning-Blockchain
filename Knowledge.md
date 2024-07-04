本知识文章主要参考书籍：《Rust权威指南》。是我本人学习时的大致过程；第一章主要是介绍Rust：后续在补充；第二章主要是举个猜数游戏的Rust小游戏，也是后续在补充！！！
# 第二章
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先举一个简单的例子来显示Rust
```
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("欢迎来到猜数游戏！")
    let secret_number = rand::thread_rng().gen_range(1..101);

    loop {
        print("请你猜测一个1到100的整数！");
        let mut guess = String::new();
        io::stdin().read_line(&mut guess).expect("无法读取！");
        let guess: u32 = match guess.trim().parse(){
            Ok(num) => num,
            Err(_) => {
                println!("请输入正确的数字，而非英文字母！);
                continue;
            },
        };
        println!("你猜测的数是：{}", guess);

        match guess.cmp(&secret_number){
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }    
}
```
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

## 第三节 函数
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **一、区分语句以及表达式**：语句是结尾包含一个分号，包括**let申明**；表达式是**结尾没有分号**，或者**调用宏**是表达式，**也包括用花括号{}括起来的一段代码，这段代码最后一个代码是不用分号结尾的，那这一段花括号也就是表达式，如果花括号括起来的最后一个式子是分号结尾，那么这个花括号是一个语句，函数因为是花括号括起来的，所以也同理**。
```
举个例子
fn main() {
    let x = 5;
    let y = {
        let x = 1;
        x + 3  // 如果花括号以这个结尾，这里就表示这个花括号是个表达式，所以可以把这个花括号的结果赋值给y
        /*x + 3;    // 如果花括号以这个结尾，这里就表示这个花括号是个语句，这里就不能赋值给y*/
    }
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **二、参数**：同样跟别的语言一样有形参和实参；
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **三、函数的返回值：**
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 一般会在申明函数的后面添加 `->`指明函数的返回值类型，但是不可以为返回值命名；
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; — 在Rust里面一般默认最后一个表达式的值是函数的返回值，想要提前返回也可以用return。
```
fn add_five(x: i32) -> i32 {
    x + 5
}

fn main() {
    let x = 7;
    let y = add_five(x);
    println!("The value of y is {}", y);        // 这里会打印出值为12
}
```

## 第四节 if..else..
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1、条件**：同其他语言一样，if中的条件语句是bool类型；**一般如果如果过多的if else那么会用match重构！**，下面的例子一般就可以用match重构，等说到match的时候在具体解释！！
```
fn main(){
    let number: i32 = 6;
    if number % 4 == 0 {
        println!("number is divisible by 4!");
    } else if number % 3 == 0 {
        println!("number is divisible by 3!");
    } else if number % 2 == 0 {
        println!("number is divisible by 2!");
    } else {
        println!("number is not divisible by 4, 3, 2!");
    }
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2、在let语句中可以使用if**：需要及其注意：**如果用if else作为表达式，那么返回的值一定要是统一类型，因为Rust是强类型语言**。
```
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };        // 如果这里的else返回的是 { "6" }，那么这个语句就会报错
    println!("The value of number is {}", number);
}
```
## 第五节 循环
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;循环总共有三种：**loop循环，while循环以及for循环**。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一、**loop循环**：类似别的语言的死循环，有两种方式退出，① ctrl + c；② **使用退出条件加break，这里可以使用一个表达式返回loop循环返回的结果**。**loop循环经常用于需要不断去验证一个东西，比如重复循环验证键盘输入等**！
```
// 举个例子
fn main() {
    let mut count = 0;
    let result = loop{
        count += 1;
        if count == 10 {
            break count * 2;
        }
    };
    println!("The value of result is {}", result);
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;二、**while循环**：同别的语言类似，**经常用于有条件的循环**！
```
//举个例子
fn main() {
    let  mut count = 3;
    while count != 0 {
        println!("{}!", count);
        count -= 1;
    }
    println!("LEFTOFF!");
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;三、**for循环**：经常用于遍历一些数据，比如遍历数组等；如果用while循环就会多index索引判断，既增长的代码，也增加了错误程度，并且变得不够灵活（如果数组内容变少，可能还需要改索引）。所以一般遍历都是用for。
```
// 举个例子
fn main() {
    let a = [10, 20, 30 ,40 ,50];
    for element in a.iter(){
        println!("the value if {}", element);
    }
}
// 针对倒计时的例子也是可以的
fn main() {
    for number in (1..4).rev() {    // 这里的(1..4)是一个range，同Python中的range一样，rev是反转的意思：先遍历出3，然后2，然后1
        println!("{}", number);
    }
    println!("LEFTOFF!");
}
```
