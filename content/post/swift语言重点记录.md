---
title: "Swift语言重点记录"
date: 2020-08-17T21:27:10+08:00
draft: false
categories: ["swift"]
---

# Classes

## Structures vs. classes

### Structures
1. Useful for representing values.
2. Implicit copying of values.
3. Becomes completely immutable when declared with let.
4. Fast memory allocation(stack).

### Classes
1. Useful for representing objects with an identity.
2. Implicity sharing of objects.
3. Internals can remain mutable even when declared with let.
4. Slower memory allocation(heap).

## A general understanding of stack and heap

### Stack

The System uses the stack to store anything on the immediate thread of execution; it's tightly managed and optimized by the CPU. When a function creates a variable, the stack stores that variable and then destroys it when the function exits. Since the stack is so strictly organized, it's very efficient, and thus quite fast.

### Heap

The system uses the heap to store instances of reference types. The heap is generally a large pool of memory from which the system can request and dynamically allocate blocks of memory. Lifetime is flexible and dynamic.

The heap doesn't automatically destroy its data like the stack does; additional work is required to do that. This makes creating and removing data on the heap a slower process, compared to on the stack.

## Key points
1. Like structures, classes are a named type that can have properties and methods.
2. Classes use references that are shared on assignment.
3. Class instances are called objects.
4. Objects are mutable.
5. Mutability introduces state, which adds complexity when managing your objects.
5. Use classes when you want reference semantics; structures for value semantics.
6. A class doesn't provide a memberwise initializer automatically.
7. In Swift, an instance of a structure is an immutable value whereas an instance of a class is a mutable object.
8. In Swift, uses === operator to check if the identity of one object is equal to the identity of another.
9. When you change the value of a struct, instead of modifying the value, you're making a new value. The keyword mutating marks methods that replace the current value with a new one. With classes, this keyword is not used because the instance itself is mutable.
10. The vary nature of classes is that they are both referenced and mutable, so you must care about their state and side effects.

