---
layout: post
title: Input/Output Routines for Assembly Programming
subtitle: SystemSoftware_06_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# I/O Routines 

## Output
- __print_int__ <br/>
prints out to the screen the value of the integer stored in EAX<br/>
eax에 있는 정수 하나를 출력<br/>
32비트의 부호있는 정수 
- __print_char__ <br/>
prints out to the screen the character whose ASCII value stored in AL<br/>
출력하고자하는 문자의 아스키 코드 값을 AL 레지스터 (8비트)에 넣은 상태에서 print_char 함수 호출하면 문자 하나 출력
- __print_string__ <br/>
prints out to the screen the contents of the string at the address stored in EAX. The string must be a C-type string (i.e. null terminated).<br/>
내가 출력할 문자열은 data segment 에 먼저 저장한 후 (define byte (db)이용)<br/>
문자열의 시작주소를 eax 레지스터에 담은 후에 print_string 함수 호출 <br/>
문자열을 정의할 때 문자열의 맨마지막 문자는 null 문자여야함.<br/>
- __print_nl__ <br/>
prints out to the screen a new line character.<br/>
개행문자 출력<br/>

## Input
- __read_int__ <br/>
reads an integer from the keyboard and stores it into the EAX register.<br/>
키보드로부터 정수를 입력받은 후 eax에 저장됨. <br/>
입력받을 수 있는 정수 크기는 32비트의 부호있는 정수 <br/>
- __read_char__ <br/>
reads a single character from the keyboard and stores its ASCII code into the EAX register.<br/>
하나의 단일 문자를 키보드로부터 입력받은 후 아스키 코드(8비트)로 변환되어 eax(32비트)에 저장됨. <br/>


# Debugging Macros
디버깅 목적으로 제공되는 매크로들 <br/>

## Dump Registers 
__dump_regs__<br/>
prints out the values of the registers (in hex) to the screen. <br/>
32비트를 16진수 형태로 출력 <br/>
8개의 범용 레지스터의 값들과 그 당시의 EIP 값, EFLAGS 값들을 출력<br/>
It also displays the bits set in the EFLAGS register. <br/>
For example, if the zero flag is 1, ZF is displayed. <br/>
If it is 0, it is not displayed. <br/>
ZF = 1 이면 ZF 출력되고, SF = 0 이면, SF 출력되지 않음. <br/>
It takes a single integer argument(인자가 하나 있음) that is printed out as well. <br/>
This can be used to distinguish the output of different dump regs commands.<br/>
dump_regs 를 여러번 호출할 때 각 dump_regs 호출을 구분하는 목적으로 사용됨. <br/>

## Dump Memory Values
__dump_mem__<br/>
prints out the values of a region of memory (in hex) and also as ASCII characters. <br/>
메모리 영역 출력<br/>
출력형태는 호출할때 지정한 메모리 값을 16진수와 아스키 문자 형태로 바꿔서 출력
It takes three comma delimited arguments. (3개의 인자)<br/>
The first is an integer that is used to label the output. <br/>
dump_mem을 여러번 호출할 때 각 dump_mem 호출을 구분하기 위한 목적. <br/>
The second argument is the address to display (This can be a label.) <br/>
The last argument is the number of 16-byte paragraphs to display after the address. <br/>
16바이트 단위로 출력하고자하는 영역의 범위를 지정<br/>
The memory displayed will start on the first paragraph boundary before the requested address.

## Dump Stack
__dump_stack__ <br/>
prints out the values on the CPU stack. <br/>
스택 내용 출력<br/>
The stack is organized as double words and this routine displays them this way. <br/>
32비트 데이터 (double word) 단위로 데이터를 스택에 넣고 빼게됨. <br/>
스택 내부의 어느 지점에 접근할 때에는 ebp 레지스터가 기준점으로 사용됨.<br/>
It takes three comma delimited arguments. (3개의 인자)<br/>
The first is an integer label (like dump regs). <br/>
dump_stack을 여러번 호출할 때 각 dump_stack 호출을 구분하기 위한 목적.<br/>
The second is the number of double words to display below the address that the EBP register holds <br/>
and the third argument is the number of double words to display above the address in EBP.<br/>
ex. dump_ stack 1, 4, 3<br/>
![ssw_ch6_1](https://user-images.githubusercontent.com/63347989/141975635-24e951f8-aa90-4add-926a-b390d245f759.PNG)<br/>




# sample_io.asm
![ssw_ch6_2](https://user-images.githubusercontent.com/63347989/141975978-fdc6b3bf-c552-40b5-9ca9-feb473e13ba7.PNG) <br/>
- %include "asm_io.inc"<br/>
sample_io.asm 파일에 또다른 어셈블리 소스 파일인 asm_io.inc를 include 하라는 의미<br/>
asm_io.inc에는 디버깅용 매크로들 정의가 담겨있음. <br/>
- main:<br/>
entry point가 main임. <br/>
링크할때 gcc 이용해서 해야함.<br/>
- 함수가 시작할 때는 enter 명령, 끝날 때는 leave, ret 명령이 필요 <br/>
- 함수를 호출할 때는 call 함수이름 

![ssw_ch6_3](https://user-images.githubusercontent.com/63347989/141977007-fa84ebcb-9626-486b-89c9-2d56189f1735.PNG)<br/>

## To Run sample_io
![ssw_ch6_5](https://user-images.githubusercontent.com/63347989/141985320-cbfa22ad-ed56-4177-a922-7972b4bed559.PNG)<br/>
![ssw_ch6_4](https://user-images.githubusercontent.com/63347989/141985245-6ac74a50-1595-4fdd-9d6c-9dc503140d01.PNG)<br/>


# skeleton.asm
![ssw_ch6_6](https://user-images.githubusercontent.com/63347989/141985471-2eb27449-6469-430e-83a2-2dd58b80ead4.PNG)<br/>
