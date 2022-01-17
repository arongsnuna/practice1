---
layout: post
title: Intel Assembly II - Control Transfer Instructions
subtitle: SystemSoftware_07_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---


프로그램은 fetch-execution cycle에 의해서 순차적으로 실행됨<br/>
프로그램의 순차적 실행 흐름을 바꿀 수 있는 명령 - control transfer instruction <br/>
프로그램 실행을 제어할 수 있는 명령<br/>

프로그램을 이루고 있는 명령들이 메모리 올라가 있는데,<br/>
PC 레지스터가 이번에 실행할 명령을 가리키고 있는 상태<br/>
PC는 어떻게 동작하냐?<br/>
PC의 값은 다음번 명령을 가리키도록 갱신됨<br/>
이번에 fetch 한 명령의 크기만큼 주소값이 변함<br/>

# Control Transfer Instructions <br/>
엉뚱한 곳으로 명령이 점프함<br/>
PC값을 다음 명령을 가리키도록 하는게 아니라 Control Transfer 명령이 제시한 주소값으로 바뀜<br/>

- 1. Unconditional transfers 무조건 분기명령 <br/>
분기명령이 지정한 곳으로 무조건 실행됨<br/>
Ex) c에서 goto 문, c에서 함수호출 (Call, 함수 호출, 무조건 분기명령), 복귀(return, 지정한 곳으로 복귀)<br/>
    - Jump 
    - Call and return (covered later) 
- 2. Conditional transfers 조건 분기명령
조건을 만족하면 분기를 하고, 조건을 불만족하면 분기가 일어나지 않고 순차명령<br/>
- 3. Software interrupts (covered later)<br/>
다양한 입출력 장치가 있고 버스로 연결되어 있는데, <br/>
Cpu는 입출력 개시 명령만 지시하고, 자기 원래하던 일(프로그램 실행, 명령어 실행)을 함<br/>
시간이 지나면 출력이 끝나면 입출력 장치가 CPU한테 출력 끝났다고 알려줌: __interrupt__ <br/>
interrupt 종류에는 hardware interrupt와 software interrupt가 있음. <br/>
IO장치가 CPU한테 보내는 신호: hardware interrupt<br/>
신호를 왜 보내냐 하면 입출력이 끝났다고 알리려고<br/>
Interrupt를 받으면 CPU가 하던 일을 멈추고, interrupt를 처리하는 함수를 실행하게 됨 = 분기임.<br/>
Interrupt는 분기를 만들어내는 하드웨어적인 원인이 됨.<br/>
<br/>
소프트웨어적 원인은 CPU가 동작하다 원인을 제공해서 (내부적인 문제) 문제를 해결하는 분기가 일어남<br/>
내부적인 문제가 생겨서 CPU의 상태가 hardware interrupt를 받은 직후와 유사한 상태로 바뀌는 것<br/>
Ex 나눗셈 명령에서 0으로 나누면 software interrupt발생 – 잘못된 명령을 처리하는 함수 실행됨<br/>
<br/>
하드웨어 인터럽트와 소프트웨어 인터럽트는 원인이 다른것임<br/>

# 1. Unconditional Transfer 무조건 분기
# jmp (=goto)
- Transfers program control to a different point in the instruction stream without recording return info <br/>
복귀에 대한 정보를 저장하지 않고 (즉 돌아오지 않음) 프로그램의 제어를 instruction stream에서 다른 곳으로 이동시키는 명령 <br/>
cf. 돌아올 것을 약속하고 분기하는 명령 = 함수 호출 call<br/>
- The destination operand can be an immediate value, a general-purpose register, or a memory location (absolute offset vs. relative offset) <br/>
Operand가 한 개임 (분기할 지점=주소)<br/>
operand는 immediate 값 or 범용 레지스터 or 메모리 주소(그 주소에 명령이 있어야 함)가 될 수 있음.<br/>
- Immediate operand can be specified by __code label__ <br/>
주소에 label을 달아놓고 label이 operand가 되기도 함 (code label)
## Code label 
- A label in the code segment that the program control is transferred to <br/>
code 세그먼트 내의 label만!
- The label is created by programmer; long(긴) and understandable(가늠될 수 있는) labels are useful 
- Colon is often appended 
- No two lines in the code segment may have the same label<br/>
Label은 주소이다. <br/>
한 코드 세그먼트내에서 같은 레이블을 사용할 수 없다. <br/>

