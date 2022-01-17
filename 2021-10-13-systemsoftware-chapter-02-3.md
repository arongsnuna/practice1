---
layout: post
title: Binary System 3
subtitle: SystemSoftware_02_3
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Binary codes

수 연산 → 이진수 형태로 
수 이외에 문자나 이미지, 비디오, 사운드 등 다른 형태의 정보들은 binary code 사용해서 저장 <br/>
Binary code : 정보를 표현하고자 할 때 정보가 가질 수 있는 모든 값을 커버할 수 있는 충분한 비트 수를 미리 정해놓고 이 비트수의 이진수 패턴과 정보의 특정 값간의 맵핑을 해놓은 것 <br/>
Ex. 알파벳(A~Z, 26가지) → 필요한 비트수는 5가 됨. 5비트는 26가지 정보를 모두 커버할 수 있게 됨. <br/>

- An n-bit binary code is a group of n bits that can assume a max of 2^n distinct combinations of 0 and 1<br/>
    - What if we had 3 combinations say 0, 1& 1/2, how many distinct combinations could we then have? What if we had 4 possible combinations?
- A 3-bit binary code can assume 8 distinct combinations
- Each combination can be assigned a number determined from the binary count from 0 to 2^n – 1, similarly for a 4-bit binary code

# Binary Coded Decimal code (BCD)

대표적인 10진수 바이너리 코드는 BCD가 있음<br/>
표현하고자하는 정보가 10가지 있는 것이기 때문에 최소 10가지의 비트패턴을 마련해야함<br/>
2^4 = 16 <br/>
4비트가 필요함<br/>
BCD는 4비트 코드<br/>
(The remaining 6 are unused)<br/>

