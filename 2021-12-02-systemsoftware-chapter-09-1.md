---
layout: post
title: Data Addressing Modes 
subtitle: SystemSoftware_09_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Data Addressing Modes 
명령을 사용할 때, 명령의 대상이 되는 오퍼랜드가 레지스터일 수도 있지만 메모리인 경우도 있음. <br/>
메모리 오퍼랜드에서 주소를 표현하는 방식.<br/>

Let's cover the data addressing modes using the __mov__ instruction. 
- Data movement instructions move data (bytes, words and doublewords) between registers and between register / memory. <br/>
mov 명령은 레지스터와 레지스터간에 혹은 레지스터와 메모리간에 가능<br/>
- Only the __movs__ (strings) instruction can have both operands in memory. <br/>
메모리와 메모리간의 명령은 movs가 유일함. <br/>
- Most data transfer instructions do not change the EFLAGS register.<br/>
데이터 트랜스퍼 명령은 eflags에 영향을 미치지 못함. <br/>

<br/>

Storage protocols 저장 규칙
- When an n-byte transfer is indicated by an address a, the memory bytes referred to are those at the address a, a+1, …, a+n-1 <br/>
mov를 통해서 n byte 크기의 데이터를 복사하고 그 데이터를 a번지에 저장한다고 했을때, n byte가 차지하는 최종적인 주소는 a번지, a+1번지, ..., a+n-1번지임. <br/>
- When an n-byte number is stored in memory, its bytes are stored in order of significance → little endian<br/>
byte의 저장순서는 significance(낮은자리 -> 높은자리 중요도) 순서(little-endian)로 함. <br/>


