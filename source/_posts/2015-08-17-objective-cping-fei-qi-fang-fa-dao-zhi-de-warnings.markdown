---
layout: post
title: "Objective-C warnings的屏蔽"
date: 2015-08-17 12:37:05 +0800
comments: true
categories: 
---
##处理编译器的警告
在项目不断发展需要兼容低版本，如iOS6.0以及更低版本的时候，编译出来就会有上百个warnings，究其原因就是因为有很多低版本的方法在高版本的SDK中被deprecated，所以我们需要做的只能是屏蔽warnins,添加toDo注释，等兼容最低版本提高时再替换方法；

####1、弃用方法的warnings屏蔽
<pre>
#pragma clang diagnostic push  
  
#pragma clang diagnostic ignored "-Wdeprecated-declarations"       
//调用废弃的方法 
#pragma clang diagnostic pop   
</pre>
<!--more-->
####2、指针类型不兼容warnings屏蔽
<pre>
#pragma clang diagnostic push   
#pragma clang diagnostic ignored "-Wincompatible-pointer-types"   
//类型不兼容的代码段 
#pragma clang diagnostic pop  
</pre>

####3、尚未使用的变量或方法用unused标记
<pre>
#pragma clang diagnostic push   
#pragma clang diagnostic ignored "-Wunused-variable"   
//有未使用的变量或者方法 
#pragma clang diagnostic pop 
</pre>

####4、保留环问题，不过建议最好直接修改代码
<pre>
the retain cycle.  
#pragma clang diagnostic push  
#pragma clang diagnostic ignored "-Warc-retain-cycles"  
    self.completionBlock = ^ {  
        ...  
    };  
#pragma clang diagnostic pop  
</pre>

以上方法可以解决大部分现有项目因为废弃或者其他原因造成的warnings
####如果需要更进一步了解
http://clang.llvm.org/docs/UsersManual.html#diagnostics_pragmas