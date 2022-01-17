---
layout: post
title: Binary System 2
subtitle: SystemSoftware_02_2
gh-repo: unhipark
gh-badge: none
tags: [systemsoftware]
comments: true
---
# Complements 보수
보수는 간단히 말해서 반대로 세어가는 수<br/>
complements are used to simplify the subtraction operation (뺄셈연산) and for logical manipulation (논리연산). <br/>

## 1. Diminished radix complement, __(r-1)’s complement__
- Given a number N in base r having n digits, the (r-1)’s complement of N is defined as 
$$ (r^n – 1) - N $$
- For decimal number, the 9’s complement is obtained by subtracting each digit from 9
    - ex. The 9’s complement of 546700 is 999999 – 546700 = 453299
    - ex. The 9’s complement of 012398 is 999999 – 012398 = 987601
- For binary numbers, the 1’s complement is obtained by subtracting each digit from 1 
(i.e. changing 1’s to 0’s and 0’s to 1’s)
    - The 1’s complement of 1011000 is 0100111 <br/>
    (1111111 - 1011000 = 0100111) 
    - The 1’s complement of 0101101 is 1010010 <br/>
    (1111111 - 0101101 = 1010010)

## 2. Radix complement, __r’s complement__
- Given a number N in base r having n digits, the r’s complement of N is defined as 
    - for N ≠ 0 $$ (r^n – 1) - N + 1 = r^n - N $$
    - for N = 0 $$ 0 $$
- 10’s complement can be formed by leaving all least significant 0’s unchanged, 
subtracting the first nonzero least significant digit from 10, and subtracting all higher 
significant digits from 9
    - The 10’s complement of 012398 is 987602
    - The 10’s complement of 246700 is 753300
- 2’s complement can be formed by leaving all least significant 0’s and the first 1 
unchanged and replacing 1’s with 0’s and 0’s with 1’s in all other higher significant 
digits
    - The 2’s complement of 1101100 is 0010100
    - The 2’s complement of 0110111 is 1001001


## 3. SOMETHING ELSE
- If the number N contains a radix point(소수점),  <br/>
the point should be removed temporarily  <br/>
in order to form the r’s or (r-1)’s complement,  <br/>
then the radix point is restored to the complemented number in the same relative position.  <br/>
소수점을 포함하고 있는 숫자에서 보수를 어떻게 구하느냐<br/>
소수점을 임시로 제거를 하고 정수 형태에서 보수를 구해놓고 <br/>
원래수와 같은 자리에 소수점을 추가하면 됨

- The complement of the complement number restores the number to its original value<br/>
보수의 보수는 원래의 수

# Subtraction using complements

CPU가 내부적으로 뺄셈 연산 동작을 수행할 때,  <br/>
자체적으로 별도의 뺄셈 방법을 가지고 있다기 보다는, <br/>
덧셈과 보수를 통해서 뺄셈을 한다. <br/>

뺄셈을 하는 대신에, 빼는 수의 보수를 덧셈을 하면 뺄셈과 동일한 결과 얻을 수 있다.  <br/>
→ __complement addition__ (보수 가산)<br/>

