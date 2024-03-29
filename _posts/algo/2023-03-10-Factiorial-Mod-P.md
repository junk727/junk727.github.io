---
title: 'N! mod P'
tag:
- 'math'
- 'ps'
---

이 글은 정말 유명한? 문제라고 할 수 있는 $\text{N}!\ \text{mod}\ p$ 문제를 다음 글을 바탕으로 공부한 내용을 정리하였다.

[N! mod p의 빠른 계산](https://infossm.github.io/blog/2019/09/17/fast-factorial-calculation/)

## $\text{N}!\ \text{mod}\ p$

당연히 Naive 하게 구현을 시도한다면 $O(n)$이 되겠지만, 조금 창의적인 발상을 시도하면 $O(nlogn)$에 구할 수 있다.

이를 이해하기 위해 라그랑주 보간법에 대한 사전지식이 필요하다. [다음](https://en.wikipedia.org/wiki/Lagrange_polynomial)을 참고하자.

## 라그랑주 보간법 - Lagrange Polynomial

$d$차 다항식은 다음과 같이 표현할 수 있다.

$$h(x)=a_0x^0+a_1x+\cdots+a_{d-1}x^{d-1}+a_dx^d$$

이때, 이를 기억하기 위해서는 차수 $a_0,a_1,\cdots,a_d$를 기억해도 되지만, $h(0),h(1),\cdots,h(d)$ 총 $d+1$개의 값을 기억하고 있어도 된다.

이는 많이 알려진 성질이므로 증명은 생략한다.

주어진 다항식에서 뽑아낸 값들을 바탕으로 다항식을 복원하는 식은 다음과 같다.

$$h(x)=\sum_{i=0}^{d} h(i) \prod_{j=0,j\neq i}^{d}\cfrac{x-j}{i-j}$$

하지만, 이 식을 전개해서 보면..

$$h(x)=\sum_{i=0}^{d} h(i) \cfrac{x(x-1)\cdots(x-i+1)(x-i-1)\cdots(x-d)}{i(i-1)\cdots 1\cdot(-1)\cdots (i-d+1)(i-d)}$$

이고, 이는 다시 정리하면 다음과 같다.

$$h(x)=\prod_{j=0}^{d}(x-j)\times \sum_{i=0}^{d}\cfrac{h(i)}{i!(d-i)!(-1)^{d-i}}\ \times \cfrac{1}{x-i}$$

식이 조금 무섭게 생겼지만 전개해서 재정리 한 것 뿐이므로 걱정하지 말자.

아무튼 앞부분은 쉽게 계산할 수 있는 형태이고, 뒷부분은 합성곱의 형태이므로 FFT/NTT를 사용해주면 된다. 

## Polynomial?

$N!$을 어떻게하면 더 빠르게 계산할 수 있을까?

바로, 이 다항식을 활용하는 것이다!

$$f_d(x)=(dx+1)(dx+2)\cdots(dx+d)$$

위 다항식의 대표적인 성질들은 다음과 같다.

* $f_N(0)=N!$  
1부터 $N$까지의 곱이다..
* $f_a(0)f_a(1)\cdots f_a(b-1)=f_{ab}(0)$  
전개해보면 $1$부터 $ab$까지의 곱이된다.
* $f_d(2x)f_d(2x+1)=f_{2d}(x)$  
전개해보면 동일하다.

그리고, 적당한 $v\approx \sqrt n$을 잡고 $f_v(0),f_v(1),\cdots,f_v(v)$를 계산하면 앞서 설명한 두번째 성질에 의해 $f_{v(v+1)}(0)=v(v+1)!$을 계산할 수 있다.

이후 남은 수들은 직접 곱해주면 된다.

시간 복잡도는 $O(v)=O(\sqrt n)$ 정도 걸리게 되는데, 이와 관련된 엄밀한 증명은 조금 더 공부한 후에 정리하도록 하겠다.

## Main Idea

앞서 설명한 라그랑주 보간법을 활용하여 $h(x)=f_d(x)$라고 하자. 그리고 $h(d+1),\cdots,h(4d+1)$을 구하면 $f_d(0),f_d(1),\cdots,f_d(4d+1)$을 구하는 것과 동치이고.

앞서 설명한 세번째 성질에 의해 $f_{2d}(0)=f_d(0)f_d(1),f_{2d}(1)=f_d(2)f_d(3),\cdots, f_{2d}(2d)=f_d(4d)f_d(4d+1)$이 된다. 

즉, $f_d$에서 $f_{2d}$를 구하는데 $O(d\ \text{log}d)$가 걸린다! 따라서 우리가 구하고자 하는 $d$까지 $O(d\ \text{log}d)$안에 구할 수 있고, $d=N$ 일때 $O(\sqrt n\ \text{log}N)$이다.

## 구현

2023-03-11
언제나 그렇듯, 이론과 구현은 별개의 영역이다..

히히 백준 숏코딩 3위 먹음