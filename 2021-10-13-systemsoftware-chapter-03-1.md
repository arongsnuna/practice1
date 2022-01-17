---
layout: post
title: Intel IA-32 Architecture 1
subtitle: SystemSoftware_03_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---
# 80x86 Evolution
Intel x86 or 80x86 <br/>
: 인텔사가 개발한 일련의 마이크로 프로세서 통칭 <br/>
Intel x86 32 bit cpu architecture 의 공식 = intel IA-32 Architecture<br/>
Computer architecture = cpu instruction set + micro architecture + system design 등이 모두 포함<br/>
프로그래머 입장에서 해당 CPU 상에서 동작하는 프로그램을 작성하기 위해서 꼭 알아야 하는 내용이 포함되어 있음<br/>

### ! 참고 ! CPU 동작 속도 단위
1. IPS : Instruction Per Second <br/>
초당 실행할 수 있는 명령의 개수 <br/>
ex. 50K IPS = 1초에 50,000개의 명령을 실행할 수 있는 속도 <br/>
2. Clock rate (Hz)<br/>
1초에 동작이 몇번 하느냐 <br/>
내부 동작의 기준이 되는 clock이라는 부품을 가지고 있고 내부 동작은 clock의 신호에 맞춰서 이뤄지게 되어있음 <br/>
Clock이 동작하는 frequency가 1초에 200만번 똑딱똑딱함 <br/>
2MHz인 경우보다 4MHz인 경우에 2배 더 빠르게 연산 <br/>
Clock rate은 주파수가 클수록 빠른 CPU <br/>
Hz의 값이 크면 클수록 같은 연산을 훨씬 더 빨리 실행할 수 있음 <br/>
3. Clock cycle time(ns)<br/> 
한번 동작하는데 몇초가 걸리느냐 <br/>
Clock cycle time이 짧을수록 빠른 CPU<br/>
1 microsecond = 1000 nanoseconds(ns)



## 4004
- 최초의 Intel CPU
- 4-bit microprocessor. <br/>
이 CPU가 한번에 처리할 수 있는 연산이 4비트 연산
- 4KB main memory.
- 45 instructions.
- 50 KIPS<br/>
초당 50,000개의 명령 실행

## 8008 (1972)
- __8-bit__ version of 4004.
- __16KB__ main memory.
- 48 instructions.

## 8080 (1974)
- 8-bit microprocessor.
- __64KB__ main memory.
- 2 MHz clock rate; 500,000 instructions/sec. <br/>
1초에 동작이 2,000,000 번 함, 초당 500,000 개의 명령 실행
- 10X faster than 8008.

## 8085 (1976)
- 출시된 직후에는 컴퓨터에서 CPU로 사용이 되다가 8-bit microprocessor는 너무 작아서 컴퓨터가 아닌 다양한 전자장비에서 여러장치를 제어하는 controller로써 많이 사용됨 
- 8-bit microprocessor - upgraded version of the 8080.
- 64KB main memory.
- __1.3 microseconds__ clock cycle time; 769,230 instructions/sec.<br/>
한번 동작하는데 1.3 microseconds가 걸림, 초당 769,230개의 명령 실행
- Intel sold 100 million copies of this 8-bit microprocessor.<br/>
1억개 넘게 판매된 베스트셀러 

## 8086 (1978) / 8088 (1979)
- X86 architecture의 기원이 되는 processor
- 8086과 8088은 거의 동일한 CPU임. <br/>
8088은 8086의 보급형 버전
- __16-bit__ microprocessor.
- __1MB__ main memory.
- 2.5 MIPS (400 ns). <br/>
초당 2,500,000 개의 명령 실행, 한번 동작하는데 400ns가 걸림 
- 4- or 6-byte instruction cache.
- Other improvements included more registers and additional instructions.
- IBM에서 8088을 CPU로 채택해서 개인용 컴퓨터 출시 → IBM personal computer xt → 최초의 개인용 컴퓨터
- IBM personal computer xt가 잘되다보니까 그 당시 컴퓨터 제조사들이 이 컴퓨터와 호환이 되는 PC를 만들어서 판매를하게 됨으로써 개인용 컴퓨터 시대가 도래
- 8088을 장착하고 있는 개인용 컴퓨터를 IBM 호환 PC, IBM compatible PC → 이때 PC가 자리잡게 되면서 누구나 컴퓨터한대씩 가져다놓고 사용