## &nbsp; &nbsp; M – N in base r having n digits  <br/>
1. Add M to the r’s complement of N $$ M + (r^n – N) = M – N + r^n $$
2. - If M ≥ N, there is an __end carry__ (n+1자리수가 됨), discard it result M – N<br/>
    ![ssw_ch2_9](https://user-images.githubusercontent.com/63347989/136246722-5a172949-db09-4042-afc7-be54a71bb44a.PNG)

    - If M < N, __no end carry__, result is r’s complement of N – M; <br/>
    take the r’s complement and place a ‘minus’ sign in front<br/> 
    ![ssw_ch2_10](https://user-images.githubusercontent.com/63347989/136246716-91d18e97-e58d-41c7-b9f8-6b433e977660.PNG)

## Example of Subtraction using Complements
Given the two binary numbers X = 1010100 and Y = 1000011, <br/>
perform the subtraction (a) X – Y and (b) Y – X

1. by using 2’s complement<br/>
    - (a) 2's complement of Y = 0111101<br/>
    X - Y = 1010100 + 0111101 = __1__ 0010001 → end carry <br/>
    Answer: X - Y = 0010001
    - (b) 2's complement of X = 0101100<br/>
    Y - X = 1000011 + 0101100 = 1101111 → no end carry <br/>
    Answer: Y - X = 1101111 <br/>
2. by using 1’s complement<br/>
    - (a) 1's complement of Y = 0111100<br/>
    X - Y = 1010100 + 0111100 = 10010000 → end-around carry <br/>
    Answer: X - Y = 0010001 <br/>
    ![ssw_ch2_11](https://user-images.githubusercontent.com/63347989/136248992-f298472b-0446-4d90-8957-90c802a74cb0.PNG)

    - (b) 1's complement of X = 0101011<br/>
    Y - X = 1000011 + 0101011 = 1101110 → no end carry <br/>
    Answer: Y - X = -(1's complement of 1101110) = -0010001<br/>

# Signed number
How do we deal with Positive and Negative numbers? <br/>
Signed magnitude 방식과 Signed complement 방식이 있는데, <br/>
어떤 방식을 사용할지는 특정한 CPU의 고유한 특성이다. <br/>

## 컴퓨터 내부에서 이진수를 표현할 때의 조건
1. 이진수의 자리수 (n 비트) 
2. 부호가 있는 정수인지 없는 정수인지 
3. 어떤 방식으로 부호 있는 수를 표시할 것인지 (magnitude, complement, ...)

## 1. Signed magnitude convention
- Represent the sign(부호) with the leftmost bit<br/>
sign bit = 0 → 양수 (+ve number) <br/>
sign bit = 1 → 음수 (-ve number) <br/>
- If the binary number is __unsigned__ , 11001 → 25 <br/>
If the binary number is __signed__ , 11001 →  – 9 (01001 → +9)
- Need to know the representation in advance
    - Above is known as Signed-magnitude representation
    - Complement the sign bit and we get the same magnitude but with the opposite sign

## 2. Signed-complement system 
more convenient and used for –ve numbers<br/>
주어진 수의 보수를 구한 다음에, 이 보수를 원래 수의 음수 표현이라 정의하면 된다.<br/>
부호를 바꾸려면 보수를 구하기만 하면 된다. <br/>
- Can use 1’s or 2’s comp <br/>
2의 보수를 사용한 방식이 제일 나은 방식이다 → 대부분의 CPU가 이 방식을 많이 사용
- -ve numbers(음수) are indicated by their complement 
- 양수는 0으로 시작, 음수는 1로 시작
- Represent 9 in binary with 8 bits
    - +9 – 00001001 → only 1 way to represent <br/>
    - -9 - 3 ways to represent a –ve number <br/>
        - 10001001 → signed-magnitude <br/>
        - 11110110 → 1’s comp <br/>
        - 11110111 → 2’s comp<br/>
    
![ssw_ch2_12](https://user-images.githubusercontent.com/63347989/137001091-7dbd6713-d698-4f6e-99f1-d7ab4756c386.PNG)

# n비트로 표현할 수 있는 부호있는 수의 범위
- 2's complement <br/>
$$ -2^{n-1} ≤ N ≤ +(2^{n-1}-1) $$
<br/>

- 1's complement <br/>
$$ -(2^{n-1}-1) ≤ N ≤ +(2^{n-1}-1) $$
<br/>

- Signed magnitude <br/>
$$ -(2^{n-1}-1) ≤ N ≤ +(2^{n-1}-1) $$
<br/>

# 부호있는 수의 연산
## 1. 덧셈
2의 보수 이용하여 표현한 수의 연산 <br/>
단순히 더하면 되고, carry는 무시하면 됨 <br/>
-ve numbers in signed-complement form (2’s comp. Most commonly used)<br/>
Simple requires only addition no sign comparison etc.<br/>
All carries are discarded<br/>
If the answer is +ve DONE<br/>
If the answer is –ve, it is in 2’s comp form<br/>
To place in a more familiar form<br/>
Take 2’s comp of the –ve answer and place a ‘—’ in front of it, <br/>
ex) –7 above is in the 2’s comp form 11111001. Take the 2’s comp of this and you get 00000111, put ‘—’ in front<br/>
![ssw_ch2_13](https://user-images.githubusercontent.com/63347989/137002051-fc0a278d-d90a-4b7a-9ca1-b0bf55fdf8f7.PNG)


## 2. 뺄셈
N의 2의 보수를 M에 더함<br/>
Carry는 버리고 <br/>
최종답이 음수이면 2의 보수로 표현되어있다고 이해<br/>
Subtraction M – N <br/>
- Take 2’s comp of N<br/>
- Add to M<br/>
- Discard any carry out of the sign bit<br/>
- If the answer is –ve, it is in 2’s comp form<br/>
- Then follow same steps as before<br/>
![ssw_ch2_14](https://user-images.githubusercontent.com/63347989/137002710-fabfb66e-c325-42a9-b86b-04d497bd24c3.PNG)
<br/>

보수 가산에서 뺄셈도 덧셈과 같은 방식으로 연산이 이루어짐<br/>
CPU 안에 산술회로를 만들 때 덧셈과 뺼셈을 따로 만드는 것이 아니라 동일한 하드웨어로 구현됨<br/>
Since now we always end up adding, the same hardware can be used to do both addition and subtraction arithmetic ; The user/program must interpret the result correctly <br/>