![ssw_ch2_15](https://user-images.githubusercontent.com/63347989/137003867-fcd461d6-f8c9-48c8-a2a2-c0de06e25f67.PNG)
<br/>
ex. (185)10 = (0001 1000 0101)BCD = (10111001)2

## BCD의 덧셈
덧셈의 결과가 1001보다 작으면 그 결과가 찐 결과<br/>
덧셈의 결과가 1001보다 크면 그 결과에 0110을 더해주면 그 값이 찐 결과값<br/>

Similar to binary addition, as long as BCD digit sum is __less than or equal to 1001__ <br/>
When sum is __greater than 1001__, __add 6=0110__ to correct it <br/>
ex1. 4+5 = 0100 + 0101 = 1001 (+9) OK as answer is <= 1001<br/>
ex2. 4+8 = 0100+1000 = 1100 (>1001), so add 0110 to this; 1100+0110 = 10010 =  0001 0010 = 12 <br/>

## 다른 십진수에 대한 바이너리 코드 종류
- 2421 
- Excess-3 
- 84-2-1 
- BCD = 8421 임

대부분의 바이너리 코드들이 weighted code임 <br/>
weighed code: 코드의 4자리에 대해서 각각 자리의 위치에 가중치가 주어져있다는 것을 의미함 <br/>
일부 decimal 코드들 중에는 십진수 하나에 대한 표현이 두가지 이상이 나올 수 있는데<br/>
가중치 때문에 이런 일들이 벌어짐 <br/>
ex. 2421 코드 같은 경우에 4 = 0100 = 1010 <br/>

2421과 Excess-3 같은 경우에 self complementing <br/>
십진수 숫자에서 9의 보수는 0과 1을 바꿈으로써 <br/>
즉 1의 보수를 구함으로써 쉽게 얻어질 수 있다<br/>
십진수에서 9의 보수관계가 있다면 해당 2421 코드 관계는 1의 보수관계가 있다는 것 => self complementing <br/>
9’s complement of a decimal number can be obtained directly by changing 1 to 0 and 0 
to 1<br/>

Gray code<br/>
연속되는 십진수 숫자 2개에 대한 이진수 코드값이 한비트 차이만 나도록 배치됨<br/>
이러한 코드의 장점은 <br/>
해당되는 gray code로 표현되어있는 데이터는 카운팅할 때 오류를 발생할 가능성을 감소시킬 수 있음<br/>
오류를 쉽게 탐지할 수 있음<br/>
Only 1 bit in the code changes when going from one number to next<br/>
Reduces electronic error in counting, which can happen when too many bits need to be changed at the same time <br/>
ex. 7 to 8 in binary → 0111 to 1000, all bits change! In gray code 0100 to 1100, only a 1 bit change <br/>

![ssw_ch2_16](https://user-images.githubusercontent.com/63347989/137005348-591f8fe8-86d8-43d7-80ae-802fb72f06d5.PNG)

# ASCII code
문자를 표기하기 위한 코드 (American Standard Code for Information Interchange)<br/>
Alphanumeric 문자임 (numbers, characters and symbols total of 128 codes) <br/>
128개의 문자에 대한 코드를 나타내고 있음<br/>
2^7=128, 적어도 7비트 코드임<br/>
대부분의 컴퓨터는 바이트 단위로 동작함 
아스키코드 하나는 그자체가 7비트니까 한 바이트 (8비트)에 담아서 표현함<br/>
(Byte is the most common unit of memory that is manipulated by the computer) <br/>

![ssw_ch2_17](https://user-images.githubusercontent.com/63347989/137005866-81959787-a3fe-46be-b919-87a5cd56e75a.PNG)

화면에 출력했을 때 형태가 보인다기 보다는, 화면이 어떤 방식으로 display 되어야하는지에 대한 것을 제어
= 제어 문자 (32개)  <br/>
Bs = backspace 한글자가 사라지게되는 <br/>
Lf = line feed \n 개행문자 <br/>
Sp = space 공백문자 <br/>

# Error detecting codes

이진 코드로 표현된 데이터를 컴퓨터 내부에서 처리를 하든지, 다른 컴퓨터에 전송을 하는 경우에, 오류가 발생할 수 있음 <br/>
Error detecting codes는 에러를 탐지하기 위해서 데이터를 저장하거나 전송하는 과정에서 이진수 코드에 추가적으로 부가하는 코드임<br/>
제일 유명한 코드는 아스키코드와 함께 사용되는 __parity code__ 가 있음<br/>
짝수 패리티 -> 주어진 아스키코드에 아스키코드에 담긴 1의 개수가 짝수개가 되도록 0 또는 1 추가 <br/>
홀수 패리티 -> 주어진 아스키코드에 아스키코드에 담긴 1의 개수가 홀수개가 되도록 0 또는 1 추가 <br/>


Errors can occur while reading and writing data or any sort of information, especially when we send information over a communication medium e.g. copper wires/ telephone lines, wireless communications <br/>
To detect errors – add redundancy (i.e. additional information) along with the data/message being sent <br/>
- ex. Add an extra Parity bit to the ASCII character to indicate its parity<br/>
ASCII A = 1000001
- Even parity – add extra bit such that the total number of bits is even<br/>
Even parity = __0__ 1000001
- Odd parity – add extra bit such that the total number of bits is odd<br/>
Odd parity = __1__ 1000001

## example of error detecting codes
Both the sender (transmitter Tx) and receiver (Rx) agree upon using a certain type of parity<br/>
패리티 사용해서 문자를 송수신하는 예<br/>
1. Tx에서 parity 각 문자에 대한 만듦 (한 문자가 7비트에다가 parity 추가해서 1바이트)
2. Rx에서 아스키코드 7비트와 그에 해당하는 parity가 맞는지 확인
3. parity가 맞지않으면 송수신 과정에서 오류가 생겼다는 것을 의미 - 송신자에게 재전송을 요청

이 방법은 홀수개의 오류가 발생했을 때 탐지가 가능하지만<br/>
짝수개의 오류가 발생하면 오류 탐지 불가능<br/>

Remember a 7 bit ASCII character is stored in an 8 bit byte – the extra bit is usually used for 
parity<br/>
To check if each ASCII character is read/written/transferred/stored correctly <br/>