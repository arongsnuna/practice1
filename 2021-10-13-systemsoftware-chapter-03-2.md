---
layout: post
title: Intel IA-32 Architecture 2
subtitle: SystemSoftware_03_2
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---
# IA-32 Architecture
- __Computer architecture__ is concerned with how the CPU acts and how it uses computer memory. <br/>
컴퓨터 architecture라는 말은 CPU가 어떻게 동작을 하는지, 어떤 방식으로 메모리를 사용하는지, 일컫는 말<br/>
즉 전체적인 시스템 디자인 <br/>
- It includes instruction set architecture (ISA), microarchitecture, system design including all the other h/w components. <br/>
    - CPU가 실행할 수 있는 명령들이 어떤것들이 있는지 __ISA__ 에 저장<br/>
    CPU에서 프로그래밍하려면 __ISA__ 에 대해서 숙지하고 있어야함 <br/>
    - __microarchitecture__ : ISA가 CPU 내에서 내부적으로 어떻게 구현되어있는지 구현방식<br/>
    microarchitecture = computer organization <br/>
    하나의 ISA를 여러가지 다양한 microarchitecture로 구현할 수 있음<br/>
    Instruction 수준의 호환성은 유지되지만 micro architecture는 지속적으로 바뀜 <br/>
    즉, 같은 명령을 구현하는데 내부방식이 세대별로 달라졌음을 의미함
    - System design 이라는 것은 cpu가 다른 하드웨어 구성요소들과 어떻게 interaction하는지를 일컫는 말 <br/>
    우선 CPU는 메모리나 입출력 장치들과 시스템 버스로 연결되어 있음  
- __IA-32 architecture__ and __Intel 64 architecture__ are the names of computer architecture starting from the Intel 8086 processor to the latest Intel Core 2 Duo and Intel Xeon processor 5100 series 
    - Intel 64 / x86-64 / AMD64
    - Intel 64 architecture= x86-64 =AMD64

# IA-32 Basic Execution Environment
![ssw_ch3_6](https://user-images.githubusercontent.com/63347989/137019669-0063d74c-e858-4322-a416-2d5e53644cb6.PNG)

## 1. Address space
- 주소 공간은 Main memory 공간을 일차원적, flat 한 형태로 사용하거나 segment 라는 것을 기본단위로 사용할 수 있음 <br/>
- 어떤 형태로 사용하든지 간에 각 응용 프로그램에 주어지는 전체 주소공간은 2^32=4GB<br/>
32bit → 프로그램이 숫자를 지정할 때 사용하는 메모리 주소 지정하는 비트수<br/>
4GB → 가상메모리이기 때문에 실제  PC에 달려있는 실제 메모리인 ram의 크기와는 상관이 없고 <br/>
multitasking이라고 해서 동시에 여러 프로그램이 실행하는 경우라고 해도,<br/>
모든 프로그램을 다합쳐서 4GB가 아니고 각 프로그램마다 4GB가 주어짐 <br/>
- Pa 기능 = physical address space라는 기능<br/> 
32비트가 4비트 늘어나서 36비트가 됨 <br/>
Pa를 사용하는 경우에 2^36 -> 64GB까지 메모리를 사용할 수 있음<br/>
- Any task or program running on an IA-32 processor can address a linear address space of up to 4 GB (2^32 bytes) and a physical address space of up to 64 GB (2^36 bytes). 

## 2. Basic program execution registers
- 응용 프로그램이 실행 도중에 접근 가능한 기본적인 register 집합들 <br/>
- The 8 general-purpose registers<br/>
    - 32bit크기 
    - user visible register
    - 프로그래머가 자기 마음대로 쓸수 있음)
- The 6 segment registers
    - 메모리사용관련
    - 4GB 가상 메모리공간을 segment단위로 나눠서 사용하게 되는데 각 segment에 대한 정보를 이 레지스터에 담아서 관리를 하게 됨
    - 크기는 16비트
    - 한 프로그램이 4GB메모리를 사용을 하면서 최대한 가질 수 있는 segment의 개수가 6개까지다  
- The EFLAGS register
    - control and status register
    - 32bit
    - 현재 cpu의 상태정보를 담고 있음)
- The EIP register
    - 다음번 실행할 명령의 위치를 담고있는 포인터를 가지고 있는 레지스터)
- to execute a set of general-purpose instructions. 
    - integer arithmetic
    - program flow control(프로그램 흐름 제어)
    - operate on bit and byte strings(비트단위, 바이트단위 연산)
    - address memory
- 프로그램이 실행하는 도중에 레지스터가 직접 접근해서 사용할 수 있는, 프로그래밍 가능한 레지스터임
- 프로그래밍 가능하지 않는 레지스터도 있느냐? 예쓰 

## 3. FPU Register
부동소수점 연산 명령이 사용하는 전용 register <br/>
- The eight x87 FPU data registers(80비트 크기), the x87 FPU control register(16비트 크기), the status register(16비트 크기), the x87 FPU instruction pointer register(48비트 크기), the x87 FPU operand (data) pointer register(40비트 크기), the x87 FPU tag register(16비트 크기), and the x87 FPU opcode register(16비트 크기) for floating-point values 
- X87은 X86 CPU가 포함하고 있는 부동소수점 연산을 수행하는 프로세서
- 이 보조프로세서를 통해서 프로그램이 부동소수점 연산을 처리하는 전용 명령 실행시킬 수 있음

## 4. MMX Registers
- SIMD instruction들이 사용하는 register 가 두가지가 있는데 그 중 첫번째 종류임<br/>
SIMD instructions: 한번에 64bit 크기의 큰 데이터를 처리할 수 있는 특수 명령
- The eight MMX registers support execution of single-instruction, multiple-data (SIMD) operations on 64-bit packed integers
- IA-32 가 기본적으로 32비트 연산을 수행하지만 <br/>
멀티미디어 같이 크기가 큰 데이터에 대한 연산은 pentium3부터 SIMD instruction 을 두고 SIMD instruction을 통해 크기가 큰 데이터를 처리함 
- MMX 레지스터 같은 경우는 이 SIMD instruction을 실행할 때 사용하는 전용 레지스터 
- 크기가 64비트  

## 5. XMM Registers
- SIMD instruction들이 사용하는 register 가 두가지가 있는데 그 중 두번째 종류임<br/>
- The eight XMM data registers and the MXCSR register support execution of SIMD operations on 128-bit packed integers

## 6. Stack 
- To support procedure or subroutine calls and the passing of parameters between procedures or subroutines, a stack and stack management resources are included in the execution environment
- IA-32에 동작하는 모든 프로그램은 자신이 사용하는 주소공간 내의 반드시 일정영역의 스택을 가지고 있어야 함
- 스택은 실행하는 프로그램이 함수 사용하는 경우에 스택이 필요함 
- 스택에는 함수가 호출될 때 함수의 파라미터를 담기도 하고 함수가 실행이 끝나고 복귀할 때 복귀유저등 관리
내 응용프로그램은 함수 하나로 이루어진 단순한 프로그램이라도 스택이 필요함 
- 스택은 프로그램이 시작할 때 무조건 주어지는 거다
