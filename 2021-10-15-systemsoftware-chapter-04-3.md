---
layout: post
title: Introduction to Assembly Programming 3
subtitle: SystemSoftware_04_3
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Pseudo-Instructions and Directives
어셈블러 프로그램을 이해하기 위해선 CPU가 직접 실행할 수 있는 machine instruction와 assembler instruction 도 알아야 하지만 <br/>
진짜 instruction은 아니지만 프로그램 구성에 여러 역할을 할 수 있는 pseudo-instruction 과<br/>
어셈블러 프로그램을 번역하는 과정에서 어셈블러에게 번역과정에 대한 여러가지 지시를 할 수 있는 directive 도 알아야 함.<br/>
<br/>
To instruct the assembler to do something or inform the assembler of something<br/>
 
- Pseudo-Instruction<br/>
가짜 instruction이긴 하지만 명령의 형태는 mnemonic도 있고 operand도 있고 머신명령과 유사함<br/>
    - Define constants 상수 정의 
    - Define memory to store data into 데이터 저장할 메모리 정의 
- Directives <br/>
디렉티브는 어셈블러가 프로그램을 번역하는 과정에서 번역과 관련된 옵션을 지정할 수 있음<br/>
어셈블러에 대한 지시자<br/>
    - Group memory into segments 세그먼트 정의 
    - Conditionally include source code 내소스에 다른 소스 코드를 인클루드
    - Include other files
    - import symbol export symbol

# Pseudo-Instructions
프로그램에서 사용할 데이터를 메모리 공간에 정의하는 의사 명령 

 - DB<br/>
 DB(= Define Byte) and friends: declaring initialized data 초기화된 데이터 정의 <br/>
