---
layout: post
title: Stack and Subprograms
subtitle: SystemSoftware_10_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# 9. Stack and Subprograms <br/>
스택은 프로그램 메모리상의 영역 중의 한군데 (프로그램을 구성하는 segment 중 하나)<br/>
Subprogram은 함수<br/>
스택이랑 subprogram이랑 무슨 연관? <br/>
함수가 실행될 때, 스택이라는 메모리가 많이 사용됨. <br/>
메모리의 단위가 segment (text, data, bss, stack …)<br/>
Text, data, bss는 명확하게 표시해주지만 stack은 명확하게 명시하지 않아도 나중에 프로그램이 실행될때, 운영체제에 의해서 프로그램에 자동적으로 주어짐<br/>

# Purpose of Stack 
Stack is a portion of memory(메모리의 일부) which is used to 
스택은 함수와 관련됨. <br/>
스택이 없으면 함수를 만들어 사용할 수 없음<br/>
프로그램 중에 함수 단위로 이루어지지 않은 프로그램은 없음. 즉 프로그램은 함수를 기반으로 이루어져있음. <br/>
스택 세그먼트가 없으면 프로그램이 실행되지 않음 . <br/>
- To save return address in subprogram call <br/>
함수 호출시에 복귀주소 저장
- To pass parameters to subprograms (including C function calls) <br/>
함수 호출시에 파라미터 전달(스택에 파라미터를 저장)<br/>
- To allocate space for local variables for subprograms <br/>
스택은 지역변수가 할당되는 공간(저장하는데 사용)<br/>
- To save registers to be preserved across subprogram calls <br/>
함수가 실행되는 동안에 프로그래머가 값을 유지시키고자 하는 그런 레지터를 스택에 저장하는 역할을 함.<br/>
특정한 레지스터의 값을 임시로 복원하는데 사용<br/>

<br/>
In Linux <br/>
스택은 운영체제에 의해서 주어지는 공간
- ESP(스택의 탑을 나타냄) is already set up by OS when a program starts <br/>
운영체제에 의해서 적정한 값을 setup 됨.
- Storing an arbitrary value in ESP would disrupt the existing stack <br/>
esp에 임의의 값을 저장하게 되면 존재하는 스택이 망가지게 되고 스택의 탑이 잘 못 지정되는 것이기 때문에 그 이후의 프로그램에 실행에 있어 심각한 오류가 발생하게 됨. <br/>

# Push and Pop
ESP register 
- Stack pointer 
- Keeping track of top of the stack <br/>
ESP는 스택의 탑을 가리키도록 값이 유지됨. <br/>