## 80286 (1982)
- 8080과 호환이 됨.
- 16-bit microprocessor very similar in instruction set to the 8086.
- __16MB__ main memory.
- 4.0 MIPS (250 ns/8MHz). <br/>
초당 4,000,000 개의 명령실행, 한번 동작하는데 250ns, 1초에 동작이 8,000,000번 함.

## 80386 (1985)
- __32-bit__ microprocessor. 최초의!!
- __4GB__ main memory.<br/>
응용프로그램한테 제공하는 개념적인 주소공간 → 가상공간 제공 (MMU를 통해)
- 12-33MHz.
- Memory management unit (MMU) added.<br/>
MMU: CPU의 여러가지 메모리 접근 관련해서 지원하는 기능들을 실제로 구현하고 있는 CPU내의 부품<br/>
MMU를 통해 가상 메모리를 컴퓨터에서 사용할 수 있게 됨<br/>
가상 메모리: 컴퓨터에 장착이 되어있는 실제 메인 메모리보다 훨씬 더 넓은 메모리 공간을 가진 것처럼 사용자들한테 보여줄 수 있음<br/>
RAM은 수 MB 정도만 달 수 있었는데 그럼에도 불구하고 MMU가 가상메모리기능을 가지고 있기 때문에 응용프로그램한테 4GB를 제공할 수 있음<br/>
- Variations: DX, EX, SL, SLC (cache) and SX.<br/>
Cache나 속도에 따라서 다양한 variation들이 존재<br/>
80386SX: 16MB through a 16-bit data bus and 24 bit address bus.<br/>

## 80486 (1989)
- 32-bit microprocessor, 32-bit data bus and 32-bit address bus.
- 4GB main memory.
- 20-50MHz. Later at 66 and 100MHz
- Incorporated an 80386-like microprocessor, 80387-like floating point coprocessor and an 8K byte cache on one package.<br/>
80387과 유사한 기능을 하는 부동소수점 coprocessor를 내장하고 있음<br/>
80387은 부동소수점 연산만을 전담하는 전용 coprocessor<br/>
80386시절에는 80387를 별도로 구매하여 메인보드에 장착을 따로 해줘야했음<br/>
- About half of the instructions executed in 1 clock instead of 2 on the 386.<br/>
대부분의 명령들이 clock cycle이 반으로 줄음 <br/>
80386에서는 2사이클에 실행하던 명령이 80486에서 1사이클이면 실행됨<br/>
효과) 같은 프로그램이 실행속도가 훨씬 빨라짐<br/>
- Variations: SX, DX2, DX4.<br/>
DX2: Double clocked version: 66MHz clock cycle time with memory transfers at 33MHz.

## --제품명이 부품번호(partnumber) 이었는데 제품명이 부품번호가 아닌 이름이 붙기 시작.--

## Pentium (1993)
![ssw_ch3_1](https://user-images.githubusercontent.com/63347989/137013358-b4501976-e677-49d1-918e-b3c8bfb6be1a.PNG)

- 32-bit microprocessor, 64-bit data bus and 32-bit address bus.
- 4GB main memory.
- 60, 66, 90MHz. <br/>
    - 1-and-1/2 100MHz version.
    - Double clocked 120 and 133MHz versions.
    - Fastest version is the 233MHz (3-and-1/2 clocked version).
- __16KB L1 cache__ (split instruction/data: 8KB each).<br/>
L1은 Level 1<br/>
CPU에 얼마나 가까이 있는지에 따라서 L1,L2,L3 cache 나눠짐 (L1이 CPU에서 제일 가까움)<br/>
Cache: 메인메모리에 있는 명령이나 데이터에 대한 복사본을 CPU의 내부의 메모리가 있어서 그 메모리에 가져다 놓은 것 <br/> 
cache 효과) cache에 있는 내용에 접근하면 메인메모리까지 가지 않아도 되니까 훨씬 빨리 명령이나 데이터에 접근할 수 있음<br/> 
Cache가 있어서 성능향상이 크게 이뤄질 수 있다<br/> 
- Memory transfers at __66MHz__ (instead of 33MHz).<br/>
System bus의 clock 이 66MHz  <br/>
System bus: CPU와 MM를 이어주는 데이터 통로, 자체적으로 clock을 가지고 있음 <br/>
- Dual integer processors.<br/>
CPU가 하는 연산 중의 일부 정수관련 연산들을 dual 두개의 프로세서가 진행해서 성능향상을 시킴

