---
layout: post
title: Machine Language
subtitle: SystemSoftware_11_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# 10. Machine Language
- The language seen by the processor<br/>
cpu가 직접 접하게 되는 언어<br/>
바이너리 코드로 이루어진 cpu instruction<br/>
- Assembly language specifies machine code<br/>
어셈블리어는 머신 코드를 표현하는 수단일뿐<br/>
- Assemble<br/>
    - The process of converting assembly code into machine code<br/>
    어셈블리 코드를 머신 코드로 변환하는 과정
- Assembler<br/>
    - A program which takes assembly language as input and produces machine language as output<br/>
    어셈블 과정을 대신해주는 sw

# Assembling simple program
![ssw_ch_1](https://user-images.githubusercontent.com/63347989/144894912-c1e90ab1-2cd6-410e-a18b-38fa35b0793e.PNG)<br/>
- reg codes - 여덟개의 범용 레지스터에 대해서 세비트의 코드로 표현하고 있음<br/>
- first byte - (한 바이트) 16진수<br/>
- second byte - (여덟 비트) 2진수<br/>
11SS SDDD 에서 S는 SOURCE, D는 DESTINATION을 의미함. <br/>
    - 만약 MOV EAX, EBX라면<br/>
    1101 1000이라서 D8을 의미함. <br/>
- 명령에 따라서 한 바이트, 두 바이트, 최대 여섯 바이트로 변환됨. <br/>
- 4바이트가 더 따라온다는 것은 두번째 오퍼랜드인 IMMEDIATE값 (더블워드 - 네바이트)이 오는 것. <br/>
- IN, OUT은 오퍼랜드가 있지만 딱 그형태로만 사용가능하기 때문에 ED EF로만 변환이 됨.<br/>
- JMP는 5바이트로 변환이됨.<br/>
- 조건부 분기 명령들은 변환이 됐을때 총 6바이트가 됨.<br/>

![ssw_ch_2](https://user-images.githubusercontent.com/63347989/144894921-66e0cf60-1753-4694-ae40-ca5b9735ec55.PNG)<br/>
어셈블리 프로그램을 어셈블하는 과정에서 만들어진 리스트 파일<br/>
리스트 파일 - OBJECT 파일을 텍스트 형태로 볼 수 있음<br/>
- MOV EDX, 0 - 5바이트짜리 명령 <br/>
1011 1010 00 00 00 00 - BA 00 00 00 00<br/>
- JZ GCD <br/>
GCD는 심볼, JZ GCD할때 GCD는 레이블이 붙어있는 주소값 - 24<br/>
근데 24를 그대로 적으면 안되고 분기명령에서 레이블 값은 그대로 적는 것이 아니라 분기명령과 대상 레이블간에 상대적인 위치 차이를 적어야 함. <br/>
이때 분기명령을 가리키는 순간 다음 명령을 가리키고 있기때문에 다음 명령의 주소와 레이블간의 주소차이를 구해야함. <br/>
24(16진수) - 13(16진수) = 11<br/>
0F 84 (11 00 00 00)<br/>
    - 인텔 CPU는 32비트 수를 메모리에 저장할 때 LITTLE ENDIAN 방식 이용<br/>
    - 이 머신 코드는 실행시에 메모리에 저장되어야 할 내용이라서 LITTLE ENDIAN 방식으로 저장해야 함. <br/>
- JMP ORD <br/>
B - 24 -> 음수가 나옴<br/>
32비트 2의 보수를 표현하고 실제 그 수를 LITTLE ENDIAN 방식으로 저장을 한다. <br/>

# General Machine Instruction Format
![ssw_ch_3](https://user-images.githubusercontent.com/63347989/144894911-af3a3021-3910-4c0a-88bc-b4a6382f8aa4.PNG)<br/>
밑에 숫자는 해당 하는 부분이 실제 차지하는 바이트 수 <br/>
- PREFIX 바이트는 OPCODE 바이트에 대한 수식을 할 수 있는 바이트 <br/>
OPCODE 바이트에다가 옵션을 추가하는 형태<br/>
- OPCODE 바이트는 특정한 명령이 무엇이냐는 나타내는 UNIQUE한 값<br/>
OPCODE 바이트가 정해지면 어떤 머신 명령인지 정해짐 <br/>
- MOD RM 바이트는 오퍼랜드를 명시하는 바이트<br/>
- 주소상수
- IMMEDIATE 상수

# Opcode space
It contains both mnemonic and operand information <br/>
Letter keys for __operand__ <br/>
opcode는 명령을 구분할 수 있는 UNIQUE한 값<br/>
- 오퍼랜드의 형태
    - __r__ register<br/>
    - __m__ memory<br/>
    - __i__ immediate<br/>
- 오퍼랜드의 크기
    - __b__ byte<br/>
    - __2__ 2 bytes<br/>
    - __v__ 32 bits or 16 bits<br/>
    - __e__ means E in 32 bits and disappears in 16 bits<br/>
    32비트짜리에서는 E, 16비트짜리에서는 없음.<br/>

ex. add rv, rmv<br/>
rv는 32비트 또는 16비트짜리 레지스터<br/>
rmv는 32비트 또는 15비트짜리 레지스터 또는 메모리<br/>


## Overview of the entire set of x86 machine code
![ssw_ch_4](https://user-images.githubusercontent.com/63347989/144895198-f08099ed-7d22-49e6-81f3-47c4a3ad8568.PNG)<br/>
ADD rv, rmv의 opcode 값은 03임. <br/>
90은 오퍼랜드가 없음. <br/>
오퍼랜드의 종류에 따라서 opcode가 달라질 수 있음. <br/>

## Example
Opcode byte 03 : ADD rv, rmv<br/>
- rv indicates that the first operand must be a 16- or a 32-bit register<br/>
- rmv means that the second operand must be a 16- or 32-bit register or memory operand <br/>
- ADD EDI, EAX<br/>
- ADD EDI, [1234H]<br/>

## The type of information conveyed by the first byte
opcode 표를 정리할 수 있음 <br/>
1. One-byte suffices to completely specify an instruction.<br/>
한바이트 명령으로 충분한 경우 <br/>
ex. F4 -> HLT<br/>
2. First byte determines which operation is to be performed, but it does not specify the operands. Second (and third) byte(s) often carries the operand information (ModRM bytes) <br/>
첫번째 바이트가 어떤 연산에 해당하는지 나타냄<br/>
이 바이트가 오퍼랜드까지 포함되지는 않음<br/>
opcode가 한바이트 이고 추가로 한바이트 또는 두바이트가 modrm 바이트가 필요로 하는 명령<br/>
ex. 89 -> MOV rmv, rv<br/>
3. The first byte specifies merely a general type of operation, and the ModRM byte is used to complete the specification of the operation as well as carry operand information <br/>
opcode 바이트만으로는 명령을 특정할 수 없고 일련의 오퍼랜드의 종류를 나타냄. <br/>
modrm 바이트는 어떤 명령인지 완성시키고 오퍼랜드 정보를 포함하고 있음. <br/>
ex. C1 -> Shift rmv, ib<br/>
It is just a pointer into the table at page 22 (Nonregister R bits) <br/>
4. It is just a pointer to the table at page 23(385 space) - 두 바이트 크기의 opcode <br/>
첫번째 opcode 바이트가 __0F__인 경우에, 두번째 opcode 바이트를 찾아보기 위해서 또다른 테이블을 참조해야하는 유형의 명령들<br/>
대부분의 조건부 명령이 여기에 속함. <br/>
5. If the first byte is in the range D8-DF, then the command is a floating point command<br/>
opcode의 첫번째 바이트가 D8-DF 사이값이고, 부동소수점 연산하는 명령들이 여기에 해당함. <br/>
6. There are also prefix bytes which modify the subsequent commands.<br/>
PREFIX 바이트를 가진 명령들. <br/>
ex. F0 -> LOCK<br/>

# The ModRM byte
Consist of three parts (1바이트 = 8비트)<br/>
![ssw_ch_5](https://user-images.githubusercontent.com/63347989/145685911-774e3aed-7843-4d97-997f-0a0b6c2a28b7.PNG)<br/>
- Bits 7 and 6 are the __Mod bits__. When the mod bits are 11, the R/M bits designate a register. <br/>
11일때, RM 비트가 레지스터를 의미함. <br/>
Otherwise the R/M bits are used for memory coding<br/>
11이 아니면, RM 비트가 메모리임. <br/>
- Bits 5, 4 and 3 are the __Reg bits__ and are often designated using a slash notation, /r.<br/>
- Bits 2, 1 and 0 are the __R/M bits__, and are used to specify or help specify a memory location except when the Mod bits are 11.<br/>
레지스터를 나타낼 수도 있고 메모리를 나타낼 수도 있는데, 구분은 MOD BIT가 함. <br/>


## rv, rmv coding 
- ADD EDI, EAX <br/>
    - Two Mod bits must be 11 <br/>
    RM 비트가 레지스터임 <br/>
    - R bits for EDI is 111 <br/>
    - R/M bits for EAX is 000 <br/>
    - Code is 1111 1000 = __F8H__ <br/>
    ![ssw_ch_6](https://user-images.githubusercontent.com/63347989/145685996-5ac5cf52-e9b5-4eea-a29d-c8f95886c8b4.PNG)<br/>
    - Combination with code __03__ (ADD EDI, EAX를 뜻하는 OPCODE) <br/>
    - -> 03 F8<br/>
- Mod bits usage (MOD BIT 값에 따른 사용)<br/>
    - 11<br/>
    ADD EDI, EAX<br/>
    오퍼랜드 2개가 레지스터<br/>
    - 00 (actual memory operand)<br/>
    ADD EDI, [EAX]<br/>
    첫번째 오퍼랜드는 레지스터이고 두번째 오퍼랜드 메모리인데, <br/>
    메모리를 나타내는 것은 BASE REGISTER 만 사용 
    - 01 and 10 (immediate displacement either ib or iv)<br/>
    두번째 오퍼랜드가 메모리이긴 한데, <br/>
    ADD EDI, [EAX + 5] -> 03 78 05<br/>
    BASE REGISTER와 8비트 DISPLCEMENT 사용<br/>
    ADD EDI, [EAX + 87654321H] -> 03 B8 21 43 65 87<br/>
    BASE REGISTER와 32비트 IMMEDIATE DISPLACEMENT가 사용됨<br/>
    
## One-Byte Address Modes – One-byte ModRM
두개의 오퍼랜드 중에서 두번째 오퍼랜드가 메모리 오퍼랜드인 경우에 해당 MOD RM 바이트가 어떤식으로 구성되었는지.<br/>
두번째 오퍼랜드가 메모리이기 때문에 MOD 비트는 00, 01,10 중에 하나임. <br/>
mod rm 바이트가 한바이트로 구성된 경우.<br/>
“Base reg + displacement” form<br/>
![ssw_ch_7](https://user-images.githubusercontent.com/63347989/145686041-44ae5af9-bfcd-4238-a4f3-ed5d67d871da.PNG)<br/>
- 모든 경우에 RM에는 100이 들어갈 수 없음. <br/>
RM에 100이 들어간 경우는 100이 특정한 베이스 레지스터의 코드가 아니라 완전한 다른 의미의 케이스를 나타냄<br/>
- MOD 비트가 00이고, RM의 값이 101이 아닌 경우 <br/>
[B] RM이 베이스 레지스터만 사용됨<br/>
- MOD 비트가 00이고, RM의 값이 101인 경우 <br/>
[D] RM이 32비트 displcement만 사용됨<br/>
- MOD 비트가 01인 경우 <br/>
[B+D] RM이 베이스 레지스터와 8비트 displacemnt가 사용됨.<br/>
- MOD 비트가 10인 경우<br/>
[B+D] RM이 베이스 레지스터와 32비트 displacement가 사용됨.<br/>

### Examples
- [B] mod = 00, ADD EAX, [EDX]<br/>
![ssw_ch_8](https://user-images.githubusercontent.com/63347989/145686125-a146782f-420a-440c-a643-4aa5da678c41.PNG)<br/>
- [B+8D] mod = 01, SUB EDI, [EDI+127]<br/>
![ssw_ch_9](https://user-images.githubusercontent.com/63347989/145686126-c4615387-9f9e-40e4-9191-1a5d8588e517.PNG)<br/>
- [B+32D] mod = 10, ADD ESI, [EDI+12345678h]<br/>
![ssw_ch_10](https://user-images.githubusercontent.com/63347989/145686127-67242250-1526-4155-a5c1-263d0b1112fe.PNG)<br/>
이때 displacemnt가 78 45 34 12 순(little endian)으로 들어가있음.<br/> 
- [32D] mod = 00 & RM = 101, MOV ESI, [12345678h]<br/>
![ssw_ch_11](https://user-images.githubusercontent.com/63347989/145686129-9857b5d0-7767-4396-b8d5-2f44fde0b053.PNG)<br/>
101은 베이스 레지스터가 사용되지 않았고 32비트 displacement만 사용됨을 의미함<br/>
여기서도 displacement가 little endian 방식으로 저장됨. <br/>
- [B+8D] mod = 01 & RM = 101, MOV ESI, [EBP]<br/>
![ssw_ch_12](https://user-images.githubusercontent.com/63347989/145686124-15aef02d-bdeb-4507-8aea-4cfa6e75f863.PNG)<br/>
RM을 EBP로 쓰고 싶을때 MOV ESI, [EBP]+0으로 해서 8비트 displacement 0 이 더해진걸로 함. <br/>
그래서 이때 mod 값은 01이 됨. <br/>

## Two-Byte Address Modes – Two-byte ModRM
mod rm 바이트가 두바이트로 구성된 경우.<br/>
“Base reg + Index reg + scale factor + displacement” form<br/>
[B+S*I+D]<br/>
![ssw_ch_13](https://user-images.githubusercontent.com/63347989/145686260-331191db-8ac6-42e4-b0f2-4ea99acc6068.PNG)<br/>
- RM 비트 100 이면 MOD RM 바이트가 하나 더있다는 의미<br/>
- 첫번째 : MOD 비트가 00이고, RM의 값이 100이고<br/>
두번째 : SCALE FACTOR로는 1(00), 2(01), 4(10), 8(11)만 가능, 인덱스 레지스터, 베이스 레지스터가 오는데 베이스 레지스터는 101이 아님 <br/>
[B+S*I]<br/>
- 첫번째 : MOD 비트가 00이고, RM의 값이 100이고<br/>
두번째 : SCALE FACTOR로는 1(00), 2(01), 4(10), 8(11)만 가능, 인덱스 레지스터, 베이스 레지스터가 오는데 베이스 레지스터는 101이 오는 경우 <br/>
[S*I+32D]
- 첫번째 : MOD 비트가 01이고, RM의 값이 100이고<br/>
두번째 : SCALE FACTOR로는 1(00), 2(01), 4(10), 8(11)만 가능, 인덱스 레지스터, 베이스 레지스터, 8비트 displacement <br/>
[B+S*I+8D]
- 첫번째 : MOD 비트가 10이고, RM의 값이 100이고<br/>
두번째 : SCALE FACTOR로는 1(00), 2(01), 4(10), 8(11)만 가능, 인덱스 레지스터, 베이스 레지스터, 32비트 displacement <br/>
[B+S*I+32D]

### Examples
- [B+S*I] MOV EAX, [EBX+ESI*2]<br/>
![ssw_ch_14](https://user-images.githubusercontent.com/63347989/145686263-38d5f993-d2c8-4ae9-aa14-a1207ffb446c.PNG)<br/>
- [B+S*I+32D] MOV EAX, [999999+EDI+EAX*4]<br/>
![ssw_ch_15](https://user-images.githubusercontent.com/63347989/145686265-dacfeaf8-2e44-437b-b3de-69acbe1a1890.PNG)<br/>
- [S*I+32D] MOV EAX, [TABLE+ESI*4]<br/>
![ssw_ch_16](https://user-images.githubusercontent.com/63347989/145686267-de86eea6-d4f0-4122-b4d9-612334555a2d.PNG)<br/>
- MOV EDI, [ESP+24]<br/>
![ssw_ch_17](https://user-images.githubusercontent.com/63347989/145686258-eb78096b-9ea0-4883-bf92-345234b5671a.PNG)<br/>
첫번째 바이트의 100은 mod rm이 두개의 바이트인것을 의미하고 <br/>
두번째 바이트의 100은 base register로 esp가 쓰였다는 것을 의미<br/>
첫번째 바이트의 mod비트가 01이라서 두번쨰 바이트의 s 랑 idx는 의미 없음. <br/>

## rmv, rv coding
- MOV EDI, EAX
    - opcode값이 두가지 case모두에 적용이 될 수 있음 <br/>
    - Using 8B -> 8B F8<br/>
    -> mov rv, rmv -> 1111 1000 = F8
    - Using 89 -> 89 C7<br/>
    -> mov rmv, rv -> 1100 0111 = C7
    - EDI goes into the least significant bits<br/>
    - EAX is coded in the R bits<br/>
    ![ssw_ch_18](https://user-images.githubusercontent.com/63347989/145686259-fbea8e25-b614-44ae-b903-4ac9288b4f23.PNG)<br/>
    - 1100 0111 = C7H<br/>

# Nonregister R bits
The first byte is insufficient to specify an operation, <br/>
첫번째 바이트만 봐서는 어떤 연산인지 확정짓기가 어려움 <br/> 
but the R bits of a ModRM byte are used to complete the specification.<br/>
mod rm의 r비트 값까지 사용해서 해당 명령이 어떤 명령인지 명시함.<br/>
- SUB ECX, 32
    - 83 -> Immed rmv, ib<br/>
    이것만 봐거 구체적으로 어떤 명령인지 알 수 없음<br/>
    이걸 알기 위해서 표를 하나 더 참조해야함<br/>
    - Immed has a SUB under /r = /5 = 101
    - Mod = 11
    - M = 001 for ECX
    - 32 = 20H
    - 83 (11 101 001) 20 = __83 E9 20__

![ssw_ch_19](https://user-images.githubusercontent.com/63347989/145686403-909687c8-2267-4e6b-a4ec-a524226407ae.PNG)<br/>
- sub - r 비트가 101임 

# 386 space 
Open 386 space followed by 0F, most of them appears first on 386<br/>
opcode의 값이 0F인 명령들 <br/>
![ssw_ch_20](https://user-images.githubusercontent.com/63347989/145686401-4bd958d2-b044-443a-9328-5cd5638e7da4.PNG)<br/>

- Sometimes one entry specifies exactly one instruction. For example, 0F CA is BSWAP EDX<br/>
두번쨰 opcode 바이트까지만으로 하나의 명령이 구성된 경우<br/>
BSWAP이라는 명령의 오퍼랜드 EDX인 경우<br/>
오퍼랜드가 하나 있음에도 불구하고 전체 명령이 0F CA임 <br/>
- Sometimes a ModRM byte is needed to convey operand information. XADD instruction whose first two bytes are 0F C1<br/>
0F 다음에 MODRM이 필요한 경우<br/>
XADD의 첫번째 OPCODE가 0F, 두번째 OPCODE가 C1<br/>
C1을 보면  XADD rmv, rv 임<br/>
XADD 다음에 두개의 오퍼랜드에 대해서 MODRM 바이트를 구성을 해야하는 그런 케이스임. <br/>
- Sometimes a ModRM byte is used to specify an operation. When 0F 01, we must check R bits of the ModRM (table at the next page)<br/>
MODRM 바이트가 쓰이긴 하는데 MODRM 바이트의 R 비트가  또다른 표에 의해서 결정됨. <br/>
MOD RM까지 표시해야지 어떤 연산인지 알 수 있음. <br/>
앞페이지 테이블에서 두번째 OPCODE 값이 01인 경우에 해당함. <br/>
01 = GLOBAL T, GLOBAL T 에 헤딩하는 명령들은 386 SPACE 표에서 확인 <br/>
- MMX instructions<br/>

![ssw_ch_21](https://user-images.githubusercontent.com/63347989/145686458-731cba05-06e9-4440-b040-6d3ac8d41d4a.PNG)<br/>

## 32-bit vs. 16-bit code
- Code example <br/>
![ssw_ch_22](https://user-images.githubusercontent.com/63347989/145686459-7a2a857a-ea78-4539-a492-972fa94ed0a5.PNG)<br/>
89 = MOV RMV RV <br/>
11 000 001 = C1<br/>
66이라는 PREFIX는 오퍼랜드 크기에 V가 사용되었을 경우에 그 명령 앞에 붙임으로써 16비트와 32비트 오퍼랜드 크기를 톡을 할수있는 그런 명령<br/>
66 89 C1 = MOV CX AX<br/>
00은 EAX를 의미하는 걸수도 있고 AX를 의미하는 걸수도 있음.<br/>
- Code for 8-bit registers<br/>
![ssw_ch_23](https://user-images.githubusercontent.com/63347989/145686456-ed471ba2-82e0-4ed4-9c9f-4ce3c0dcdf46.PNG)<br/>
8비트 레지스터에 관한 코드가 독자적을 존재함 <br/>
MOV AL, CL -> 8A C1<br/>

