---
layout: post
title: Intel Assembly III - Data Transfer Instructions
subtitle: SystemSoftware_08_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Data Transfer Instructions
data transfer instruction은 명령의 오퍼렌드들(레지스터나 메모리 immediate값)간의 데이터를 복사하는데 사용하는 명령
1. General data movement (데이터 복사)
2. Exchange (두개의 데이터를 교환)
3. Stack manipulation (스택에 있는 데이터 접근)
4. Type conversion (데이터 타입 변환)
5. String operations (스트링 전용 명령)

# General Data Movement
## mov Instruction
mov : covered earlier <br/>
mov destination-operand, source-operand<br/>
source-operand 값이 destinatin-operand로 복사됨. <br/>
Type of data movement <br/>
가능한 mov의 경우, 아래의 경우 제외하고는 불가능함. <br/>
![ssw_ch8_1](https://user-images.githubusercontent.com/63347989/144062575-aa90f01a-5dd1-4b53-9b80-3f6bcc025b24.PNG)<br/>

## Conditional Move Instructions (조건부 mov 명령)
cmov
- moves data only if a condition is true.<br/>
조건을 만족해야 데이터를 복사함.<br/>
- Conditions are set by a previous instruction and include Carry, Zero, Sign, Overflow and Parity (status flag):<br/>
![ssw_ch8_2](https://user-images.githubusercontent.com/63347989/144062680-e7c6ace5-1e0c-472c-87b6-905e5c83d4d8.PNG)<br/>
- There are many variations of this instruction<br/>
많은 종류가 있음
- Variations of Conditional Move (cmov의 다양한 종류)
![ssw_ch8_3](https://user-images.githubusercontent.com/63347989/144062791-4cf43975-1f2e-4ad6-b8a8-a688a93d73b0.PNG)

# Exchange Instructions
xchg<br/>
- Exchanges the contents of a register with the contents of any other register or memory location.<br/>
레지스터 내용을 다른 레지스터나 메모리 내용과 교환 <br/>
- It can NOT exchange segment registers or memory-to-memory data.<br/>
세그먼트 레지스터 교환 불가능, 메모리간의 교환도 불가능. <br/>
- Byte, word and doublewords can be exchanged using any addressing mode (except immediate, of course).<br/>
크기가 맞아야함. <br/>
![ssw_ch8_4](https://user-images.githubusercontent.com/63347989/144063079-1c9366d0-882c-4941-acb2-95d70e7a2118.PNG)<br/>

bswap<br/>
- Swaps the first byte with the forth, and the second byte with the third.<br/>
첫번째 바이트와 네번째 바이트를 바꾸고, 두번째 바이트와 세번째 바이트를 바꿈. <br/>
- Used to convert between little endian and big endian(16진수):<br/>
![ssw_ch8_5](https://user-images.githubusercontent.com/63347989/144063076-57c7be10-1b45-4101-844f-051eaa341b59.PNG)<br/>
즉 엔디언을 바꾸는 연산

# CF) !! Endianness !!
Endianness refers to the order of the bytes, comprising a word, in computer memory. It also describes the order of byte transmission over a digital link. <br/>
endianness: 여러개의 바이트로 구성된 데이터를 메모리에 저장할 때, 어떤순서대로 저장해야하나? <br/>
멀티바이트 데이터를 저장하거나 전송할 때 바이트들의 저장 또는 전송 순서 를 일컫는 말. <br/>
- Big-endian <br/>
: the most significant(높은자리숫자) byte of a word is stored at a particular memory address <br/>
and the subsequent bytes are stored in the following higher memory addresses, <br/>
the least significant(낮은자리숫자) byte thus being stored at the highest memory address.<br/>
![ssw_ch8_15](https://user-images.githubusercontent.com/63347989/144064965-cf715302-7ab7-4383-a002-5bd481ac3de4.PNG)<br/>
- Little-endian <br/>
: reverses the order and stores the least significant byte at the lower memory address with the most 
significant byte being stored at the highest memory address.<br/>
![ssw_ch8_16](https://user-images.githubusercontent.com/63347989/144064959-72076e0c-f27c-4f5d-a839-0f71f9aae02b.PNG)<br/>
<br/>
- Big-endian is the most common format in data networking; fields in the protocols of the Internet protocol suite, such as IPv4, IPv6, TCP, and UDP, are transmitted in big-endian order. For this reason, big-endian byte order is also referred to as __network byte order__ .<br/>
대부분의 네트워킹 프로토콜들이 big-endian 방식을 사용하고 있음.<br/>
만약 두 컴퓨터가 통신한다고 했을때, H1은 little-endian을 사용하고 있고, H2는 big-endian을 사용한다면, 전송하는 순간에 변환이 필요하고 수신하는 순간에는 변환이 필요하지 않음. <br/>
- Little-endian storage is popular for microprocessors, in part due to significant influence on microprocessor designs by Intel (the Intel x86 processors use little-endian) <br/>
- Mixed forms also exist, for instance the ordering of bytes in a 16-bit word may differ from the ordering of 16-bit words within a 32-bit word. Such cases are sometimes referred to as __mixed-endian__ or __middle-endian__. There are also some __bi-endian__ processors that operate in either little-endian or big-endian mode.<br/>
mixed-endian or middle-endian: double word(word 두개)가 있을 때, word가 배치된 endian과 각 word안에 byte가 배치된 endian이 서로 다름. <br/>
bi-endian: 프로세서의 설정에 의해서 little-endian으로 동작할 수도 있고 big-endian으로 동작할 수도 있음. <br/>

# Stack Manipulation - Push and Pop
The push, pop, pusha, and popa move data to and from the stack<br/>
- push: 스택의 탑에 넣는 과정<br/>
The source of the data may be any 16- or 32-bit register, immediate data, any segment register, any word or doubleword of memory data<br/>
    - Operation of push<br/>
        - push<br/>
    ![ssw_ch8_7](https://user-images.githubusercontent.com/63347989/144063485-da19dd63-f162-4878-8cbb-8a5204d591d7.PNG)<br/>
    스택 한칸이 더블 워드, 한칸 차이는 4바이트가 차이남. <br/>
    주소의 번지가 낮아지는 쪽으로 스택이 커진다. <br/>
    esp가 먼저 push 명령에 의해서 pointer값(다음번스택의 탑이 될 위치로)이 바뀜 <br/>
    바뀐 esp 지점으로 값이 복사되어 들어감. <br/>
    esp는 가장 최근에 스택에 푸시된 데이터를 가르키고 있음.<br/>
        - pusha<br/>
        오퍼렌드가 없음. <br/>
    ![ssw_ch8_8](https://user-images.githubusercontent.com/63347989/144063486-7b79cbda-e5ab-464a-9247-181d5c0cc8ff.PNG)<br/>
    범용레지스터 8개의 모든 값을 스택에 일정한 순서를 가지고 푸시됨. <br/>
    pusha 명령은 CPU 상태(그 순간의 일련의 레지스터 값)를 저장할 때 사용됨. <br/>
- pop: 스택의 탑에 있는 값을 꺼내오는 과정<br/>
The source of the data may be any 16- or 32-bit register, any segment register (except for cs), any word or doubleword of memory data.
    - Operation of pop<br/>
        - pop<br/>
    ![ssw_ch8_9](https://user-images.githubusercontent.com/63347989/144063488-ae818794-1a93-4bdc-8d3c-085786cbe82f.PNG)<br/>
    pop되고 나면, n-8은 더이상 스택의 일부가 아님. <br/>
    값이 사라지는 건 아니고 스택의 탑 위치만 조정이 되는 것임. <br/>
        - popa<br/>
        오퍼렌드없음.<br/>
    ![ssw_ch8_10](https://user-images.githubusercontent.com/63347989/144063477-b4d7061d-71d8-43e2-bfba-15a9d700ebd8.PNG)<br/>
    n-36번지부터 위쪽으로 32바이트가 가장 최근에 pusha에 의해서 저장된 값들이라면, popa을 실행하면 각각의 레지스터에 값들이 copy가 되는데, 다만 중간에 esp값은 복사되지않고 무시됨(왜냐면 esp값이 바뀌면 스택의 탑이 바뀌는 것임). <br/>

### Address Loading Instructions
- lea (load effective address)<br/>
Loads any 32-bit register with the address of the data, as determined by the instruction addressing mode.<br/>
오퍼렌드에 있는 32비트 레지스터에 두번째 오퍼렌드에 표시돼있는 데이터의 주소를 로드하는 명령. <br/>
![ssw_ch8_11](https://user-images.githubusercontent.com/63347989/144063832-2ed32503-8f02-46ce-8175-8dbc85c65690.PNG)<br/>
[ebx + ecx*4 + 100]은 메모리의 특정한 주소를 나타내는 수식이고 이 값이 100(두번쨰 오퍼랜드가 명시하고 있는 유효주소값)이라며, <br/>
eax에 100이라는 주소값이 들어가게하는 명령<br/>
 

- lds, les, lfs, lgs and lss: <br/>
Load a 32-bit offset address and then ds, es, fs, gs, or ss from a 48-bit memory location.<br/>
![ssw_ch8_12](https://user-images.githubusercontent.com/63347989/144063823-5ec4f2e6-ad29-4c1d-a5f1-1d6247342190.PNG)<br/>
lds: LIST가 나타내고자하는 유효주소를 edi레지스터에 넣고, LIST에 대한 세그먼트 주소는 해당 데이터 세그먼트에 로드함.  <br/>
edi와 ds가 LIST라는 두번째 오퍼랜드의 유효주소 값으로 로드됨. <br/>
lfs: DATA1에 대한 유효주소가 세그먼트 주소 fs와 offset을 나타내는 esi(첫번째 오퍼랜드)로 로드됨. <br/> 
- lea versus mov<br/>??????????????
![ssw_ch8_13](https://user-images.githubusercontent.com/63347989/144063829-92c14418-2185-461c-8b30-02e004a35002.PNG)<br/>
[edi]의 유효주소를 ebx에 복사하는 것.<br/>
edi가 가리키고 있는 value(100번지의 값)를 ebx에 로드하는 것. <br/>
첫번째 명령과 세번째 명령은 같은 것임. <br/>
NOTE: lea calculates the ADDRESS given by the right arg and stores it 
in the left arg!

# Type Conversion Instructions
데이터 타입을 변환하는 명령들<br/>
- Simple conversion 오퍼랜드 없음<br/>
    - cbw (convert byte(8비트) to word(16비트), 확장) <br/>
    AX ← sign-extend of AL<br/>
    AL레지스터에 확장하고자 하는 8비트 데이터가 들어있다고 가정하면, 16비트로 확장하여 AX 레지스터 담음. <br/>
    sign-extension 사용 - 추가로 생성되는 위쪽 16비트는 기존에 있는 사인비트(부호비트)의 값들로 채워짐. <br/>
    ![ssw_ch8_21](https://user-images.githubusercontent.com/63347989/144185834-19d8aeeb-84a4-4d89-b8b1-6204dedfd575.PNG)<br/>
    - cwde (convert word(16비트) to doubleword(32비트) extended)<br/>
     EAX ← sign-extend of AX<br/>
    sign-extension 사용 <br/>
    - cwd (convert doubleword, 16비트를 32비트로 확장하는 명령)<br/>
     DX:AX ← sign-extend of AX<br/>
     32비트가 DX와 AX에 나뉘어서 저장됨. <br/>
     used before 16-bit division<br/>
     DX와 AX는 16비트의 나뉘는수에 해당하는 형식. <br/>
     cwd는 16비트 나눗셈을 실행하기 직전에 실행하는 명령. <br/>
    - cdq (convert doubleword(32비트) to quadword(64비트))<br/>
     EDX:EAX ← sign-extend of EAX<br/>
     EDX와 EAX는 32비트의 나뉘는수에 해당하는 형식.<br/>
     used before 32-bit division<br/>
    cdq는 32비트 나눗셈을 실행하기 직전에 실행하는 명령.<br/>

- Move with sign or zero extension<br/>
sign extension 또는 zero extension 해서 move가 이뤄지는 명령. <br/>
__movsx__ and __movzx__ (80386 and up only)<br/>
Move-and-sign-extend and Move-and-zero-extend:<br/>
![ssw_ch8_14](https://user-images.githubusercontent.com/63347989/144064022-fb664b60-45fe-4b18-be98-8115ca13d00c.PNG)<br/>
destination operand 크기가 source operand 크기의 2배. <br/>
__movsx cx(16비트), bl(8비트)__<br/>
bl을 sign extension해서 cx에 복사(mov)함.
__movzx eax, DATA2__<br/>
DATA2를 zero extension 해서 eax에 복사함. <br/>
zero extension - 확장해서 늘어나는 부분을 다 0으로 채우는 확장. <br/>


# String Operations
movs, lods, stos, ins, outs<br/>
- ins, outs은 입출력과 관련된 명령이라서 공부하지 않음. 
- Allow data transfers of a byte, a word or a double word, or if repeated, a block of each of these.<br/>
기본단위가 byte, word, double word인 일련의 블록 데이터(byte끼리 모인, word끼리 모인, double word끼리 모인)를 data transfer할 수 있는 연산. <br/>
- The D flag-bit (direction), esi and edi are implicitly used.<br/>
esi와 edi가 다음번 명령을 가리키도록 갱신되는데, 이때 d flag가 역할을 함. <br/>
    - D = 0: Auto increment edi and esi
    - Use __cld__ instruction to clear this flag
    - D = 1: Auto decrement edi and esi
    - Use __std__ instruction to set it.
- edi: accesses data in the __extra segment__. Can NOT override.<br/>
extra segment 무조건..!
- esi: accesses data in the __data segment__. Can be overridden with segment override prefix.<br/>
data segment가 default이긴한데, 다른 segment가 짝이 되도록 override 할 수는 있음. <br/>

--------------------------
![ssw_ch8_22](https://user-images.githubusercontent.com/63347989/144275826-315b6e02-2151-4c19-a2da-2a2c4a544ad2.PNG)<br/>
- extra segment - 데이터를 추가로 저장하는 공간<br/>
스트링은 data segment와 extra segment에 저장되어있는 데이터<br/>
- 스트링은 같은 크기의 데이터가 연속적으로 메모리에 나열(저장)되어 있는것. <br/>
- movs는 메모리 투 메모리임.  <br/>
한번의 movs가 일어나면, data segment에서 esi가 가리키고 있는 데이터를 읽어서, edi가 가리키고 있는 곳으로 카피됨. <br/> 
esi와 edi는 다음번 스트링을 가리키도록 갱신됨. <br/>
- lods은 data segment에 저장되어있는 스트링 데이터를 카피해서 eax 레지스터에 넣는것. (load string) <br/> 
- stos은 eax레지스터에 있는 데이터를 extra segment에 저장함. (store string)<br/>
- lods나 stos 둘다 a 레지스터가 쓰임. <br/>
    - 더블워드일 경우) eax,<br/>
    - 워드일 경우) ax,<br/>
    - 바이트일 경우) al 레지스터가 사용될 것임. <br/>

esi가 가리키고 있는 곳에서 lods 함. <br/>
esi가 가리키고 있는 곳이 다음번 load할 주소. <br/>
edi가 가리키고 있는 곳으로 stos됨.<br/>
esi나 edi가 load나 store된후, 자동적으로 string의 다음 데이터를 가리키도록 갱신됨. <br/>

![ssw_ch8_23](https://user-images.githubusercontent.com/63347989/144275819-ee31ff91-67c4-49fc-aad4-ebd41dcad22f.PNG)<br/>
메모리 한쪽에는 data segment 다른 한쪽에는 extra segment<br/>
data segment안에 스트링 쭈루룩 저장되어있고, extra segment에도 쭈루룩 저장되어있다. <br/>
esi는 data segment내에서 다음번 string 연산을 일어날 곳을 가리키고 있고, <br/>
edi는 extra segment내에서 다음번 string 연산을 일어날 곳을 가리키고 있음. <br/>
eax는 lods, stos 동작할때, 실제 데이터가 load되고 store되는데 관여하는 레지스터 <br/>
ecx는 rep 붙어있을 경우에, ecx에 있는 값만큼 반복함. <br/>

------------------------------

## lods
- lodsb(byte단위), lodsw(word단위), and lodsd(double word 단위)
- Loads al, ax or eax with data stored at the data segment (or extra segment) + offset given by esi
- esi is incremented or decremented afterwards<br/>
![ssw_ch8_17](https://user-images.githubusercontent.com/63347989/144066958-e41b28e9-e846-4e02-b3e6-4119c51c9ac5.PNG)<br/>
- 오퍼랜드가 없음. <br/>
- 바이트 단위의 스트링을 접근하는데, data segment에 esi가 가리키고있는 한 바이트를 al 레지스터에 카피를 하고, 카피가 끝났으면 자동적으로 esi 1 증가 또는 감소함. <br/>
- 더블 워드 단위의 스트링을 접근하는데, data segment에 esi가 가리키고있는 한 더블워드를 eax 레지스터에 카피를 하고, 카피가 끝났으면 자동적으로 esi 4 증가 또는 감소함.
- 디폴트는 data segment인데, extra segment로 override할 수도 있음.<br/>

## stos
- stosb(byte단위), stosw(word단위), and stosd(double word 단위)<br/>
- Stores al, ax or eax to the extra segment (es) + offset given by edi (es cannot be overridden)
- edi is incremented(DF=0) or decremented(DF=1) afterwards:<br/>
![ssw_ch8_18](https://user-images.githubusercontent.com/63347989/144066961-7e0083df-ebd7-4899-b001-5b025fb3adae.PNG)<br/>


## movs
- 메모리 투 메모리 데이터 카피
- movsb, movsw, and movsd
- Moves a byte, word or doubleword <br/>
from data segment and offset esi <br/>
to extra segment and offset edi
- Increments/decrements both edi and esi:<br/>
![ssw_ch8_19](https://user-images.githubusercontent.com/63347989/144066963-a531dae2-6ac5-4443-9c69-adc2e15c7b80.PNG)<br/>

## rep Prefix
- Executes the instruction ecx times.
- NOTE: rep does not make sense with the lods instruction.<br/>
![ssw_ch8_20](https://user-images.githubusercontent.com/63347989/144066955-5860a689-0555-4882-ad4a-b3fc559159f1.PNG)<br/>
반복문을 쓰지 않더라고, rep stosw를 쓰면 일정한 횟수(ecx에 있음)만큼 반복되게 실행됨.<br/>
lods 앞에 rep를 붙여봤자 의미없음.<br/>
계속 load해봤자 최종 결과는 마지막에 저장되는 값만 eax에 남게됨. <br/>
터미널 한줄에 보통 80글자 가능. <br/>
한 화면 25줄이 있음 .<br/>
25*80은 터미널 한 화면에 채울 수 있는 글자수<br/>
07(검은바탕에흰글자,normal)20(공백문자의 아스키코드)H<br/>
위코드를 실행하면, 터미널 상의 한 화면을 클리어하는 코드임. <br/>

------

오퍼랜드가 두개인 경우, 둘 중 하나는 무조건 레지스터여야함. <br/>
mov의 경우 오퍼랜드 두개 모두 메모리인 경우가 불가능함.<br/>
이때 메모리 투 메모리를 쓰려면 movs를 써야함.<br/>
