---
layout: post
title: Introduction to Assembly Programming 2
subtitle: SystemSoftware_04_2
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# INSTRUCTION
x86 CPU 같은 경우에는 300개가 넘는 명령들을 가지고 있음.<br/>
예제를 이해하기 위한 최소한의 instruction.

## The MOV instruction
operand의 개수: 2<br/>
기능: data copy<br/>
operand의 형태에 따라서 다양한 MOV가 있을 수 있는데 
- MOV reg, imm<br/>
reg에 imm 대입, reg 값을 특정한 상수로 설정
    - Reg stands for register
    - Imm stands for immediate value<br/>
    명령 자체에 포함된 상수
    - Example
        - MOV EAX, 54 (10진수)
        - MOV AX, 036H (16진수) <br/>
        AX의 값은 명확하지만 036H의 값은 명확하지 않으므로 명확한 값에 맞춤.
        - MOV AL, ‘A’(A의 ascii 코드값 8비트)
        - MOV AL, -129 ; not valid<br/>
        부정확한 명령, 오류 발생<br/>
        AL이 8비트 이기 때문에 부호 있는 정수 (-129)도 8비트로 표현 가능해야함.<br/>
        부호 있는 수는 2의 보수로 표현하고 8비트 부호 있는 수로 표현 가능한 가장 작은 수는 -128 임.
        - MOV AL, 999 ; not valid<br/>
        8비트 부호 없는 정수로 표현 가증한 가장 큰 수는 255임. <br/>

 - MOV reg, reg
    - Copies from the second reg into the first one <br/>
    두번째 register 값을 첫번째 regiter 값으로 복사함. <br/>
    첫번째 reg = destination operand - 연산을 실행한 후에 연산의 결과가 담기는 operand <br/>
    두번째 reg = source operand - 연산 과정에서 그 값만 사용하는 operand
    - Example
        - MOV EAX, EBX <br/>
        두 레지스터 모두 32 bit임.
        - MOV EBX, DX ; not valid  <br/>
        EBX 는 32 비트, DX 는 16비트 라서 오류 발생.
 
 - Ambiguity problem: MOV BL, AH
    - AH : register ‘AH’ or hex number ‘A’ 헷갈림
    - NASM requires all hex numbers begin with one of the digits 0, 1, …, 9. →  AH is the name of register! <br/>
    16진수 숫자는 무조건 수로 시작하게 돼있음. <br/>
    만약 AH가 16진수가 되려면, 0AH가 되었어야함. 

## Addition Instruction
operand의 개수: 2<br/>
기능: add<br/>
operand의 형태에 따라서 다양한 ADD가 있을 수 있는데 
 - ADD reg, imm<br/>
 reg - destination operand, imm - source operand
    - Add the immediate value imm to the register reg
    - Example
        - ADD BL, 10 ; let BL = BL + 10

- ADD reg, reg
    - Add the contents of the second register to the first one<br/>
    두번째 레지스터의 값을 첫번째 레지스터에 더해라  
    - Example
        - ADD BL, AL ; let BL = BL + AL <br/>
        BL is changed and AL is not changed !!

## Subtraction Instruction
operand의 개수: 2<br/>
기능: subtract<br/>
operand의 형태에 따라서 다양한 SUB가 있을 수 있는데 
 - SUB reg, imm
    - Subtract the immediate value from the register
    - Example
        - SUB BL, 10 ; let BL = BL - 10
 - SUB reg, reg
    - Subtract the contents of the second register from the first
    - Example
        - SUB BL, AL ; let BL = BL – AL <br/>
        BL is changed and AL is not changed

## Multiplication Instruction
operand의 개수: 1<br/>
기능: multiply<br/>
8비트 곱셈(8비트 수 2개) → 결과값 16비트<br/>
16비크 곱셈(16비트 수 2개) →  결과값 32비트<br/>
32비트 곱셈(32비트 수 2개) → 결과값 64비트<br/>

