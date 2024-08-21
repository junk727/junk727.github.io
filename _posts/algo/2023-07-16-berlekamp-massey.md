---
title: 'Berlekamp-Massey Algorithm' 
tag:
- 'math'
published: true
---
<!--

>체
* https://huiu-lee.tistory.com/8
* https://mathphysics.tistory.com/1154
* http://www.ktword.co.kr/test/view/view.php?m_temp1=3857
* https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=rollrat&logNo=221673145133
* https://mathworld.wolfram.com/FiniteField.html
* https://math.mit.edu/classes/18.783/2021/LectureNotes3.pdf

>흐름
* https://repository.lib.ncsu.edu/bitstream/handle/1840.4/8351/ISMS_1966_502.pdf?sequence=1&isAllowed=y
* http://crypto.stanford.edu/~mironov/cs359/massey.pdf
* https://en.wikipedia.org/wiki/Berlekamp%E2%80%93Massey_algorithm#cite_ref-3
* https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction
* http://neilsloane.com/doc/Me111.pdf

>BCH
* https://en.wikipedia.org/wiki/BCH_code#Decoding
* https://www.youtube.com/watch?v=16aggpH4Meg
* https://www.youtube.com/watch?v=UPvJ2J2qGRk
* http://www.ktword.co.kr/test/view/view.php?m_temp1=4161
* https://math.stackexchange.com/questions/358182/primitive-polynomials
* https://math.stackexchange.com/questions/3090351/find-the-elements-of-the-extension-field-using-primitive-polynomial-over-gf4

>Math
* https://math.berkeley.edu/~apaulin/AbstractAlgebra.pdf
* https://aerospacekim.tistory.com/53

/!-->

벌레캠프-매시 알고리즘 <s>어떻게 알고리즘 이름이 벌레캠프</s>, 아마 PS를 즐겨하다보면 한 번쯤은 들어보았을 알고리즘이다.

많은 사람들이 이를 주어진 수열로부터 가장 짧은 선형 점화식을 구하는 알고리즘이라고 알고 있을텐데, 이는 알고리즘이 가지는 의미의 반만 알고있는 것이다!

이번 글에서는 벌레캠프-매시 알고리즘의 과거와 현재, 미래를 모두 함께 살펴보자!

## tl'dr

암호학 분야에서 BCH 코드가 사용됨

-> Berlekamp가 BCH 코드를 디코딩하는 방법에 관한 논문을 발표함

-> Massey가 이 논문을 보더니, 어? 이거 사실 LFSR 찾는거랑 BCH 디코딩하는거랑 동치임!! 

-> 그래서 이거 야무지게 쌈싸서 선형점화식 찾는 알고리즘으로 확장!

-> 그리고 이게 동시대에 열심히 개발되던 Reed-solomon error correction의 발전에 한 몫 기여함!

-> 그런데 이걸 또 reed 랑 sloane이 보더니 모듈러 m 연산 취한 경우에도 확장할 수 있겠는데? 하면서 만든게 Reeds-sloane algorithm! 즉, 벌레캠프 매시의 확장판

## 개요

벌레캠프-매시 알고리즘의 역사는 통신 오류로부터 시작한다.

통신 오류란 무엇일까? 이어폰을 끼고 노래를 듣는데 갑자기 들리지 않는다던가, 엘리베이터를 타면 전화가 잘 되지 않는 것과 같이 말 그대로 통신을 하는데 오류가 생기는 것이다.

통신 오류를 완벽히 막는 것은 사실상 불가능하기에 우리가 해야할 일은 오류를 찾고, 고치는 작업이다.

대표적으로 데이터에 포함된 1의 개수가 홀수인지 짝수인지에 따라 데이터에 추가적으로 비트 하나를 더 붙여주는 Parity Bit 방식과, 2차원으로 Parity Bit를 적용하는 Parallel Parity Bit 방식이 있다.

그리고 또 하나 우리가 이 글에서 주목할 방법, 바로 BCH 코드이다.

## BCH 코드

BCH 코드, Bose-Chadhuri(<s>차두리</s>)-Hocquenghem Code의 줄임말로써 DVD, USB, 하드 드라이브 등 다양한 기기에 사용되고 있는 방법 중 하나이다.

여러개의 오류를 수정할 수 있고, 구현이 꽤나 간단하다는 장점을 지니고 있다.

### Prerequisties

암호학을 제대로 공부하기 위해서는 추상대수학의 꽃이라고 불리는 **체**에 대해서 잘 알고 있어야한다.

