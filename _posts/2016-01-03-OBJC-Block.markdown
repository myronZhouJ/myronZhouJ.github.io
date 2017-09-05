---
layout: post
title:  "OBJC中Block的实现原理"
date:   2016-01-03 14:32:45
categories: jekyll update
---

main.m中的代码
```
    #include "stdio.h"
    void (^tempBlock)();
    int main(int argc, char * argv[]){
        int a = 12;
        __block int b = 13;
        void (^block)() = ^{
            printf("Hello %d---%d",a,b);
        };
        b = 14;
        block();
        tempBlock = block;
        return 0;
    }
```
clang -rewrite-objc main.m 编译生成main.cpp

main.cpp中相关代码：

```
    struct __block_impl {
        void *isa;
        int Flags;
        int Reserved;
        void *FuncPtr;
    };

    void (*tempBlock)();

    struct __Block_byref_b_0 {
        void *__isa;
        __Block_byref_b_0 *__forwarding;
        int __flags;
        int __size;
        int b;
    };

    struct __main_block_impl_0 {
        struct __block_impl impl;
        struct __main_block_desc_0* Desc;
        int a;
        __Block_byref_b_0 *b; // by ref
        __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, int _a, __Block_byref_b_0 *_b, int flags=0) : a(_a), b(_b->__forwarding) {
            impl.isa = &_NSConcreteStackBlock;
            impl.Flags = flags;
            impl.FuncPtr = fp;
            Desc = desc;
        }
    };

    static void __main_block_func_0(struct __main_block_impl_0 *__cself) {
        __Block_byref_b_0 *b = __cself->b; // bound by ref
        int a = __cself->a; // bound by copy

        printf("Hello %d---%d",a,(b->__forwarding->b));
    }

    static void __main_block_copy_0(struct __main_block_impl_0*dst, struct __main_block_impl_0*src) {_Block_object_assign((void*)&dst->b, (void*)src->b, 8/*BLOCK_FIELD_IS_BYREF*/);}

    static void __main_block_dispose_0(struct __main_block_impl_0*src) {_Block_object_dispose((void*)src->b, 8/*BLOCK_FIELD_IS_BYREF*/);}

    static struct __main_block_desc_0 {
        size_t reserved;
        size_t Block_size;
        void (*copy)(struct __main_block_impl_0*, struct __main_block_impl_0*);
        void (*dispose)(struct __main_block_impl_0*);
    } __main_block_desc_0_DATA = { 0, sizeof(struct __main_block_impl_0), __main_block_copy_0, __main_block_dispose_0};

    int main(int argc, char * argv[]){
        int a = 12;
        __attribute__((__blocks__(byref))) __Block_byref_b_0 b = {(void*)0,(__Block_byref_b_0 *)&b, 0, sizeof(__Block_byref_b_0), 13};
        void (*block)() = ((void (*)())&__main_block_impl_0((void *)__main_block_func_0, &__main_block_desc_0_DATA, a, (__Block_byref_b_0 *)&b, 570425344));
        (b.__forwarding->b) = 14;
        ((void (*)(__block_impl *))((__block_impl *)block)->FuncPtr)((__block_impl *)block);
        tempBlock = block;
        return 0;
    }
```

可以看到编译时block是通过函数指针和结构体来实现的，而对于__block修饰的变量也是通过结构体包裹变量，然后传递此结构体的指针实现