operand에 명시된 상수가 할당된 메모리 영역의 초기값 <br/>
DB가 사용된 그 지점이 전체 프로그램 내에서 내가 정의하고자 하는 한 바이트의 그 지점에 해당 <br/>
한 바이트를 지정을 했으면 프로그램의 다른 부분에서 이 위치를 참조할 수 있어야 하기 때문에 DB 명령에 label이 붙어서 사용됨.<br/>
    - L1 db 0x55 ; byte labeled L1 with initial value 0x55<br/>
        - 한 바이트를 정의하고 최초로 주어지는 값을 16진수 55로 할당. <br/>
        - L1은 지금 정의된 한 바이트를 프로그램의 다른 부분에서 호칭하는 역할.
    - L2 db 0x55, 0x56, 0x57 ; three bytes in succession <br/>
        - 연속된 여러 바이트를 정의할 수 있음. <br/>
        - 세 개의 바이트를 정의하고 각각의 초기값으로 16진수 55, 56, 57이 주어짐. <br/>
        - label은 DB가 정의된 곳의 주소에 해당하는 심볼인데, 이 경우 첫번째 바이트를 가르킴. <br/>
        - 두번째 바이트는 L2 + 1, 세번째 바이트는 L2 + 2로 표현하면 됨. <br/>
    - L3 db ‘a’, 0x55 ; character constants are OK<br/>
        - 두 바이트 정의 <br/>
        - 첫번째 초기값은 문자 상수. 이 경우에는 'a'의 ascii 코드값이 초기값으로 설정됨. <br/>
    - L4 db ‘hello’, 13, 10, ‘$’ ; so are string constants
        - 8 바이트 정의
        - 'hello'는 문자열 5 바이트
        - 그리고 십진수 13 한바이트, 10 한바이트, &#36;에 해당하는 ascii 코드값 한바이트
    
 - DW   <br/>
 DW (= Define Word)<br/>
 한 워드를 할당하는 pseudo-instruction<br/>
 크기: 16비트 (연속된 2 바이트)
    - L5 dw 0x1234 ; 0x34 0x12<br/>
    ![ssw_ch4_12](https://user-images.githubusercontent.com/63347989/137494950-f12719c6-9f42-4269-a906-260524c342f8.PNG)<br/>
    0x34 가 낮은 번지(L5)에, 0x12가 높은 번지(L5+1)에 저장됨. → little-endian <br/>
    - L6 dw ‘a’ ; 0x61 0x00 (it’s just a number)<br/>
    초기값은 문자하나만 주어짐 
    - L7 dw ‘ab’ ; 0x61 0x62 (character constant)
    - L8 dw ‘abc’ ; 0x61 0x62 0x63 0x00 (string)<br/>
    무조건 2바이트로 돼야해서 2바이트가 채워지지 않으면 00이 들어감. 

- DD<br/>
DD (= Define Double Word)<br/>
Word가 두개 있는 것. <br/>
크기: 32비트 (연속된 4 바이트)
    - L9 dd 0x12345678 ; 0x78 0x56 0x34 0x12<br/>
    78이 L9 에, 56이 L9+1에, 34가 L9+2에, 12가 L9+3에 저장됨. → little-endian <br/>
    little-endian은 CPU가 그렇게 저장하라고 정함. <br/>
    - L10 dd 1.234567e20 ; floating point constant 부동 소수점 저장

- DQ <br/>
DQ (= Define Quadable Word)<br/>
크기: 64비트 (연속된 8 바이트)
    - L11 dq 1.234567e20 ; double-precision float 부동 소수점 저장 (DD보다 precison이 높음)

- RESB and friends: declaring uninitialized data<br/>
RESB = Reserve Byte<br/>
DB랑 비슷한데 차이점은 DB는 초기값을 지정하지만 RESB는 초기화되지 않음. 
    - buffer resb 64 ; reserve 64 bytes
    - wordvar resw 1 ; reserve a word(16비트단위)
    - doublevar resd 2 ; reserve two doubles(32비트단위) → 64비트 할당
    - realarray resq 10 ; array of 10 reals (64비트단위) → 640비트 할당 → 80바이트 할당

- TIMES: __prefix__ to __repeat__ instructions or data<br/>
독립적으로 사용불가. DB나 RESB와 함께 사용됨. <br/>
    - zerobuf times 64 db 0 ; reserve 64 bytes <br/>
    0으로 초기화된 한 바이트를 64번 반복 → 0으로 초기화된 64바이트 할당
    - buf times 100 resb 1 ; same as resb 100 → 초기화되지않은 100바이트 할당 
    - buffer db ‘hello, world’ ; to make the total length of 12바이트 문자열 정의<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;times 64-($-buffer) db ‘ ‘ ; buffer up to 64 일련의 바이트를 공백으로 채우겠다. <br/>
    &#36; : Location couter, 변수값, &#36;라는 변수가 사용된 지점의 주소값
    &#36;-buffer : 12<br/>
    64-12 = 52 바이트 <br/>
    정리하자면, buffer라는 것을 정의해서 'hello, world'로 초기화하고 나머지 뒷부분(52바이트)을 공백으로 채운다. 

- EQU: defining constants<br/>
심볼을 정의하는 psuedo instruction
    - ten equ 10<br/>
    ten이라는 심볼의 값은 10이다. <br/>

# Directives
최종 실행파일에 어떠한 영향을 미치지 않음. <br/>
어셈블러가 프로그램을 번역을 할 때 어떠한 방식으로 번역을 하라고 지정을 할 수 있는 지시자<br/>

- SECTION or SEGMENT<br/>
section의 기능과 segment의 기능은 똑같음.<br/>
    - Defines and changes segments<br/>
    하나의 세그먼트 정의 <br/>
    세그먼트: 프로그램을 구성하는 메모리의 사용단위
    - 사용방법<br/>
    프로그램에서 section이라는 directive가 사용된 지점부터 새로운 세그먼트가 시작이 되는 것.<br/> 
    하나의 세그먼트는 section이 사용된 지점부터 다음번 section이 사용된 직전이나 프로그램 끝까지임.<br/>
    - Segment names: .text .data .bss - operand 
    operand: 1개 <br/>
    세그먼트 타입을 지정할 수 있는 피연산자. <br/>
    ex1. section.text → code segment 정의 (명령들), 무조건 있어야 함<br/>
    ex2. section.data → 프로그램의 data들이 있음(초기화된 전역변수들), optional <br/>
    ex3. section.bss → 초기화되지 않은 전역변수들이 있음, optional

- GLOBAL
    - Exports symbols to other modules<br/>
    심볼을 다른 모듈에 export함. <br/>
    내가 가진 심볼을 타 모듈에게 공표하는 것 <br/>
    그래서 타모듈이 내 심볼을 사용할 수 있게끔 하는 것.<br/>
    심볼: label<br/>
        ```
         global _main //내 프로그램에서 정의돼있는 _main을 타 모듈에서 사용할 수 있게끔
        _main: 
                ; som code //_main은 내 코드에서 정의한 심볼(label)
        ```
- EXTERN
    - Imports symbols from other modules<br/>
    내 프로그램 이외에서 정의된 심볼을 내 프로그램에서 사용하겠다는 선언
    ```
        extern _printf, _scanf
    ```

# 예제 Hello, World

![ssw_ch4_13](https://user-images.githubusercontent.com/63347989/137531656-bf7cf908-96ed-4608-9470-5cf09ab7dcae.PNG)

<br/>

- msg db 'Hello, world!', 0x0A<br/>
0x0A는 ascii코드로 10진수 10이고,<br/>
값이 10인 경우에 line feed라고 있고 개행 문자임. <br/>
- len equ &#36; - msg <br/>
&#36; : 변수, &#36; 사용된 라인이 속한 세그먼트 내에서 offset(차지하는 상대적인 위치)이 얼마인가. <br/>
location counter <br/>
&#36; = 14 바이트 msg(label, segment 내에서의 위치) = 0 바이트 <br/>
label값은 해당 세그먼트의 시작점으로부터 label이 정의된 지점까지의 상대적인 위치<br/>
msg값이 0인 이유는 data segment 부분에 바로 정의되었기 때문임. <br/>
len = 14 msg → 문자열 길이
- global _start <br/>
export<br/>
ld한테 알려주기 위해서 global (export)함. 
- _start<br/>
소스코드를 어셈블한 다음에 링크과정을 거쳐야하는데 <br/>
링크할때 ld라는 링커사용 <br/>
ld는 자기가 링크하는 프로그램에서 반드시 _start라는 심볼(label)이 정의되어있기를 기대함. <br/>
_start가 왜 정의되어있어야 하냐? ld가 기대하는 프로그램의 entry 포인트임. (=프로그램의 진입점) <br/>
- 화면에 출력하는 기능은 입출력 기능이기 때문에 응용 프로그램이 자체적으로 수행할 수 없음. <br/>
운영체제한테 화면에 출력해달라고 요청해야함. <br/>
5줄의 출력 명령들은 운영체제한테 hello world 문자열을 알려주면서 출력해달라고 요청하는 코드임. <br/>
3줄의 프로그램 종료 명령들은 프로그램 종료를 운영체제한테 요청하는 코드임. <br/>
즉 두 코드 다 운영체제한테 요청하는 코드임.<br/>
- mov eax, 4<br/>
운영체제한테 기능을 요청할 때는 운영체제가 정의한 기능에 대한 번호를 eax 레지스터에 넣기로 돼있음.<br/>
eax에 4를 넣는다는 것은 출력기능을 요청하기 위한 준비단계.
- mov ebx, 1<br/>
출력의 대상이 화면임을 지정. 
- mov ecx, msg<br/>
msg = 0<br/>
ecx에다가 msg가 정의된 주소인 0을 넣어라. <br/>
- int 0x80<br/>
call kernel <br/>
커널은 운영체제임. <br/>
운영체제한테 요청할 것이 있으면 사용하는 명령. <br/>
즉 여기서는 출력요청을 하는 것. <br/>
- mob eax, 1<br/>
운영체제한테 기능을 요청<br/>
eax에 1를 넣는다는 것은 정상종료를 요청하기 위한 준비단계.<br/>
- xor ebx, ebx<br/>
xor는 exclusive or, 같은 값끼리 xor하면 결과값은 늘 0이 됨.<br/>
왜 0으로 만드냐? 운영체제한테 종료기능을 요청하면서 종료하는 상태값을 ebx에 넣도록 돼있음.<br/>
대부분의 프로그램 경우, 정상종료 를 하게 되면 그 상태를 0이라는 숫자로 표현. <br/>


## ! 어셈블리 프로그램을 어셈블(번역)하고, 링크하는 과정 !

앞 페이지에서 본 어셈블리 소스코드는 hello.asm에 저장이 되었다.<br/>

- To produce hello.o object file:<br/>
nasm이라는 어셈블러로 번역을 하면 hello.o라는 object code가 만들어진다. <br/>
    ```
     nasm -f elf hello.asm
     // -f elf 은  object file을 생성할 때 생성할 object file의 형식 지정
     // 사용할 형식은 elf
     ```
- To produce hello.lst list file too:<br/> 
추가적인 리스트 파일도 만든다. 
    ```
     nasm -f elf hello.asm –l hello.lst
     ```
- To produce hello ELF executable:<br/>
object code를 ld라는 링커를 가지고 최종실행가능한 hello라는 실행파일을 만들어낸다. <br/>
형식은 ELF 타입<br/>
    ```
     ld -s -o hello hello.o
     // -o는 실행파일의 이름을 지정
     // -s는 ld가 실행파일을 생성할 때는 실행에 필요한 명령과 데이터 뿐만 아니라 실행과는 무관한 몇가지 정보인 심볼 테이블이 있는데, -s는 심볼 테이블을 붙이지 않음
     ```

## 리스트 파일
![ssw_ch4_14](https://user-images.githubusercontent.com/63347989/137535677-c3e9b749-3a7e-4e01-a4f4-ea685daba2cc.PNG)
<br/>

object 파일 안에는 machine instruction만 담겨있기 때문에 시각적으로 이해불가능<br/>
lst 파일은 내 프로그램이 어셈블된 결과를 문자 파일 형태로 출력함으로써 어떤 방식으로 어셈블되었는지 이해할 수 있음.<br/>

- 오른쪽에는 .asm파일과 똑같이 담겨있음
- 48 H 65 e 6C l .... - coninue 문자
- len equ &#36; - msg 에 해당하는 offset이 없음<br/>
equ는 상수를 정의만 하지 object code에 어떤 내용을 추가하지는 않음.
