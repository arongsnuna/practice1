---
layout: post
title: Intel IA-32 Architecture 4
subtitle: SystemSoftware_03_4
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---

# Modes of Operation
32비트 intel CPU들은 실제 동작하는 CPU가 동작하는 몇가지 모드가 있음
메모리 사용이 크게 영향을 받음

- The IA-32 architecture supports three basic operating modes. The operating mode determines which instructions and architectural features are accessible. <br/>
세가지 동작모드는 운영체제가 컴퓨터 부팅하는 과정에서 선택함<br/>
어떤 모드인지 따라서 실행할 수 있는 명령들과 기능들에 차이가 있음<br/>

1. Protected mode 32비트 
2. Real-address mode 16비트
3. System management mode – 디버깅할 때 사용

일반적으로 Protected mode와 Real-address mode 중에서 하나에서만 동작함<br/>
두 모드는 메모리 사용에 있어서 큰 차이가 남


# IA-32 Memory Models
![ssw_ch3_14](https://user-images.githubusercontent.com/63347989/137115629-c111027a-11b0-4cd3-ae12-3d127c04ba77.PNG)
<br/>

1. Flat memory model 
CPU mode가 protected mode일 때 사용가능 <br/>
32 비트 (4기가바이트) 주소공간을 flat한, 일차원적인 주소공간으로 놓고 사용<br/>
4GB가 각 프로그램마다 주어짐<br/>
프로그램을 만들 때 프로그램의 최대 크기가 4GB<br/>
4GB 주소 공간 = Linear Address Space 선형 주소 공간 <br/>
Linear Address Space 중에서 특정한 위치 어딘가를 지칭할 때 사용하는 주소를 Linear Address(선형주소) 라고 함<br/>
선형주소는 숫자인데 32비트 임<br/>
프로그램이 flat memory model을 사용하겠다 결정했으면 실제 4GB를 어떻게 사용할지는 온전히 프로그램의 몫임<br/>

2. Segmented memory model 
CPU mode가 protected mode일 때 사용가능 <br/>
제일 많이 사용되고 중요한 모델임<br/>
선형 주소 공간이 4GB 주어짐<br/>
하나의 프로그램이 4GB를 여러 개의 segment 단위로 나눠서 사용<br/>
Segment는 하나의 작은 프로그램을 구성하는 논리적인 단위<br/>
Segment를 구성하는 요소에는 <br/>
code segment(program의 명령들), <br/>
data segment, <br/>
stack segment(프로그램이 실행되려면 스택이 있어야 함), <br/>
4GB 공간을 segment 단위로 나누고 <br/>
각 segment 단위를 적절하게 선형 주소 공간에 적절하게 배치해놓고 메모리 사용<br/>
Segment 하나는 논리적인 단위이기 때문에 크기가 정해져있는 게 아님<br/>
한 세그먼트의 최대 크기는 4GB임<br/>
하나의 세그먼트는 선형주소공간에서 연속된 영역에 위치함 (선형주소공간 상에서 연속성을 가지는 최소한의 단위)<br/>
세그먼트 간에는 연속적이지 않아도 된다<br/>
하나의 주소에 해당하는 숫자가 두개가 필요함<br/>
어떤 segment를 지정하는지=segment selector, 그 segment 내에서 어디 있는지=offset<br/>
![ssw_ch3_15](https://user-images.githubusercontent.com/63347989/137115768-f1db9404-deb8-4c7a-83d5-6df77ed4d575.PNG)<br/>
Flat model이나 segmented model이나 결국엔 linear address space에 프로그램이 위치하는 것인데<br/>
linear address space도 하나의 가상적인 메모리 공간임<br/>
실제 물리적은 ram으로 맵핑이 되려면 page를 통해서 또 한번의 주소변환이 이뤄져야 함<br/>

3. Real-address memory model
CPU mode가 Real-address mode일 때 사용가능 <br/>
Linear address space = 1MB<br/>
SEGMENT 하나의 최대 크기가 64KB<br/>
2^16 = 64 KB<br/>
하나의 프로그램이 사용할 수 있는 전체 주소가 64KB 임. <br/>
선형 주소 공간을 segment 단위로 나눠서 사용<br/>
논리주소가 segment selector(segment의 시작주소) + offset (시작주소와 얼마나 떨어져있는지)<br/>

Segmented memory model 와 Real-address memory model 결정적으로 다른 것은 offset의 크기<br/>
Segmented memory model offset의 크기 32비트<br/>
Real-address memory model offset의 크기 16비트
<br/>

# Program in Memory
![ssw_ch3_16](https://user-images.githubusercontent.com/63347989/137115985-31ed403d-abf2-4b66-8866-2a1c807d531f.PNG)
<br/>

Segment 관련해서 <br/>
가상 주소 공간: 프로그램을 만들어서 코드 작성 후 컴파일을 하고 링크를 해서 <br/>
실행파일이 나오면 그 실행파일의 모습<br/>
 혹은 실행파일 형태에서 실행이 시작되는 그 순간의 메모리 모습<br/>
4GB<br/>
가상 주소 공간 (실행파일)이 나눠져있음<br/>
Code(text) 영역 – CPU 명령들 모여있음<br/>
Data 영역 – 전역 변수에 해당하는 공간<br/>
Heap 영역 – 프로그램마다 사용할 수도 안 할수도 있는데, 동적인 메모리를 할당하는 영역<br/>
Stack 영역 – 프로그램마다 하나씩 필요, 함수와 관련된 여러가지 상황들, 지역 변수<br/>

Code나 data 영역은 크기가 일정하지만<br/>
Stack과 heap 영역은 가변적인 크기를 갖는다는 의미에서 화살표 표시함<br/>

Linear address space는 CPU가 실제 접근하고 관리할 수 있는 주소 공간<br/>
4GB<br/>
가상 주소 공간에 있는 code, data, heap, stack이 실행되려면 linear address space의 어떤 지점에 위치되어야 함<br/>
가상 주소 공간을 이루고 있는 영역 = segment<br/>

Code도 하나의 독립적인 segment, data 도 하나의 독립적인 segment, heap 도 하나의 독립적인 segment<br/>
, stack 도 하나의 독립적인 segment<br/>
4개의 세그먼트로 이루어진 응용프로그램<br/>
실행되기 위해서 선형 주소 공간에 올라갈 때(할당되어질때) 세그먼트 단위로 올라감(load된다)<br/>
올라갈때의 각 segment의 위치는 운영체제가 정하는 것<br/>

세그먼트는 프로그램을 이루고 있는 논리단위이다<br/>
세그먼트는 선형 주소 공간에서 일정한 영역을 차지한다<br/>
세그먼트 간에는 독립적이고 분리되어 있다<br/>
Flat memory model – Segmented memory model – Real-address memory model 에서 세그먼트 단위로 메모리가 올라감.<br/>

가상 주소공간에 있는 code segment 특정 위치의 주소는 두가지로 표현됨
1. code segment의 시작 주소
2. 그 시작주소와 얼마나 떨어져있는지 = offset

선형 주소 공간에 있는 code segment 특정 위치의 주소는 두가지로 표현됨
1. code segment의 시작 주소
2. 그 시작주소와 얼마나 떨어져있는지 = offset

이때 두 offset은 동일함
시작주소는 다르지만 말이다..

# Real- Address Memory Model in Real-Address Mode
- Only mode available to the 8086 / 8088 (intel CPU가 16비트였을때) and real-address mode for IA-32 architecture. 
    - Allow the processor to address only the first 1MB of memory. <br/>
    이 모드는 메모리 주소를 16비트로 표현하는 모드<br/>
    이 모드를 사용할 경우에 메인 메모리는 최대 1MB까지만 접근가능<br/>
    세그먼트를 기반으로 하는 메모리 모델이기 때문에 프로그램이 메모리상에서 세그먼트로 이루어져있다고 가정<br/>
    세그먼트 하나의 크기가 최대 64KB까지만 허용됨<br/>
- Segments and Offsets 
    - segment(16비트값) : offset(16비트값) 
    - Effective address = Segment address + an offset.

![ssw_ch3_17](https://user-images.githubusercontent.com/63347989/137116491-f00fe18c-749d-4c1d-8dbf-c2d7b0891e3f.PNG)

리얼 모드에서 1000:F000<br/>
1000가 segment register에 들어가 있음<br/>
1000을 실제 주소변환을 하게 되면 왼쪽으로 4비트 shift를 함 → 10000<br/>
접근하고자 하는 주소의 세그먼트 시작점 주소는 10000 번지이다<br/>
10000+F000(offset값) → 1F000 – 최종 우리가 원하는 선형 주소 공간 상에서의 선형 주소<br/>
→	20비트짜리로 변환됨<br/>
→	Real-ADDRESS MODEL에서의 주소변환<br/>
<br/>
메모리를 참조하는 모든 경우에 이 변환이 일어남<br/>
이 변환은 cpu가 함<br/>
이런 주소를 제시하는 프로그래머의 몫이지만<br/>
이 주소를 20비트 값으로 변환 과정은 CPU의 MMU(MAIN MEMORY UNIT)가 함<br/>
모든 메모리 접근에 대해서 말이다. <br/>

Segments and Offsets: 
    - Syntax is usually given as seg_addr:offset, <br/>
    e.g. 1000:F000 in the previous example to specify 1F000H. <br/>
    segment 와 offset 둘다 레지스터에 담겨서 사용이 되고 <br/>
    1000은 segment register에 담기고<br/>
    Offset 은 특정한 범용 레지스터에 담겨서 사용됨<br/>
    어떤 범용 레지스터 offset을 담느냐는 미리 정해진 규칙이 있음<br/>
    어느 세그먼트의 offset이냐에 따라서 범용레지스터가 달라짐<br/>
    - Implicit combinations of segment registers and offsets are defined for memory references. 
    - For example, the code segment (CS) is always used with the instruction pointer (IP for real mode or EIP for protected mode). <br/>
    Code segment의 어떤 특정한 지점의 위치의 주소이다.<br/>
    Segment register – CS register가 사용됨<br/>
    Offset – IP register 가 사용됨
- Examples 
    - CS:IP <br/>
    EIP : instruction pointer register<br/>
    IP: EIP의 16비트 버전 레지스터
    - SS:SP, SS:BP <br/>
    SS: 스택 세그먼트 레지스터<br/>
    SP -스택의 탑<br/>
    BP – 스택의 탑이 아닌 다른 부분
    - DS:AX, DS:BX, DS:CX, DS:DX, DS:DI, DS:SI, DS:8- bit_literal, DS:16-bit_literal
    - ES:DI 
    - FS nd GS have no default.
    
![ssw_ch3_18](https://user-images.githubusercontent.com/63347989/137116663-ed41205d-c908-45a5-8302-5f9e23bc5058.PNG)
