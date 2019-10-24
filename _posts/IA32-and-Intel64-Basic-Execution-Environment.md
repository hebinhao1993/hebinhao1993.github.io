---
title: Intel 64 和 IA-32 基本运行环境
date: 2019-06-24 15:14:44
tags:
---

本文是对 *intel 64 and IA-32 Architecture Software Developer's Manual* 中第3章*Basic Excution Environment*的翻译。

本章内容描述了 Intel 64 或 IA-32 汇编语言程序员所能见的基本的执行环境(basic execution environment)。描述了处理器如何执行指令，以及它如何保存和操作数据。本章所讲的执行环境(execution environment)包括内容(地址空间 address space ), 通用数据寄存器(general-purpose data register)， 段寄存器(segment register)， 标志寄存器(flag register)， 指令指针寄存器(instruction pointer register)。

<!-- more -->

## 工作模式 MODES OF OPERATION

IA-32 架构支持3种基本的工作模式： 保护模式(protected mode)， 实地址模式(real-address mode)， 系统管理模式(system management mode)。工作模式决定了哪些指令以及架构特性是否可用

* **保护模式 (Protected mode)**： 该模式是处理器的自然状态(native state)。这种模式有一个特性：它能够在一个保护的，多任务环境中直接执行 “实地址模式 real-address mode”的8086软件。这个特性被称为**虚拟8086模式 virtual-8086 mode**，虽然虚拟8086模式并不是一个真正的处理器模式。虚拟8086模式实际上仅仅是保护模式的一个特性。
* **实地址模式(real-address mode)**： 这种模式实现了 Intel 8086 处理器的编成环境，并且在其基础上有所扩展(比如转换到保护模式或系统管理模式的能力)。上电或重启后，处理器处于实地址模式。
* **系统管理模式(system management mode)**： 该模式为操作系统提供了一个透明的机制来实现平台特定的汉书，比如 power mannagement 和 system security。 当外部SMM interrupt pin (SMI#) 激活时，或者从APIC接受到SMI时，处理器就会进入系统管理模式。处于该模式时，处理器会保存程序的基本上下文，并且拥有一个独立的地址空间。

### Intel 64 架构

Intel 64 架构增加了 IA-32e mode。 IA-32e mode 有2种子模式：

* **兼容模式 (Compatability mode)**： 兼容模式保证了遗留下来的16-bit和32-bit应用无需重新编译就能够运行在 64-bit 操作系统。
* **64bit mode**：这种模式使一个 64-bit 操作系统能够运行可访问 64-bit 线性地址空间的程序。

## 基本运行环境概述 OVERVIEW OF THE BASIC EXECUTION ENVIRONMENT

任何运行在 IA-32 处理器的程序拥有一组资源来执行指令以及保存代码、数据和状态信息。这些资源组成了 IA-32 处理器的基本运行环境。Intel 64 处理器支持 IA-32 的基本运行环境，以及一个类似的环境(IA-32e mode下 能够执行 64-bit 程序和 32-bit 程序。)

* **地址空间 Address Space**： 任何运行在 IA-32 处理器的程序能够寻址到一个最大4GBytes(2^32 bytes)的线性地址空间，以及一个最大64GBytes(2^36 bytes)的物理地址空间。
* **Basic program execution registers**: 8个32-bit通用寄存器(GPRs)， 6个16-bit段寄存器， 32-bit EFLAGS 寄存器， 32-bit EIP寄存器组成了执行一组通用指令的基本运行环境。
* **x87 FPU registers**： 略
* **MMX registers**： 略
* **XMM registers**： 略
* **YMM registers**： 略
* **Bounds registers**： 略
* **BNDCFGU and BNDSTATUS**： 略
* **Stack**： 栈执行了调用子程序以及参数传递的存在。

除了在这些基本运行环境提供的资源外， IA-32 架构还提供了以下的资源，作为系统架构的一部分。

* **I/O Port**： 略
* **Control register**： 5个控制寄存器(CR0 到 CR4)决定了处理器的工作模式以及当前执行任务的特性。
* **Memory management register**： GDTR，IDTR，任务寄存器(task register)，LDTR共同决定了保护模式下内存管理的数据结构的位置。
* **Debug register**： DR0到DR7
* **Memory type range registers (MTRRs)**： 略
* **Machine specific registers (MSRs)**： 处理器提供了许多特定的寄存器，用来控制和报告处理器的性能。
* **Machine check register**： Machine check registers 包含了一系列的control, status, error-reporting MSRs。用来检测和报告硬件错误。
* **Performance monitoring counters**： 略

### 64-Bit Mode Execution Environment

* **地址空间 Address Space**： 任何运行在 IA-32 处理器，处于64-bit模式的程序能够寻址到一个最大2^64 bytes的线性地址空间，以及一个最大2^52 bytes的物理地址空间。软件能够查询 CPUID 以查看处理器所支持的物理地址大小。
* **Basic program execution registers**: 16个64-bit通用寄存器(GPRs)， 6个16-bit段寄存器， 64-bit RFLAGS 寄存器， 64-bit RIP寄存器组成了执行一组通用指令的基本运行环境。
* **XMM registers**： 略
* **YMM registers**： 略
* **BND registers, BNDCFGU and BNDSTATUS**： 略
* **Stack**： The stack pointer is 64 bit. Stack size is not controlled by a bit in the SS descriptor(as it in non-64-bit modes) nor can the pointer size be overridden by an instruction prefix.
* **Control registers**： 控制寄存器被扩展到64-bit。同时添加了一个新的控制寄存器(the task priority register: CR8 or TPR)。
* **Debug registers**： 略
* **Descriptor table register**： GDTR(global descriptor table register) 和 IDTR(interrupt table register) 扩展到 10 bytes，所以它们能够处理 64-bit的基地址。 LDTR(local descriptor table register) 和 TR(task register) 同样扩展到能够处理 64-bit地址。

## 内存组织 memory organization

处理器总线上寻址的内存称为 **物理内存(physical memory)**。物理内存是一个 8-bit byte 的序列。每个byte都有一个唯一的地址，称为 **物理地址(physical address)**。如果处理器不支持64位架构，那么 **物理地址空间(physical address space)** 的范围从0到最大2^36-1，即64G Bytes。64位架构会有所不同。

### IA-32 memory models

IA-32 有3种内存模型，分别是**Flat memory model**， **Segmented memory model**， **Real-Address mode memory model** 。

* **平坦模型(Flat memory model)**：对程序而言，内存就好像一个平坦的，连续的地址空间，这个空间被称为**线性地址空间 Linear Address Space**。 代码、数据以及栈都包含在这个地址空间内。**线性地址空间 Linear Address Space**可以字节寻址的，其地址范围从0到2^32-1。在这个地址空间内的任何一个字节的地址被称为**线性地址 Linear Address**。

* **分段模型(Segmented memory model)**：对程序而言，内存是一组独立的内存空间，这些内存空间被称为段(segments)。 代码、数据、栈通常被包含在不同的段中。访问某个段的某个字节，程序需要一个logical address。 这个logcial address包含了一个segment selector和一个offset。(logical address通常被称作far pointer)。IA-32可以有16383个不同类型和大小的segments，每个segment最大可以有2^32 bytes。

对于芯片内部，系统定义的所有segment都映射(map)到处理器的线性地址空间。访问一个内存地址，处理器会将每个logical address转换成linear address。这个转换对应用程序而言是透明的。

* **实地址模式内存模型(Real-Address mode memory model)**：

这是Intel 8086处理器的内存模型。这个模型仅为了兼容性而存在。略。

### 分页与虚拟内存 (paging and virtual memory)

无论是Flat memory 还是 segmented memory， 线性地址空间总是映射到物理地址空间，或直接或通过分页。当直接映射的时候，每个线性地址空间都与物理地址空间一一对应。当使用IA-32结构的分页机制，线性地址空间被分页， 映射到虚拟地址空间。 虚拟地址空间然后再安需映射到物理地址空间。分页机制对应用程序是透明的。所有的应用程序看到的都是它的线性地址空间。

此外， IA-32 架构的分页机制还支持：

* **Physical Address Extensions (PAE)**： PAE 支持对大于 4 GBytes 的物理地址空间寻址。
* **Page Size Extension**： 4 MByte page

### Memory Organization in 64-Bit Mode

Intel 64位架构支持物理地址空间大于64GByte； IA-32 处理器的实际物理地址要看具体实现。在 64-bit mode 中，架构是支持 64-bit 线性地址的，但支持64-bit的处理器可能只实现了小于 64-bit的。线性地址空间映射到物理地址空间是通过 PAE 分页机制来实现的。

### 工作模式 vs. 内存模型(Modes of Operation vs. Memory Model)

当为IA-32或者Intel 64 处理器编写代码时，程序员需要了解当执行该代码时，处理器将处于什么operation mode 以及 何种memory model将被使用。处理器的operation mode与memory model的关系如下：

**保护模式(Protected mode)**: 处理器处于保护模式时，能够使用任何内存模型。(real-addressing mode memory model在virtual-8086下使用)。

**实地址模式(Real-address mode)**: 处理器处于实地址模式时，只支持实地址模式内存模型(real-address mode memory model)。

**系统管理模式(System managemenr mode)**： 当处于SMM，处理器使用一个单独的地址空间，称为the system management RAM(SMRAM)。这种memory model与real-address mode model 有点类似。

**兼容模式(Compatibility mode)**： 兼容模式下与 32-bit 保护模式的内存模型一致。分段(segmentation)机制与 32-bit 保护模式的分段机制是一样的。

**64-bit mode**： 分段通常是关闭了(并不完全)，使用平坦的 64-bit 线性地址空间。 特别提出， 64-bit mode中， 处理器把CS，DS，ES和SS的段基址(segement base)当作是0。这将线性地址等效于一个有效地址(effective address)。在64-bit mode 中， 分段模型和实地址模式内存模型是不可用的。

### 32-bit地址与运算数 和 16bit地址与运算数 (32-Bit and 16Bit Address and Operand Sizes)

处于保护模式时， IA-32 处理器能够配置位 32-bit 的地址(address)以及操作数(operand size)， 或者 16-bit 的地址(address)以及操作数(operand size)。如果是 32-bit 的地址和操作数，那么最大线性地址 或者 segement offset 是 FFFFFFFFH (2^32-1); 操作数通常是 8-bit 或 32 bit。 如果是 16-bit的地址和操作数，最大的线性地址 或者 segement offset 是 FFFFH (2^16-1); 操作数通常是 8-bit 或者 16-bit。

当使用 32-bit 寻址时， 一个logical address(far point)由一个 16-bit segment selector 和一个 32-bit 的 offset 组成。
当使用 16-bit 寻址时， 一个logical address(far point)由一个 16-bit segment selector 和一个 16-bit 的 offset 组成。

指令前缀(instruction prefixes)允许临时覆写(override)默认的地址大小(address size) and/or 操作数大小(operand size)。

当处于protected mode时， 当其执行的代码所属的segment descriptor定义了其默认的address and operand size。segment descriptor是一个系统的数据结构，对应用程序而言通常不可见。汇编指令(assembler directives)允许为程序选择默认的addressing and operand size。然后汇编器(assembler)以及其他工具可以建立起合适的segment descriptor。

当处于real-address mode时，默认的addressing and operand size是16 bits。 An address-size override can be used in real-address mode to enable 32-bit addressing. However, the maximum allowable 32-bit linear address is still 000FFFFFH (2^20-1)。

### Extented Physical Addressing in Protected Mode

从 P6 系列开始， IA-32 架构支持寻址最大 64 GBytes (2^36 bytes)的物理内存。应用程序并不直接在物理内存上寻址，它们在一个最大 4 GBytes的线性地址空间中寻址。通过虚拟内存管理机制，将线性地址空间映射到物理地址空间。

### Address Calculation in 64-Bit Mode

在绝大多数情况，64-bit mode 为代码、数据、栈一个平坦的地址空间。在 64-bit mode (如果没有address-size override)，有效地址(effective address)是 64-bit 的。一个有效地址的计算使用一个 64-bit 的基地址和变址寄存器(index registers)和标记扩展位移(sign-extend displacements)。

64-bit mode的平坦地址空间中，线性地址等效与有效地址，因为基地址为0。但当FS和GS段使用了一个非0的基地址时，这个规则就会失效。在 64-bit mode 中，在加上完整的 64-bit 基地址前，有效地址模块已经被添加，并且有效地址被截断了。基地址从来不会被截断。

64-bit mode 中 instruction pointer 被扩展到 64-bit。64-bit 的 instruction pointer 被称为 RIP。

通常来说， 64-bit mode 中移位(displacements)和立即数(immediates)并没有扩展到 64-bits。它们依然被限制为 32-bit, 并且在有效地址计算中是符号扩展(sign-extended)。但是在 64-bit mode 中， 64-bit 的移位和立即数依然在 MOV 指令中提供了。

### 规范寻址(Canonical Addressing)

由于处理器不一定完全实现 64-bit 线性地址，如最早实现 Intel 64 架构的 IA-32 处理器只支持 48-bit 线性地址。那么在 64-bit 模式，一个地址有一定的规范，若处理器只实现了 48-bit 线性空间，那么从 63 bit 到 47 bit(msb, most significant implemented bit)， 必须全为0或者全为1 (取决于 bit 47)。这种形式的地址称为规范形式。

## 基本程序执行寄存器 BASIC PROGRAM EXECUTION REGISTERS

IA-32 架构提供16个基本的寄存器：

* **General-purpose registers**： 8个用来保存操作数和指针的寄存器。
* **Segment registers**： 6个段选择器。
* **EFLAGS(program status and control) register**： EFLAGS 寄存器报告了程序运行的状态，以及允许的限制。
* **EIP(instruction pointer) register**： 32-bit 指向下一个执行指令。

### General-Purpose Registers

32-bit 通用寄存器 EAX, EBX, ECX, EDX, ESI, EDI, EBP 和 ESP 用来保存逻辑和数学运算的操作数，地址计算的操作数，内存指针。

虽然所有这些寄存器都是通用的，但是 ESP 寄存器还被用来保存栈指针，因此ESP不应随意当成通用的寄存器。

* **EAX**： 操作数和结果数据的累加器
* **EBX**： 指针，指向DS段中的数据
* **ECX**： 字符串和循环的计数器
* **EDX**： I/O pointer
* **ESI**： 指针，指向DS段中的数据；字符串操作的来源指针
* **EDI**： 指针，指向ES段中的数据；字符串操作的来源指针
* **ESP**： 栈指针(SS段)
* **EBP**： 指针，指向栈上的数据(SS段)

#### General-Purpose Registers in 64-Bit Mode

64-bit mode 中，有16个通用寄存器，且默认的操作数是 32-bit 的。但是通用寄存器能够处理 32-bit 或者 64-bit 操作数。 如果指定了 32-bit 操作数， 那么 EAX，EBX，ECX，EDX， EDI， ESI， EBP， ESP， R8D - R15D是可用的。如果指定了 64-bit 操作数， 那么 RAX, RBX, RCX, RDX, RDI, RSI, RBP, RSP, R8 - R15 是可用的。 R8D - R15D / R8 - R15 代表了8个新的通用寄存器。

略

### Segment Registers

略

#### Segment Registers in 64-Bit Mode

在 64-bit mode， CS，DS，ES，SS被认为其基地址为0。 FS， GS例外。

略

### EFLAGS Register

略

#### Status Flags

略

#### DF Flag

略

#### System Flags and IOPL Field

略

#### RFLAGS Register in 64-Bit Mode

略

## INSTRUCTION POINTER

略

### Instruction Point in 64-Bit Mode

略

## OPERAND-SIZE AND ADDRESS-SIZE ATTRIBUTES

处理器处于保护模式时，每个代码段都有一个默认的操作数大小(operand-size)属性和地址大小(address-size)属性。这些属性通过段描述副中的 D 标志来决定。当 D 标志被设置，那么将会使用 32-bit 操作数大小属性和地址大小属性。当 D 标志 被清除， 那么将会使用 16-bit的属性。

操作数大小(operand-size)属性决定了操作数的大小，当 16-bit 操作数大小属性决定了处理器能使用 8-bit 或者 16-bit 的操作数。32-bit 操作数大小属性决定了处理器能使用 8-bit 或者 32-bit 的操作数。

同理，16-bit 地址大小属性决定了段偏移(segment offset)和移位(displacement)是 16-bit 的， 一个段被限制在 64KBytes 内。 32-bit 地址大小属性决定了段偏移(segment offset)和移位(displacement)是 32-bit 的， 允许寻址 4GBytes 空间。

可以通过指令前缀修改某条指令的默认操作数大小属性和地址大小属性。

### Operand-Size and Address-Size in 64-Bit Mode

64-bit mode 中，默认的地址大小是 64-bit， 默认的操作数大小是 32-bit。当然同样可以用指令前缀来修改默认值。

略

## OPERAND ADDRESSING

IA-32 架构的指令可以有0个或多个操作数。操作数来源可以是：

* 指令自身(立即操作数， immediate operand)
* 寄存器
* 内存
* I/O 端口

当一条指令要返回数据到一个目标操作数时，可以返回到：

* 寄存器
* 内存
* I/O 端口

### Immediate Operands

略

### Register Operands

源操作数和目标操作数(source operand and destination operand)可以是以下任意寄存器，取决于执行的指令：

* 32-bit 通用寄存器(EAX, EBX, ECX, EDX, ESI, EDI, ESP, EBP)
* 16-bit 通用寄存器(AX, BX, CX, DX, SI, DI, SP, BP)
* 8-bit 通用寄存器(AH, BH， CH, DH， AL， BL， CL， DL)
* 段寄存器(CS, DS, SS, ES, FS, GS)
* EFLAGS寄存器
* x87 FPU寄存器
* MXX 寄存器
* XMM 寄存器
* 控制寄存器(CR0 - CR4) 系统表指针寄存器(GDTR， LDTR， IDTR, task register)
* debug 寄存器
* MSR 寄存器

#### Register Operands in 64-Bit Mode

* 64-bit 通用寄存器(RAX, RBX, RCX, RDX, RSI, RDI, RSP, RBP, R8 - R15)
* 32-bit 通用寄存器(EAX, EBX, ECX, EDX, ESI, EDI, ESP, EBP，R8D - R15D)
* 16-bit 通用寄存器(AX, BX, CX, DX, SI, DI, SP, BP, R8W - R15W)
* 8-bit 通用寄存器(使用 REX 前缀可用：AL，BL，CL，DL，SIL，DIL，SPL，BPL， R8L - R15L；不使用 REX 前缀可用：AL，BL，CL，DL，AH，BH，CH，DH)
* 段寄存器(CS, DS, SS, ES, FS, GS)
* RFLAGS寄存器
* x87 FPU寄存器
* MXX 寄存器
* XMM 寄存器
* 控制寄存器(CR0 - CR4 以及 CR8) 系统表指针寄存器(GDTR， LDTR， IDTR, task register)
* debug 寄存器
* MSR 寄存器
* RDX:RAX寄存器对，代表一个 128-bit 操作数

### Memory Operands

略

#### Memory Operands in 64-Bit Mode

略

### Specifying a Segment Selector

略

#### Specifying a Segment Selector in 64-Bit Mode

略

### Specifying an Offset

* **Displacement**： 一个 8-bit 或 16-bit 或 32-bit的值。
* **Base**： 在通用寄存器中的值
* **Index**： 在通用寄存器中的值
* **Scale factor**： 2 或 4 或 8

offset = Base + (Index * Scale) + Displacement

#### Specifying an Offset in 64-Bit Mode

* **Displacement**： 一个 8-bit 或 16-bit 或 32-bit的值。
* **Base**： 在 64-bit 通用寄存器中的值
* **Index**： 在 64-bit 通用寄存器中的值
* **Scale factor**： 2 或 4 或 8

* **RIP + Displacement**

### Assembler and Compiler Addressing Modes

略

### I/O Port Addressing

略
