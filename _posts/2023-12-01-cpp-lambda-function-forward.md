---
layout: post
title:  c++  function forward
categories: [vscode, cpp]
description: cpp forward
keywords: forward, cpp, function
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# c++ 函数forward

c++ forward一般来说减少数据拷贝，能提升程序性能；用在function上会有什么效果；
预期应该是`use_func`` 更高效

## cpp debug

- 代码比较
```cpp
#include<array>
#include<stdio.h>
#include<thread>
template <typename Fn>
void use_func(Fn&& fn) {
    std::thread t2(std::forward<Fn>(fn)); 
    t2.join();
}

template <typename Fn>
void use_func2(const Fn* fn) {
    std::thread t2((*fn)); 
    t2.join();
}

template <typename Fn>
void use_func3(Fn fn) {
    std::thread t2((fn)); 
    t2.join();
}

int main(int argc, char* argv[]) {
    std::array<int, 100> blob;
    int x = argc;
    
    auto lambda = [blob, x](){ 
        printf("This lambda is invoked from %d, %d", blob.size(), x); 
    };
    use_func(lambda);
    auto lambda2 = [blob, x](){ 
        printf("This lambda is invoked from %d, %d", blob.size(), x); 
    };
    use_func2(&lambda2);

    auto lambda3 = [blob, x](){ 
        printf("This lambda is invoked from %d, %d", blob.size(), x); 
    };
    use_func3(lambda3);
}
```
放到godbolt 查看汇编代码


- 汇编代码

使用 -O0

```x86asm
.LC0:
        .string "This lambda is invoked from %d, %d"
