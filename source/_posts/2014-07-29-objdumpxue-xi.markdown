---
layout: post
title: "objdump学习"
date: 2014-07-29 09:36:08 +0000
comments: true
categories: tools
---

##1. 简单程序hello world
    #include <stdio.h>

    int main(int argc, char** argv) {
        printf("hello world!!\n");
        int a = 0x123;
        int b = 0x234;
        int c = a + b;
        printf("c is: 0x%x\n", c);
        return 0;
    }

编译gcc hello.c -o hello
###1.1 gcc -S
gcc hello.c -S -o hello.s       
查看hello.s内容cat hello.s  

		.file   "hello.c"
		.section        .rodata
	.LC0:
		.string "hello world!!"
	.LC1:
		.string "c is: 0x%x\n"
		.text
	.globl main
		.type   main, @function
	main:
	.LFB0:
		.cfi_startproc
		pushq   %rbp
		.cfi_def_cfa_offset 16
		movq    %rsp, %rbp
		.cfi_offset 6, -16
		.cfi_def_cfa_register 6
		subq    $32, %rsp
		movl    %edi, -20(%rbp)
		movq    %rsi, -32(%rbp)
		movl    $.LC0, %edi
		call    puts
		movl    $291, -4(%rbp)
		movl    $564, -8(%rbp)
		movl    -8(%rbp), %eax
		movl    -4(%rbp), %edx
		leal    (%rdx,%rax), %eax
		movl    %eax, -12(%rbp)
		movl    $.LC1, %eax
		movl    -12(%rbp), %edx
		movl    %edx, %esi
		movq    %rax, %rdi
		movl    $0, %eax
		call    printf
		movl    $0, %eax
		leave
		ret
		.cfi_endproc
	.LFE0:
		.size   main, .-main
		.ident  "GCC: (Ubuntu/Linaro 4.4.7-1ubuntu2) 4.4.7"
		.section        .note.GNU-stack,"",@progbits
###1.2 objdump指令
####1.2.1 objdump -s hello
显示所有section内容     

    objdump -s -j .rodata hello

    hello:     file format elf64-x86-64

    Contents of section .rodata:
    400688 01000200 68656c6c 6f20776f 726c6421  ....hello world!
    400698 21006320 69733a20 30782578 0a00      !.c is: 0x%x..  
    只显示.rodata部分
####1.2.2 objdump -S hello
    objdump -S -j .rodata hello
    hello:     file format elf64-x86-64

    Disassembly of section .rodata:

    0000000000400688 <_IO_stdin_used>:
    400688:       01 00 02 00 68 65 6c 6c 6f 20 77 6f 72 6c 64 21     ....hello world!
    400698:       21 00 63 20 69 73 3a 20 30 78 25 78 0a 00           !.c is: 0x%x..

##各段含义
[汇编中bss,data,text,rodata,heap,stack,意义](http://blog.sina.com.cn/s/blog_8053938901014gih.html)      
bss段：   

    BSS段（bsssegment）通常是指用来存放程序中未初始化的全局变量的一块内存区域。BSS是英文BlockStarted by Symbol的简称。BSS段属于静态内存分配。

data段：   

    数据段（datasegment）通常是指用来存放程序中已初始化的全局变量的一块内存区域。数据段属于静态内存分配。       

text段：

    代码段（codesegment/textsegment）通常是指用来存放程序执行代码的一块内存区域。这部分区域的大小在程序运行前就已经确定，并且内存区域通常属于只读,某些架构也允许代码段为可写，即允许修改程序。在代码段中，也有可能包含一些只读的常数变量，例如字符串常量等

rodata段：
    
    存放C中的字符串和#define定义的常量

##ARM汇编
[ARM汇编指令集](http://yulin724.wikidot.com/rwpaper:arm-assembly-instructions#toc12)        
####BL
BL是arm汇编中用来调用子程序的指令，它把BL后面一条指令的地址放到R14寄存器里，R15寄存器(PC当前指针地址)就设置成要跳往的地址。这样在这个子程序返回时，再mov PC, R14就可以返回到BL后面的地址了
####BEQ
BEQ指定是跳转指令，但是跳转要满足一定的条件，例：CMP    R1，#0    BEQ  Label    即当R1和0相等的时候程序跳到标号Label处执行。BEQ执行后不调回去。
####LDR/STR
ARM是RISC结构，数据从内存到CPU之间的移动只能通过L/S指令来完成，也就是ldr/str指令。比如想把数据从内存中某处读取到寄存器中，只能使用ldr，比如：   

    ldr r0, 0x12345678

就是把0x12345678这个地址中的值存放到r0中。而mov不能干这个活，mov只能在寄存器之间移动数据，或者把立即数移动到寄存器中，这个和x86这种CISC架构的芯片区别最大的地方。