이 글에서는 필수적인 내용들만 최대한 간단하게 설명하고자 한다.

---

* 체

체는 대수적 구조 중 하나로써, 쉽게 설명하자면 덧셈과 곱셈 연산이 자유롭게 이루어질 수 있는 집합을 말한다. (이때, 뺄셈은 음수를 더하는 것으로, 나눗셈은 곱셈의 역원을 곱하는 것으로 볼 수 있다)

체는 우리에게 꽤나 익숙한 구조인데, 대표적인 예시로 유리수 집합 $\mathbb{Q}$, 실수 집합 $\mathbb{R}$, 복소수 집합 $\mathbb{C}$ 등이 있다.

* 유한체

체는 모두 무한집합이다. 그야 당연히 연산을 통해 새로운 원소를 계속해서 만들어 낼 수 있기 때문이다. 하지만, 체의 원소가 유한한 **유한체**를 만들 수 있는 방법이 있다.

바로 **모듈러 연산**이다. 특정 수로 나눈 나머지를 계산하여 나머지가 같은 경우 그들은 모두 동치인 것으로 취급하는 것이다. 이때, 그 수는 소수 $p$의 거듭제곱 꼴이다($p,p^2,\cdots,p^n$). 

그리고 이들은 다음과 같이 표기할 수 있다.

$$\text{GF}(p^n), \mathbb{F}_{p^n}, \text{GF}(q)$$

이때, $n>1$인 경우, $\text{GF}(p^n)$은 계수가 $\text{GF}(p)$의 원소인 다항식으로 표현할 수 있다.

더 자세한 내용은 조금 후에 밑에서 알아보자.

---

* 원시 원소 

유한체 $\text{GF}(q)$에 대해 $\alpha\in \text{GF}(q)$가 존재하여 $\alpha$의 거듭제곱으로 0을 제외한 유한체의 모든 원소를 표현할 수 있을 때 $\alpha$를 $\text{GF}(q)$**의 원시 원소**라고 부른다.

Q. $\text{GF}(5)$의 원시 원소는?

A. 2,3

\* $2, 2^2, 2^3\equiv 3, 2^4\equiv 1$

\* $3, 3^2\equiv 4, 3^3\equiv 2, 3^4\equiv 1$

---

* 원시 다항식

유한체 $\text{GF}(q)$에 대해 다항식 $p(x)$가 존재하여 $p(x)$의 모듈러 연산에 의한 원시 원소의 거듭제곱이 $\text{GF}(q)$를 구성할 때, $p(x)$를 $\text{GF}(q)$의 원시 다항식이라고 부른다.

말이 조금 어려워서 의미가 잘 안 와닿을 수 있는데, 다음 예시를 통해 이해해보자.

Q. $\text{GF}(2^3)$의 원시 다항식은?

A. $p(x)=x^3+x+1$

\* $\text{GF}(2^3)$의 원시 원소를 $\alpha$라고 하자. 부정원 $z$를 사용해서 $z=\alpha$라고 한다면 다음이 성립한다.

<center>

|$\alpha$의 거듭제곱|$\text{GF}(2^3)$의 원소|
|:----:|:----:|
|$\alpha^1$|$z$|
|$\alpha^2$|$z^2$|
|$\alpha^3$|$z+1$|
|$\alpha^4$|$z^2+z$|
|$\alpha^5$|$z^2+z+1$|
|$\alpha^6$|$z^2+1$|
|$\alpha^7$|$1$|

</center>

이때, 다항식의 계수들은 모두 $\text{GF}(2)$의 원소이므로 $z^3\equiv -z-1 \equiv z+1 \pmod {z^3+z+1}$이다. $\text{GF}(2)$에서 $1\equiv -1 \pmod 2$이기 때문이다!

이후 생성된 다항식 간의 연산이 잘 성립하므로, 원시원소로부터 체를 구성할 수 있음을 확인할 수 있다!

---

* 인수분해

$x^{q-1}-1$은 $\text{GF}(q)$의 0이 아닌 원소들 $\beta_1,\beta_2,\cdots,\beta_{q-1}$에 의해 다음과 같이 인수분해 될 수 있다.

$$x^{q-1}-1=(x-\beta_1)(x-\beta_2)\cdots(x-\beta_{q-1})$$

\* $\beta$는 원시원소 $\alpha$와 적당한 정수 $r$에 대해 $\beta=\alpha^r$로 표현될 수 있다.

