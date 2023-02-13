---
title: 모듈러 역원 - Modular Inverse
tag:
-math
-ps
---

## 유클리드 알고리즘 - Euclidean Algorithm

### 정의

두 양의 정수 $a,b\ (a<b)$에 대하여

$$b=aq_1+r_1\\a=r_1q_2+r_2\\r_1=r_2q_3+r_3\\\vdots\\r_{n-2}=r_{n-1}q_n+r_n\\r_{n-1}=r_nq_{n+1}$$

일때, $a,b$의 최대공약수는 $r_n$이다.<br>
즉, $gcd(a,b)=gcd(a,r_1)$이 성립한다.

### 증명

$G=gcd(a,b)$라고 하자.

그러면 $a=GA, b=GB$ $(A,B$는 서로소$)$가 성립하는데, 이를 $b=aq_1+r_1$에 대입하면 $GB=GAq_1+r_1$이 성립한다. 

이때 좌변이 $G$의 배수이므로 우변도 $G$의 배수여야한다. 즉, $r_1=R_1G$이 성립하고, 대입하여 정리하면 $B=Aq_1+R_1$이 됨을 알 수 있다.

따라서 $a,r_1$은 $G$를 공약수로 가진다. 이때 $G$가 $a,r_1$의 최대공약수임을 증명하기 위해서는 $a=AG, r_1=G(B-Aq_1)$에서 $A, (B-Aq_1)$이 서로소임을 보이면 된다.

$A, (B-Aq_1)$이 공약수 $k$를 가진다고 해보자. 즉, $A=mk, B-Aq_1=nk$이다. 이때, $A$를 대입하면 $B-mkq_1=nk$이고, $B=k(n+mq_1)$이 성립한다. 즉, $A,B$ 또한 공약수 $k$를 가지게 되어 서로소라는 조건에 위배되는 것이다! 

따라서 $A, (B-Aq_1)$은 서로소이고, 이에 따라 $gcd(a,b)=G=gcd(a,r_1)$임이 증명되었다.

### 코드

이는 재귀 함수를 이용하여 간단하게 표현할 수 있다.

```cpp
int gcd(int a, int b)
{
    return (b?gcd(b,a%b):a);
}
```
앞서 설명한 정의에 따라 $gcd(a,b)=gcd(a,r_1)=\cdots gcd(r_{n-1},r_n)$이 성립하는데, 이때 $r_{n-1}$을 $r_n$으로 나눈 나머지가 $0$이 되면 최대공약수가 $r_n$이 됨을 알 수 있다. 따라서 나머지가 $0$이 될때가지 재귀적으로 반복해주면 된다.

## 베주 항등식 - Bezout's Identity
### 정의

적어도 둘 중 하나가 0이 아닌 정수 $a,b$가 있고, $gcd(a,b)=d$ 일때 다음이 성립한다.
$$ax+by=kd; k,x,y\in Z$$

즉, $ax+by$의 형태로 표현될 수 있는 가장 작은 자연수는 $d$이고, 표현되는 모든 정수는 $d$의 배수이다.

### 증명

1\. $ax+by$의 형태로 표현될 수 있는 가장 작은 자연수가 존재한다.

집합 S를 다음과 같이 정의하자. 

$$S=\{m|m=ax+by>0, x,y\in Z\}$$

이때 $S\subset N$이고 

$$\begin{matrix}
|a|=a=a\times1+b\times0\in S & a>0\\
|a|=-a=a\times-1+b\times0\in S & a<0\\
b\in S & a=0,b\neq0\\
\end{matrix}$$

