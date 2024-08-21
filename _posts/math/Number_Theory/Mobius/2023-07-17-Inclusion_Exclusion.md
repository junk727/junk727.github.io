---
title: Inclusion-Exclusion
published: true
---

우선 이 글은 다음 블로그들을 참고하여 내가 공부했던 내용을 정리한 글이다!

* https://rkm0959.tistory.com/184

* https://xy-plane.tistory.com/16

# 포함 배제의 원리

일반적인 포함 배제 원리의 형태

$$|\cup_{i} A_i| = \sum_{i} |A_i| - \sum_{i< j} |A_i \cap A_j| + \sum_{i < j < k} |A_i \cap A_j \cap A_k| - \cdots  + (-1)^{n-1} | A_1 \cap A_2 \cap \cdots \cap A_n|$$

위 식을 이해하는 또 다른 방법은 바로 **원소별 기여**를 보는 것이다!

적당한 $x \in \cup_i A_i$를 잡고, $x$가 $A_1,\cdots,A_n$ 중 정확히 $k$개의 집합에만 속한다고 하자. 그러면 $x$가 존재함으로써 위 식에서 좌변은 1만큼 증가하고, 우변도 마찬가지로 1만큼 증가해야 할 것이다.

$\displaystyle \sum_i A_i$에 속하는 횟수는 $k$번, $\displaystyle \sum_{i<j} A_i \cap A_j$에 속하는 횟수는 $\binom{k}{2}$이고, 이를 일반화 시켜보면 우변에 추가되는 값들은 다음과 같이 표현할 수 있다!
$$\sum_{i=1}^{n} (-1)^{i-1}\binom{k}{i}$$

그런데 이항정리에 의해 $\displaystyle \sum_{i=0}^{n} (-1)^i\binom{k}{i}=\sum_{i=0}^{k}(-1)^i\binom{k}{i}=(1-1)^k=0$이므로 우변에 추가되는 값은 결국 $i=0$일 때인 **1**이다.

이를 더 확장시켜 부분집합이 적당한 함수에 의해 값을 가지는 경우, 즉 $S \sub U$에 대해 $f(s)\in \R$인 경우를 살펴보자. 그리고 부분집합 $A$에 대해 $\displaystyle g(A)=\sum_{S\sub A} f(S)$라고 정의하자.

우리가 증명하고자 하는 것은 $\displaystyle f(A)=\sum_{S\sub A}(-1)^{|A|-|S|}g(S)$이다.

위 식을 다시 정리하면 다음이 성립한다.

$$f(A)=\sum_{S\sub A}(-1)^{|A|-|S|}\sum_{T\sub S}f(T)=\sum_{T\sub S\sub A}(-1)^{|A|-|S|}f(T)$$

그리고 이는 $T=A$일 때 $f(T)$를 제외한 부분의 값은 $1$이므로 $f(A)$가 추가되고, $T\neq A$일 때는 다음을 계산해보면 된다.

$$\sum_{T\sub S\sub A}(-1)^{|A|-|S|}$$

이때, $|A|=m+n, |T|=m$이라고 하면 $|S|=m+k\ (0\leq k\leq n)$인 $S$는 $\binom{n}{k}$개가 존재할 것이다.  따라서, 위 식은 또 다시 변형이 가능하다.

$$\sum_{T\sub S\sub A}(-1)^{|A|-|S|}=\sum_{k=0}^{n}\binom{n}{k}(-1)^{n-k}=(1-1)^n=0$$

짜잔! 증명에 성공했다.

# The bridge

그런데 이놈이 위에서 나온 포함-배제의 원리와 어떤 연관성이 있는것인가..?에 대한 의문이 생길 수 있다. 그리고 이는 기존 포함 배제의 원리를 하나의 함수로 보고 일반화 시킨 것으로 간주할 수 있다!

즉 $f$를 다음과 같이 정의함으로써 연결고리를 만들 수 있다.

$A_1,A_2,\cdots,A_n$이 존재하고, $U=\lbrace 1,2,\cdots,n \rbrace$의 부분집합 $S$에 대해 $f(S)$는 속하는 집합$(A_i)$의 인덱스$(i)$의 모음이 $U-S$인 원소의 개수

즉, 예를 들어 $f({1})=\lvert A_1^C \cap A_2 \cap A_3 \rvert$이고, 전체집합에 대해 $f(U)=\lvert \cup_i A_i\rvert$이므로, 이는 대응을 시켜보면 기존 포함 배제의 원리와 동일한 식임을 확인할 수 있다.

# 곱셈적 함수 - Multiplicative Function

서로소인 두 정수의 곱셈을 보존하는 수론적 함수를 말한다!

$$f(ab)=f(a)f(b),\ when \gcd(a,b)=1$$

[여기를 참고](https://en.wikipedia.org/wiki/Multiplicative_function)

대표적인 함수들로는 $\tau,\sigma_k,\phi,\text{Id}$ 등이 있다.


# 뫼비우스 함수 - Mobius Function

$$\mu(n) \equiv \begin{cases} (-1)^{\lambda(n)} & (n \text{ is square-free integer}) \\ 0 & (\text{otherwise}) \end{cases}$$

square-free integer는 제곱 인수가 없는 정수, 즉 무승수를 뜻한다. 이 또한 두 수가 서로소인 경우 소인수가 겹치지 않아 소인수의 지수에 영향을 주지 않으므로 자명하게 곱셈적 함수가 된다.

# The Bridge 

새로운 함수 $\mu(X)=(-1)^{|X|}$라고 정의하자. 

포함 배제의 원리를 증명할 때 가장 핵심이 되는 부분은 다음이었다.

$$\sum_{T\sub S\sub A}(-1)^{|A|-|S|}=0, \quad T\subsetneq A$$

여기서 $T\sub S \subsetneq A$를 $A-S\sub A-T$로, $(-1)^{|A|-|S|}$를 $\mu(A-S)$로, $A-S$를 $X$로 치환해주면

$$\sum_{X\sub U}\mu(X)=0$$

이 된다! 이때 부연설명을 덧붙이면 $S\sub A$이기에 $|A|-|S|=|A-S|$가 성립하고, $T\subsetneq A$이므로 $A-T\neq\varnothing$이다.

그리고 더 나아가 $\mu(X)$를 $X$가 집합이 아니라 중복이 허용되는 multiset인 경우로 확장시켜볼 수 있다.

$X$가 multiset인 경우에는 그냥 $\mu(X)=0$으로 정의해주면 기존 증명에서의 $\sum_{X\sub U}\mu(X)=0$이 유지되므로 적절하다고 볼 수 있겠다.

# Mobius Function in Number Theory

이제 뫼비우스 함수를 정수론의 영역으로 조금 더 끌어올 수 있겠다. 자연수 $n$을 소인수 분해하여 $p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$와 같이 표기하고, 이를 다시 집합의 형태로 $\lbrace p_1,\cdots,p_2,\cdots,p_k\rbrace$로 표시하자. 그리고 집합의 포함 관계를 자연수의 나누어 떨어짐 관계로 해석한다면 그대로 집합 $n$에 이를 적용시킬 수 있다.

즉, 다음이 성립한다.

$$\sum_{d|n}\mu(d) = \begin{cases} 1 & (n = 1) \\ 0 & (n > 1) \end{cases}$$

* partial order(부분 순서 집합)에서는 뫼비우스 함수를 항상 정의할 수 있다?!