따라서, 위 식에서 $x=\beta_k$를 대입하면 $\beta_k^{q-1}=(\alpha^r)^{q-1}=(\alpha^{q-1})^r=1\ (\because a^{q-1}=1$ by Fermat's Little Theorem $)$

\* 예시

$\text{GF}(5)$에서 0이 아닌 원소들은 $\lbrace 1,2,3,4 \rbrace$이다. 따라서 다음이 성립한다.

$$x^4-1=(x-1)(x-2)(x-3)(x-4)$$

---

* 최소 다항식

최소 다항식 $f(x)$는 앞서 인수분해한 식들을 바탕으로 특정 $\beta$값들을 넣고 원시 다항식으로 모듈러 연산을 했을 때 0이 되는 차수가 가장 작고, 계수가 $\text{GF}(p)$의 원소인 다항식을 말한다.

즉, 인수분해된 식에서 일부 항들끼리 모은 다항식이라고 보면 된다. 

이 또한 잘 안 와닿을 수 있는데, 예시를 통해 살펴보자.

Q. $\text{GF}(2^3)$ 위에서 원시 다항식이 $z^3+z+1$ 일때, 최소 다항식은?

<center>

|$\alpha^k$|$\text{GF}(2^3)$의 원소|최소 다항식|
|:----:|:----:|:----:|
|$\alpha^1$|$z$|$x^3+x+1$|
|$\alpha^2$|$z^2$|$x^3+x+1$|
|$\alpha^3$|$z+1$|$x^3+x^2+1$|
|$\alpha^4$|$z^2+z$|$x^3+x+1$|
|$\alpha^5$|$z^2+z+1$|$x^3+x^2+1$|
|$\alpha^6$|$z^2+1$|$x^3+x^2+1$|
|$\alpha^7$|$1$|$x+1$|

</center>

그리고 이를 바탕으로, $x^7-1=(x-\alpha)(x-\alpha^2)\cdots(x-\alpha^7)$을 다음과 같이 쓸 수 있다.

$$\begin{align*}
f_1(x)&=(x-\alpha)(x-\alpha^2)(x-\alpha^4)=x^3+x+1 \\
f_2(x)&=(x-\alpha^3)(x-\alpha^5)(x-\alpha^6)=x^3+x^2+1 \\
f_3(x)&=(x-\alpha^7)=x+1
\end{align*}$$

$$\therefore x^7-1=f_1(x)f_2(x)f_3(x)$$

---

* 생성 다항식

생성 다항식 $g(x)$는 앞서 구한 최소 다항식 $f_1(x),\cdots,f_k(x)$의 최소공배수이다.

$$g(x)=\text{LCM}(f_1(x),\cdots,f_k(x))$$

---

자, 이제 BCH 코드의 작동 원리를 이해하기 위해 필요한 사전지식들을 어느정도 알아본 것 같다. 

혹시 이와 관련된 내용이 더 궁금해진다면 <s>당신은 지금 추상대수학과 사랑에 빠진것이다!</s> 추상대수학 관련 내용들을 더 찾아보자.

### 인코딩 과정

기본적으로 BCH 코드와 같은 블록 부호에서는 다음과 같은 방식을 통해 부호화를 한다.

![block_code](../img/block_code_g.png#only-light)
![block_code](../img/block_code_b.png#only-dark)

그리고 이러한 방식을 간단히 $(n,k)$로 표기하기도 한다.

검출하고 싶은 오류의 개수를 $t$라 하고, 소수 $p$에 대해 적당한 $n=p^k-1$ 꼴의 정수 $n$을 잡자.

이때, 생성 다항식은 다음과 같이 주어진다.

$$g(x)=\text{LCM}(f_1(x),f_2(x),\cdots,f_{2t}(x))$$

그리고, $g(x)$의 차수가 $n-k$, 곧 검증 데이터의 길이로 정의한다.

예를 들어, $\text{GF}(16)$ 위에서 $t=1$인 경우, 생성 다항식은 

$$g(x)=\text{LCM}((x^2+x+2),(x^2+x+3))=x^4+x+1$$
 
이라고 할 수 있고, 이때, $\deg(g(x))=n-k=4$이므로 11 비트의 데이터를 4비트의 검증 비트와 함께 송신할 수 있다.

이후 송신하는 방법에는 두가지가 있다.

보내고자 하는 문자열 $p(x)$를 인자로 사용하는 경우 생성 다항식과의 곱 $s(x)=p(x)g(x)$를 보낸다.

또는, 문자열 $p(x)$를 하나의 접두사처럼 사용하는 경우 $p(x)$에 $x^{n-k}$만큼을 곱해서 차수를 밀어주고, $p(x)x^{n-k}$를 $g(x)$로 나눈 나머지 $r(x)$를 빼준다. 즉, $s(x)=p(x)x^{n-k}-r(x)$를 보낸다.

이때, 식에서 알 수 있듯이, $s(x)$는 $g(x)$의 배수이다.

위의 예시에 이어 $(15,11)$ BCH 코드를 통해 데이터 $p(x)=\lbrace 1,0,0,1,0,1,1,0,1,0,1\rbrace$를 송신하고자 하는 경우,

$$\begin{align*}
s(x)&=p(x)g(x)=(x^{10}+x^7+x^5+x^4+x^2+1)(x^4+x+1) \\ &=x^{14}+x^{10}+x^9+x^7+x^3+x^2+x+1 = \lbrace 1,0,0,0,1,1,0,1,0,0,0,1,1,1,1 \rbrace
\end{align*}$$

또는

$$\begin{align*}
s(x)&=p(x)x^{n-k}-r(x)=(x^{14}+x^{11}+x^9+x^8+x^6+x^4)-(x^2+x+1) \\ &=x^{14}+x^{11}+x^9+x^8+x^6+x^4+x^2+x+1 = \lbrace 1,0,0,1,0,1,1,0,1,0,1,0,1,1,1 \rbrace
\end{align*}$$

을 송신하면 된다. 보통 조금 더 편리한 두번째 방법을 많이 사용하는 편이다.

### 오증과 오류 다항식

이제 인코딩된 데이터를 받았을 때, 어떻게 디코딩 할 수 있는지 알아보자.

송신된 데이터를 $s(x)$, 수신된 데이터를 $r(x)$라고 하자.  이때, 우리가 검증할 수 있는 오류의 개수는 $0\leq \nu \leq t$개 이다. 

먼저, $g(x)$의 근$(\alpha,\cdots,\alpha^{n-k})$들을 $r(x)$에 대입함으로써 오류가 발생했는지 확인할 수 있다. 

$r(x)$에 대입하여 나온 값을 **오증**이라고 하며,위에서 보았듯이 $s(x)$는 $g(x)$의 배수이기 때문에, 오증이 0이 아닐 때 오류가 발생했음을 알 수 있다.

이를 바탕으로 오류가 생긴 위치가 $i_1,i_2,\cdots,i_{\nu}$일 때 오류 다항식 $e(x)$를 다음과 같이 정의할 수 있다.

$$e(x)=r(x)-s(x)=e_{i_1}x^{i_1}+\cdots+e_{i_{\nu}}x^{i_{\nu}}=\sum_{k=1}^{\nu}e_{i_k}x^{i_k}$$

이때, 오류가 발생한 위치 $X_k$와 오류 값 $Y_k$를 다음과 같이 정의할 수 있다.

$$\begin{align*}
X_k&=\alpha^{i_k} \\ Y_k&=e_{i_k}
\end{align*}$$

이로써 오증은 깔끔하게 다시 적을 수 있다.

$$S_j=r(\alpha^{j+1})=s(\alpha^{j+1})+e(\alpha^{j+1})=e(\alpha^{j+1}) \\ =\sum_{k=1}^{\nu}e_{i_k}\alpha^{i_k(j+1)} = \sum_{k=1}^{\nu}Y_kX_k^{j+1}$$

### 오류 위치 다항식

오류 위치 다항식 $\Lambda(x)$를 다음과 같이 정의하자.

$$\begin{align*} 
\Lambda(x)&=\prod_{k=1}^{\nu}(1-X_kx) \\ &=1+\Lambda_1x+\Lambda_2x^2+\cdots+\Lambda_{\nu}x^{\nu} 
\end{align*}$$

이때, 자명하게 $\Lambda(x)$는 $X_k^{-1}$을 해로 가지므로 

$$\Lambda(X_k^{-1})=1+\Lambda_1X_k^{-1}+\cdots+\Lambda_{\nu}X_k^{-\nu}=0$$

양변에 $Y_kX_k^{j+\nu+1}$을 곱해주면

$$Y_kX_k^{j+\nu+1}+\Lambda_1Y_kX_k^{j+\nu}+\cdots+\Lambda_{\nu}Y_kX_k^{j+1}=0$$

이를 $k=1$부터 $\nu$까지 더해주게 되면..

$$\begin{align*}
\sum_{k=1}^{\nu} \Lambda(X_k^{-1})Y_kX_k^{j+\nu+1} &= \sum_{k=1}^{\nu}Y_kX_k^{j+\nu+1}+\Lambda_1\sum_{k=1}^{\nu}Y_kX_k^{j+\nu}+\cdots+\Lambda_{\nu}\sum_{k=1}^{\nu}Y_kX_k^{j+1} \\ &= S_{j+\nu}+\Lambda_1S_{j+\nu-1}+\cdots+\Lambda_{\nu}S_j \\ &=0
\end{align*}$$

수학적인 내용들만 쭉 적어보았는데, 이들이 의미하는 것은 바로 오증 수열 $S_0,S_1,\cdots,S_{n-k-1}$이 오류 위치 다항식을 계수로 하는 선형 점화식을 만족한다는 것이다!!

정말 신기하지 않은가? 나는 개인적으로 이 부분에서 꽤나 큰 놀라움을 얻었다..!

## 벌레캠프 매시 알고리즘

이제 우리가 흔히 알고있는 벌레캠프 매시 알고리즘의 목적인, 수열의 최소 선형 점화식을 구하는 단계에 도착했다. 원래 벌레캠프는 $\nu$와 $\Lambda_k$를 구하는 알고리즘을 개발하였는데, 위에서 알아본 것처럼 이 과정이 곧 수열의 최소 선형 점화식을 찾는 것과 동치임을 매시가 증명한 것이다.

그러면 이제 본격적으로 점화식을 찾는 알고리즘을 살펴보자.

### 점화식의 오차

새롭게 점화식의 오차를 앞서 구한 식으로 정의할 것이다.

$$d=S_{j+\nu}+\Lambda_1S_{j+\nu-1}+\cdots+\Lambda_{\nu}S_j$$

이때, $d=0$인 경우에는 점화식을 성공적으로 찾았다고 말할 수 있다.

그러면 이제 $j$를 $0$부터 차례대로 증가시키면서 $d$가 $0$인지 아닌지를 판별하여 작업해주면 된다. 그런데 이때 주의해야 할 점이 바로 이전의 오증에 대해서는 여전히 값이 $0$으로 유지되면서 현재 오증을 고쳐야한다는 것이다.

그리고 이는 벌레캠프 매시의 핵심 식에 의해 해결된다.

$$\Lambda_{\text{new}}(x)=\Lambda(x)-dd_{\text{old}}^{-1}x^{j-f}\Lambda_{\text{old}}(x)$$

이때, $\Lambda_{\text{old}}(x)$는 이전에 사용했던 오류 위치 다항식을, $f$는 그 오류 위치 다항식에서 점화식을 만족하지 못한 처음 $j$값(최소값)을, $d_{\text{old}}$는 그 때의 오차 $d$를 의미한다.

위 식이 성립하는지 확인하려면 그냥 정의에 따라 대입해보면 된다.

$$\begin{align*}
d_{\text{new}} &= S_{j+\nu}+\Lambda_{\text{new1}}S_{j+\nu-1}+\cdots+\Lambda_{\text{new}\nu}S_{j} \\
&= S_{j+\nu}+\Lambda_1(x)S_{j+\nu-1}+\cdots+\Lambda_{\nu}S_j-dd_{\text{old}}^{-1}(S_{f+\nu}+\Lambda_{old1}S_{f+\nu-1}+\cdots+\Lambda_{\text{old}\nu}S_f) \\
&= d - dd_{\text{old}}^{-1}d_{\text{old}} \\
&= 0
\end{align*}$$

즉, 오류 위치 다항식을 수정함으로써 오증을 $0$으로 만들어 주었다! 그리고 이전 오증에 대해서는 위 식에서 $j$ 대신 $j-1$을, $f$ 대신 $f-1$을 넣어도 식은 여전히 성립하므로, 여전히 $0$을 유지하게 된다.

### $\Lambda_{\text{old}}(x)$

하지만 아직 이전 다항식, $\Lambda_{\text{old}}(x)$로 무엇을 골라야할지 결정되지 않았다. 물론 바로 전 다항식을 사용해도 되겠지만, 우리가 필요한 것은 **최소** 선형 점화식이기에, $\Lambda_{\text{new}}(x)$의 차수가 최소가 되도록 만들어주어야 한다.

아까 말했던 벌레캠프 매시의 핵심 식을 다시 보면 $\Lambda(x)$의 차수는 $\nu$이고, $\Lambda_{\text{old}}(x)$의 차수는 $\nu_{\text{old}}$이므로 $\Lambda_{\text{new}}(x)$의 차수는 $\max(\nu,j-f+\nu_{\text{old}})$임을 알 수 있다.

즉, 오증이 $0$이 아닌 경우가 나타나면, 새로운 오류 위치 다항식을 계산해준 뒤 $\nu$가 $j-f+\nu_{\text{old}}$보다 작은 경우에만 $\Lambda_{\text{old}}(x)$를 $\Lambda(x)$로 수정해주면 된다!

### 초기값

이제 마지막으로 초기의 오류 위치 다항식을 정해주어야 한다.

즉, 처음 $j_0$개의 오증 값이 모두 $0$이고, $S_{j_0}\neq 0$이면 자명하게 선형 점화식의 길이는 적어도 $j_0$ 보다는 커야한다. 즉, $\nu$의 초기값은 $j_0+1$이 되어야 한다. 또한 $\Lambda(x)$의 초기값은 어차피 추후에 자동으로 수정될 것이므로 아무렇게 해주어도 상관이 없으나, 간단하게 

$$\Lambda(x)=0+0x+\cdots+0x^{j_0+1}$$

정도로 두는게 편하다. 그리고 $\Lambda_{\text{old}}(x)=1$, $\nu_{\text{old}}=0$으로 두고 $f=j_0$에서 $d_{\text{old}}=S_{j_0}$가 생겼다고 두는게 자연스럽다.

이제 모든 준비를 끝마쳤고, 차례대로 $j$를 따라 수정을 거듭하게 되면 오증 수열에 대한 최소 선형 점화식을 구할 수 있게 된다!

## 코드

```cpp
const int mod = 998244353;
typedef long long ll;

ll ipow(ll x, ll p)
{
	ll ret = 1, piv = x;
	while(p)
    {
		if(p & 1) ret = ret * piv % mod;
		piv = piv * piv % mod; p >>= 1;
	}
	return ret;
}

vector<int> berlekamp_massey(vector<int> x){
	vector<int> ls, cur;
    //ls  : 이전 오류 위치 다항식
    //cur : 현재 오류 위치 다항식
	int lf, ld;
    //lf : 이전 오류 위치 다항식이 점화식을 만족하는데 실패한 첫 j값
    //ld : d_old

	for(int i=0; i<x.size(); i++)
    {
		ll t = 0; //오증
		for(int j=0; j<cur.size(); j++)
			t = (t + 1ll * x[i-j-1] * cur[j]) % mod;

        //오증이 0인 경우 Pass
		if((t - x[i]) % mod == 0) continue;

        //초깃값 설정
		if(cur.empty()){
			cur.resize(i+1);
			lf = i;
			ld = (t - x[i]) % mod;
			continue;
		}

        //-dd^-1 
        //유한체에서 나눗셈은 곱셈 역원을 곱해주는 것과 동치
		ll k = -(x[i] - t) * ipow(ld, mod - 2) % mod;
        
        //핵심 식에서 두번째 항 부분
		vector<int> c(i-lf-1);
		c.push_back(k);
		for(auto &j : ls) c.push_back(-j * k % mod);

        //크기 맞추기
		if(c.size() < cur.size()) c.resize(cur.size());

        //현재 오류 위치 다항식 수정
		for(int j=0; j<cur.size(); j++){
			c[j] = (c[j] + cur[j]) % mod;
		}

        //오류 위치 다항식 차수에 따라 수정 여부 결정
		if(i-lf+(int)ls.size()>=(int)cur.size()){
			tie(ls, lf, ld) = make_tuple(cur, i, (t - x[i]) % mod);
		}
		cur = c;
	}

	for(auto &i : cur) i = (i % mod + mod) % mod;
	return cur;
}

```

## 맺음말

이로써 벌레캠프 알고리즘의 전반적인 설명을 마친다. 이번 글을 통해 어쩌면 단순히 넘어갔을 벌레캠프 알고리즘을 재미있게 공부해 볼 수 있는 계기가 되었기를 바란다.

추가적으로 이러한 선형 점화식을 찾는 알고리즘에 더 관심이 생기는 사람은 '리드 슬론 알고리즘'과 '보스탄 모리 알고리즘'을 찾아본다면 궁금증을 해결하는데 큰 도움이 될 것이다!