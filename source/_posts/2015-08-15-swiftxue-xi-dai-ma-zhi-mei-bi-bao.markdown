---
layout: post
title: "Swift学习 -代码之美——闭包"
date: 2015-08-15 15:21:06 +0800
comments: true
categories: 
---
##闭包之美
闭包在Swift语言中感觉充分诠释了代码的美，同时也反衬出swift语言的强大无比！

###本文要点

* 闭包表达式在函数调用中的简化
* 尾随闭包
* 闭包捕获变量和常量
* 闭包是引用类型

<!--more--> 

## 闭包表达式
我们先了解闭包的基本表达式，然后再用实际的例子去看下闭包表达式神奇的精简过程。

<pre>
{(parameters) -> return in statements }
</pre>
#####闭包表达式替代函数
我们先了解闭包的基本表达式，然后再用实际的例子去看下闭包表达式神奇的精简过程。
<pre class=”brush: Objective-C; gutter: true;”>
var names = ["Mike", "John", "Tom", "Kitty"]

//从小到大排序
func sortFun(s1 : String, s2 : String2) -> Bool {
	return s1 < s2;
}

var sortedNames = sorted (names, sortFun);
</pre>

1、由函数表达式变成闭包的标准模式：
<pre>
//  第1步
var sortedNames1 = sorted(names,{(s1:String, s2:String) -> Bool in return s1 < s2})
</pre>

2、省略参数类型
<pre>
//  第2步
var sortedNames2 = sorted(names,{(s1, s2) -> Bool in return s1 < s2})
</pre>

3、省略返回值类型
<pre>
//  第3步
var sortedNames3 = sorted(names,{(s1, s2) in return s1 < s2})
</pre>

4、省略返回语句
<pre>
// 第4步
var sortedNames4 = sorted(names,{(s1, s2) in s1 < s2})
</pre>
5、省略相同形式内容
<pre>
//  第5步
var sortedNames5 = sorted(names,{$0 < $1})
</pre>
6、OMG!没了！
<pre>
//  第6步
var sortedNames6 = sorted(names,<)
</pre>

##尾随闭包
尾随闭包是指闭包作为参数放在函数的最后面，如果函数最后一个参数是闭包，在调用函数的时候，可以将闭包写在函数外面，如果函数只有一个参数，则函数后圆括号都可以省略；用上面的闭包带入函数里面；
<pre>
//第1步
sorted(names, {$0 < $1})
//第2步
sorted(names){$0 < $1}
//第3步
sorted{$0 < $1}
</pre>

##捕获闭包的常量和变量
闭包能够在上下文中捕获常量和变量，即使常量和变量的作用域不再存在，闭包捕获的常量和变量仍然会保存最后一次使用他们的值，继续使用；
<pre>
import Foundation

func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}
//即使在incrementor返回后，runningTotal仍会保留最近一次使用的值
let incrementByTen = makeIncrementor(forIncrement: 10)

println(incrementByTen())
println(incrementByTen())
println(incrementByTen())
</pre>


输出结果：
<pre>
10
20
30
</pre>

如果获得新的runningTotal值，再次调用makeIncrementor函数即可，如
<pre>
let incrementByTen = makeIncrementor(forIncrement: 10)
</pre>

##闭包是引用类型
由于闭包有时候会很大，所以Swift语言将其设计成引用类型，引用和C语言指针差不多，多个变量或者常量引用一块内存空间；
<pre>
let incrementByTen = makeIncrementor(forIncrement: 10)
var incrementByTen1 = incrementByTen //与incrementByTen指向同一个函数
var incrementByTen2 = incrementByTen
</pre>