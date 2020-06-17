---
title: "Swift捕获列表浅析"
date: 2020-06-17T14:55:33+08:00
draft: false
categories: ["swift"]
---

## Closures（闭包）
捕获列表（capture list）和闭包密不可分。

Closures capture values from the enclosing scope.

因为swift是一门安全的语言，闭包延长了它所使用的任何对象的生命周期，以此来保证这些对象是可用的和合法的。这个自动的安全性很好，但它也容易带来一些副作用，比如容易导致引用循环。最常见的场景就是闭包延长了一个对象的生命周期，而这个对象同时也捕获了该闭包。闭包，也是引用类型。

捕获列表就是为了解决上述问题。在本文中，我们不讨论捕获列表是如何打破闭包的引用循环，而主要讨论当捕获列表捕获不同类型、不同语义的变量时，这些被捕获的变量具有怎样的行为。
话不多说，看代码吧，具体细节可以参看注释：
~~~
import Foundation

// MARK: - 值类型的捕获列表，具有值语义
do {
    var string: String = "initial value"
    // 没有使用捕获列表
    let f = { print(string) }
    string = "new value"
    // 输出了new value，因为f持有对string的引用
    f()
}

do {
    var string: String = "initial value"
    // 使用了正常赋值版本的捕获列表
    let f = { [str = string] in print(str) }
    string = "new value"
    // 输出了initial value，因为string是值类型，str是string的shadowed copy，我觉得可以理解为赋值操作导致的结果
    f()
}

do {
    var string: String = "initial value"
    // 使用了简写版本的捕获列表
    let f = { [string] in print(string) }
    string = "new value"
    // 同上，输出了initial value，因为string就是一个shadowed copy
    f()
}

// MARK: - 引用类型的捕获列表，但具有值语义

do {
    var string: NSString = "initial value"
    // 没有使用捕获列表
    let f = { print(string) }
    string = "new value"
    // 输出了new value，因为f持有对string的引用
    f()
}

do {
    var string: NSString = "initial value"
    // 使用了正常赋值版本的捕获列表
    let f = { [str = string] in print(str) }
    string = "new value"
    // 输出了initial value，虽然string是引用类型，但string具有值语义，str是string的shadowed copy，我觉得可以理解为赋值操作导致的结果
    f()
}

do {
    var string: NSString = "initial value"
    // 使用了简写版本的捕获列表
    let f = { [string] in print(string) }
    string = "new value"
    // 同上，输出了initial value，因为string就是一个shadowed copy
    f()
}

// MARK: - 引用类型的捕获列表，同时也具有引用语义
class TestClass {
    var value = "initial value"
}

do {
    let testClass: TestClass = TestClass()
    // 没有使用捕获列表
    let f = { print(testClass.value) }
    testClass.value = "new value"
    // 输出了new value，因为f持有对testClass的引用
    f()
}

do {
    let testClass: TestClass = TestClass()
    // 使用了正常赋值版本的捕获列表
    let f = { [localTestClass = testClass] in print(localTestClass.value) }
    testClass.value = "new value"
    // 输出了new value，因为testClass是引用类型，同时具有引用语义，localTestClass是testClass的shadowed copy，我觉得可以理解为赋值操作导致的结果
    f()
}

do {
    let testClass: TestClass = TestClass()
    // 使用了简写版本的捕获列表
    let f = { [testClass] in print(testClass.value) }
    testClass.value = "new value"
    // 同上，输出了new value，因为testClass是引用类型，同时具有引用语义，testClass就是一个shadowed copy
    f()
}

~~~