## PUSH instruction
PUSH EAX <br/>
(오퍼렌드 하나) EAX에 있는 4바이트 DOUBLE WORD를 ESP레지터의 PUSH 해라. <br/>
한칸에 한 바이트 들어갈 수 있음. <br/>
__PUSH EAX__
&nbsp; SUB ESP, 4 <br/>
&nbsp; MOV [ESP], <br/>
스택은 주소가 감소하는 방향으로 메모리가 채워짐<br/>
그리고 EAX 들어간 값이 LITTLE-ENDIAN 방식으로 스택으로 들어감 (낮은 번지 수가 높은 번지 수에 들어감)<br/>
![ssw_ch_1](https://user-images.githubusercontent.com/63347989/144408213-397544d4-27a3-4a7b-9a1c-dbbedc0c38ca.PNG)<br/>

## POP instruction 
꺼낸 데이터를 스택에서 제외됨. <br/>
아랫쪽 그림에서 위쪽 그림처럼 됨. <br/>
__POP EBX__ <br/>
&nbsp; MOV EBX, [ESP] <br/>
&nbsp; ADD ESP, 4
<br/>
The order of pops is the exact reverse of the order of the pushes<br/>
PUSHA와 POPA은 정확히 반대 ! <br/>
&nbsp; push eax <br/>
&nbsp; push ebx <br/>
&nbsp; pop ebx <br/>
&nbsp; pop eax <br/>
-> 이렇게 해야 원래의 값으로 돌아옴. <br/>


# CALL and RET
## CALL 
CPU 명령인데, CALL하고 (32bit 주소-오퍼렌드 하나),, <br/>
함수 호출 명령 (jmp 명령과 비슷하게 분기명령) <br/>
- Pushes the address of the next instruction on the stack, and <br/>
다음 명령(call 다음에 있는 명령)의 주소(복귀주소)를 스택에 push하고 
지정된 주소(call 명령의 파라미터)로 jmp 
- Jumps to certain address <br/>
분기함. <br/>

## RET
함수의 실행이 끝났을때 호출한 지점으로 돌아가는 명령 <br/>
오퍼렌드 없음<br/>
- Pops off an address , <br/>
스택의 탑에 있는 32비트 주소(정상이라면 복귀주소)를 꺼내서
- and Jumps to that address <br/>
그 주소로 분기함. <br/>

It is very important that one manage the stack correctly so that the right number is popped off by RET instruction
스택의 탑에 올바른 주소가 들어가있어야할 책임은 프로그래머에 있음. <br/>
탑에 올바른 주소가 들어가있도록 하는게 중요함. <br/>

![ssw_ch_2](https://user-images.githubusercontent.com/63347989/144408219-e34c7b7f-080a-4355-8169-b12d6417c8d2.PNG)
<br/>
mov 명령의 주소값을 stack에 push하고 분기<br/>
ret이 이뤄지는 그 순간에 스택의 탑에서 꺼내서 그곳으로 분기가 이뤄짐 <br/>

# Calling Convention 함수 호출 규칙
함수를 만들 떄 함수 호출 규칙을 지켜서 만들자.
## Call and return 
- A subprogram is invoked with a CALL instruction and returns via a RET <br/>
함수 호출인 CALL 명령을 통해서 하고, 복귀는 RET 명령을 통해서 한다.

## Parameter passing 
함수는 파라미터를 가지고 있는 경우가 있는데, <br/>
내 파라미터를 전달하는데 사용되는 규칙. <br/>
- Parameters are pushed by the caller <br/>
caller가 스택에 push해서 파라미터를 전달<br/>
- Parameters on the stack are accessed using EBP by subprogram <br/>
함수가 파라미터에 접근할때는 스택에 그냥 들어있는 상태에서 ebp 레지스터를 base 레지스터로 해서 스택에 있는 파라미터에 접근하도록 함. (indirect addressing해서 접근) <br/>
- Parameters are removed by the caller <br/>
함수호출이 끝나서 return 하면 Return 직후에 caller가 파라미터 제거<br/>

## Local variables 지역 변수
- Local variables are allocated on the stack <br/>
함수가 실행되는 동안에는 지역변수가 사용될 수 있은데,<br/>
그 지역변수를 스택에 할당함.<br/>
- Local variables are accessed using EBP too 
스택에 할당된 지역변수에 접근할 때는 파라미터 접근할때와 동일하게,<br/>
ebp 레지스터를 통해서 접근함. <br/>

## Return value 
Return value is passed via EAX register<br/>
복귀값(함수가 caller한테 전달하는 값)은 EAX 레지스터를 사용해서 전달. <br/>

# Calling Convention - [Parameter Passing]
Parameters to a subprogram may be passed on the stack <br/>
파라미터는 스택에 담겨서 전달이 된다. <br/>
- Parameters are pushed by the caller in the reverse order that they appear in the call <br/>
파라미터가 여러개 있을때, 스택에 담는 순서는 함수호출때 보이는 순서의 역순임. <br/>
- Parameters on the stack are not popped off by the subprogram, instead they are accessed from the stack itself <br/>
파라미터가 스택에 들어가있는데 꺼내지 말고 스택에 담긴채로 접근을 하자. <br/>
- Parameters on the stack are accessed using indirect addressing with base register __EBP__ (not ESP) by subprogram, like [EBP+8] <br/>
스택에 담긴채로 접근을 할때 base 레지스터로 ebp 레지스터를 사용해서 indirect addressing 해서 접근함. <br/>
- Parameters are removed after RET instruction, i.e. also by the caller <br/>
복귀 명령 바로 직후에, caller가 스택에서 파라미터를 제거함.<br/>
아래와 같은 경우 때문에, 함수보다 caller가 제거하는게 편함. <br/>
    - To support varying number of arguments <br/>
    함수를 호출할때마다 파라미터의 개수가 바뀌는 경우가 있음 <br/>
    ex. c - printf<br/>
    - ADD or POP instructions<br/>
    esp 스택 포인터 레지스터에 값을 더하는 것만으로도 파라미터를 제거할 수 있음. <br/>
    pop 명령도 가능<br/>


## Accessing Parameters with ESP
스택에 담긴채로 접근을 할때 base 레지스터로 esp 레지스터가 아니라, __ebp__ 레지스터를 사용해서 indirect addressing 해서 접근함. <br/>
왜 스택접근할때 esp 레지스터가 base 레지스터로 적절하지 않은가?<br/>
스택에 push 해서 데이터가 늘어날때 낮은 번지쪽으로 늘어남. <br/>
![ssw_ch_3](https://user-images.githubusercontent.com/63347989/144408220-5eaa5098-759c-4897-94da-8acce18b6732.PNG)<br/>
함수 호출 직후의 스택의 탑에는 복귀주소가 있음 <br/>
그 이전에는 파라미터가 들어가 있음.<br/>

![ssw_ch_4](https://user-images.githubusercontent.com/63347989/144408226-e31428b1-bf9d-4881-9cca-f02300ca2843.PNG) <br/>
함수가 스택을 사용하게 되면 esp가 바뀌게 되어서 이때 스택의 탑은 함수 데이터임. <br/>
esp는 함수가 별도의 용도로 스택을 쓰게 됨으로써 esp의 값이 계속 바뀜. <br/>
<br/>
그래서 스택에 접근할때 esp가아니라 ebp가 base 레지스터로 사용됨<br/>

## Accessing Parameters with EBP
![ssw_ch_5](https://user-images.githubusercontent.com/63347989/144408227-9edfc262-5504-4c10-a2b6-55ee2ca012d2.PNG)<br/>
스택에 데이터를 넣거나 빼거나 할때는 늘 4바이트 (double word) 크기로 push와 pop을 함. <br/>
- parameter <br/>
함수 호출하는 측에서 call 명령 전에 파라미터를 스택에 넣어놓는다. <br/>
- return address <br/>
call 명령이 실행이 되면서 call 명령의 동작의 일환으로 return address가 들어감. <br/>
- saved EBP<br/>
    - ebp 기준으로 스택에 접근하기 위해서 ebp 레지스터 값을 함수가 실행되는 내내 고정된 값으로 지정할 필요가 있음.<br/>
    함수가 시작되는 첫머리에 그당시 ebp 레지스터 값을 스택에 push함<br/>
    이전 호출한 쪽이 사용한 ebp 레지스터 값을 보존하고자하는 의도로 ebp를 스택에 push 함. <br/>
    - 그 당시 esp 레지스터 값을 ebp에 카피를 함. <br/>
    그래서 ebp는 saved ebp를 가리키고 있는 곳을 가리키게 됨. <br/>
    ebp는 함수의 진행이 끝날때까지 값이 바뀌지 않음. <br/>
- subprogram data<br/>
함수가 진행되는 동안 esp는 얼마든지 값이 변할 수 있음 .<br/>
이때 ebp가 값을 고정적으로 가지고 있기 때문에 esp 값이 변하는 건 상관이 없음 <br/>


## General Caller and Callee Form with Parameter Passing
![ssw_ch_6](https://user-images.githubusercontent.com/63347989/144408185-1267d93a-3fa7-4c3e-aa40-8535cd6ac136.PNG)<br/>
- caller: 함수 호출 <br/>
파라미터 값: 1 (4바이트)<br/>
add esp, 4 <br/> 
esp에 4를 더하면 파라미터 위에 있는 스택을 가리키게 됨으로써 스택에서 파라미터를 제거하는 것임.<br/> 
그렇다고 스택의 내용이 사라지는 것은 아님. <br/> 
그래도 스택의 탑이 바꼈기 때문에 스택의 탑 아래쪽은 더이상 스택의 일부가 아님. <br/> 
- callee: 함수 자신<br/>
함수의 이름: subprogram<br/>
![ssw_ch_5](https://user-images.githubusercontent.com/63347989/144408227-9edfc262-5504-4c10-a2b6-55ee2ca012d2.PNG)<br/> 
이 모양 그대로 진행됨. <br/> 
그리고 리턴하기 직전에 pop ebp해서 original ebp 값을 복구함<br/> 
pop ebp를 성공적으로 진행이되면 esp는 복귀주소를 가리키게 됨. <br/> 
리턴 명령에 의해서 함수 호출 직후 명령으로 돌아가게 되면 그때 당시의 스택의 탑은 파라미터(1)이 들어가있음.<br/> 


# Calling Convention - [Local Variables on the Stack]
함수에서 전역변수의 값을 변경하는 코드를 포함하고 있지 않으면 지역변수만 사용했다는 것임. <br/>  
The stack can be used as a convenient location for local variables 
- The stack is exactly where C program stores normal (automatic ) variables <br/> 
c 에서 automatic 변수들은 스택에 저장됨. <br/> 
- To make subprogram reentrant <br/> 
함수는 자기 자신을 호출할 수 있는데, 무조건 스스로를 다시 호출한다고 해서 늘 올바른 동작을 하는 것은 아님. <br/> 
reentrant하다는 것은 실행했을때 문제가 발생하지 않고 자기 자신을 호출할 수 있는 상태임. <br/>
- (지역변수할당) Local variables are stored right after the saved EBP value in the stack, by subtracting the number of bytes required from ESP <br/> 
    - saved EBP 값이 있는 바로 다음에 지역변수를 저장함. <br/> 
    - 실제로 local variable을 할당하는 동작은 원하는 바이트 수만큼 esp 레지스터 값을 감소시킴으로써 할당 가능. <br/> 
    esp 레지스터가 감소하면 감소된만큼 스택에 빈공간이 생기는 것임. <br/> 
    어떤 함수에 지역변수가 20바이트 필요하다 했을때, 20바이트를 할당하는 실제방법은 esp레지스터 값은 -20하는 것임. <br/> 
- (할당된 후 사용) Indirect addressing with base register EBP is used to access local variables<br/> 
ebp레지스터를 base로 해서 할당된 공간을 indirect addressing함으로써 지역변수를 사용할 수 있게됨. <br/> 

## General Caller and Callee Form with Local Variables 
![ssw_ch_7](https://user-images.githubusercontent.com/63347989/144408191-bcad4b5b-5406-4d54-b1e5-54d075b949b3.PNG)<br/> 
sub esp, LOCAL_BYTES <br/> 
지역변수를 할당하는 과정 <br/> 
스택의 빈공간을 확보하는 과정<br/> 

## example 1
![ssw_ch_8](https://user-images.githubusercontent.com/63347989/144408194-fac0a4e3-9f2b-4e4c-b0f8-ed953c2c42b2.PNG)<br/> 
mov eax, [ebp-12]<br/> 
calling convention - 복귀되는 순간에 eax에 복귀값을 남기자.<br/> 
지역변수 c를 eax에 카피함.<br/> 
mov esp, ebp <br/> 
지역변수 할당을 해제하는 명령<br/> 
add esp, 8 <br/> 
파라미터 제거 , 파라미터가 2개였기 때문에 8 더함. <br/> 

### Stack Frame for Example 1
![ssw_ch_9](https://user-images.githubusercontent.com/63347989/144408198-0279bb54-d9a4-4757-b9f4-a5cdb89fba52.PNG)<br/> 
파라미터나 지역변수에 직접 접근할 때는 ebp 기준으로 접근함. <br/> 
초록색 네모 - stack frame or call frame <br/> 
이 stack frame은 함수가 호출되었기 때문에 생김. <br/> 
이 stack frame은 함수의 호출이 종료돼서 리턴하게 되면 없어짐. <br/> 

## example 2
![ssw_ch_10](https://user-images.githubusercontent.com/63347989/144408202-ec9c23f0-ec50-4ca1-83d9-39088a7dd508.PNG)<br/> 
- reigister int i; <br/> 
c에서 지역변수는 기본적을 스택에 저장이 되는데, <br/> 
지연변수 앞에 register라는 키워드가 붙어있으면 해당 지역변수를 스택이 아니라 특정 레지스터에 할당함. <br/> 
(*sump = sum)<br/> 
=(mov [ebx], eax)<br/> 

### Stack Frame for Example 2
![ssw_ch_11](https://user-images.githubusercontent.com/63347989/144408205-29689b65-93b0-495f-992d-09f02e39aaf5.PNG)

## ENTER and LEAVE instruction
![ssw_ch_12](https://user-images.githubusercontent.com/63347989/144408207-02cd0a3d-508a-4836-b0ef-56d44283e1d3.PNG)<br/> 
- enter 명령: (파라미터 2개 - 지역변수 크기, 무조건 0)
```
push ebp
mov ebp, esp 
sub esp, LOCAL_BYTES 
```
- leave 명령:<br/> 
```
mov esp, ebp<br/> 
pop ebp
```

# Nested(중첩된) Call Frames
어떤 함수들로 이루어진 프로그램이 실행되는 도중의 스택의 모습 <br/>  
함수의 호출은 중첩이 될 수 있음. <br/> 
함수 a가 b를 호출했고, b가 진행이 되고, b가 리턴되기 전에, b가 또 c를 호출할 수 있음. <br/> 
한 함수호출이 리턴하기 전에 또다른 함수를  호출하는 경우<br/> 
중첩될때마다 call frame이 스택에 쌓여감. <br/> 
One call frame created per each subprogram call<br/> 
![ssw_ch_13](https://user-images.githubusercontent.com/63347989/144408210-90d24c47-6541-4cba-8953-b677c60dce39.PNG)<br/> 

# Subprogram Calls in Assembly Program
Caller (Before Call) :
- Push arguments, last to first
- CALL the function
Callee:
- Save caller's EBP and set up callee’s stack frame (or ENTER instruction)
- Allocate space for local variables
- Save registers as needed (or PUSHA instruction)
- Perform the task
- Store return value in EAX
- Restore registers (or POPA instruction)
- Restore caller's stack frame (or LEAVE instruction)
- Return
Caller (After Return) :
- POP arguments, get return value in EA

# Multi-module Programs
큰 프로그램의 경우, 소스코드를 분리하게 됨. <br/> 
- A Multi-module program is one composed of more than two object files<br/> 
multi-module program: 두개이상의 object 파일로 구성되어있는 프로그램<br/> 
- In order for module A to use a label defined in B, the extern directive must be used
- Labels cannot be accessed externally by default. If a label can be accessed from other modules, it must be declared global in its module by global directive
![ssw_ch_14](https://user-images.githubusercontent.com/63347989/144408211-8a5ae06b-5c44-44f9-85e4-8a4f5fe2c796.PNG)<br/> 
소스코드 두개로 이루어진 하나의 프로그램 <br/> 
get_int나 print_sum의 정의는 sub에 있지만 main에서 호출됨. <br/> 
main에서는 get_int랑 print_sum이 다른곳에 정의되어있다고 알려줘야함. (exter get_int, print_sum)<br/> 
sub에서는 global을 통해서 export해줌. <br/> 

# Interfacing Assembly with C
1. Assembly routines are usually used with C for the following reasons<br/> 
프로그램이 여러개의 함수로 구성이 되는데,<br/> 
일부 함수는 어셈블리어로, 일부 함수는 고급언어(c,..)로 작성되는 경우가 있음. <br/> 
c에서 일부함수를 어셈블리어로 만들 수 있음.<br/> 
- Direct access is needed to H/W features that are difficult or impossible to access from C<br/>
특정한 하드웨어의 기능을 사용하기 위해서 레지스터 역할을 해야한다든지<br/> 
- The routine must be as fast as possible and the programmer can hand-optimize the code better than the compiler can<br/> 
어떤 함수는 전체적인 성능에 민감해서 최대한 빨리 순응이 될 수 있도록 만드는게 필요할때 
2. Most of the C calling convention have already been specified<br/> 
서로간의(c-어셈블리어) calling convention이 맞아야함.<br/> 

## Calling Assembly from C Function
c 함수에서의 calling convention<br/> 
- Saving registers <br/> 
    - C assumes that a function __maintains__ the values of the following registers: EBX, ESI, EDI, EBP, CS, DS, SS, ES<br/> 
    위 레지스터와 관련해서는, 함수 시작할 때 push 했다가 함수가 끝나기 직전에 pop하는 동작이 있음. <br/> 
    - The EBX, ESI, EDI values must be preserved because C uses these registers as register variables
- Label of function<br/> 
    - Most C compilers prepend __a single underscore (_)__ character at the beginning of the names of function and global/static variables<br/> 
    함수 이름을 a라고 지었으면, 실제 object 파일에 들어가있는 이름의 형태는 _a임.<br/> 
    전역변수의 이름도 마찬가지임. <br/> 
- Passing parameters<br/> 
    - The arguments of a function are pushed on the stack in the reverse order that they appear in the function call<br/> 
    파라미터는 스택에 저장이 되는데 호출시 보이는 순의 역순으로 저장이 됨. <br/> 
- Returning values <br/> 
    - All integral types (char, int, enum, …) are returned in the EAX register<br/> 
    리턴하는 데이터 타입이 정수형이면 다 eax 레지스터에 담김. <br/> 
    - If smaller than 32-bits, they are extended to 32-bits when stored in EAX<br/> 
    32비트보다 작은 경우엔, 확장돼서 eax에 담김. <br/> 
    - 64-bit values are returned in the EDX:EAX pair<br/> 
    64비트의 경우는 edx 레지스터를 동원해서 eax 레지스터와 함께 담김. <br/> 
- Other calling conventions<br/> 
    - __cdecl__(calling convention 이름) versus __stdcall__(컴파일러에 따라서 다른 이름의 calling convention) calling convention
    - In gcc,<br/>
    ``` void f ( int ) __attribute__ ((cdecl)) ```
    c컴파일러에 따라서 특정한 함수를 정의하고 호출할때 이 함수는 어떤 calling convention을 사용하라고 지정할 수도 있음. <br/> 

# Command Line Arguments
![ssw_ch_15](https://user-images.githubusercontent.com/63347989/144408212-cba2ac8f-0d24-435f-8d62-0f4935abc1e6.PNG)<br/> 
프로그램이 실행을 시작할때 최초의 스택이 어떠한지<br/> 
초록색 박스 - 스택의 최초 call frame <br/> 
old ebp 위에는 메인함수의 복귀주소가 있음. <br/> 
00000004(int argc(argument의 개수)), BFEEC494(char *argv[]) - 메인 함수의 파라미터 <br/> 