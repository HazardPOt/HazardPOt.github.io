---  
layout:     post
title:      SwiftUI State有什么用
subtitle:   SwiftUI中State的具体用法和实战
date:       2020-08-15
author:     POt
header-img: img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:      
    -   
    -   
    -   
---

# @State 有什么用？
@State 解释为一个给给定类型的持久化值，通过这个值view可以读取并监控这个数值。

用大白话讲，@State就是一个标签，贴之前视图是不可以修改这个值;贴了之后，只要你修改这变量，界面就会跟着同步修改。这个是现代界面语言都是支持的特性。

实例：
制作一个按钮：

```
@State var isChecked: Bool = false    //    @State随时监控isChecked这个变量 如果isChecked更新，则下面Image也会更新 从而实现按钮功能

Image(systemName: self.isChecked ? "checkmark.square.fill" : "square")
                .imageScale(.large)
                .padding(.trailing)
                .onTapGesture {
                self.isChecked.toggle()
                
```

如果不添加@State这个标签，则isChecked不可更改
