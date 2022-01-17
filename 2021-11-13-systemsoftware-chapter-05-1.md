---
layout: post
title: Intel Assembly I – Arithmetic & Logic Operations
subtitle: SystemSoftware_05_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Arithmetic & Logic Operations

![ssw_ch5_1](https://user-images.githubusercontent.com/63347989/141644540-33eb66c2-c078-4caa-9507-2c9969c986cb.PNG)

산술연산과 논리연산의 차이점<br/>
산술연산은 연산대상이 수<br/>
논리연산은 연산대상이 단순한 비트 단위로 봄 (비트 단위의 연산)<br/>

EFLAGS – 현재 CPU의 상태 32bit register<br/>
32비트 중에서 6개의 비트에 관심있음<br/>
산술 연산과 논리 연산의 명령실행의 결과에 대한 부가적인 정보가 flag값에 반영 – 값이 변할 수 있음<br/>
EFLAG - status flag <br/>
Some bits of EFLAGS can change for arithmetic and logic instructions
- Z (result zero) <br/>
결과값이 0이면 ZF = 1로 set
- S (result positive)<br/>
결과값이 음수이면 SF = 1로 set<br/>
결과값의 최상위 비트의 값과 항상 같음<br/>
비트값이 0이거나 양수이면 최상위 비트값이 0이 됨.<br/>
- C (carry out) <br/>
덧셈 과정에서 최상위 비트로부터 한자리 올림이 발생했을 때 CF = 1로 set. (부호 없는 정수)
- P (result has even parity)<br/>
가장 낮은 자리 한 바이트 (8비트)에서 비트값이 1의 개수가 짝수이면 PF = 1<br/>
나머지 바이트들과는 관련이 없음. <br/>
홀수 parity를 맞추기 위해서 odd parity<br/>
- A (half carry out) <br/>
가장 낮은 자리 한 바이트 (8비트)에서 반을 나눠서, 아랫쪽 4비트와 관련이 있음.<br/>
4비트에서 덧셈을 하는 과정에서 3번 비트에서 4번 비트에서 올림이 발생했을 때 AF = 1<br/> (BCD숫자에 대한 연산)
- O (overflow occurred)<br/>
연산을 했는데, 연산의 결과값이 32비트로 표현하기에는 너무 크거나 작은 경우 OF = 1<br/>
(부호 있는 정수)<br/>

ex. add eax, ebx <br/>
eax - destination operand,<br/>
ebx - source operand<br/>

? 부호있는 정수, 부호없는 정수, BCD인지 누가 정함?<br/>
-프로그래머가 정함<br/>
변수선언 int x; signed integer라 선언 – x는 무조건 부호있는 정수  <br/>
Unsigned y – y는 무조건 부호없는,,<br/>
CPU는 부호있는 정수인지 없는 정수인지 상관 안씀<br/>

# Addition
Addition(단순 덧셈), Increment(증가), Add-with-carry(carry를 포함하는 덧셈) and Exchange-and-add<br/>
![ssw_ch5_2](https://user-images.githubusercontent.com/63347989/141645769-7b5fbaaf-90a4-4b00-9c4f-c6486d12779a.PNG)<br/>
[ ] – 메모리 피연산자 (operand) <br/>
- add
- inc <br/>
inc는 operand가 1 증가시키는 동작 (operand 1개)<br/>
byte는 메모리만 봐서 크기를 알 수 없어서 한 바이트라는 것을 알려줌 <br/>
- adc <br/>
adc는 캐리를 포함하는 덧셈 (Carry Flag까지 더하는 것) <br/>
CF=0이라면 adc는 add와 같음. <br/>
32비트보다 더 큰 비트는 표현x, 계산x<br/>
64비트수 두 개 더하는 연산할 때<br/>
반으로 쪼개서 첫번째 윗 수는 eax, 아랫 수는 ebx에 따로따로 넣어서 <br/>
낮은 자리 숫자끼리 더하고 높은 자리 숫자끼리 더하면 됨.<br/>
낮은 자리 숫자끼리 더할 때는 add 명령을 쓰면 됨<br/>
이때 발생한 carry는 CF에 저장하게 됨. <br/>
그러나 높은 자리 두개를 더할 때는 add를 쓰면 안되고 낮은 자리 계산에 올림이 발생할 수도 있어서 CF까지 더해서 즉, adc를 해줘야 함 <br/>
![ssw_ch5_5](https://user-images.githubusercontent.com/63347989/141646145-8ef96048-502e-496c-9d59-47d3180fba5a.PNG)<br/>
- xadd<br/>
ecx += ebx 그리고 ebx 값은 ecx의 원래 값이 대입됨.<br/>
첫번째 operand, 두번째 operand 둘 다 바뀜<br/>

# Subtraction <br/>
Subtraction, Decrement and Subtract-with-borrow<br/>
![ssw_ch5_3](https://user-images.githubusercontent.com/63347989/141645771-8aaf19b4-f53b-4bdf-bb2b-067fae390303.PNG)<br/>
- sub <br/>
일반적인 뺄셈<br/>
- dec <br/>
operand 1개<br/>
dec edi // edi register의 값을 1 감소시키는 역할
- sbb <br/>
carry를 포함하는 뺄셈 <br/>

# Comparison <br/>
표현은 비교연산이지만 실제로 일어나는 동작은 뺄셈과 동일한 동작임.<br/>
- The operands are subtracted, but the result is not stored; Changes only the flag bits.
- Often followed with a conditional branch:<br/>

![ssw_ch5_4](https://user-images.githubusercontent.com/63347989/141645767-712bfff3-6394-46cc-a307-0456c76d149f.PNG)<br/>
-  cmp <br/>
operand 2개 <br/>
cmp al, 10H // al - 10H 뺄셈 연산을 하되 결과값이 저장되지는 않음.<br/>
근데 왜 뻄? 결과값이 변하진 않지만 Status flag 가 바뀜<br/>
cmp 다음에는 조건부 분기 명령이 오게 되어있음. <br/>
- jae  조건 분기 명령<br/>
- cmpxchg : compare and exchange<br/>
operand 가 두개<br/>

## INC and DEC
- INC
    - operand 가 1개 (register) 
    - Increment reg value 
    - Same to ADD reg, 1 
- DEC
    - operand 가 1개 (register)
    - Decrement reg value 
    - Same to SUB reg, 1

## CMP 명령의 예시<br/>
![ssw_ch5_6](https://user-images.githubusercontent.com/63347989/141649566-ef52200f-17c8-4c02-8a13-c4e105b842c3.PNG)<br/>
CMP EBX, EAX <br/>
이때 뺄셈 연산이 이루어지지만 연산의 결과값이 EBX에 저장되지는 않음. <br/>
그러나 status flag 값들이 변해서 cmp 다음에 오는 조건부 분기 명령이 영향을 받음(분기를 하거나 안하거나..) <br/>
JL : EBX가 EAX 보다 작으면 SIB로 분기해라 <br/>

# Multiplication and Division
덧셈과 뺄셈 연산에서는 대상 operand가 signed인지, unsigned인지 구분하지 않음. <br/>
- mul/div: Unsigned.
- imul/idiv: Signed integer multiplication/division.
- al always holds the multiplicand (or ax or eax).
- Result is placed in ax (or dx and ax or edx or eax).<br/>
8비트 곱셈에서는 ax <br/>
16비트 곱셈에서는 dx, ax<br/>
32비트 곱셈에서는 edx, eax<br/>

![ssw_ch5_7](https://user-images.githubusercontent.com/63347989/141649702-7f4fdb9b-aecc-4794-9d2c-3b4d18dd047c.PNG)<br/>
- mul bl <br/>
곱하는 수가 8비트 bl 이고 곱해지는 수는 al 레지스터에 있음. <br/>
8비트 두수를 곱해서 생긴 16비트 수는 ax에 저장됨. <br/>
- imul cx, dx, 12H<br/>
cx = dx * 12H 
- mul ecx <br/>
곱하는 수가 ecx에 들어가있고 곱해지는 수는 eax에 있음. <br/>
32비트 두수를 곱해서 생긴 64비트 수는 edx와 eax에 저장이 됨. <br/>
<br/>
- C and O bits are cleared if most significant 8 bits of the 16-bit product are zero 
(result of an 8-bit multiplication is an 8-bit result).<br/>
8비트 두수 곱셈을 했는데 결과적으로 16비트 수가 나올텐데 16비트 수의 상위 8비트가 0라면 CF, OF 가 clear됨. <br/>
즉 8비트 두수 곱셈을 했는데 여전히 8비트 수라면 CF, OF가 clear 됨. <br/>

- Division by zero and overflow generate errors.<br/>
0으로 나눌수 없으니까 operand가 0이면 에러 발생<br/>
dibision by zero = exception. <br/>
exception: cpu가 명령을 실행하는 도중에 치명적인 오류를 발생한 것. <br/>
exception 발생하면 프로그램이 강제 종류됨(프로그램이 죽음). 
- Overflow occurs when a small number divides a large dividend.<br/>
매우 큰수를 매우 작으로 나뉘게 됐을 때 오버플로우 발생. <br/>
몫이든 나머지든 정해진 비트수(자릿수)로 수용이 안될 경우. 

![ssw_ch5_8](https://user-images.githubusercontent.com/63347989/141649705-9975b5bf-5eee-4551-8342-e911fa5b8d4b.PNG)<br/>
- div cl<br/>
8비트 나눗셈, 나뉘어지는 수는 16비트임. <br/>
16비트 ax를 8비트 cl로 나눠서 몫이 al 에, 나머지가 ah에 저장됨. <br/>
- idiv cx <br/>
16비트 나눗셈, 나뉘어지는 수는 32비트(dx|ax)임. <br/>
32비트 dx|ax를 16비트 cx로 나눠서 몫이 ax에, 나머지가 dx에 저장됨. <br/>

# AND, OR, and XOR
Allow bits to be set, cleared and complemented<br/>
operand의 일부 비트 값을 조작(set, clear, complement)하는데 사용이 됨. <br/>
set: 비트를 1로 만듬<br/>
clear: 비트를 0으로 만듬<br/>
complement: 비트의 값을 0을 1로, 1을 0으로 바꿈<br/>
Commonly used to control I/O devices.<br/>
Logic operations always clear the carry and overflow flags.<br/>
논리 연산 실행 후 CF, OF는 clear 됨. <br/>

## AND: 0 AND anything is 0.
1 and 1 일때만 1 <br/>
Commonly used with a MASK to clear bits<br/>
![ssw_ch5_9](https://user-images.githubusercontent.com/63347989/141650402-12461e1c-0835-422a-b877-7eee5338a3e3.PNG)<br/>
clear하고 싶은 부분은 0으로, 값을 그대로 유지하고 싶은 부분은 1로 해서 and 연산함<br/>
주어진 값은 al, 내가 설정한 값(mask)은 bl <br/>
and 연산 후 결과값은 al에 저장<br/>
이때 mask는 8비트 크기의 위쪽 4비트를 clear시키는 and mask<br/>

## OR: 1 OR anything is 1.
0 or 0 일떄만 0 <br/>
Commonly used with a MASK to set bits<br/>
![ssw_ch5_10](https://user-images.githubusercontent.com/63347989/141650404-3471223a-906b-4cbb-8bea-7026dc3744bf.PNG)<br/>
8비트 값이 있을 때 아랫쪽 4비트를 set하고 싶을 때<br/>
나머지 위쪽 4비트는 값이 그대로 나옴<br/>
이 mask는 8비트 크기의 하위 4비트를 set시키는 or mask<br/>

## XOR: Truth table: 0110.
두비트 값이 같으면 0, 다르면 1나옴<br/>
Commonly used with a MASK to complement bits<br/>
![ssw_ch5_11](https://user-images.githubusercontent.com/63347989/141650397-ef325c58-460a-4d96-9d53-7153199ee9c2.PNG)<br/>
바x 원래의 비트들의 반대<br/>
이 mask는 complement하고 싶은 비트들은 1을 대응시키고 그대로 유지하고 싶은 비트들은 0을 대응<br/>
8비트 중에서 아랫쪽 4비트를 complement 시키는 xor mask<br/>

# TEST
TEST: Operates like the AND but doesn't effect the destination.<br/>
Test는 결과값이 저장되지는 않지만 결과에 따른 flag register 값만 변함 <br/>
Sets the Z flag to the complement of the bit being tested:<br/>
![ssw_ch5_12](https://user-images.githubusercontent.com/63347989/141650400-5ff8fc24-2814-4776-b6a6-d2c1c5f7bd4c.PNG)<br/>
Al(8비트)와 4(0000 0100)을 and 시킴 – 0에 해당하는 값은 0이 나오고, 나머지 한비트는 al의 값에 따라 달리짐. <br/>
al register의 비트위치 2번에 해당하는 값이 0인지 1인지 test하는 명령<br/>
jz – 분기해라 조건 분기 명령 (ZF=1이면 LABEL로 분기해라)<br/>

- BT: Test the bit, BTC: Tests and complements...
- NOT (logical one's complement)<br/>
operand 1개<br/>
1의 보수를 구하는 명령 <br/>
- NEG (arithmetic two's complement - sign of number inverted)<br/>
operand 1개 <br/>
2의 보수를 구하는 명령 <br/>
![ssw_ch5_13](https://user-images.githubusercontent.com/63347989/141650401-02391dcb-9d1d-4796-b2d0-c8404799fc9c.PNG)<br/>

# Shift
일정한 비트 수만큼 왼쪽 또는 오른쪽으로 값들을 이동을 시킴<br/>
종류는 4가지. <br/>
![ssw_ch5_15](https://user-images.githubusercontent.com/63347989/141650960-cbf93ecb-2031-4375-a8bc-4c958288410e.PNG)<br/>

Shift: Logical shifts insert 0, arithmetic right shifts insert sign bit.<br/>
Shift 대상 오퍼랜드와 shift count 오퍼랜드로 구성; count 오퍼랜드는 최대 5bit만 유의미함 (32자리 이상 shift 하는 것은 의미 없음)<br/>
shift 된 비트(사라지는 비트)는 CF로 들어감<br/>

SHL과 SAL은 기본적으로 동작이 동일<br/>
- unsigned multiplication of the destination operand by powers of 2<br/>
3비트 왼쪽으로 shift 하게 되면 2의 3승으로 곱하는 효과가 있다. 
- do not work for signed values

SHR<br/>
- unsigned division of the destination operand by powers of 2.<br/>
2의 n승으로 나눗셈하는 효과가 있다. 

SAR<br/>
- 빈자리는 원래 값의 sign bit로 채움<br/>
- signed division of the destination operand by powers of 2.<br/>

![ssw_ch5_14](https://user-images.githubusercontent.com/63347989/141650920-fdeeb9c2-17fa-41d1-928f-3c9f5af7c307.PNG)<br/>

## SHL/SAL Instruction Operation 왼쪽으로 shift하는 예시
![ssw_ch5_16](https://user-images.githubusercontent.com/63347989/141651187-30c70498-afc1-4819-9978-460043f284f4.PNG)<br/>

## SHR Instruction Operation 오른쪽으로 논리 shift하는 예시
![ssw_ch5_17](https://user-images.githubusercontent.com/63347989/141651233-7e5ad671-ddcb-4007-a106-5a11785d149e.PNG)<br/>

## SAR Instruction Operation 오른쪽으로 산술 shift하는 예시
![ssw_ch5_18](https://user-images.githubusercontent.com/63347989/141651236-cebbc519-dd8d-47ae-b1fb-566cc254073f.PNG)<br/>
원래 operand의 최상위 비트 값으로 새로 채워짐. <br/>

# Double Precision Shift
shld(left) / shrd(right) <br/>
![ssw_ch5_19](https://user-images.githubusercontent.com/63347989/141651232-1bfd85a0-4821-48cc-81d7-491c34b306cc.PNG)<br/>
- eax를 오른쪽으로 12만큼 shift 해라. 
- 왼쪽에 빈 비트들은 ebx에서 가져와서 채워라. <br/>
- shld dst, src, count 
    - operand가 3개
    - dst: reg / mem
    - src : register-only
    - count : CL or 8-bit immediate

## SHLD and SHRD Instruction Operations 
![ssw_ch5_20](https://user-images.githubusercontent.com/63347989/141651313-1d2613f2-cef9-40cc-a174-f9dd5cba2c88.PNG)

# Rotate
Rotate: Rotates bits from one end to the other or through the carry flag.<br/>
한쪽으로 빠져나간 비트들이 반대편 방향의 빈곳으로 채워지는 동작이 이루어짐. <br/>
rotate의 종류 4가지. <br/>
![ssw_ch5_25](https://user-images.githubusercontent.com/63347989/141651643-27bd6a0f-cac4-478a-bd2a-bcc723fc2499.PNG)<br/>

![ssw_ch5_21](https://user-images.githubusercontent.com/63347989/141651360-cd9b92bb-0cd5-4464-94f2-cfbdcf86d60c.PNG)<br/>

Commonly used to operate on numbers wider than 32-bits:<br/>
32비트보다 더 큰 수에 사용되기도 함. <br/>
![ssw_ch5_22](https://user-images.githubusercontent.com/63347989/141651361-354945f8-f6b3-43e0-a19a-09c76de13ef3.PNG)<br/>
46비트경우, 16비트 레지스터 세개 dx, bx, ax 에 넣음. <br/>

## ROL, ROR, RCL, and RCR Instruction Operations
![ssw_ch5_23](https://user-images.githubusercontent.com/63347989/141651364-d055fbd4-c2c6-40c8-b120-b9264de33798.PNG)<br/>
<논리연산의 경우><br/>
왼쪽으로 나간 값이 오른쪽으로 들어오기도 하지만,<br/>
왼쪽으로 나간 값이 CF로 들어가기도 함. <br/>
5비트 rotate 했을 때 마지막 비트가 CF에 들어감. <br/>
<산술연산의 경우><br/>
CF 한비트까지 포함해서 총 33 비트값을 rotate 한다고 생각하면 됨.<br/>

# Bit/String Scan
Bit Scan Instruction (80386 and up):<br/>
- Scan through an operand searching for a 1 bit.<br/>
operand에 대해서 1의 값을 가지는 비트위치가 어디인지를 알아내는 명령
- Zero flag is cleared if a 1 bit is found, position of bit is saved in destination register<br/>
비트값이 1인 비트가 찾아지면 ZF가 clear됨. <br/>
![ssw_ch5_24](https://user-images.githubusercontent.com/63347989/141651365-ba6dd4d7-6ed1-4bd8-b762-a4fcad26edaf.PNG)<br/>
- bsr bl, cl<br/>
오른쪽부터 cl을 스캔하면서 비트값이 최초로 1인 위치를 bl에 넣음. <br/>
cl에 1없으면 destination값이 바뀌지 않음. <br/>

String Scan Instructions:<br/>
스트링 연산과 관련된 명령<br/>
- scasb/w/d compares the al/ax/eax register with a byte block of memory and sets the flags.
- Often used with repe and repne
- cmpsb/w/d compares 2 sections of memory data.