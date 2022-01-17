---
layout: post
title: Introduction of System Software 3
subtitle: SystemSoftware_01_3
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# SOFTWARE의 구분

1. 응용 소프트웨어 : 최종 사용자를 목적을 만족시키는 소프트웨어 (프로그램)
2. 시스템 소프트웨어 : 다른 프로그램 또는 소프트웨어가 필요한 기능을 제공<br/>
    - ex. OS(운영체제), 프로그램을 개발하는데 필요한 소프트웨어 <br/>
    - 시스템 소프트웨어는 컴퓨터 하드웨어 상에서 직접 동작하는 경우가 많음 <br/>
    - 시스템 프로그래밍: 시스템 소프트웨어의 기능을 이용하여 기계를 직접 작동하는 프로그램을 작성하는 일

# SYSTEM SOFTWARE
## 1. Assembler
프로그램이 컴퓨터 상에서 실행가능하려면, 프로그램이 기계어로 표현되어야 한다. <br/>
왜냐하면 기계어만이 CPU가 직접적으로 이해하고 실행할 수 있는 유일한 언어이기 때문이다. <br/>
하지만, 기계어는 2진수 형태라서 다루기 어렵다. <br/>
기계어의 대안으로 나온 것이 __어셈블리어(Assembly language)__ 이다. <br/>
<br/>
어셈블리어 특징<br/>
1. mnemonic instructions : 연산기호 사용<br/>
2. symbolic address <br/>

어셈블러 : 어셈블리어를 자동적으로 기계어로 번역하는 프로그램

![ssw_ch1_13](https://user-images.githubusercontent.com/63347989/136045622-fae9ade9-391a-4707-bff4-9f1e4a13a1cd.PNG)

어셈블리어를 컴퓨터에서 바로 실행할 수 없어서 <br/>
어셈블러를 통해 기계어 코드로 변환한 후, 컴퓨터에서 실행한다. <br/>

## 2. Linker
 여러 개의 object 모듈간의 상호 기억 장소 참조를 정리하여 함께 실행될 수 있도록 한다.

![ssw_ch1_15](https://user-images.githubusercontent.com/63347989/136048754-cca7a419-2e5e-47ec-9e6b-7eaf76fbfb88.PNG)
<br/>

큰 프로그램을 개발할 때, 개발 상의 편의를 위해서 object 모듈로 쪼갠다. <br/>
궁극적으로 이 프로그램이 실행되려면 합쳐져야 하는데,<br/>
합치는 과정에서 서로간 참조를 정리하여 실행단위로 합친다. <br/>
→ 이 과정을 수행하는 시스템 소프트웨어가 Linker이다. <br/>
&nbsp; &nbsp; Linker의 output이 최종 프로그램 (실행가능한 단위)


## 3. Loader
load module을 기억장치에 적재하는 프로그램<br/>
Linker가 만들어낸 실행가능한 프로그램을 load module에 저장한다. <br/>
Loader는 load module을 메인 메모리로 읽어들여서 프로그램을 실행하는 시스템 소프트웨어이다. <br/>
<br/>

어셈블러가 로더의 기능까지 포함하는 경우
- 기억 장소의 낭비
- 매번 번역하는 번거로움이 있음

## 4. Macro Processor
매크로 호출을 매크로에서 정의된 원래 명령어로 대치<vr/>
macro : 미리 정의된 절차에 따라서 입력 sequence가 출력 sequence 으로 변환이 되는 규칙, 패턴 <br/>

## 5. Compiler
고급언어 프로그램을 받아들여 목적프로그램을 만든다. <br/>
고급언어 프로그램과 동등한 역할을 하는 기계어로 이루어진 프로그램 <br/>
linker의 입력이 되기도 한다.

## 6. Interpreter
고급언어 프로그램의 각 문장을 기계어로 바꿈<br/>
ex. Java
<br/>

__GNU C Compiler__<br/>
C 언어의 개발과정 중 컴파일 단계 <br/>

![ssw_ch1_14](https://user-images.githubusercontent.com/63347989/136042952-b9d19c7c-3c93-4345-bc41-d9b018c0c74c.PNG)<br/>
preprocessor : 전처리 (macro processor 동작)<br/>
prog.i : C 언어 소스코드 형태의 text 파일 <br/>


# NASM
Netwide Assembler <br/>
- open source assembler that runs under Linux and DOS<br/>
- debugging에 유리, 많은 기능을 가짐<br/>
- 산업계에서도 많이 사용됨<br/>
- http://nasm.sourceforge.net/ <br/>


# 운영체제
운영체제 : 하드웨어를 관리하고, 응용프로그램에 실행 환경을 제공하는 시스템소프트웨어
## 운영체제의 기능
- Process management
- Memory management
- CPU scheduling
- File & I/O subsystems
- Disk scheduling
- UtilitieS
## Linux Operating System
Unix Operating system
- 하드웨어 제조사와 독립적인 최초 운영체제
- Bell Labs에서 개발
- AT&T version
- BSD(Berkeley Software Divion) version
Linux
- non-proprietary version of Unix
- created by Linus Torvals
- mostly installed on Intel-based hardware