## Pentium Pro (1995)
![ssw_ch3_2](https://user-images.githubusercontent.com/63347989/137013359-db82aabf-78a1-4688-bf12-afc14e95c93d.PNG)

- P6 micro architecture 처음으로 사용됨 <br/>
원래는 p5 micro architecture 였음
- 32-bit microprocessor, 64-bit data bus and 36-bit address bus.
- __64GB__ main memory.
- Starts at 150MHz.
- 16KB L1 cache (split instruction/data: 8KB each).
- __256KB L2 cache__ <br/>
메모리의 복사본을 일차적으로 L2 cache에 두고 <br/>
L2 cache 복사본을 일부 L1 cache에 둠<br/>
CPU 입장에서 같은 데이터라고 할지라도 L1 cache에 있는 걸 제일 빨리 접근할 수 있음 
- Memory transfers at 66MHz.<br/>
bus의 clock rate → 66MHz
- __3 integer processors__

## Pentium II (1997)
![ssw_ch3_3](https://user-images.githubusercontent.com/63347989/137013361-5d94c071-d331-4896-9ff4-ec71cecac023.PNG)

- 32-bit microprocessor, 64-bit data bus and 36-bit address bus.
- 64GB main memory.
- Starts at 233MHz. (clock rate)
- __32KB__ split instruction/data L1 caches (16KB each).
- __Module integrated 512KB L2 cache__ (133MHz). <br/>
CPU 바깥에 떨어져서 장착이 됨 → bus로 연결이 됨
- Memory transfers at 66MHz to 100MHz (1998). <br/>
System bus 접근 속도

## Pentium III (1999)
![ssw_ch3_4](https://user-images.githubusercontent.com/63347989/137013352-a5888f5e-36fd-4f6b-a991-3f7bcfc508fb.PNG)
- 32-bit microprocessor, 64-bit data bus and 36-bit address bus.
- 64GB main memory.
- 800MHz and above.
- 32KB split instruction/data L1 caches (16KB each).
- __On-chip 256KB L2 cache__ <br/>
CPU칩 안에 위치. 그래서 접근 속도가 빨라짐. <br/>
Pentium II에서는 CPU 밖에 위치했었음.
- Memory transfers 100MHz to 133MHz.<br/>
System bus 접근 속도 

## The Intel Pentium M Processor (2003-2006) 
- 모바일 (노트북에 사용됨)
- On-die, primary(L1 cache) 32-KByte __instruction__ cache and 32-KByte write-back __data__ cache <br/>
Cache의 내용이 명령인 경우) 프로그램이 실행되면서 명령이 바뀔일이 없음 읽기 전용 데이터임<br/>
Cache의 내용이 데이터인 경우) 프로그램이 실행되면서 데이터의 값들이 바뀔 수 있음, 복사본의 내용이 바뀌었으니까 메모리에 있는 원본 내용도 바껴야함<br/>
- On-die, second-level cache (up to 2 MByte) (L2 cache) 
- Advanced Branch Prediction and Data Prefetch Logic <br/>
CPU가 동작을 하면서 명령 fetch 해오는데 미리 3-4개씩 fetch해옴 → 속도향상, 성능향상 
- Support for MMX technology, Streaming __SIMD instructions__, and the SSE2 instruction set <br/>
SIMD instructions: 한번에 64bit 크기의 큰 데이터를 처리할 수 있는 특수 명령 
- A 400 or 533 MHz, Source-Synchronous Processor System Bus <br/>
Bus speed 
- Advanced power management using Enhanced Intel SpeedStep® technology <br/>
모바일 용이니만큼 power management를 잘할 수 있는 speedstep 기술이 있음 

## The Intel Core Duo and Intel Core Solo Processors (2006-2007) 
![ssw_ch3_5](https://user-images.githubusercontent.com/63347989/137017077-c6bf8d44-2737-49a5-af06-9409c98365db.PNG)

- 32bit processor
- 2MB L2 cache: Intel® Smart Cache which allows for efficient data sharing between two processor cores 
- Improved decoding and SIMD execution 
- Intel® Dynamic __Power Coordination__ and Enhanced Intel® Deeper Sleep to reduce power consumption 
- Intel® Advanced Thermal Manager which features digital thermal sensor interfaces <br/>
발열관리 기술 추가
- Support for power-optimized 667 MHz bus 

## Pentium 4 (2000~2006)
- Netburst microarchitecture <br/>
(Pentium 3까지는 p6 microarchitecture)
- 1.4 to 1.9GHz and the latest at 3.20 GHz and 3.46GHz (Hyper-Threading) <br/>
Hyper-Threading : 명령을 실행하는 과정에서 최대한 병행 수행이 가능한 연산들은 동시에 같이 실행함으로써 명령실행속도를 높이는 기술 
- 1MB/512KB/256KB L2 cache. 
- 800 MHz (about 6.4GB/s)/533 MHz (4.3 GB/s)/ 400MHz (3.2 GB/s) system bus. 
- Specialized for streaming video, game and DVD applications (144 new SIMD 128-bit instructions). <br/>
32bit CPU이지만 32bit이상이 되는 큰 데이터를 한번에 처리할 수 있는 명령  <br/>
즉 128bit 명령을 실행할수 있는 simd instruction  
→ 이슈가 멀티미디어 처리였는데 수나 문자데이터보다 volume이 크기 때문에 멀티미디어 데이터를 잘 처리할 수 있는 simd instruction이 제공이 됐다 <br/>

## Mobile Pentium 4-M / Pentium 4E / Pentium 4 EE 
- 전력소모를 제어할 수 있는 기능을 들어가 있는 CPU

## --0본격적으로 64 bit cpu 출시 !!!!!!!!!!!!!!!--

## Itanium / Itanium2 (2001/2002~2010) 
- 독자적인 명령집합을 가지고 있음<br/>
명령집합의 이름을 IA-64 <br/>
IA-64는 기존의 intel CPU가 가지고있던 명령과는 전혀 호환이 되지않는 뜬금없는 명령 집합을 가지고 있는 CPU
- 서버급 CPU
- 733, 800MHz(Itanium), 900MHz 
- 1.6GHz (Itanium2) 
- IA-64 architecture 

## Pentium 4F / Pentium D (2005) 
- Pentium D는 pentium 4F의 dual core 버전<br/>
둘 다 intel 64라는 64-bit 명령집합을 지원을 함 
- NetBurst microarchitecture 
- Intel Extended Memory 64 Technology 
- Compatible with AMD’s AMD64 architecture 호환이 됨 
- PentiumD: desktop dual-core 64-bit x86-64 microarchitecture with Net Burst microarchitecture 

## Intel Core 2 (2006) 
- 64 bit processor
- core(microarchitecture's mame) microarchitecture 
- Two cores on one die 
- 64KB L1 cache per core
- Intel VT-x support <br/>
 virtual machine 기능 지원하는 cpu 기능 
- ~3.0GHz clock rate 

## Xeon (2004~) 
- 업계의 실질적이 표준 
- 서버급 CPU
- 서버나 work station 급 고급 사양의 데스크탑이 아닌 고급 사양의 컴퓨터에 장착되는 인텔의 64비트 cpu

## Core i3/Core i5/ Core i7 (2010 -) 
- → 로 갈수록 고급사양<br/>
- Core의 개수와 thread 개수가 다름<br/>
    - Core i3 → core 2 tread 4<br/>
    - Core i5 → core 4 thread 4<br/>
    - Core i7 → core 4 thread 8<br/>
- Cache의 크기도 차이가 남
- 그 외의 기능과 성능은 대동소이함 
- 공통: GPU 내장돼있음 
- 1st generation: Nehalem microarchitecture(2010) 
- 2nd/3rd generation: Sandy Bridge/Ivy Bridge microarchitecture (2011-2012) 
- 4th generation: Haswell (2013) 
- 5th generation: Broadwell (2014) 
- 6th generation: Skylake (2015-2019) 
- Kaby lake, coffee lake, cannon lake, ice lake, comet lake, …<br/>
세대별로 차이가 나는 것은 지원하는 ram의 규격 소켓의 규격(소켓: 마더보드에 cpu칩을 꼽을 때 소켓의 규격)<br/>
속도면에서 6세대정도 4GHz정도 나옴 <br/>
Comet lake 같은 경우 <br/>
    - core 가 최대 10개 정도<br/>
    - Hyper threading 전모델에서 지원<br/>
    - Clock rate 5.3GHz됨<br/>
    - ram규격 ddr4까지 지원<br/>










