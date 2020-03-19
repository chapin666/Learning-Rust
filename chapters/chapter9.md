# Rust 条件判断

编程语言是模拟现实生活的，如果你有哪块搞不清楚，那么只要从现实生活中找例子即可。

我们的生活，大部分都是流水线似的，比如

```
睡醒 -> 早餐 -> 上班/上学 -> 午餐 -> 上班/上学 -> 回家 -> 晚饭 -> 睡觉
```

虽然站在每天的角度考虑，生活真的是顺风顺水，流水作业，但是，精确到每件事，就会有很多小插曲

比如
```
睡醒 -> (如果天还没亮则继续睡，否则起床吃早餐)
```

比如
```
如果是周六或周日，就不用上班或上课
```

也就是说，具体到每件事，都是根据一个条件作出判断，如果条件为真则怎么样，如果条件为假又怎么样，等等。

因为编程语言也是模拟生活的，所以，编程语言一般情况下，从入口代码开始，一条一条往下执行，比如

```
fn main()
{
    let name = "小明";        // 名字
    let firstMeet = false;   // 是否第一次见

    println!("{},你好!",name);
    println!("很高兴见到你");
    println!("我是零基础教程");
    println!("你叫什么名字");
}
```

上面这段代码，会从上往下一行一行的执行。所以输出的结果就是

```
你好!
很高兴见到你
我是零基础教程
你叫什么名字
```

但有时候，我们会想： 如果是第一次见则输出问候语，如果不是第一次见，则直接输出名字即可

对于这种现实生活中常见的需求，我们在程序中有要怎么模拟呢 ？

## 编程语言中的条件判断

我们说过编程语言也是模拟现实生活，针对上面提到的 如果...就... 或者 如果...就...否则就... 选择判断，我们称之为 **条件判断**

也就是说，条件判断就是

```
如果 ....  就 ...
```
或者

```
如果 ....  就 ... 否则 ....
```

或者
```
如果 .... 就 .... 否则如果 ... 就 ... 否则如果 ... 就 .... 否则 ....
```

绝大多数的编程语言都有着相同的条件判断语句，它们都是针对一个条件的真或假作出不同的选择，基本的流程图如下

![](/images/if.png)

|条件判断语句|	说明|
|--|--|
|if 语句|	if 语句用于模拟现实生活中的 如果...就...|
|if...else 语句|	if...else 语句用于模拟 如果...就...否则...|
|else...if 和嵌套if 语句 |	嵌套if 语句用于模拟 如果...就...如果...就...|
|match 语句 |	match 语句用于模拟现实生活中的 老师点名 或 银行叫号|

## if 语句

我们经常会说 如果肚子饿了就吃饭，如果今天天晴，我们就去郊游，如果....。

我们把这种 如果...就 语句叫做 条件语句，其中 肚子饿了、天晴 等叫做 条件，而 吃饭、郊游 叫做条件为真时要执行的 动作。

Rust 语言中使用 if 语句来模拟现实生活中这种 **如果...就** 的情况。

### if 语句语法

```
if boolean_expression {
    // boolean_expression 为 true 时要执行的语句
    // statement(s) 
}
```

因此，if 语句简单来说，就是

1. 如果 boolean_expression 为 true ，则执行大括号里的 statement(s)。
2. 如果 boolean_expression 为 false ，则执行大括号后面的第一条语句。

把 **如果肚子饿了就吃饭** 格式化为 if 语句就是

```
if 肚子饿了 {
   吃饭
}
```

### 范例

我们写一个范例，使用 if 语句来模拟 如果数字大于 0 则输出 正数。

```
fn main(){
   let num:i32 = 5;
   if num > 0 {
      println!("正数");
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
正数
```

## if else 语句

生活既有着流水线式的日复一日，也有着只有一个选择题的如果。 然而只有一个选择题的如果真的只有一个选择吗？

不是的，不是还有另一个选择，就是什么都不做吗？

如果另一个选择是做些什么，那么，不就变成了

```
如果 ... 就 ... 否则就...
```

编程语言的创建者早就想到了这种二选一的情况，特意为 if 语句后面跟着一个可选的 else 语句。

这种二选一的 if 语句，我们称之为 if else 语句。

### if..else 语句语法格式
```
if boolean_expression {
   // 如果 boolean_expression 为真则执行这里的代码
} else {
   // 如果 boolean_expression 为假则执行这里的代码
}
```

### if..else 语句流程图

![](/images/if_else_statement.jpg)


1. 在 if else 语句中，if 语句才是最主要的。如果 条件 为真，就没 else 语句啥事了。

2. 其实 if 语句后面的 else 语句是可选的。就像我们所说的，如果条件为假就什么都不做，那要 else 语句有什么用呢？else 语句的唯一作用，就是 if 语句中的 条件 为假时做些什么，执行些什么

### 范例

我们写一段代码，使用 if else 语句来判断一个数是否偶数或奇数，如果是偶数则输出 偶数 如果是奇数则输出 奇数

```
fn main() {
   let num = 12;
   if num % 2==0 {
      println!("偶数");
   } else {
      println!("奇数");
   }
}
```
编译运行以上 Rust 代码，输出结果如下
```
偶数
```

## 嵌套 If 语句

