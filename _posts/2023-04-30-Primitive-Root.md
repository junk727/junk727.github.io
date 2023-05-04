---
title: '원시근' 
tag:
- 'math'
published: true
---

이번 글에서는 원시근, 이산로그, 이산제곱근 등에 대해서 알아보고자 한다.

* <https://rkm0959.tistory.com/187>

* <https://blog.naver.com/mindo1103/221234423247>

* <https://blog.naver.com/PostView.nhn?blogId=ssinznday&logNo=222310211660>

## 위수

자연수 $n$과 $\gcd(a,n)=1$을 만족하는 정수 $a$가 존재할 때, $a^k\equiv 1\pmod n$을 만족하는 최소의 양의 정수 $k$가 법 $n$에 대한 $a$의 위수이다. 그리고 이는 다음과 같이 표기한다.

$$\text{ord}_n(a)=k$$

예를 들어 보자면 $\text{ord}_8(3)$은 $3^2\equiv 1\pmod 8$이므로 $2$가 된다.

### 오일러 정리

$a,n$이 서로소인 양의 정수일 때 다음이 성립한다.

$$a^{\varphi(n)}\equiv 1\pmod n$$

잉여계를 통해 증명할 수 있다.

이때, 위수의 정의에 의해 $k$는 $\varphi(n)$보다 작거나 같은 자연수임을 알 수 있다. 또한 여기서 더 나아가 $\text{ord}_n(a) \ \vert \ \varphi(n)$임을 증명할 수 있다.

## 원시근

자연수 $n$에 대해 $\gcd(a,n)=1$을 만족하는 정수 $a$가 $\text{ord}_n(a)=\varphi(n)$을 만족하면 $a$를 법 $n$에 대한 **원시근**이라고 정의한다.

즉, 법 $n$에 대해서 위수가 $\varphi(n)$이 되는 정수 $a$를 말하는 것이다.

원시근은 다음과 같은 재미있는 성질들을 지닌다.

$k$를 법 $n$에 대한 $a$의 위수라고 할 때,

* $a^i\equiv a^j \pmod n  \Leftrightarrow i\equiv j \pmod k$

일반성을 잃지 않고 $i\geq j$라고 하면 $i=j$일 때는 자명하고, $i>j$일 때 $a^{i-j}\equiv 1\pmod n$이 된다. 이때, $k\ \vert \ i-j$이므로 $i\equiv j \pmod k$를 만족한다.

---

* $a,a^2,\cdots,a^k$는 법 $n$에서 서로 합동이 아니다.

$1\leq i \leq j \leq k$일때 $a^i\equiv a^j \pmod n$이면 $i\equiv j \pmod k$이므로 $i=j$이다. 따라서 $i\neq j$이면 $a^i\not\equiv a^j \pmod n$임은 증명되었다. 

* 즉, $a$가 법 $n$의 원시근이면 $\lbrace a,a^2,\cdots a^{\varphi(n)}\rbrace$은 법 $n$에 대한 기약잉여계이다.

---

* $\text{ord}_n(a)=k$이면 모든 자연수 $h$에 대해 $\text{ord}_n(a^h)=\cfrac{k}{\gcd(k,h)}$이다. 

$d=\gcd(k,h), r=\text{ord}_n(a^h)$이라고 하면 $\frac{h}{d}$는 자연수임은 자명하다. 이때 $(a^h)^{\frac{k}{d}}\equiv (a^k)^{\frac{k}{d}}\equiv 1 \pmod n$이 성립한다. 따라서 $r\ \vert \ \frac{k}{d}$이므로 $r\leq\frac{k}{d}$이다. 

또한 $(a^h)^r\equiv a^{hr} \equiv 1 \pmod n$이므로 $k\ \vert \ hr$, $\frac{k}{d}\ \vert \ (\frac{h}{d})\cdot r$인데 $\gcd(\frac{k}{d},\frac{h}{d})=1$이므로 $\frac{k}{d}\ \vert \ r$이다. 따라서 $\frac{k}{d}\leq r$이고, 또 $r\leq\frac{k}{d}$이므로 $r=\cfrac{k}{d}$이다.

---

앞서 증명한 사실들을 이용하면 법 $n$에서 원시근이 존재할 경우, 원시근의 개수를 구할 수 있다.

$a$가 법 $n$에 대한 원시근이라고 할 때, $\lbrace a,a^2,\cdots,a^{\varphi(n)}\rbrace$은 법 $n$에 대한 기약잉여계이므로, 또 다른 원시근 $b$가 존재한다면 $b$는 $a,a^2,\cdots,a^{\varphi(n)}$ 중 오직 하나의 정수와 법 $n$에 대해 합동이다.

이것이 뜻하는 바는 법 $n$에 대한 원시근은 $a,a^2,\cdots,a^{\varphi(n)}$에 존재한다는 것이다. 따라서 원시근의 개수는 1부터 $\varphi(n)$까지의 자연수 중 $\varphi(n)$과 서로소인 자연수의 개수와 같고, 이는 $\varphi(\varphi(n))$이다.

## 원시근의 존재성

원시근을 가질 수 있는 자연수는 

$$2,4,p,p^k,2p^k$$

꼴만 존재한다. 이때, $p$는 홀수 소수

### $p$=소수

모든 홀수 소수 $p$는 원시근을 가지며, 이때 원시근의 개수는 $\varphi(\varphi(p))=\varphi(p-1)$개이다.



## 이산로그

$$a^k\equiv r \pmod n$$

을 만족하는 

### 정의

### Baby Step Giant Step

### Pohlig-Hellman

## 이산제곱근

### 정의

### Adleman-Manders-Miller