---
layout: post
title: Introduction of System Software 1
subtitle: SystemSoftware_01_1
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---


# COMPUTER SYSTEM
## 컴퓨터 시스템의 구성
<BR/>

![ssw_ch1_1](https://user-images.githubusercontent.com/63347989/135997131-1df940a8-f5a9-46b4-93d6-c531f0c87a88.PNG)

1. 사용자<br/>
ex. 사람, 다른 컴퓨터 (서버의 사용자는 클라이언트라는 또다른 컴퓨터)

2. 응용프로그램<br/>
사용자의 문제해결을 돕는 소프트웨어 <br/>
ex. word processor, web browser, ...

3. 운영체제 (시스템 소프트웨어 포함)<br/>
응용 프로그램이 작동하기 위해서 하드웨어가 필요한데,<br/> 
운영체제가 응용 프로그램의 하드웨어 사용에 개입해서 적절하게 사용할수 있도록 통제

4. 하드웨어<br/>
컴퓨팅(= 컴퓨터가 특정한 목적을 위해서 동작하는 것) 하는데 필요한 자원<br/>
ex. cpu, main memory, io device ...

## COMPUTER HARDWARE
<br/>

![ssw_ch1_2](https://user-images.githubusercontent.com/63347989/135997576-e204529a-0753-4aed-914f-36c6e07d81d7.PNG)


1. CPU : 프로세서 처리기, 중앙처리 장치<br/>
컴퓨터 프로그램을 실행하는 장치<br/>
    - CU (Control Unit) <br/>
    프로그램이 실행이 원만하게 이루어질 수 있도록 제어하는 장치 
    - ALU (Arithmetic Logic Unit)(산술 논리 연산장치) <br/>
    프로그램의 구체적인 명령 실행 
2. MM (Main Memory)<br/>
실행중인 프로그램이 머무는 곳
3. Input Output device 입출력장치 <br/>
Input + HDD + Ouput<br/>
컴퓨터 외부와 입출력을 주고받거나 데이터를 보관하는 역할

## BASIC ELEMENTS
<br/>

![ssw_ch1_3](https://user-images.githubusercontent.com/63347989/135999148-1dca53f8-65ce-4669-a201-74f2b1152bb0.PNG)

- Processor (CPU)
- Main Memory
    - volatile : 휘발성 (전원 on일때만 기억 유지)
    - referred to as real memory or primary memory (현재 실행중인 프로그램과 관련)
- I/O devices
    - secondary memory devices : 보조 기억 장치
    - communications equipment : 통신 장치
    - terminals : 터미널
- System bus
    - communication among processors, memory, and I/O modules

<br/>
실행중인 프로그램은 메인 메모리에 머뭄<br/>
프로그램 안에는 cpu가 실행할 수 있는 명령들이 있는데 버스를 통해서 명령이 cpu로 이동해 실행됨 <br/>
입출력 명령은 메모리에서 io device로 가서 명령 실행<br/>
즉, 공통의 메인 메모리를 두고 CPU와 IO device가 경쟁<br/>
<br/>

## HOW MODERN COMPUTER WORKS
=stored program 방식 = 폰 노이만 방식 <br/>

![ssw_ch1_4](https://user-images.githubusercontent.com/63347989/136001017-6f7fca74-b129-4865-a731-89c430c239cf.PNG)

*interrupt : 입출력이 완려되었다는 신호
<br/>

## COMPUTER PROGRAM
<br/>

__컴퓨터 프로그램__ <br/>
: CPU가 실행해야 할 구체적인 내용을 한 단계씩 순서대로 기술한 명령(instruction)의 연속<br/>

__컴퓨터 프로그램이 실행된다__<br/>
: 프로그램 내의 instruction들이 CPU에 의해서 '순차적'으로 한번에 하나씩 실행됨을 의미함<br/>

__CPU instruction (명령)__ = machine language (기계어)<br/>
: 이진수 (0과 1)로 표시된 것으로 컴퓨터의 프로세서가 이해하는 유일한 언어 <br/>
(각 기계 간에 호환성 없음, 모든 CPU는 자기자신만의 명령어를 씀)
<br/>

## FETCH-EXECUTION CYCLE
<br/>

- ENIAC(최초의 컴퓨터) does not store program (플러그와 점퍼 케이블을 가지고 연결)<br/>
- Stored program 방식의 컴퓨터 (von Neumann 폰노이만) <br/>
![ssw_ch1_5](https://user-images.githubusercontent.com/63347989/136000741-22e20f91-3dd7-4732-9d37-5778e532413b.PNG)
    - 현재의 대부분의 컴퓨터 (스마트폰, 테스크탑, 랩탑, 태블릿, ...)
    - 계산 -> 프로그램의 수행(running)을 의미
    - memory: 프로그램이 저장된 장소
    - processor: 계산을 수행하는 기계의 부분
    - computer program: list of computer instruction
    - __Fetch-Execution Cycle에 의해서 CPU 동작__


![ssw_ch1_6](https://user-images.githubusercontent.com/63347989/136000734-a1cd3e04-7165-41c5-8f15-b86be65d77f9.PNG)

1. The processor fetches an instruction from memory : fetch 단계
2. The processor executes the instruction : excution 단계
3. The processor cycles back to step “fetch”

한 명령에 의해서 Fetch와 Execution이 이뤄지면 Fetch-Execution Cycle 이 완성이 됨<br/>
Fetch-Execution Cycle은 Fetch와 Execution의 무한반복임 (전원 on일 동안 무한반복)<br/>
Fetch 와 Execution을 하려면 메모리가 필요함<br/>
=> REGISTER 필요함! 
<br/>
<br/>
(다음장에 계속...)