## jmp 사용 예시
![ssw_ch7_1](https://user-images.githubusercontent.com/63347989/142731001-47189b0b-01c9-414b-90a8-3db11b83d409.PNG) <br/>
Program continues adding 2 to the EAX register forever; it is a infinite loop <br/>
Xyz – code label //xyz는 add 명령이 있는 주소임 <br/>
끝나지 않음

## jmp의 종류
점프는 3가지 종류가 있음<br/>
3가지를 구분하는 기준은, 점프의 거리 (레이블의 주소 – 점프 명령의 주소)<br/>
점프 명령의 위치와 점프의 대상이 되는 분기점 간의 거리<br/>

1. near jump<br/>
within a segment (default jump)<br/>
가장 일반적인 점프<br/>
한 세그먼트 내에서의 점프<br/>
![ssw_ch7_2](https://user-images.githubusercontent.com/63347989/142732651-73d08e04-a56c-44f9-8465-35592139ec05.PNG)<br/>

2. far jump <br/>
allows control to move to another code segment<br/>
점프의 대상이 되는 지점과 점프 명령이 서로 다른 세그먼트에 있을 때 사용 <br/>
code segment가 2개 있어야함 segment를 가로질러서 사용됨<br/>
![ssw_ch7_3](https://user-images.githubusercontent.com/63347989/142732652-0bb58a11-2731-42c6-a76b-14c5c48c30ec.PNG)<br/>

3. short jump <br/>
 uses a single byte to store the displacement of the jump (+127/-128 bytes)<br/>
주소 차이 = displacement of the jump, 변위<br/>
near 점프의 특수한 케이스<br/>
차이가 single byte로 표현할 수 있으면 short jump<br/>
![ssw_ch7_4](https://user-images.githubusercontent.com/63347989/142732762-56e895a3-f421-4bea-9047-73f444a763f0.PNG)<br/>

# 2. Conditional Transfer 조건 분기
Conditional jump is a jump carried out on the basis of a truth value <br/>
조건 분기는 특정한 값에 기반을 하고 jmp 명령이 제시한 값에 만족하는 경우 분기함. <br/>
Decisions are based on one-bit in __eflags__ : ZF, SF, CF, OF, AF, and PF and IF
- JZ branches only if ZF is set (Zf=1이면 분기함)
- JNZ branches only if ZF is unset (Zf=0이면 분기함)
- JO branches only if OF is set 
- JNO branches only if OF is unset 
- JS branches only if SF is set 
- JNS branches only if SF is unset 
- JC branches only if CF is set 
- JNC branches only if CF is unset 
- JP branches only if PF is set 
- JNP branches only if PF is unset

## 조건 분기 명령 예시
![ssw_ch7_5](https://user-images.githubusercontent.com/63347989/142732927-82c32ad7-7937-4a90-9f06-afb1e3300719.PNG)<br/>
분기명령 바로 직전에는 대부분 status 값에 영향을 미치는 명령(산술 논리 연산)이 함께 사용되곤 함. <br/>
JS : SF = 1 분기해라<br/>
near jump 임. <br/>
<br/>
SUB EBX, EAX 의 결과에 따라서 달라짐.<br/>
EAX가 더 크면 분기가 일어나고 아니면 조건을 만족하지 않기 때문에 순차실행을 함.<br/>

## Programs with loops
![ssw_ch7_6](https://user-images.githubusercontent.com/63347989/142733124-3c4ccc34-a55d-4ae2-b3a4-e594124a4ec3.PNG)<br/>
JNZ RPT => ZF = 0이면 RPT로 가라. <br/>
반복횟수: 분홍색 루프에 진입할 때의 EBX 레지스터의 담긴 값.<br/>
반복이 되면서 ecx += eax가 되고 있음. <br/>
즉, 이 코드는 ecx += eax*ebx<br/>

## Comparison-based Jump 
조건부 분기 명령 실행하려면 분기 명령 직전에 flag 값을 변경할 수 있는 산술 명령 연산을 사용함 <br/>
그중에서 cmp가 많이 쓰임. <br/>

__cmp vleft, vright__ <br/>
- For unsigned integer 부호없는 정수 뺼셈 <br/>
: ZF(연산결과가 0인지) and CF(부호없는 정수 연산에서의 overflow 표시) <br/>
	- If vleft == vright, ZF == 1 and CF == 0 <br/>
	결과값이 0이 나옴, carry 발생 x
	- If vleft > vright, ZF == 0 and CF == 0 <br/>
	결과값이 0이 아님, carry 발생 x
	- If vleft < vright, ZF == 0 and CF == 1 <br/>
	결과값이 0이 아님, carry 발생 o
- For signed integer 부호있는 정수 뺄셈<br/>
: ZF(연산결과가 0인지), OF(부호있는 정수 연산에서의 overflow 표시), and SF(부호 flag) 
	- If vleft == vright, ZF == 1, OF == 0, and SF == 0 <br/>
	- If vleft > vright, ZF == 0 and OF == SF 
	결과값이 0이 아님<br/>
	![ssw_ch7_7](https://user-images.githubusercontent.com/63347989/142733425-72e2b41b-716d-40be-94aa-5e8e466020e8.PNG)<br/>
		- vleft > vright >0 <br/>
		5-3 = 2 <br/>
		OF = 0 (오버플로우 발생 X), SF = 0 (양수라서)
		- 0 > vleft > vright <br/>
		-2-(-3) = 1 <br/>
		OF = 0 (오버플로우 발생 X), SF = 0 (양수라서)
		- vleft > 0 > vright <br/>
		3-(-2) = 5<br/>
		OF = 0 (오버플로우 발생 X), SF = 0 (양수라서) <br/>
		!!주의!! vleft 매우 큰 수, vright 매우 작은 수 하면 overflow 발생할 수 있음<br/>
		OF = 1(오버플로우 발생 O), SF = 1 !!!!!!!!!!!!!!!!!!!!!!!!
	- If vleft < vright, ZF == 0 and OF ≠ SF <br/>
	![ssw_ch7_8](https://user-images.githubusercontent.com/63347989/142733519-cdca6f46-6bae-4b47-8809-9ea828f5de76.PNG) <br/>
		- vright > vleft > 0
		- 0 > vright > vleft
		- vright > 0 > vleft
		!!주의!! vright 매우 큰수, vleft 매우 작은 수 하면 overflow 발생<br/>
		OF = 1 (오버플로우 발생), SF = 0 <br/>
		OF XOR SF = 1
<br/>

## 점프 명령의 이름들 
조건 분기 명령의 종류가 여러가지인데, 대소관계에 따라서 쓰면 됨. <br/>
![ssw_ch7_9](https://user-images.githubusercontent.com/63347989/142733624-b6cef73d-9912-40c0-a7ea-0a2df4cf2e98.PNG)
- Signed <br/>
JL(작거나) = JNGE(크거나 같지 않은) <br/>
L : Less than, G : Greater than, E : Equal, N : Not<br/>
- Unsigned <br/>
B : Below (작거나), A : Above (크거나)

### 부호있는 정수인지, 부호없는 정수인지는 어떻게 구분하고 판단?
프로그래머가 일관성있게 주장하면 됨. <br/>
1111 1111 -> Unsigned – 255, Signed - -1<br/>

## Unsigned Conditional Jumps 
![ssw_ch7_10](https://user-images.githubusercontent.com/63347989/142733625-530febac-0fdd-40e9-82dc-f7a11fa6541b.PNG)

## Signed Conditional Jumps
![ssw_ch7_11](https://user-images.githubusercontent.com/63347989/142733626-e14bda7e-309a-4cb8-91e7-7c0262cc11bf.PNG)

## Translating Standard Control Structures
## if
- if statements <br/>
![ssw_ch7_12](https://user-images.githubusercontent.com/63347989/142733627-b63133c7-3b84-40bd-929d-491a088f2b0c.PNG)<br/>
먼저 cmp 명령이 오고 그 다음에 분기 명령 jxx <br/>
Jxx를 만족하면 endif로 가고, 불만족하면 그대로 쭉가면 됨. <br/>
- if-else statements <br/>
![ssw_ch7_13](https://user-images.githubusercontent.com/63347989/142733628-967a3008-7a1b-4904-aa30-68e1cd2a476f.PNG) <br/>
jxx else_block을 만족하면 else_block으로 이동, 불만족하면 그대로 쭉가면 됨.<br/>
불만족할 때, 쭉가다보면 else_block을 마주쳐서 else_block 가기전에 endif으로 이동할 수 있게 해줘야함.<br/>
그래서 jmp endif는 무조건적으로 필요함. <br/>

## while
- while loops <br/>
![ssw_ch7_14](https://user-images.githubusercontent.com/63347989/142733629-ce7661ce-8c94-490b-b7ca-465f9b8d4a2d.PNG) <br/>
jmp while – 처음으로 돌아감<br/>
매번 loop의 앞머리 부분에서 반복을 계속할지 결정함.<br/>
- do – while loops<br/>
![ssw_ch7_15](https://user-images.githubusercontent.com/63347989/142733631-5da1d819-1004-4cb3-b49d-270519ddf083.PNG) <br/>
loop의 마지막 부분에서 반복을 계속할지를 결정함. <br/>
최소한 한번은 body_of_loop이 실행됨.<br/>

## Loop Instruction 
Combination of decrement ecx and jnz conditional jump. <br/>
	- Decrement ecx <br/>
	ecx에 반복횟수
	- If ecx != 0, jump to label <br/>
	else fall through. <br/>
	ecx 값이 0이 아니면 label로 점프함. <br/>
	0이면 loop 다음 명령으로 진행됨.<br/>
LOOP, LOOPE (loop while equal), LOOPZ (loop while zero), LOOPNE (loop while not equal), and LOOPNZ (loop while not zero)<br/>
![ssw_ch7_16](https://user-images.githubusercontent.com/63347989/142733622-0abe0073-c4e0-456b-9fc5-9599746a3509.PNG)<br/>
 