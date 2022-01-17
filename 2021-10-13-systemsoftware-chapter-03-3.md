---
layout: post
title: Intel IA-32 Architecture 3
subtitle: SystemSoftware_03_3
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Basic Program Execution Registers 
## 1. General-purpose registers. 
![ssw_ch3_7](https://user-images.githubusercontent.com/63347989/137022857-be43004f-9664-467d-bfc4-f1a2a5fba176.PNG)<br/>
- 32bit
- EAX EBX ECX EDX까지는 간단하게 a, b, c, d 레지스터라고 부르기도 함<br/>
- These eight registers are available for storing operands and pointers. <br/>
오퍼렌드(피연산자)나 포인터를 담는데 사용<br/>
피연산자가 메모리에 있는 경우도 있음 <br/>
메모리마다 주소(포인터)가 있는데 그 주소값(포인터)를 범용레지스터가 담고 있음
### _General Purpose Registers_
- The 32-bit general-purpose registers EAX, EBX, ECX, EDX, ESI, EDI, EBP, and ESP are provided for holding the following items: 
    - Operands for logical and arithmetic operations <br/>
    논리연산과 산술연산의 피연산자 담음
    - Memory pointers 메모리 주소(포인터)를 담음
        - Operand가 CPU 안에 있을 경우, 범용 레지스터에 자체적으로 담아서 진행을 하면 됨
        - Operand가 Memory에 있는 경우, 메모리 주소를 범용 레지스터에 담아서 operand를 지칭하면 됨
    - Operands for address calculations <br/>
    프로그램 내에서 자체적으로 주소 계산을 할 때에도 범용레지스터가 사용됨
- The ESP register holds the stack pointer and as a general rule should not be used for another purpose.<br/> 
SP: stack pointer - 현재 실행 중이 프로그램의 스택의 탑을 나타냄 <br/>
범용 레지스터이지만 __스택의 탑__ 을 나타내는 용도로만 쓰임<br/>
ESP register에 임의의 값을 주면 ESP 값이 바뀌는 그 순간부터 프로그램이 정상 동작되지 않음<br/>
범용이기 하되 특정한 상황에서는 특정한 register가 특정한 용도가 있음<br/>
그 용도가 없으면 마음대로 사용해도 됨<br/>

- Many instructions assign specific registers to hold operands (eg. string instructions - ECX, ESI, and EDI ) <br/>
String instruction을 실행할 때에는 ECX, ESI, and EDI 가 미리 정해진 용도가 존재함

- EAX: Accumulator: Referenced as EAX, AX, AL or AH 
    - Used for mult(곱셈), div(나눗셈), etc. 
    - Used to hold an offset. 위 상황 외에는 마음대로 사용해도 상관없다

- EBX: Base Index 
    - Used to hold the offset of a data pointer. 
    - Pointer to DS segment 

- ECX: Count
    - Used to hold the count for some instructions, REP and LOOP. <br/>
    REP LOOP 은 반복동작 명령이어서 ECX가 반복횟수를 담고 있음.
    - Used to hold the offset of a data pointer. 

- EDX: Data 
    - Used to hold a portion of the result for mult(곱셈), of the operand for div(나눗셈). 
    - I/O pointer 입출력 명령 실행할 때 입출력 명령의 대상이 되는 데이터를 EDX REGISTER 가 가리키고 있음

- ESI: Source Index 
- EDI: Destination Index 
    - Holds the base source/destination pointer for string instructions.
    - ESI, EDI 인덱스 레지스터
        - STRING 레지스터 – 배열에 접근할 때의 전용 레지스터 
        - STRING 레지스터가 동작할 때 인덱스 레지스터는 미리 정해진 용도가 따로 있음

- ESP: Stack pointer 
    - STACK의 탑을 가리키는 용도
- EBP: Base Pointer 
    - Holds the base pointer for memory data transfers.
    - Pointer to data on the stack<br/>
    스택에 있는 무언가에 접근할 때 스택의 탑은 늘 ESP가 가르키지만 <br/>
    스택의 탑이 아닌 중간 부분에 접근할 때 접근 위치에 대한 포인터(주소)를 EBP가 담고있음<br/>
    스택의 내부에 대한 포인터

### _Alternate General-Purpose Register Names_
![ssw_ch3_11](https://user-images.githubusercontent.com/63347989/137023949-b6453335-ceec-4903-9560-ce2a70d3e1b8.PNG)<br/>
8개의 범용 레지스터 중에서 일부는 별도의 이름이 붙어있음
- EAX의 아래쪽 16bit를 AX라고 부름 → 별도의 16bit 레지스터로 사용 가능<br/>
- AX 16bit 중 아래쪽 8bit를 AH라고 하고, 위쪽 8bit를 AL이라고 함 → 독자적인 8bit 레지스터로 사용가능 <br/>
AH와 AL이 AX와 EAX의 일부이고 AX는 EAX의 일부임<br/>
- 그래서 AL 레지스터의 값을 바꾸게 되면 <br/>
AX 레지스터의 값에 영향을 미치게 되고 <br/>
결국 EAX 레지스터의 값에도 영향을 미침 
- 과거 16bit, 8bit CPU 시절 레지스터 이름을 현재까지도 사용하기 위해서 고안된 것임<br/>
→ Backword compatibility 하위호환성을 유지하기 위해서
- 그림 상에서 이름이 안 붙어 있는 부분은 이름이 없음 <br/>
별도의 이름을 붙여서 접근할 수 있는 부분이 아님
- EBX, ECX, EDX도 위 규칙이 동일하게 적용이 됨
- EBP, ESI, EDI, ESP 는 16bit까지는 위 규칙이 동일하게 적용이 되지만<br/>
16bit가 반으로 쪼개지는 규칙은 없음

## 2. Segment registers. 16bit
![ssw_ch3_8](https://user-images.githubusercontent.com/63347989/137022859-951ab3b8-269b-4131-9daf-dfe2b9037a75.PNG)<br/>
- 16bit
- These registers hold up to six segment selectors. 
- Segment selector는 특정 segment에 대한 여러가지 정보를 담고 있음
- The segment registers, CS, DS, SS, ES, FS, and GS hold 16-bit segment selectors (A segment selector is a special pointer that identifies a segment in memory). <br/>
Segment Registers는 Segment selector를 담는데 사용함<br/>
Segment selector는 선형 주소 공간 상에 특정한 세그먼트의 시작 위치에 대한 정보를 담고 있는 자료구조 (주소는 아님)<br/>
<br/>

- CS (Code Segment): 
    - In real mode, this specifies the start(시작주소) of a 64KB memory segment. 
    - In protected mode, it selects a descriptor(=selector). <br/>
    코드 세그먼트에 대한 셀렉터를 담고있는 레지스터
- DS (Data Segment): <br/>
    - Similar to the CS except this segment holds data. 
- ES (Extra Segment): 
    - Data segment used by some string instructions to hold destination data.<br/>
    추가적인 data segment가 있는 경우에 ES register가 사용됨<br/>
    DS는 프로그램마다 하나가 있는데 <br/>
    DS 외에 추가적인 데이터 세그먼트에 경우 ES 가 사용되는 것임
- SS (Stack Segment): 
    - Similar to the CS except this segment holds the stack. 
    - ESP and EBP hold offsets into this segment. 
- FS and GS 
    - Allows two additional memory segments to be defined.<br/>
    여분의 세그먼트로 이루어짐<br/>
    기본은 CS, DS, ES, SS를 사용하는데 더 필요하면 FS와 GS가 쓰임<br/>
    한 프로그램이 가질 수 있는 최대 세그먼트는 6개이다.


## 3. EFLAGS (program status and control) register.
![ssw_ch3_9](https://user-images.githubusercontent.com/63347989/137022862-611c0229-342b-4674-85c7-2a99da4930a8.PNG)<br/>
- 32bit
- The EFLAGS register report on the status of the program being executed and allows limited (application program level) control of the processor.<br/>
현재 실행중인 프로그램의 상태 나타내고 제한적이나마 CPU의 동작을 control<br/>
- Flag 종류 세가지  status flags, a control flag, system flags<br/>
- The initial state(초기값) of the EFLAGS register is 00000002H(16진수). <br/>
Bits 1, 3, 5, 15, and 22 through 31 of this register are reserved=현재 사용되지 않음. <br/>
초기값: 컴퓨터의 전원을 켜서 컴퓨터가 최초로 동작을 시작할 때 EFLAGS 에 주어지는 값<br/>
비트값 31(왼):가장높은자리수 ------ 0(오):가장낮은자리수<br/>
- ELFAGS 값은 범용 레지스터와 다르기 때문에 직접 data 값을 읽거나 변경할 수 없음<br/>
대신에 값을 읽거나 변경할 수 있는 명령들이 있음
- The following instructions can be used to move groups of flags to/from the procedure stack or the EAX register: LAHF, SAHF, PUSHF, PUSHFD, POPF, and POPFD <br/>
    - LAHF, SAHF – EAX 레지스터 명령들
    - PUSHF, PUSHFD, POPF, and POPFD – 스택 명령들
- When a call is made to an interrupt / exception handler procedure, the processor automatically saves the state of the EFLAGS registers on the procedure stack<br/>
    - interrupt: CPU가 동작할 때에는 외부에서 신호가 올 때가 있음<br/>
    중간장치라고 불리우는 입출력 장치들이 CPU한테 보내는 신호들<br/>
    Interrupt가 발생하면 CPU는 반드시 특정한 interrupt handler를 실행하도록 되어있음<br/>
    Interrupt handler procedure (함수) 가 시작할 때 반드시 그 당시의 EFLAGS register값을 스택에 저장하도록 하고 있음
    - Exception은 interrupt에 준하는 사건이 CPU 내부에서 발생했을 때를 말함<br/>
    CPU가 동작하다가 에러와 같은 비정상적인 상황이 벌어지면 exception 발생<br/>
    Exception 발생 직후의 CPU의 상태는 interrupt 받은 직후의 상태와 완전히 동일<br/>
    Exception이 발생해도 exception handler procedure(함수)가 작동됨<br/>
    함수를 통해 EFLAGS register값이 스택에 저장됨

![ssw_ch3_12](https://user-images.githubusercontent.com/63347989/137027293-800098ef-5c75-47b7-94ad-25b8eee695b7.PNG)<br/>

- 각 비트가 하나의 flag에 해당<br/>
예외적으로 IOPL만 두 비트가 하나의 flag에 쓰임<br/>
- 그림상에서 회색으로 표시된 부분은 미정비트고 현재 사용되지 않고 있음<br/>
- 초기값은 다 0이고 1번 비트만 1로 시작함
- 사용되는 비트가 18개 
- 세부류로 나눌 수 있음 – S: status flag, C: control flag, X: system flag<br/>
이 중에서 system flag는 운영체제와 같은 시스템 소프트웨어에서만 참조하거나 값을 변경할 수 있음<br/>
일반 응용 프로그램들은 system flag에 접근할 수 없음

### 1. Status flags
The status flags (bits 0, 2, 4, 6, 7, and 11)(6개의 flag) of the EFLAGS register indicate the results of arithmetic instructions, such as the ADD, SUB, MUL, and DIV instructions. <br/>
직전에 실행한 산술 연산 명령어의 상태를 나타냄<br/>
방금 전에 덧셈연산을 실행을 했다고 한다면 <br/>
Status flag는 합 결과 외의 부가적인 정보들 저장

- CF (bit 0) __Carry flag__ — Set if an arithmetic operation generates a carry or a borrow out of the most-significant bit of the result; cleared otherwise. This flag indicates an overflow condition for unsigned-integer arithmetic. <br/>
발생: CF=1, 노발생: CF=0<br/>
덧셈을 할 때 최상위 비트(맨 왼쪽 비트)에서 자리올림 (carry) 발생여부를 나타냄 <br/>
뺄셈을 할 때 borrow 발생여부를 나타냄<br/>
부호 없는 정수 연산에서의 overflow 조건(carry 발생)을 나타냄

- PF (bit 2) __Parity flag__ — Set if the least-significant byte of the result contains an even number of 1 bits; cleared otherwise. <br/>
산술연산 결과값 중 맨아래 한 바이트(8비트)에 대해서 <br/>
1의 개수가 짝수개이면 set이 됨(PF=1).<br/>
홀수개이면 PF=0<br/>
32비트(8비트 4개) 연산을 했을 때 PF는 맨 오른쪽(아래쪽) 한 바이트만 반영<br/>
![ssw_ch3_13](https://user-images.githubusercontent.com/63347989/137114481-c3c8c18b-dbac-4606-a9b6-20400a56c216.PNG)
 <br/>

- AF (bit 4) __Adjust flag__ — Set if an arithmetic operation generates a carry or a borrow out of bit 3 of the result; cleared otherwise. This flag is used in binary-coded decimal (BCD) arithmetic. <br/>
산술연산결과의 비트 위치 3번에서 carry나 borrow가 발생한 경우에 AF = 1<br/>
담겨있는 내용이 BCD라고 했을 때 BCD의 최상위 비트가 비트 3번임<br/>
BCD 연산의 carry 발생여부를 나타낼 때 AF가 사용됨<br/>

- ZF (bit 6) __Zero flag__ — Set if the result is zero; cleared otherwise. <br/>
연산 결과값이 0이면 ZF=1

- SF (bit 7) __Sign flag__ — Set equal to the most-significant bit of the result, which is the sign bit of a signed integer. (0 indicates a positive value and 1 indicates a negative value.) <br/>
산술연산의 결과에서 최상위 비트(제일 왼쪽비트)의 값과 동일한 값을 가짐<br/>
부호있는 정수를 표현할 때 intel CPU는 2의보수로 표현<br/>
그래서 어떤 32비트 숫자가 부호있는 수라면 아마도 2의 보수로 표현되었을 것이고<br/>
2의 보수의 최상위 값은 정수가 양수이면 0이고 음수이면 1인데<br/>
그것을 의미하는 것임<br/>
그래서 SF는 산술연산의 결과값의 부호값임<br/>

- OF (bit 11) __Overflow flag__ — Set if the integer result is too large a positive number or too small a negative number (excluding the sign-bit) to fit in the destination operand; cleared otherwise. This flag indicates an overflow condition for signed-integer (two’s complement) arithmetic<br/>
정수 연산 결과가 너무 큰 양수이거나 너무 작은 음수이면 OF=1 <br/>
부호있는 정수의 범위 <br/>
4비트에서 표현할 수 있는 가장 큰수가 +7 인데 연산결과가 +7보다 큰 경우나<br/>
표현할 수 있는 가장 작은 수 -8보다 연산결과가 더 작은 경우에<br/>
OF=1 로 set 지금 같은 경우가 overflow<br/>
Signed integer 연산의 overflow 조건을 나타냄 <br/>

Overflow 조건<br/>
CF → 부호없는 정수 연산<br/>
AF → BCD 연산<br/>
OF → 부호있는 정수 연산<br/>
Intel CPU가 가지고 있는 산술 연산 명령은 종류가 딱 한가지 <br/>
즉 하나의 덧셈 명령을 가지고 부호있는 정수와 부호없는 정수 연산을 하는데<br/>
그 구분을 overflow 조건을 가지고 구분함<br/>

### 1-1. Status flags의 추가사항들
- Of these status flags, only the CF flag can be modified directly, using the STC, CLC, and CMC instructions. Also the bit instructions (BT, BTS, BTR, and BTC) copy a specified bit into the CF flag. <br/>
Status flag는 일반적으로 산술 연산 명령을 실행함에 따라서 값이 바뀜<br/>
CF flag에 한해서는 산술 연산 명령 외에 주어지는 STC, CLC, and CMC 와 같은 명령을 통해서 CF 값을 바꿀수 있음 <br/>
비트를 다루는 비트 관련 명령들이 있는데 (BT, BTS, BTR, and BTC) 이 명령들에 관해서는 특정한 비트값을 CF에 저장하도록 할 수도 있음<br/>
- The status flags allow a single arithmetic operation to produce results for three different data types: unsigned integers, signed integers, and BCD integers. <br/>
Status flag를 사용하면 단일 산술 연산 명령을 사용해서 세가지의 서로다른 연산을 수행할 수 있음<br/>
Add 명령을 가지고 부호있는 연산, 부호없는 연산, BCD 연산을 할 수 있음<br/>
- The condition instructions Jcc (jump on condition code cc), SETcc (byte set on condition code cc), LOOPcc, and CMOVcc (conditional move) use one or more of the status flags as condition codes and test them for branch, set-byte, or end-loop conditions (조건문 명령)<br/>
Status flag 그당시의 값들에 따라서 분기를 하거나 루프를 반복하거나 이런일들이 벌어짐<br/>
즉 이들은 다 현재 특정한 status flag의 따라서 명령이 실행될 수도 있고 실행되지않을 수 있는 조건문 명령임

### 2. Direction flag
- The direction flag (DF, located in bit 10 of the EFLAGS) controls string instructions (MOVS, CMPS, SCAS, LODS, and STOS). <br/>
10번 비트에 위치, 스트링 명령들을 제어<br/>
스트링 연산은 메모리 상의 데이터가 연속적으로 배치된 경우를 스트링이라고 하는데 배치된 데이터의 크기는 일정해야 함<br/>
예를 들어서 한 바이트 단위씩 100개가 나열되어 있다면 그것은 스트링<br/>
스트링 – 배열<br/>
DF는 배열의 각 원소에 접근할 때 접근방향을 제어함<br/>
- Setting the DF flag causes the string instructions to auto-decrement (to process strings from high addresses to low addresses). <br/>
비트의 값이 set되어있다는 것은 비트의 값이 1로 설정되어 있다는 것을 의미<br/>
DF=1 – 스트링 명령의 동작이 자동감소하는 방향으로 되어있음 값이 큰거에서 작은 방향으로<br/>
주소 번지수가 높은 번지수가 낮은 번지수로 이동을 하면서 스트링 연산<br/>
- Clearing the DF flag causes the string instructions to auto-increment (process strings from low addresses to high addresses). <br/>
비트의 값이 clear되어있다는 것은 비트의 값이 0으로 설정되어 있다는 것을 의미<br/>
DF=0 – 자동 증가하는 방향으로 스트링 명령이 되어있음<br/>
- The STD and CLD instructions set and clear the DF flag, respectively.<br/>
set하고 clear하는 명령은 STD와 CLD 명령을 통해 

## 4. EIP (instruction pointer) register.
![ssw_ch3_10](https://user-images.githubusercontent.com/63347989/137022863-5602b53a-90a4-416d-8b1d-c5df92c87a96.PNG)<br/>
- 32bit
- The EIP register contains a 32-bit pointer to the next instruction to be executed.<br/>
다음 실행할 명령의 메모리 주소값
- The instruction pointer (EIP) register contains the offset in the current code segment for the next instruction to be executed. <br/>
현재 실행중인 code segment(실행할 명령들 가득) 내에서 다음번 실행할 명령의 offset을 가지고 있는 register<br/>
    - Segment 는 일종의 메모리 영역
    - Code segment는 명령들만 모아놓은 segment
    - Code segment 안에 다음번 실행할 명령들이 어딘가에 있음
    - Offset : segment 시작점 에서부터 다음 실행할 명령의 위치간의 거리차이 (주소차이)
 
- It is advanced from one instruction boundary to the next in straight-line code or it is moved ahead or backwards by a number of instructions when executing JMP, Jcc, CALL, RET, and IRET instructions. <br/>
    - EIP 값은 하나의 명령이 실행됨에 따라서 다음 명령의 offset을 가리키도록 증가함<br/>
    EIP register 가 현재 실행할 명령의 offset을 가지고 있다가 그 실행이 끝나게 되면 자동적으로 다음 명령을 가리키게 됨<br/>
    EIP 값이 자동적으로 바뀌는 이 동작은 프로그래밍하는게 아니라 자동적으로 그렇게 됨<br/>
    - 때로는 프로그래머가 인위적으로 JMP, Jcc, CALL, RET, and IRET 와 같은 특정 명령을 실행함으로써 다음 명령이 아니라 프로그래머가 지정한 특정한 위치로 EIP 값을 바꾸게 할 수 있음<br/>
    위 명령들은 프로그래머의 의도대로 바꿀수있는 명령들임<br/>

- The EIP register cannot be accessed directly by software; it is controlled implicitly by control-transfer instructions (such as JMP, Jcc, CALL, and RET), interrupts, and exceptions. The only way to read the EIP register is to execute a CALL instruction and then read the value of the return instruction pointer from the procedure stack. <br/>
EIP register는 프로그램이 직접 접근할 수 없음<br/>
하지만 분기명령 같은 경우는 직접 접근할 수 있음<br/>
CALL 이라는 함수 실행 명령한 후 스택에 저장된 복귀 주소값을 읽어내는 방법이 있음(제한된 경우)
- The EIP register can be loaded indirectly by modifying the value of a return instruction pointer on the procedure stack and executing a return instruction<br/>
값을 바꾸는 경우 함수와 관련해서 return 명령을 쓰면 스택의 복귀주소를 사용해서 EIP 값이 새로 지정될 수 있음


