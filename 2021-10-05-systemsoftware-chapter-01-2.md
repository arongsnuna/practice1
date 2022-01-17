---
layout: post
title: Introduction of System Software 2
subtitle: SystemSoftware_01_2
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# REGISTER
CPU가 Fetch-Execution Cycle을 통해 동작하는데, Fetch와 Execution 을 하려면 메모리가 필요하다. <br/>
__Register__ : CPU 내부의 메모리<br/>
- 크기가 워드 단위 (한워드 정도 (약 32비트) -> 숫자하나 정도를 담을 수 있는 메모리)
- CPU 하나에 register가 여러 개 있음

### MM(Main Memory)과 Register 차이
MM과 Register은 기능상 비슷하지만 전혀 다르다.<br/>
MM는 현재 실행중인 프로그램 자체를 저장하는데 사용하고 <br/>
Register은 Fetch-Execution cycle에서 현재 진행중인 instruction과 관련된 내용만 저장하는데 사용한다. <br/>
<br/>
또한, 접근 속도 차이도 크다. <br/>
Register은 메모리가 내부에 있기 때문에 접근 속도가 굉장히 빠르지만 (한 clock cycle 내에 접근가능)<br/>
MM은 RAM 자체의 접근 속도도 느리고, BUS를 통해서 접근하기 때문에 접근 속도가 굉장히 느리다.  <br/>

# REGISTER의 구분

1. User-visible registers <br/>
 - Enable programmer to minimize main-memory references by optimizing register use
 - Execution 단계에서 instruction을 실행할 때 (연산을 실행할 때) 연산의 대상이 되는 데이터를 보관하는데 사용하는 레지스터
 - 사용자(프로그래머)가 값을 읽거나 변경가능

2. Control and status registers<br/>
- CPU의 동작(Fetch-Execution Cycle 반복)을 제어하는 레지스터 
- Used by processor to control operating of the processor)
- Used by privileged operating-system routines to control the execution of programs

## User-visible registers
CPU instruction(machine language) 실행 과정에서 직접 접근하게 되는 레지스터 <br/>
연산의 대상(피연산자) 데이터 저장 <br/>

May be referenced by machine language<br/>
Available to all programs - application programs and system programs<br/>

### Types of registers
1. Data 
2. Address <br/>
연산하는 데이터가 CPU내에 있지 않고 MM 에 있는 경우, User-visible register에 MM의 주소를 담을 수 있다. <br/>
주소를 지정하는 방식에 따라
    - Index <br/>
    MM 상에 같은 타입의 데이터 여러 개가 연속적으로 배치되어 있는 형태(=배열)의 몇 번째 데이터에 접근하는지 지정<br/>
    Involves adding an index to a base value to get an address
    - Segment pointer<br/>
    CPU에 따라서 실행중인 프로그램이 MM에 적재될 때 segment 단위로 프로그램이 MM에 배치된다. (CPU 특성)<br/>
    Segment 기반의 MM를 사용하는 경우에 어느 segment 에 접근해야하는지, segment내에서 어느 부분에 특정하게 접근해야하는지를 user visible register 를 통해서 지정하게 된다.<br/>
    When memory is divided into segments, memory is referenced by a segment and an 
offset
    - Stack pointer<br/>
    Stack: 프로그램 실행에 있어서 반드시 필요로 하는 메모리 영역<br/>
    연산의 대상을 스택에 저장하기도 한다. <br/>
    Stack의 Top을 통해서만 접근 가능 <br/>
    Points to top of stack<br/>

## Control and status registers
CPU의 전반적인 동작(=Fetch-Execution Cycle의 반복)을 원활하게 이루어지도록 하는데 역할을 함
1. PC (Program Counter) <br/>
PC는 다음 fetch 단계에서 실행할 instruction의 MM 상의 주소를 가지고 있다.<br/>
CPU는 매번 실행할 instruction을 MM에서 fetch해오는데, PC register가 담고 있는 메모리 주소에서 실행할 instruction을 가져온다 <br/>
Contains the address of an instruction to be fetched

2. IR (Instruction Register)<br/>
IR은 Fetch 단계에서 MM으로부터 가져온 instruction을 CPU내에 담아둔다.<br/>
즉, IR 의 내용은 현재 실행중인 명령의 내용이다. <br/> 
다음 fetch 단계에서 MM로부터 새 명령이 운반되어오면 IR에 덮어씌어진다. <br/>
Contains the instruction most recently fetched

3. PSW (Program Status Word)<br/>
PSW는 현재 CPU의 상태에 대한 정보를 담고 있다. <br/>
PSW의 크기는 CPU가 지정해놓은 word 크기 <br/>
ex. PSW의 크기가 32bit 라면, 32가지 정보를 담을 수 있음 (한 비트에 한가지 정보 저장 가능)<br/>
    - Interrupt enable/disable : 입출력 장치와의 통신 중에 interrupt가 발생할 수 있는데, interrupt 신호를 접수 받거나 무시하거나 도전할 수 있는 비트도 있음<br/>
    - Supervisor/user mode : 현재 cpu의 실행모드 제어 
    - Condition codes(=Flags) : 최근 실행한 명령의 연산 결과에 대한 부가적인 정보들 <br/>
    PSW에는 condition 코드라 부르는 비트가 있음<br/>
    Condition codes은 직후에 이뤄지는 명령 실행시에 영향을 미침<br/>
    (ex. Positive / Negative result, Zero, Overflow )