에 따라 $S\neq\varnothing$이므로 [정렬성의 원리](https://en.wikipedia.org/wiki/Well-ordering_principle)에 의해 최소 원소를 가지게 된다.

2\. 최소 원소가 $gcd(a,b)$이다.

$a,b$의 공약수를 $e$라고 하자. 따라서, $a=Ae, b=Be$가 성립한다. 

따라서 $d=ax+by=Aex+Bey=e(Ax+By)$이므로 $e$는 $d$의 약수이며, 이는 $a,b$의 모든 공약수가 $d$의 약수임과 동치이다. 즉 $d$는 최대공약수이다.

3\. 표현되는 모든 정수는 $d$의 배수이다.

$\forall z\in S$일 때, 다음이 성립한다.

$$z=dq+r\ (0\le r<d)\\ z=as+bt\ (s,t\in Z)$$

이때, 만약 $z$가 $d$의 배수가 아니라면 $r\neq0$이고

$$r=z-dq=(as+bt)-q(ax+by)=a(s-qx)+b(t-qy)\in S$$
인데, 이때 이는 $d$가 최소원소라는 조건에 모순이므로 $z$는 $d$의 배수이다.

<!--
## 디오판토스 방정식
>정의

정수 $a,b,c$에 대해 디오판토스 방정식 $ax+by=c$는 $gcd(a,b)=d|c$ 를 만족할 때

$$x=\frac{b}{d}k+x_0, y=-\frac{a}{d}k+y_0$$
형태의 일반해를 가진다. $(x_0,y_0$은 특이해(singular solution)$)$

>증명

특이해가 $x_0,y_0$일 때 다른 해를 $x', y'$라고 하자. 그러면 
$$ax_0+by_0=ax'+by'=c\\a(x'-x_0)=b(y_0-y')$$
가 성립한다.

이때 $a,b$의 최대공약수가 $d$이므로 $a=dt, b=ds$ $(t,s$는 서로소$)$라고 할 수 있다. 이를 통해 다음이 성립함을 알 수 있다.

$$a(x'-x_0)=b(y_0-y')\\ dt(x'-x_0)=ds(y_0-y')\\ t(x'-x_0)=s(y_0-y')\\ s|(x'-x_0),\ r|(y_0-y')\\ x'-x_0=sk,\ y_0-y'=rk\\ x'=x_0+sk,\ y'=y_0-rk$$

즉, 다시 정리하면 
$$x=x_0+\frac{b}{d}k,\ y=y_0-\frac{a}{d}k$$
이다.
-->

## 확장 유클리드 알고리즘 - Extended Euclidean Algorithm

### 정의

앞서 설명한 베주 항등식에서 계수 $a,b$가 주어졌을 때 $ax+by=d, d=gcd(a,b)$의 해를 구해주는 알고리즘이다.

### 과정

$ax+by=d$ 꼴에서 $a$를 $a=bq+r$로 표현해보자. 

$$(bq+r)x+by=d\\ b(qx+y)+rx=d\\ bx'+ry'=d$$

즉, 계수 $a,b$에 대한 해를 게수 $b,r$을 통해 구한 해를 이용하여 구할 수 있는 것이다.

$$x=y'\\ y=x'-qy'\\ q=\frac{a}{b}$$

유클리드 알고리즘과 마찬가지로 재귀적으로 반복되다가 나머지가 0이 될 때 종료된다. 이때, $ax+0\times y=d$에서 $a$가 최대공약수이므로 해는 $x=1,\ y=0$이 된다.

### 코드

```cpp
typedef pair<int,pair<int,int>> pip;

pip eGCD(int a, int b)
{
    if(!b) return {a,{1,0}};

    pip ret = eGCD(b,a%b);

    int d = ret.first;
    int xp = ret.second.first;
    int yp = ret.second.second; 

    return {d,{yp,xp-(a/b)*yp}};
}
```
## 모듈러 역원 - Modular Inverse
### 정의

우선, 항등원이란 무엇일까?

수학적인 정의를 찾아보자면 집합 $A$가 연산 $*$에 대해 닫혀있을 때 $A$의 임의의 원소 $a$에 대하여 $$a*e=e*a=a$$를 성립시키는 집합 $A$의 원소 $e$를 말한다.  

예를 들어 덧셈에 대한 항등원은 $0$, 곱셈에 대한 항등원은 $1$이다.

<br>이를 통해 역원을 정의할 수 있다.

항등원 $e$가 존재할 때 $A$의 어떤 원소 $a$에 대하여 

$$a*x=x*a=e$$

를 성립시키는 집합 $A$의 원소 $x$. 역원의 경우 연산에 대해 유일하게 존재하는 항등원과 다르게 여러개가 존재할 수도 있다.

예를 들어 덧셈에 대한 역원은 원소 $a$가 있을 때 $-a$가 된다.

<br>자, 그러면 모듈러 연산의 **곱셈**의 역원은 무엇일까?

바로 다음과 같이 정의된다.

$$a\times a^{-1}\equiv 1\;(mod\ m)$$

이때, 역원 $a^{-1}$은 $a,m$이 서로소 일때만 존재한다.

### 증명

1\. 역원은 $a,m$이 서로소 일때만 존재한다.

$a\times a^{-1}\equiv 1\;(mod\ m)$에서 $a\times a^{-1}=km+1$이라고 할 수 있는데, 이는 

$$ax+my=1$$ 

꼴의 베주 항등식으로 생각할 수 있다. 따라서 $gcd(a,m)=1$ 일때만 정수해를 가질 수 있다.

### 과정

단순하게 역원을 브루트포스로 구하게 된다면 

```cpp
for(int i=1;i<=m;i++)
{
    if((a*i)%m==1)
    {
        x=i;
        break;
    }
}
```
다음과 같이 시간복잡도가 $O(m)$이 걸리게 된다.

하지만 주어진 식을 다시 적어보면 

$$a\times a^{-1}+m\times(-k)=1$$

이고, 계수가 $a,m$인 베주 항등식이므로 앞서 설명한 확장 유클리드 알고리즘을 사용하면 더 빠르게 구할 수 있다.

그런데, 만약 $m$이 소수인 경우 [페르마의 소정리](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)를 이용하여 구할 수 있다! 이에 따라 $a,m$이 서로소라면 $$a^{m-1}\equiv 1\;(mod\ m)$$이 성립하고, $a^{m-1}=a\times a^{m-2}\equiv 1\;(mod\ m)$ 이므로 역원은 $a^{m-2}$가 된다.

### 코드
```cpp
typedef pair<int,pair<int,int>> pip;

pip eGCD(int a, int b)
{
    if(!b) return {a,{1,0}};

    pip ret = eGCD(b,a%b);

    int d = ret.first;
    int xp = ret.second.first;
    int yp = ret.second.second; 

    return {d,{yp,xp-(a/b)*yp}};
}

int inverseMod(int a, int m)
{
    pip ans = eGCD(a,m);
    if(ans.first!=1) return -1;
    return ans.second.first
    //or return (ans.second.first+m)%m;
}
```