# Data Addressing Modes 종류
- Register <br/>
오퍼랜드가 레지스터에 있는 경우. <br/>
mov eax, ebx <br/>
![ssw_ch_1](https://user-images.githubusercontent.com/63347989/144284267-a9b1c30b-46e8-4029-9229-8d6a530acb90.PNG)<br/>
- Immediate <br/>
mov ch, 0x4b <br/>
![ssw_ch_2](https://user-images.githubusercontent.com/63347989/144284270-2658c6e2-873a-443d-8754-97b2b3b03d6c.PNG)<br/>

- Direct (eax), Displacement (other regs) <br/>
mov [0x1234], eax ;direct mode<br/>
[] - 메모리 모드, indirction operator(연산자 역할) <br/>
direct는 []안에 상수를 적어넣음. <br/>
direct와 displacement 구분은 register operand가 eax인 경우에는 direct, eax가 아닌 나머지 레지스터인 경우에는 displacement로 구분됨. <br/>
![ssw_ch_3](https://user-images.githubusercontent.com/63347989/144284273-e6462d22-e506-4397-92c9-38fc1bf80d8d.PNG)<br/>

- Register Indirect <br/>
mov [ebx], cl <br/>
[]안에 레지스터를 사용 <br/>
ebx에 현재 들어가있는 값을 주소로 간주함<br/>
Any of eax, ebx, ecx, edx, ebp, edi or esi may be used. <br/>
![ssw_ch_4](https://user-images.githubusercontent.com/63347989/144284276-5491c34c-b0c0-4907-b7f8-cbb991c7dbee.PNG)

- Base-plus-index <br/>
mov [ebx (base register) + esi (index register)], ebp<br/> 
메모리 오퍼랜드에서 두개의 레지스터를 사용하는데 하나는 base register, 다른 하나는 index register 역할을 함. <br/>
현재 ebx의 값과 현재 esi의 값을 더한 값이 최종 메모리 오퍼랜드가 의미하는 주소 = 유효주소 <br/>
Any combination of eax, ebx, ecx, edx, ebp, edi or esi. <br/>
![ssw_ch_5](https://user-images.githubusercontent.com/63347989/144284280-e64420e9-efc3-4cb2-91fb-d36b675e011e.PNG)

- Register relative <br/>
mov cl, [ebx+4] <br/>
레지스터와 주소상수가 덧셈 형식으로 표현되어있음. <br/>
실제 이 표현이 지칭하고있는 최종주소는 ebx 현재값 + 4 임. <br/>
그 값에 해당하는 메모리 주소에서 한바이트 읽어서 cl 레지스터에 복사함. <br/>
A second variation includes: mov eax, [ebx+ARR] <br/>
arr처럼 심볼이 사용될 수도 있음.<br/>
label자체가 주소임.<br/>

![ssw_ch_6](https://user-images.githubusercontent.com/63347989/144284242-ae09e201-b799-4bf8-9edd-6e2bcec64ae4.PNG)


# X86 Indirect Addressing Modes
위에서 본 다양한 형태를 아우를수있는 기본적인 형태 <br/>
![ssw_ch_7](https://user-images.githubusercontent.com/63347989/144284246-662e1f83-1d90-47c3-9a9a-0a6c00bd9394.PNG)<br/>
indirect addressing mode - operand에서 메모리를 지정할 때 [](indirection operand)사용해서 그안에 다양한 형태의 메모리주소를 나타낼 수 있음.<br/>
1. base - register가 올 수 있음
2. index(register가 올 수 있음) * scale
3. displacement - 주소 상수

## Displacement Addressing
- Displacement instructions are encoded with up to 7 bytes (32 bit register and a 32 bit displacement). <br/>
- To access a statically allocated scalar operand <br/>
정적으로 할당되어있는 단일 데이터에 접근할때<br/>
![ssw_ch_8](https://user-images.githubusercontent.com/63347989/144284250-6b8e6658-0302-402f-985d-d3a4a5680bbf.PNG)<br/>
[]안에 displacement(주소상수)만 사용됨. <br/>
상대방 레지스터가 a레지스터가 아닌 경우 - displacement addressing


## Direct addressing 
- Transfers between memory and al, ax and eax. <br/>
- Usually encoded in 3 bytes, sometime 4<br/>
![ssw_ch_9](https://user-images.githubusercontent.com/63347989/144284253-a35ca4a7-2d38-4e6c-8d6a-0defee1c14cf.PNG)<br/>
상대방 레지스터가 a레지스터인 경우 - direct addressing

## Register Indirect Addressing
- Offset stored in a register is added to the segment register. Used for dynamic storage of variables and data structures <br/>
레지스터에 담긴 수(offset)이 세그먼트의 시작점에 더해짐.<br/>
![ssw_ch_10](https://user-images.githubusercontent.com/63347989/144284255-be0c6da8-4e5a-4b7f-aa9f-9aa1f6bd5917.PNG)<br/>
ebx에 담겨있는 주소값이 최종 접근하고자하는 메모리 주소임. <br/>
ebx - 포인터 변수같은거임.<br/>
- The memory to memory mov is allowed with string instructions. <br/>
    - Any register EXCEPT __esp__ for the 80386 and up. <br/>
    esp를 제외한 어떤 레지스터도 사용가능. <br/>
    - For eax, ebx, ecx, edx, edi and esi: The data segment is the default. <br/>
    addressing하고자하는 메모리 세그먼트 영역이 데이터 세그먼트인 경우 eax ebx ecx edx edi esi가 사용이 되고, 
    - For ebp: The stack segment is the default. <br/>
    addressing하고자하는 대상 메모리가 스택인 경우 ebp가 사용됨. <br/>
    - Some versions of register indirect require special assembler directives byte, word, or dword <br/>
    ![ssw_ch_11](https://user-images.githubusercontent.com/63347989/144284258-6f72eafd-5cd9-4f29-9e83-043bf4ebce29.PNG)<br/>
    mov al, [edi]<br/>
    edi가 100이라는 수를 가지고 있다고 가정할때, 메모리 100번지에 있는 한 바이트를 al에 넣어라. <br/>
    al은 크기가 명확하기 때문에 오퍼랜드 앞에 키워드를 명시해줄 필요가 없음 <br/>
    mov [edi], 0x10<br/>
    상수 16진수 10을 100번지에 넣어라 - 애매함 <br/>
    왜냐하면 상수 10이 바이트단위의 10인지 워드 단위의 10인지 위 명령만 봐서는 모호함. <br/>
    실제로 위 코드 작성시 에러 발생. <br/>
    그래서 이런경우 크기를 operand에 정확히 명시해줄 필요가 있기 때문에 byte, word, dword키워드를 오퍼랜드 앞에 적어줘야함 <br/>
    - Does [edi] address a byte, a word or a double-word? Use<br/>
    ![ssw_ch_12](https://user-images.githubusercontent.com/63347989/144284260-ffd8fdcf-9567-4a34-af59-954ccecb29d3.PNG)<br/>
    10이라는 상수를 100번지에 한바이트 카피해라. <br/>

![ssw_ch_13](https://user-images.githubusercontent.com/63347989/144284264-3c98c77f-e675-43d6-bd01-a7cd8db7346a.PNG)<br/>
data segment의 시작점으로부터 offset만큼 떨어져있음.<br/> 
offset이 ebx레지스터 값임.<br/>
ebx의 값에 따라서 다른 위치를 가리키게 됨.<br/>

### Register Indirect Addressing의 에시
![ssw_ch_14](https://user-images.githubusercontent.com/63347989/144285080-5e5075ce-4bba-4dfb-99fe-3d2eb06f0d1a.PNG)<br/>
mov eax, [esi]<br/>
esi의 값이 16진수 200인데, 200번지에서 더블워드를 eax 레지스터로 복사해라. <br/>
adc [edi], eax<br/>
eax의 더블워드를 edi가 가르키는 번지(100번지)에 캐리를 포함해서 더해라. <br/>

## Register Relative Addressing
- Effective address(유효주소) computed as: seg_base(segment 시작점) + base(base register) + constant(주소 상수). <br/>
-Same default segment rules apply with respect to ebp, ebx, edi and esi. <br/>
어떤 세그먼트 레지스터가 사용될지는 base 레지스터가 무엇인지에 따라서 달라지는데, 
ebp - stack segment<br/>
ebx, edi, esi - data segment<br/>
- Displacement constant is any 32-bit signed value. <br/>
![ssw_ch_15](https://user-images.githubusercontent.com/63347989/144285083-67a31fc3-3b56-4f00-9d9c-2bc9766892cf.PNG)
mov eax, [ebx(base register)+100H(주소상수)]<br/>
주소상수 +  base register가 되어도 괜찮음. <br/>

### 종류
1. Base+displacement <br/>
- An index into an array when the element size is not 2, 4, or 8 bytes; The displacement encodes the static offset to the beginning of the array, while the base register holds the results of a calculation to determine the offset to a specific element within the array <br/>
배열이지만, 원소의 크기가 전형적인 2, 4, 8 바이트가 아님. <br/>
displacement는 배열의 시작점을 나타내고 index 첨자에 따른 몇번째 원소는 base register에 담겨있음. <br/>
- To access a field of a record; the base register holds the address of the beginning of the record, while the displacement is an static offset to the field <br/>
레코드에서 필드에 접근하는데 사용함.<br/>
여러개가 모여있는 자료구조에서 모여있는 각각의 크기가 2, 4, 8 바이트로 정형화되어있지않은 그런 자료구조 접근에 base + displacement가 적합한 addressing mode 이다. 
- A important special case is access to parameters in a procedure activation record (the base register in this case is EBP) <br/>
함수호출시에 activation record에 접근할때 좋음<br/>
![ssw_ch_16](https://user-images.githubusercontent.com/63347989/144285085-ae19477c-647f-4bf2-83c4-d9738f8283b4.PNG)<br/>
ebx register가 가리키는 곳이 배열의 시작점이고, <br/>
시작점으로부터 특정 원소까지를 displacement 레지스터로 나타냄<br/>



2. (Index*scale)+displacement <br/>
- Index into a __static array__ when the element size is 2, 4, or 8 bytes<br/>
전형적인 일정한 배열에서 사용할 수 있음. <br/>
배열의 시작점은 displacement가 가리키고, 배열의 인덱스에 해당하는 값은 index 레지스터가 가지면서, 만약에 그 배열 원소들이 word scale 이면 크기 2, double word scale이면 크기 4, quodaurable이면 크기 8로 지정할 수 있음. <br/>
![ssw_ch_17](https://user-images.githubusercontent.com/63347989/144402545-ff3683c4-153b-475d-aae2-ad7dcc518ed7.PNG)
<br/>
esi - index, displacement - array<br/>
esi * 4 - 4바이트 크기의 정수형 배열<br/>
특정한 몇번째 배열 원소인지는 esi가 가리키고 있음. <br/>

## Base-Plus-Index Addressing
displacement는 사용되지않은 경우<br/>
- Effective address computed as: seg_base + base + index. <br/>
- Base registers: Holds starting location of an array. <br/>
ebp, esp (stack) / ebx, … (data) <br/>
- Index registers: Holds offset location. <br/>
edi, esi, Any 32-bit register except esp. <br/>
- frequently used to access the elements of a dynamic array. A dynamic array is an array whose base address can change during program execution. <br/>
![ssw_ch_18](https://user-images.githubusercontent.com/63347989/144285346-be1bbe48-e13f-4ae6-8df1-2c46a6898194.PNG)<br/>


## Base-Plus-Index-plus-displacement Addressing
- Effective address computed as: seg_base + base + index + constant(displacement). <br/>
- Designed to be used as a mechanism to address a two-dimensional array (the displacement holds the address of the beginning of the array) <br/>
__이차원 배열__ 을 접근할때 사용하는 addressing mode<br/>
- One of several instances of an array of records (displacement is an offset to a field within the record) <br/>
![ssw_ch_19](https://user-images.githubusercontent.com/63347989/144285495-69d4bb53-ba96-4211-8205-7d72a0784390.PNG)<br/>

--------------

### Arrays
이차원 배열 접근 <br/>
![ssw_ch_22](https://user-images.githubusercontent.com/63347989/144285731-cae1346c-a1b7-46ed-9fa8-c01165356d10.PNG)<br/>

400: 한 줄에 수가 100개씩, 숫자 한개는 4바이트(1 더블워드)-> 4*100, 한행에 숫자들이 차지하는 메모리 공간의 크기<br/>
20: 20번째 줄부터 접근<br/>
전체 배열의 시작주소로부터 20번째 줄의 시작주소는 400 * 20인것.<br/> 

-----------

아우터 루프가 한번 돌때마다 접근하고자하는 행을 한행씩 접근하려고 함. <br/>
50: 50 ~ 54까지만 관심있음.<br/>

--------------

이너루프<br/>
abc: 배열의 시작주소이자 displacement<br/>
edx: 베이스 레지스터의 역할(지금 접근하고있는 행의 시작주소)<br/>
esi: 인덱스 레지스터의 역할(접근하고자하는 열)<br/>
<br/>
eax랑 0을 비교해서<br/>
같으면 ebx 증가<br/>
다르면 NOZ로 건너뜀<br/>
<br/>
noz에서 eax랑 1을 비교해서<br/>
같으면 ecx 증가<br/>
<br/>
그리고나서 esi는 무조건 증가함. - 다음열을 가리키도록<br/>
<br/>
그리고 esi랑 55를 비교해서 <br/>
55보다 작으면 inl 반복<br/>

-----------------
add edx ;행바꿈<br/>

---------------

## Scaled-Index Addressing 
- Effective address computed as: seg_base + base + constant*index <br/>
base register와 index register가 사용되는데<br/>
index register가 scale factor(constant)가 곱해진 형태<br/>
- Indexing two-dimensional array when the elements of the array are 2, 4, or 8 bytes in size<br/>
이차원 배열 접근에 유용한 addressing mode<br/>
![ssw_ch_20](https://user-images.githubusercontent.com/63347989/144285497-df003995-60b5-4271-85dc-7a0d46a6c842.PNG)<br/>
mov eax, [ebx+4*ecx]<br/>
ebx - base register <br/>
ecx - index register<br/>

![ssw_ch_21](https://user-images.githubusercontent.com/63347989/144285487-9c0fbb0c-74fa-483c-9449-eb1f3bac4f2c.PNG)



![ssw_ch_23](https://user-images.githubusercontent.com/63347989/144285726-f3d02fc1-ceb3-4272-971c-72787372462e.PNG)