! 주의<br/>
특정한 CPU마다 register의 이름이 달라질 수 있다. (ex. Intel CPU에서 PC = IP or EIP)<br/>

# Program Execution of a Hypothetical Machine 
가상머신에서의 프로그램 실행

![ssw_ch1_7](https://user-images.githubusercontent.com/63347989/136019320-8356bbc4-07e0-407f-aaa6-6411fc85121b.PNG)

1. CPU<br/>
- ALU : 실제 연산을 수행하는 부분 (ALU 제외 나머지는 CPU에서 사용하는 레지스터)
- Control and Status Register<br/>
    - PC<br/>
    - IR<br/>
- User Visible Register<br/>
    - MAR, MBR, AC <br/>
    CPU가 MM에 있을 때 접근하는 레지스터<br/>
    - I/O AR, I/O BR <br/>
    CPU에서 입출력을 실행할 때 사용하는 레지스터<br/>


2. Main Memory<br/>
MM에 program (instruction + data)이 적재되어 있는 모습을 표현 <br/>
프로그램이 동작하기 시작하면 CPU가 각각의 명령들을 하나씩 가져닥 실행을 하게됨 <br/>
MM에서 한 칸이 한 바이트 크기, 이 바이트가 n 개 있음 -> n 바이트<br/>
각 바이트 마다 오른쪽에 숫자가 있음 이 숫자는 각바이트의 메모리 주소 (D1 = -1)

3. I/O Module <br/>
모든 입출력 장치에는 장치마다 내부에 buffer가 있다. <br/>
buffer는 입출력 연산중인 입출력에 사용되는 데이터를 임시로 보관하는 메모리 (일종의 메모리)

<br/><br/>
모든 CPU는 자신만의 고유의, 일련의 Register set을 가지고 있다. <br/>
이 가상의 머신에서는 User Visible Register 로 MAR, MBR, AC, I/O AR, I/O BR가 있다고 가정했다. <br/>
<br/>
User Visible Register<br/>
- Memory address register (MAR) <br/>
CPU가 명령을 실행하는 과정에서 메모리에 있는 데이터에 접근하는데 사용하는 레지스터<br/>
MAR은 데이터의 주소를 나타낸다. <br/>
(Specifies the address for the next read or write)
- Memory buffer register (MBR)<br/>
메모리에 있는 연산할 데이터를 가져와서 임시로 보관하는 레지스터<br/>
MAR에서 가져온 데이터를 MBR에 보관을 한다. => MAR과 MBR은 pair로 구성된다.<br/>
(Contains data written into memory or receives data read from memory
- Accumulator(AC)<br/>
본연의 용도가 따로 있긴 하지만, MBR과 비슷한 용도로 사용되는 레지스터
- I/O address register<br/>
CPu가 메모리 접근할 때 사용하는 것과 유사하게, 입출력시 사용되는 레지스터<br/>
입출력할 데이터가 메모리에 있는데 그 메모리 주소를 가지고 있음
- I/O buffer register<br/>
해당 입출력 장치의 버퍼 주소가 담긴 상태에서 입력이나 출력할 대상 데이터를 가지고 있음

## Characteristics of a Hypothetical Machine
![ssw_ch1_8](https://user-images.githubusercontent.com/63347989/136023887-b8626b35-187a-4e3e-a24e-58d783c99111.PNG) <br/>
명령의 포맷은 16 비트이다. <br/>
4 비트는 opcode (이 명령이 어떤 연산을 수행하는지 표시하는 부분) <br/>
12 비트는 메모리 주소 표시하는 부분<br/>
Opcode가 나타내는 연산의 대상이 되는 데이터가 메모리의 어디에 있냐를 12비트의 주소를 통해 표시

![ssw_ch1_9](https://user-images.githubusercontent.com/63347989/136023889-ed962fdf-5482-48dd-8cf2-2ad45e2b5700.PNG)<br/>
이 가상머신에서 사용할 데이터의 포맷<br/>
최상위 한비트 s는 부호비트 (양수 음수) <br/>
S=0 양수, S=1 음수<br/>
15 비트는 magnitude (나타내고자하는 정수의 절댓값)<br/>
15 비트로 표현하고자하는 수의 크기를 나타내고 제일 왼쪽 한비트는 부호를 정하는 형식으로 데이터 표현

![ssw_ch1_10](https://user-images.githubusercontent.com/63347989/136023892-99a2668e-932d-466e-a2e2-948c2aafc2f7.PNG)

![ssw_ch1_11](https://user-images.githubusercontent.com/63347989/136023884-f7554d20-330d-400f-b359-26de492c6fb6.PNG)<br/>
4 비트에 들어갈 실제 의미있는 4 비트 값들의 예<br/>
0001 - 해당명령이 뭐하는 명령인지 load 명령 (load = CPU가 메모리에 있는 데이터를 레지스터로 복사하는 동작)<br/>
0010 - ac 값을 메모리로 store 한다 (store = CPU가 내부 레지스터가 가지고 있는 값을 복사해서 메모리의 특정 위치로 복사하는 동작) <br/>
0101 - ac값과 메모리 값을 덧셈을 하는 동작 <br/>

! M -> R : load<br/>
! R -> M : store<br/>

## Example of Program Execution
PC의 특성때문에 순차적으로 실행된다. <br/>
![ssw_ch1_12](https://user-images.githubusercontent.com/63347989/136028114-8dafbc31-fd39-4c55-9180-4c359709d5fe.jpg)
<br/>
<br/>
다음 장에 계속 ...