![ssw_ch4_4](https://user-images.githubusercontent.com/63347989/137353750-f8fe33b5-f139-4d15-bee1-d5603675e753.PNG)<br/>

- MUL reg
    - Multiplier is in reg(=Q)
    - Multiplicand is always in __A register(=P)__ and result are always in __A and D register__ <br/>
    명령 자체에는 명시적으로 표시되어 있지 않음. = implied operand
    ![ssw_ch4_5](https://user-images.githubusercontent.com/63347989/137355385-efc5c45e-f0ed-48b3-afc0-0f0b0f5cb785.PNG)<br/>
    반은 D 레지스터에, 반은 A 레지스터에 저장됨. <br/>
    32비트를 한번에 저장하면 되지 왜 반씩 나누어 저장하냐? <br/>
    CPU가 32비트 모드에서 동작하더라도 예전 16비트 곱셈과의 호환성을 유지하기 위해서 !
    - MUL command has no immediate form<br/>
    MUL 명령에 상수를 쓸 수 없음. 
    - Example
        - MUL BH (8비트 곱셈) ; let AX = AL x BH
        - MUL BX (16비트 곱셈); let DX:AX = AX x BX<br/>
        DX:AX는 16비트씩 나누어 저장했다는 것. 
        - MUL EBX (32비트 곱셈) ; let EDX:EAX = EAX x EBX

## Division Instruction
operand의 개수: 1 <br/>
기능: divide <br/>
![ssw_ch4_6](https://user-images.githubusercontent.com/63347989/137356526-bb2489f0-21f9-473b-8e0a-fe75af81c05e.PNG) <br/>

- DIV reg(=Q)
    - DIV resemble MUL syntax; and is essentially its inverse<br/>
    ![ssw_ch4_7](https://user-images.githubusercontent.com/63347989/137356731-84e728bf-23cb-4c39-bbad-a0eaf50a0c92.PNG)<br/>
    32 bit-form → 64bit 수 P(dividend)를 32 bit 수 Q로 나눗셈을 하고 몫(quitient)과 나머지(remainder)가 각각 32bit가 됨.  <br/>
    나뉘는수 (=P)가 implied operand<br/>
    - Example<br/>
    8 비트 나눗셈
        - When AX = 17, BH = 3<br/>
        DIV BH ; AH = 2 and AL = 5

# Constants 상수의 표현
NSAM 어셈블러 규칙으로 소개

- Numeric constants 숫자 상수<br/>
4가지: 10진수, 16진수(표현하는 방법 3가지), 8진수(표현하는 방법 2가지), 2진수<br/>
![ssw_ch4_8](https://user-images.githubusercontent.com/63347989/137357733-34b5b848-f199-47c2-ad22-8a3509b6e91d.PNG)<br/>

- Character constants 문자 상수
    - A character constant consists of up to four characters enclosed in either single or double quotes.<br/>
    ' 나 " 로 막으면 문자 상수가 됨.<br/>
    1글자(8bit), 2글자(16bit), 4글자(32bit)만 문자 상수임.<br/> 
    ex. 'A', "A", 'AB', 'ABCD' 
    - A character constant with more than one character will be arranged with __little−endian order__ in mind<br/>
    1글자이상, 즉 2글자, 4글자가 문자상수로 다루어진 경우에 메모리 상의 표현은 little_endian order를 따름.<br/>
    ![ssw_ch4_11](https://user-images.githubusercontent.com/63347989/137359374-a96e29c7-6309-492a-b044-5c4ef93069a1.PNG)<br/>
    abcd - 0x64(d)63(c)62(b)61(a)


- String constants 문자열 상수<br/>
문자열: 임의의 개수(1,2,4 제외)의 문자가 연속적으로 이어짐.<br/>
일반적인 명령에서 operand로 사용될 수 없음. <br/>
ex. mov eax, 'xyz' → 크기가 맞지않음 → 불가능
    - only acceptable to some __pseudo−instructions__ <br/>
    ![ssw_ch4_9](https://user-images.githubusercontent.com/63347989/137357970-c6133860-ed89-4c4f-b33d-9ffa08f85e60.PNG)<br/>
    최대 크기의 문자 상수의 연속

-  Floating-point constants<br/>
    - pseudo-instruction(dd, dq)의 operand로 사용될 수 있음. 
![ssw_ch4_10](https://user-images.githubusercontent.com/63347989/137357965-d8b0bd72-8007-48ad-9342-f4e3afaf8d8b.PNG)<br/>
부동소수점 표현은 숫자 __.__ 일련의 숫자 or e  지수 