main::{lambda()#1}::operator()() const:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 32
        mov     QWORD PTR [rbp-24], rdi
        mov     rax, QWORD PTR [rbp-24]
        mov     eax, DWORD PTR [rax+400]
        mov     rdx, QWORD PTR [rbp-24]
        mov     QWORD PTR [rbp-8], rdx
        mov     ecx, 100
        mov     edx, eax
        mov     rsi, rcx
        mov     edi, OFFSET FLAT:.LC0
        mov     eax, 0
        call    printf
        nop
        leave
        ret
main::{lambda()#2}::operator()() const:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 32
        mov     QWORD PTR [rbp-24], rdi
        mov     rax, QWORD PTR [rbp-24]
        mov     eax, DWORD PTR [rax+400]
        mov     rdx, QWORD PTR [rbp-24]
        mov     QWORD PTR [rbp-8], rdx
        mov     ecx, 100
        mov     edx, eax
        mov     rsi, rcx
        mov     edi, OFFSET FLAT:.LC0
        mov     eax, 0
        call    printf
        nop
        leave
        ret
main::{lambda()#3}::operator()() const:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 32
        mov     QWORD PTR [rbp-24], rdi
        mov     rax, QWORD PTR [rbp-24]
        mov     eax, DWORD PTR [rax+400]
        mov     rdx, QWORD PTR [rbp-24]
        mov     QWORD PTR [rbp-8], rdx
        mov     ecx, 100
        mov     edx, eax
        mov     rsi, rcx
        mov     edi, OFFSET FLAT:.LC0
        mov     eax, 0
        call    printf
        nop
        leave
        ret
main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 1680
        mov     DWORD PTR [rbp-1668], edi
        mov     QWORD PTR [rbp-1680], rsi
        mov     eax, DWORD PTR [rbp-1668]
        mov     DWORD PTR [rbp-4], eax
        lea     rax, [rbp-832]
        lea     rdx, [rbp-416]
        mov     ecx, 50
        mov     rdi, rax
        mov     rsi, rdx
        rep movsq
        mov     eax, DWORD PTR [rbp-4]
        mov     DWORD PTR [rbp-432], eax
        lea     rax, [rbp-832]
        mov     rdi, rax
        call    void use_func<main::{lambda()#1}&>(main::{lambda()#1}&)
        lea     rax, [rbp-1248]
        lea     rdx, [rbp-416]
        mov     ecx, 50
        mov     rdi, rax
        mov     rsi, rdx
        rep movsq
        mov     eax, DWORD PTR [rbp-4]
        mov     DWORD PTR [rbp-848], eax
        lea     rax, [rbp-1248]
        mov     rdi, rax
        call    void use_func2<main::{lambda()#2}>(main::{lambda()#2} const*)
        lea     rax, [rbp-1664]
        lea     rdx, [rbp-416]
        mov     ecx, 50
        mov     rdi, rax
        mov     rsi, rdx
        rep movsq
        mov     eax, DWORD PTR [rbp-4]
        mov     DWORD PTR [rbp-1264], eax
        sub     rsp, 8
        sub     rsp, 408
        mov     rax, rsp
        mov     rdx, rax
        lea     rax, [rbp-1664]
        mov     ecx, 50
        mov     rdi, rdx
        mov     rsi, rax
        rep movsq
        mov     rax, rsi
        mov     rdx, rdi
        mov     ecx, DWORD PTR [rax]
        mov     DWORD PTR [rdx], ecx
        call    void use_func3<main::{lambda()#3}>(main::{lambda()#3})
        add     rsp, 416
        mov     eax, 0
        leave
        ret
void use_func<main::{lambda()#1}&>(main::{lambda()#1}&):
        push    rbp
        mov     rbp, rsp
        push    rbx
        sub     rsp, 40
        mov     QWORD PTR [rbp-40], rdi
        mov     rax, QWORD PTR [rbp-40]
        mov     rdi, rax
        call    main::{lambda()#1}& std::forward<main::{lambda()#1}&>(std::remove_reference<main::{lambda()#1}&>::type&)
        mov     rdx, rax
        lea     rax, [rbp-24]
        mov     rsi, rdx
        mov     rdi, rax
        call    std::thread::thread<main::{lambda()#1}&, , void>(main::{lambda()#1}&)
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::join()
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        jmp     .L21
        mov     rbx, rax
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        mov     rax, rbx
        mov     rdi, rax
        call    _Unwind_Resume
.L21:
        mov     rbx, QWORD PTR [rbp-8]
        leave
        ret
void use_func2<main::{lambda()#2}>(main::{lambda()#2} const*):
        push    rbp
        mov     rbp, rsp
        push    rbx
        sub     rsp, 40
        mov     QWORD PTR [rbp-40], rdi
        mov     rdx, QWORD PTR [rbp-40]
        lea     rax, [rbp-24]
        mov     rsi, rdx
        mov     rdi, rax
        call    std::thread::thread<main::{lambda()#2} const&, , void>(main::{lambda()#2} const&)
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::join()
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        jmp     .L25
        mov     rbx, rax
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        mov     rax, rbx
        mov     rdi, rax
        call    _Unwind_Resume
.L25:
        mov     rbx, QWORD PTR [rbp-8]
        leave
        ret
void use_func3<main::{lambda()#3}>(main::{lambda()#3}):
        push    rbp
        mov     rbp, rsp
        push    rbx
        sub     rsp, 24
        lea     rax, [rbp-24]
        lea     rsi, [rbp+16]
        mov     rdi, rax
        call    std::thread::thread<main::{lambda()#3}&, , void>(main::{lambda()#3}&)
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::join()
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        jmp     .L29
        mov     rbx, rax
        lea     rax, [rbp-24]
        mov     rdi, rax
        call    std::thread::~thread() [complete object destructor]
        mov     rax, rbx
        mov     rdi, rax
        call    _Unwind_Resume
.L29:
        mov     rbx, QWORD PTR [rbp-8]
        leave
        ret
vtable for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >:
        .quad   0
        .quad   typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >::~_State_impl() [complete object destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >::~_State_impl() [deleting destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >::_M_run()
vtable for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >:
        .quad   0
        .quad   typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >::~_State_impl() [complete object destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >::~_State_impl() [deleting destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >::_M_run()
vtable for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >:
        .quad   0
        .quad   typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >::~_State_impl() [complete object destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >::~_State_impl() [deleting destructor]
        .quad   std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >::_M_run()
typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >:
        .string "*NSt6thread11_State_implINS_8_InvokerISt5tupleIJZ4mainEUlvE1_EEEEEE"
typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >:
        .quad   vtable for __cxxabiv1::__si_class_type_info+16
        .quad   typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#3}> > >
        .quad   typeinfo for std::thread::_State
typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >:
        .string "*NSt6thread11_State_implINS_8_InvokerISt5tupleIJZ4mainEUlvE0_EEEEEE"
typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >:
        .quad   vtable for __cxxabiv1::__si_class_type_info+16
        .quad   typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#2}> > >
        .quad   typeinfo for std::thread::_State
typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >:
        .string "*NSt6thread11_State_implINS_8_InvokerISt5tupleIJZ4mainEUlvE_EEEEEE"
typeinfo for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >:
        .quad   vtable for __cxxabiv1::__si_class_type_info+16
        .quad   typeinfo name for std::thread::_State_impl<std::thread::_Invoker<std::tuple<main::{lambda()#1}> > >
        .quad   typeinfo for std::thread::_State
```


## 结论
从上面可以看到， 比较所有的汇编代码，实际情况 在使用function 值传递（`use_func3`）反而更高效;

总结起来就是只要不涉及大块内存数据分配，通常情况使用值传递能更高效。

## 参考
[how-to-pass-lambda-functions-in-C++/](https://johannesugb.github.io/cpu-programming/how-to-pass-lambda-functions-in-C++/)

[C++ best practice: Pass use-only \(not stored\) lambda argument to function by const-reference or by forwarding-reference \(a.k.a. universal-reference\)](https://stackoverflow.com/questions/65562986/c-best-practice-pass-use-only-not-stored-lambda-argument-to-function-by-con)