现实生活中肯定不会只有 如果...不然就 这种二选一的情况，还存在多个 如果 的情况。

比如 如果今天天晴，我们就去郊游，如果下雪，我们就堆雪人，如果下雨，我们就逛商场，如果下暴雨，我们就在家看电视。

面对多个 如果 这种情况，我们可以使用 嵌套 if 语句。

嵌套 if 语句用于测试多种条件。

### 语法

嵌套 If 语句的语法格式如下

```
if boolean_expression1 {
   // 当 boolean_expression1 为 true 时要执行的语句
} else if boolean_expression2 {
   // 当 boolean_expression2 为 true 时要执行的语句
} else {
   // 如果 boolean_expression1 和 boolean_expression2 都为 false 时要执行的语句
}
```

是不是看起来有点复杂了。

使用嵌套 if 语句，也就是 if...else if... 语句时需要牢记几个点：

- 任何一个 if 或者嵌套 if 语句可以有 0 个或 1 个 else 语句，但 else 语句必须出现在 if else 后面，也就是出现在最后。

- 任何一个 if 或者嵌套 if 语句可以有 0 个或多个 if else 语句，但所有的 if else 语句都必须出现在 else 语句之前。

- 一旦某个 else if 中的条件 boolean_expression1 返回 true，那么后面的 else if 和 else 语句都不会运行。

### 范例

我们使用嵌套 if 语句来写一段代码，判断某个值是 大于、小于、等于 0。

```
fn main() {
   let num = 2 ;
   if num > 0 {
      println!("{} is positive",num);
   } else if num < 0 {
      println!("{} is negative",num);
   } else {
      println!("{} is neither positive nor negative",num) ;
   }
}
```

编译运行以上 Rust 代码，输出结果如下

```
2 is positive
```

## match 语句

match 语句用于检查某个当前的值是否匹配一组/列值 中的某一个。

如果放到现实生活中，那么 match 语句类似于 老师点名 或者 银行叫号。当老师叫一个名字，比如 小明 时，叫 小明 的那个人就会应答，比如说 到。

如果你会 C 语言，那么 Rust 中的 match 表达式则类似于 C 语言中的 switch 语句。

对 match 语句有了个大概的印象之后，我们来看看 Rust 中的 switch 语句的语法格式

### match 语句语法格式

Rust 中一个基本的 match 语句语法格式如下

```
match variable_expression {
   constant_expr1 => {
      // 语句;
   },
   constant_expr2 => {
      // 语句;
   },
   _ => {
      // 默认
      // 其它语句
   }
};
```

match 语句有返回值，它把 匹配值 后执行的最后一条语句的结果当作返回值。

```
let expressionResult = match variable_expression {
   constant_expr1 => {
      // 语句;
   },
   constant_expr2 => {
      // 语句;
   },
   _ => {
      // 默认
      // 其它语句
   }
};

```
我们来分析下上面这两个 match 语句的语法：

1. 首先要说明的是 match 关键字后面的表达式不必括在括号中。也就是 variable_expression 不需要用一对 括号(()) 括起来。

2. 其次，match 语句在执行的时候，会计算 variable_expression 表达式的值，然后把计算后的结果和每一个 constant_exprN 匹配，使用的是 全等于 也就是 === 来匹配。如果匹配成功则执行 => {} 里面的语句。

3. 如果 variable_expression 表达式的值没有和任何一个 constant_exprN 匹配，那么它会默认匹配 _。因此，当没有匹配时，默认会执行 _ => {} 中的语句。

4. match 语句有返回值，它把 匹配值 后执行的最后一条语句的结果当作返回值。

5. _ => {} 语句是可选的，也就是说 match 语句可以没有它。

6. 如果只有一条语句，那么每个 constant_expr2 => {} 中的 {} 是可以省略的。

### 范例

看起来 match 语句有点复杂，我们直接拿几个范例来说明下

```
fn main(){
   let state_code = "MH";
   let state = match state_code {
      "MH" => {println!("Found match for MH"); "Maharashtra"},
      "KL" => "Kerala",
      "KA" => "Karnadaka",
      "GA" => "Goa",
      _ => "Unknown"
   };
   println!("State name is {}",state);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Found match for MH
State name is Maharashtra
```

上面的范例中，state_code 对应着语法中的 variable_expression，MH、KL、KA、GA 对应这语法中的 constant_expr1 、constant_expr2 等等。

因为我们的 variable_expression 的值为 MH 和值为 MH 的第一个 constant_expr1 匹配，因此会执行 ```{println!("Found match for MH"); "Maharashtra"}```。 然后将执行的最后一条语句的结果，也就是 "Maharashtra" 作为整个表达式的值返回。

这样，我们的 state 变量就被赋值为 Maharashtra。

### 范例 2

上面这个范例是匹配的情况，如果不匹配，那么就会执行 _ => 语句

```
fn main(){
   let state_code = "MS";
   let state = match state_code {
      "MH" => {println!("Found match for MH"); "Maharashtra"},
      "KL" => "Kerala",
      "KA" => "Karnadaka",
      "GA" => "Goa",
      _ => "Unknown"
   };
   println!("State name is {}",state);
}
```

编译运行以上 Rust 代码，输出结果如下

```
Unknown
State name is Unknown
```