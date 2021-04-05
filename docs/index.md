# Describing Greedy Solution of Lights Out Puzzle Using Linear Algebra

<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$'], ['\\(', '\\)']],
    displayMath: [['$$', '$$'], ['\\[', '\\]']]
  },
  svg: {
    fontCache: 'global'
  }
};
</script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;300;400;500;700;900&display=swap" rel="stylesheet">
<link rel="stylesheet" href="//cdn.jsdelivr.net/font-nanum/1.0/nanumbarungothic/nanumbarungothic.css">
<style>
body,h1,h2,h3,h4,h5,h6{
    font-family: 'Noto Sans KR', Calibri, Helvetica, Arial, sans-serif;
}
</style>

사실 백준 [14939번 - 불 끄기](http://noj.am/14939) 문제를 보고 생각난거 적는 사이트입니다. 논문 아님

## 서론

### 1. Lights Out 퍼즐은 무엇인가

처음으로 만들어진 Lights Out 퍼즐은 5×5 보드 위에 각 칸마다 전구와 스위치가 하나씩 존재하고, 스위치를 누르면 해당 칸의 전구와 가로세로 방향으로 인접한 전구가 토글된다는 규칙이 있는 퍼즐이다. 퍼즐을 처음 시작하면 랜덤하게 켜진 전구와 꺼진 전구가 분포하며 위의 규칙대로 스위치를 눌러서 보드 위의 모든 불을 끄는 퍼즐이다.

### 2. Lights Out 퍼즐의 일반적인 해결법

#### 1) Light Chasing 방법을 사용한 해법

일반적인 해결법으로는 Light Chasing 방법이라는 그리디한 방법을 사용한다. 이 방법은 아래와 같은 과정으로 진행한다.

1. 가장 윗줄부터 켜진 불을 끈다. 불을 끌 때에는 해당 칸의 아래 스위치를 토글하여 불을 끈다.
1. 이 방법을 가장 윗줄부터 차례로 진행하면 가장 아랫줄에만 켜진 전구가 남는다.
1. 가장 아랫줄에 남아있는 전구의 형태에 따라 가장 윗줄의 몇 개의 전구를 킨다.
1. 가장 윗줄부터 1번~2번의 방법을 적용하면 남아있는 모든 전구가 꺼진다.

#### 2) 행렬을 사용한 해법

먼저 보드와 스위치를 누르는 경우를 각각 $\mathbf{b}$와 $\mathbf{x}$라는 벡터로 나타낸다. 이 때 벡터의 각 원소는 아래와 같은 칸의 전구 상태 / 스위치를 누르는 경우를 나타낸다.

<table>
<tr>
<td>$b_1$, $x_1$</td>
<td>$b_2$, $x_2$</td>
<td>$b_3$, $x_3$</td>
<td>$b_4$, $x_4$</td>
<td>$b_5$, $x_5$</td>
</tr>
<tr>
<td>$b_6$, $x_6$</td>
<td>$b_7$, $x_7$</td>
<td>$b_8$, $x_8$</td>
<td>$b_9$, $x_9$</td>
<td>$b_{10}$, $x_{10}$</td>
</tr>
<tr>
<td>$b_{11}$, $x_{11}$</td>
<td>$b_{12}$, $x_{12}$</td>
<td>$b_{13}$, $x_{13}$</td>
<td>$b_{14}$, $x_{14}$</td>
<td>$b_{15}$, $x_{15}$</td>
</tr>
<tr>
<td>$b_{16}$, $x_{16}$</td>
<td>$b_{17}$, $x_{17}$</td>
<td>$b_{18}$, $x_{18}$</td>
<td>$b_{19}$, $x_{19}$</td>
<td>$b_{20}$, $x_{20}$</td>
</tr>
<tr>
<td>$b_{21}$, $x_{21}$</td>
<td>$b_{22}$, $x_{22}$</td>
<td>$b_{23}$, $x_{23}$</td>
<td>$b_{24}$, $x_{24}$</td>
<td>$b_{25}$, $x_{25}$</td>
</tr>
</table>

그런데 보드의 상태는 켜져있거나 꺼져있는 상태이고 토글되며, 스위치도 2번 누르면 다시 원래 상태로 돌아오기 때문에 $\mathbf{b}$와 $\mathbf{x}$ 모두 $\mathbb{Z}_2^{25}$ 위의 벡터여야 할 것이다.

이제 Lights Out 퍼즐에서 스위치를 눌렀을 때 전구가 토글되는 경우 어떤 전구가 토글되는지를 생각해보면 아래와 같은 식을 세울 수 있다.

$$
\displaylines{
\mathbf{b}+A\mathbf{x}=0\\
\matrix{A}=\begin{pmatrix}Z&I&O&O&O\\I&Z&I&O&O\\O&I&Z&I&O\\O&O&I&Z&I\\O&O&O&I&Z\end{pmatrix}\\
Z=\begin{pmatrix}1&1&0&0&0\\1&1&1&0&0\\0&1&1&1&0\\0&0&1&1&1\\0&0&0&1&1\end{pmatrix}
}
$$

그리고 $\mathbf{b}$와 $\mathbf{x}$ 모두 $\mathbb{Z}_2^{25}$ 위의 벡터이므로 $A$는 $\mathbb{Z}_2^{25\times 25}$ 상의 행렬일 것이다. 그러면 다음의 식을 만족한다.

$$
\displaylines{
\mathbf{b}+A\mathbf{x}=\mathbf{b}+A\mathbf{x}-2A\mathbf{x}=\mathbf{b}-A\mathbf{x}=0\\
\mathbf{b}=A\mathbf{x}
}
$$

이제 $\mathbb{Z}_2^{25\times 25}$ 상에서의 $A$의 역행렬을 구하면 $\mathbf{x}=A^{-1}\mathbf{b}$의 식을 사용하여 어떤 스위치를 누르면 퍼즐을 해결할 수 있는지 구할 수 있다.

이제 여기서 한 가지의 의문점이 생긴다. 과연 $A$의 역행렬이 존재할까? Lights Out 퍼즐의 관점에서 다시 써보자면, 어떤 상태의 보드에 대해서도 해가 존재할까?

### 3. Lights Out 퍼즐은 아무 때나 해결할 수 있는가...를 알아보기 전에

$n\times m$ 보드에 대해 역행렬(정사각행렬이 아닌 경우 유사역행렬)의 존재성을 찾는 것은 곧 $(nm)×(nm)$ 크기의 행렬의 역행렬이 존재하는 것인지 찾는 것이기 때문에 매우 힘들다. 당장 10×10 보드라면 100×100 크기의 행렬의 역행렬의 존재 가능성을 판별해야 한다. 정사각행렬의 역행렬을 판별하는 과정은 [위키피디아](https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations#Matrix_algebra)에 따르면 일반적인 방법이 $O(n^3)$, 속도를 극대화한 알고리즘의 경우 $O(n^{2.373})$의 시간 복잡도를 가진다고 한다. 그렇다면 10×10 보드라면 $O((10\times 10)^3)$의 시간 복잡도를 필요로 한다. $n\times m$ 보드에 대하여는 $O((nm)^3)$의 시간 복잡도를 필요로 하게 된다. 따라서 문제를 쪼개서 필요한 연산을 줄일 필요가 있다. 이 방법을 찾는 것이 이 문서의 목표이다.

## 본론

### 1. Lights Out 퍼즐은 아무 때나 해결할 수 있는가

결론부터 말하자면 항상 해결할 수 있는 것은 아니다. 보드의 크기에 따라 항상 해결할 수 있는 경우도 있고 그렇지 않은 경우도 있다. 행렬 $A$의 역행렬이 존재하지 않아도 $\mathbf{b}=A\mathbf{x}$를 만족하는 $\mathbf{x}$가 존재할 수도 있다. 어떤 보드에 대해서 해가 존재하지 않는지, 그리고 해가 존재한다면 몇 개나 존재하는지도 같이 알아보도록 하겠다.

Lights Out 퍼즐의 난이도는 보드의 크기가 커질수록 매우 어려워진다. 따라서 가장 작은 버전의 퍼즐인 2×2 크기부터 시작하여 점점 크기를 늘려가면서 알아보겠다.

### 2. 2×2 보드

#### 1) Light Chasing 1단계(가장 아랫줄만 남기기)를 행렬로 옮기기

*서론에서와 같이 이 퍼즐을 나타내는 행렬과 벡터의 원소는 모두 $\mathbb{Z}_2$의 성질을 따르기로 한다.*

먼저 변수의 index는 아래와 같이 나타내기로 한다.

<table>
<tr>
<td>$b_1$, $x_1$</td>
<td>$b_2$, $x_2$</td>
</tr>
<tr>
<td>$b_3$, $x_3$</td>
<td>$b_4$, $x_4$</td>
</tr>
</table>

이제 Light Chasing 방법을 사용하는 것을 행렬로 나타내보겠다. 먼저 가장 윗줄($b_1$과 $b_2$)에 불이 켜져있다면 각각에 대해 $x_3$와 $x_4$를 토글한다. 그 과정을 나타내보면 아래와 같이 된다.

$$
\displaylines{
\mathbf{b}_0=\mathbf{b}\\
\mathbf{b}_1=\mathbf{b}_0+A\mathbf{x}_1\\
\begin{pmatrix}b_{11}\\b_{12}\\b_{13}\\b_{14}\end{pmatrix}=\begin{pmatrix}b_{01}\\b_{02}\\b_{03}\\b_{04}\end{pmatrix}+\begin{pmatrix}Z_2&I_2\\I_2&Z_2\end{pmatrix}\begin{pmatrix}0\\0\\b_{01}\\b_{02}\end{pmatrix}=\begin{pmatrix}b_{01}+b_{01}\\b_{02}+b_{02}\\b_{03}+b_{01}+b_{02}\\b_{04}+b_{01}+b_{02}\end{pmatrix}=\begin{pmatrix}0\\0\\b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\end{pmatrix}\\
Z_2=\begin{pmatrix}1&1\\1&1\end{pmatrix},I_2=\begin{pmatrix}1&0\\0&1\end{pmatrix}
}
$$

이 과정을 통해 보드의 아랫줄에만 켜진 전구가 남게 되었다. 켜진 전구의 조합에 따라 가장 윗줄의 스위치를 토글하는데, 어떤 것을 토글해야 하는지는 이제 구해야 한다.

#### 2) Light Chasing 2단계(가장 윗줄 토글해서 아랫줄 지우기)를 행렬로 옮기기

가장 윗줄의 스위치($x_1$, $x_2$)를 적당히 토글한 후 1단계와 같은 방법을 다시 사용하면 가장 아랫줄의 켜진 전구도 모두 끌 수 있다. 어떤 것은 누르고 어떤 것은 누르지 않을지는 다음 식을 통해 구한다. 이제 가장 윗줄의 스위치 중 가장 왼쪽의 것($x_1$)을 토글하고 1단계 방법을 다시 적용하는 과정을 나타내보겠다.

$$
\displaylines{
\mathbf{b}_{1,0}=A\begin{pmatrix}1&0&0&0\end{pmatrix}^T=\begin{pmatrix}1&1&1&0\end{pmatrix}^T\\
\mathbf{b}_{1,1}=\mathbf{b}_{1,0}+A\mathbf{x}_{1,1}=\begin{pmatrix}1\\1\\1\\0\end{pmatrix}+\begin{pmatrix}Z_2&I_2\\I_2&Z_2\end{pmatrix}\begin{pmatrix}0\\0\\1\\1\end{pmatrix}=\begin{pmatrix}0\\0\\1\\0\end{pmatrix}
}
$$

이 때 누른 모든 스위치를 $\chi_1$, 남는 보드를 $\mathbf{b}_{1,f}$라고 써보면 아래와 같은 식을 만족한다.

$$
\displaylines{
\chi_1=\begin{pmatrix}1&0&0&0\end{pmatrix}^T+\mathbf{x}_1=\begin{pmatrix}1&0&1&1\end{pmatrix}^T\\
\mathbf{b}_{1,f}=\mathbf{b}_{1,1}=\begin{pmatrix}0&0&1&0\end{pmatrix}^T
}
$$

마찬가지의 방법으로 $\chi_2$와 $\mathbf{b}_{2,f}$를 구해보면 아래와 같다.

$$
\displaylines{
\chi_2=\begin{pmatrix}0&1&1&1\end{pmatrix}^T\\
\mathbf{b}_{2,f}=\mathbf{b}_{2,1}=\begin{pmatrix}0&0&0&1\end{pmatrix}^T
}
$$

그리고 $\mathbf{b}_{i,f}$와 $\chi_i$는 아래와 같은 관계를 만족한다.

$$
\mathbf{b}_{i,f}=A\chi_i
$$

이제 구한 $\mathbf{b}_{i,f}$와 $\chi_i$를 사용하여 최종 해를 구하는 과정을 기술해보겠다.

#### 3) Light Chasing 3단계(최종해 구하기)를 행렬로 옮기기

1단계를 진행한 후 남은 보드에는 가장 아랫줄만 남아있다. 이제 여기에 $\mathbf{b}_{i,f}$를 적당히 더하면 보드의 전구가 모두 꺼지는 경우가 있을 것이다. 식으로 나타내면 아래와 같다.

$$
\displaylines{
\mathbf{b}_f+c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}=0\\
\mathbf{b}_f=c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}
}
$$

그리고 이 때 사용하는 스위치는 아래와 같이 구할 수 있다.

$$
\displaylines{
\mathbf{b}_f=c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}\\
\mathbf{b}_f=A\mathbf{x}_f\\
c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}=c_1A\chi_1+c_2A\chi_2\\
\therefore\mathbf{x}_f=c_1\chi_1+c_2\chi_2
}
$$

이제 1단계에서 사용했던 관계식인 $\mathbf{b}_1=\mathbf{b}_0+A\mathbf{x}_1$을 사용하면 아래와 같이 유도할 수 있다.

$$
\displaylines{
A\mathbf{x}=\mathbf{b}_0=\mathbf{b}_1+A\mathbf{x}_1=A\mathbf{x}_1+c_1A\chi_1+c_2A\chi_2\\
\mathbf{x}=\mathbf{x}_1+c_1\chi_1+c_2\chi_2
}
$$

위의 식에서 구하지 않은 상태로 남아있는 것은 이제 $c_1$과 $c_2$뿐이다. 이 상수는 새로운 행렬 방정식 하나를 풀면 구할 수 있다. 그 전에 새로운 변수 $\beta_i$를 선언한다. $\beta_i$는 $\mathbf{b}_{i,f}$의 아래부터 2개의 원소를 원소로 가지는 벡터이다. 그러면 $\beta_i$는 아래와 같은 관계를 만족한다.

$$
\begin{pmatrix}b_{13}\\b_{14}\end{pmatrix}=c_1\beta_1+c_2\beta_2
$$

이를 조금 변형시켜서 쓰면 아래와 같이 된다.

$$
\begin{pmatrix}b_{13}\\b_{14}\end{pmatrix}=\begin{pmatrix}\beta_1&\beta_2\end{pmatrix}\begin{pmatrix}c_1\\c_2\end{pmatrix}=B\mathbf{c}
$$

이를 만족하는 $\mathbf{c}$를 찾아서 아래의 식에 대입하면 해가 나온다.

$$
\mathbf{x}=\mathbf{x}_1+X\mathbf{c}(X=\begin{pmatrix}\chi_1&\chi_2\end{pmatrix})
$$

#### 4) 검산

이제 위의 식을 실제 값으로 바꾸어 계산해보겠다.

$$
\displaylines{
B=\begin{pmatrix}\beta_1&\beta_2\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}\\
\begin{pmatrix}b_{13}\\b_{14}\end{pmatrix}=B\mathbf{c}=I_2\mathbf{c}\\
\therefore\mathbf{c}=\begin{pmatrix}b_{13}\\b_{14}\end{pmatrix}\\
X=\begin{pmatrix}\chi_1&\chi_2\end{pmatrix}=\begin{pmatrix}1&0\\0&1\\1&1\\1&1\end{pmatrix}\\
\mathbf{x}=\mathbf{x}_1+X\mathbf{c}=\begin{pmatrix}0\\0\\b_{01}\\b_{02}\end{pmatrix}+\begin{pmatrix}b_{13}\\b_{14}\\b_{13}+b_{14}\\b_{13}+b_{14}\end{pmatrix}\\
=\begin{pmatrix}0\\0\\b_{01}\\b_{02}\end{pmatrix}+\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\\b_{03}+b_{04}\\b_{03}+b_{04}\end{pmatrix}=\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\\b_{01}+b_{03}+b_{04}\\b_{02}+b_{03}+b_{04}\end{pmatrix}\\
=\begin{pmatrix}1&1&1&0\\1&1&0&1\\1&0&1&1\\0&1&1&1\end{pmatrix}\mathbf{b}
}
$$

실제로 $\mathbb{Z}_2$ 위에서의 $A$의 역행렬을 구해보면 $A^{-1}=A$이다. 따라서 $\mathbf{x}=A^{-1}\mathbf{b}=A\mathbf{b}$이므로 위와 같이 구해도 원래와 같은 해를 구할 수 있음을 알 수 있다. 하지만 기존 방법과 다른 점이라면 4×4 크기의 행렬인 A가 포함된 방정식을 풀지 않고 2×2 크기의 행렬인 $B$가 포함된 방정식을 풀어서 해를 구했다는 점이다.

#### 5) 요약

과정을 다시 정리해보면 아래와 같이 된다.

##### (1) 초기 상태

$$\mathbf{b}_0=\mathbf{b}$$

##### (2) 가장 아랫줄만 남은 상태

$$
\displaylines{
\mathbf{b}_f=\mathbf{b}_1=\mathbf{b}_0+A\mathbf{x}_1\\
\mathbf{x}_1=\begin{pmatrix}0&0&b_{01}&b_{02}\end{pmatrix}^T\\
\mathbf{b}_f=\mathbf{b}_0+A\begin{pmatrix}0&0&b_{01}&b_{02}\end{pmatrix}^T\\
=\begin{pmatrix}0&0&b_{01}+b_{02}+b_{03}&b_{01}+b_{02}+b_{04}\end{pmatrix}^T
}
$$

##### (3) 아랫줄에 남은 전구에 따라 눌러야 하는 스위치 구하기

$$
\displaylines{
\mathbf{b}_{1,0}=A\begin{pmatrix}1&0&0&0\end{pmatrix}^T=\begin{pmatrix}1&1&1&0\end{pmatrix}^T\\
\mathbf{b}_{1,f}=\mathbf{b}_{1,1}=A\begin{pmatrix}0&0&1&1\end{pmatrix}^T+\mathbf{b}_{1,0}=\begin{pmatrix}0&0&1&0\end{pmatrix}^T\\
\beta_1=\begin{pmatrix}b_{1,f3}&b_{1,f4}\end{pmatrix}^T=\begin{pmatrix}1&0\end{pmatrix}^T\\
\chi_1=\begin{pmatrix}1&0&1&1\end{pmatrix}^T\\
\mathbf{b}_{2,f}=\mathbf{b}_{2,1}=\begin{pmatrix}0&0&0&1\end{pmatrix}^T\\
\beta_2=\begin{pmatrix}b_{2,f3}&b_{2,f4}\end{pmatrix}^T=\begin{pmatrix}0&1\end{pmatrix}^T\\
\chi_2=\begin{pmatrix}0&1&1&1\end{pmatrix}^T\\
B=\begin{pmatrix}\beta_1&\beta_2\end{pmatrix}\\
X=\begin{pmatrix}\chi_1&\chi_2\end{pmatrix}
}
$$

##### (4) 최종해 구하기

$$
\displaylines{
\beta_f=\begin{pmatrix}b_{f3}&b_{f4}\end{pmatrix}^T=B\mathbf{c}=\mathbf{c}\\
\mathbf{b}=\mathbf{b}_1+A\mathbf{x}_1\\
A\mathbf{x}=AX\mathbf{c}+A\mathbf{x}_1\\
\mathbf{x}=X\mathbf{c}+\mathbf{x}_1\\
X\mathbf{c}=X\beta_f=\begin{pmatrix}1&0\\0&1\\1&1\\1&1\end{pmatrix}\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\end{pmatrix}=\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\\b_{03}+b_{04}\\b_{03}+b_{04}\end{pmatrix}\\
\mathbf{x}=\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\\b_{03}+b_{04}\\b_{03}+b_{04}\end{pmatrix}+\begin{pmatrix}0\\0\\b_{01}\\b_{02}\end{pmatrix}=\begin{pmatrix}b_{01}+b_{02}+b_{03}\\b_{01}+b_{02}+b_{04}\\b_{01}+b_{03}+b_{04}\\b_{02}+b_{03}+b_{04}\end{pmatrix}\\
=\begin{pmatrix}1&1&1&0\\1&1&0&1\\1&0&1&1\\0&1&1&1\end{pmatrix}\begin{pmatrix}b_{01}\\b_{02}\\b_{03}\\b_{04}\end{pmatrix}=A\mathbf{b}_0=A\mathbf{b}
}
$$

### 3. 3x3 보드

*역시 여기서도 모든 변수는 $\mathbb{Z}_2$에 속한다.*

*새로운 기호 $\displaystyle{\sum_{\mathbf{b}} m(1,4,5,...)}$를 제시한다. 의미는 벡터 $\mathbf{b}$의 1번째, 4번째, 5번째, ... 의 원소의 합이다. (카르노 맵에서 사용하는 기호에서 가져왔다)*

#### 1) 1단계

변수의 index는 아래와 같이 나타낸다.

<table>
<tr>
<td>$b_1$, $x_1$</td>
<td>$b_2$, $x_2$</td>
<td>$b_3$, $x_3$</td>
</tr>
<tr>
<td>$b_4$, $x_4$</td>
<td>$b_5$, $x_5$</td>
<td>$b_6$, $x_6$</td>
</tr>
<tr>
<td>$b_7$, $x_7$</td>
<td>$b_8$, $x_8$</td>
<td>$b_9$, $x_9$</td>
</tr>
</table>

이제 Light Chasing 방법을 사용한다. 가장 윗줄($b_1$, $b_2$, $b_3$)에 불이 켜져있다면 각각에 대해 $x_4$, $x_5$, $x_6$를 토글한다. 그 과정을 나타내보면 아래와 같이 된다.

$$
\displaylines{
\mathbf{b}_0=\mathbf{b}\\
\mathbf{b}_1=\mathbf{b}_0+A\mathbf{x}_1\\
\mathbf{b}_1=\mathbf{b}_0+\begin{pmatrix}Z_3&I_3&O_3\\I_3&Z_3&I_3\\O_3&I_3&Z_3\end{pmatrix}\begin{pmatrix}0\\0\\0\\b_{01}\\b_{02}\\b_{03}\\0\\0\\0\end{pmatrix}\\
=\begin{pmatrix}b_{01}+b_{01}\\b_{02}+b_{02}\\b_{03}+b_{03}\\b_{04}+b_{01}+b_{02}\\b_{05}+b_{01}+b_{02}+b_{03}\\b_{06}+b_{02}+b_{03}\\b_{07}+b_{01}\\b_{08}+b_{02}\\b_{09}+b_{03}\end{pmatrix}=\begin{pmatrix}0\\0\\0\\\sum_{\mathbf{b}_0} m(1,2,4)\\\sum_{\mathbf{b}_0} m(1,2,3,5)\\\sum_{\mathbf{b}_0} m(2,3,6)\\\sum_{\mathbf{b}_0} m(1,7)\\\sum_{\mathbf{b}_0} m(2,8)\\\sum_{\mathbf{b}_0} m(3,9)\end{pmatrix}\\
Z_3=\begin{pmatrix}1&1&0\\1&1&1\\0&1&1\end{pmatrix},I_3=\begin{pmatrix}1&0&0\\0&1&0\\0&0&1\end{pmatrix},O_3=\begin{pmatrix}0&0&0\\0&0&0\\0&0&0\end{pmatrix}
}
$$

이 과정을 통해 보드의 가장 윗줄을 지우고 아래 두 개의 줄만 켜진 전구가 남게 되었다. 이제 두 번째 줄에 대해 Light Chasing을 한 번 더 진행해야 한다.

$$
\displaylines{
\mathbf{b}_2=\mathbf{b}_1+A\mathbf{x}_2\\
\mathbf{x}_2=\begin{pmatrix}0&0&0&0&0&0&b_{14}&b_{15}&b_{16}\end{pmatrix}^T\\
=\begin{pmatrix}0&0&0&0&0&0&\sum_{\mathbf{b}_0} m(1,2,4)&\sum_{\mathbf{b}_0} m(1,2,3,5)&\sum_{\mathbf{b}_0} m(2,3,6)\end{pmatrix}^T\\
\mathbf{b}_2=\begin{pmatrix}0\\0\\0\\0\\0\\0\\b_{14}+b_{15}+b_{17}\\b_{14}+b_{15}+b_{16}+b_{18}\\b_{15}+b_{16}+b_{19}\end{pmatrix}=\begin{pmatrix}0\\0\\0\\0\\0\\0\\\sum_{\mathbf{b}_0} m(1,3,4,5,7)\\\sum_{\mathbf{b}_0} m(4,5,6,8)\\\sum_{\mathbf{b}_0} m(1,3,5,6,9)\end{pmatrix}
}
$$

즉, 1단계를 진행한 결과는 다음과 같다.

$$
\displaylines{
\mathbf{b}_f=\mathbf{b}+A\mathbf{x}_f
\mathbf{b}_f=\mathbf{b}_2=\mathbf{b}_1+A\mathbf{x}_2=\mathbf{b}_0+A\mathbf{x}_1+A\mathbf{x}_2=\mathbf{b}+A(\mathbf{x}_1+\mathbf{x}_2\\
\mathbf{x}_{+}=\mathbf{x}_1+\mathbf{x}_2=\begin{pmatrix}0\\0\\0\\b_{01}\\b_{02}\\b_{03}\\\sum_{\mathbf{b}_0} m(1,2,4)\\\sum_{\mathbf{b}_0} m(1,2,3,5)\\\sum_{\mathbf{b}_0} m(2,3,6)\end{pmatrix}
}
$$

#### 2) 2단계

2×2 보드에서 했던 것과 같이 가장 윗줄의 스위치 중 가장 왼쪽의 것($x_1$)을 토글하고 1단계 방법을 다시 적용하는 과정을 나타내보겠다.

$$
\displaylines{
\mathbf{b}_{1,0}=A\begin{pmatrix}1&0&0&0&0&0&0&0&0\end{pmatrix}^T\\
=\begin{pmatrix}1&1&0&1&0&0&0&0&0\end{pmatrix}^T\\
\mathbf{b}_{1,1}=\mathbf{b}_{1,0}+A\mathbf{x}_{1,1}=\begin{pmatrix}1\\1\\0\\1\\0\\0\\0\\0\\0\end{pmatrix}+\begin{pmatrix}Z_3&I_3&O_3\\I_3&Z_3&I_3\\O_3&I_3&Z_3\end{pmatrix}\begin{pmatrix}0\\0\\0\\1\\1\\0\\0\\0\\0\end{pmatrix}=\begin{pmatrix}0\\0\\0\\1\\0\\1\\1\\1\\0\end{pmatrix}\\
\mathbf{b}_{1,2}=\mathbf{b}_{1,1}+A\mathbf{x}_{1,2}=\mathbf{b}_{1,1}+A\begin{pmatrix}0&0&0&0&0&0&1&0&1\end{pmatrix}^T\\
=\begin{pmatrix}0&0&0&0&0&0&0&1&1\end{pmatrix}^T
}
$$

이 때 누른 모든 스위치를 $\chi_1$, 남는 보드를 $\mathbf{b}_{1,f}$라고 써보면 아래와 같은 식을 만족한다.

$$
\displaylines{
\chi_1=\begin{pmatrix}1&0&0&0&0&0&0&0&0\end{pmatrix}^T+\mathbf{x}_1+\mathbf{x}_2\\
=\begin{pmatrix}1&0&0&1&1&0&1&0&1\end{pmatrix}^T\\
\mathbf{b}_{1,f}=\mathbf{b}_{1,2}=\begin{pmatrix}0&0&0&0&0&0&0&1&1\end{pmatrix}^T
}
$$

마찬가지의 방법으로 다른 $\chi_i$와 $\mathbf{b}_{i,f}$를 구해보면 아래와 같다.

$$
\displaylines{
\mathbf{b}_{2,f}=\mathbf{b}_{2,2}=\begin{pmatrix}0&0&0&0&0&0&1&1&1\end{pmatrix}^T\\
\chi_2=\begin{pmatrix}0&1&0&1&1&1&0&0&0\end{pmatrix}^T\\
\mathbf{b}_{3,f}=\text{x-flip}(\mathbf{b}_{1,f})=\begin{pmatrix}0&0&0&0&0&0&1&1&0\end{pmatrix}^T\\
\chi_3=\text{x-flip}(\chi_1)=\begin{pmatrix}0&0&1&0&1&1&1&0&1\end{pmatrix}^T
}
$$

$\mathbf{b}_{3,f}$와 $\chi_3$의 경우 1번 스위치를 누르는 경우와 거울상이므로 단순히 가로 방향으로 뒤집은 값으로 해도 계산한 결과와 똑같이 나온다.

그리고 2×2 때와 마찬가지로 $\mathbf{b}_{i,f}$와 $\chi_i$는 아래와 같은 관계를 만족한다.

$$
\mathbf{b}_{i,f}=A\chi_i
$$

#### 3) 3단계

1단계를 진행한 후 남은 보드($\mathbf{b}$)와 $\mathbf{b}_{i,f}$에 대해 아래의 식을 만족하는 $c_i$가 있다고 하고 식을 써보겠다.

$$
\displaylines{
\mathbf{b}_f+c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}+c_3\mathbf{b}_{3,f}=0\\
\mathbf{b}_f=c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}+c_3\mathbf{b}_{3,f}
}
$$

그리고 이 때 사용하는 스위치는 아래와 같다.

$$
\displaylines{
\mathbf{b}_f=c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}+c_3\mathbf{b}_{3,f}\\
\mathbf{b}_f=A\mathbf{x}_f\\
c_1\mathbf{b}_{1,f}+c_2\mathbf{b}_{2,f}+c_3\mathbf{b}_{3,f}=c_1A\chi_1+c_2A\chi_2+c_3A\chi_3\\
\therefore\mathbf{x}_f=c_1\chi_1+c_2\chi_2+c_3\chi_3
}
$$

이제 $\mathbf{b}_2$에 대한 관계식을 사용하여 해를 구할 수 있다.

$$
\displaylines{
A\mathbf{x}=\mathbf{b}_0=\mathbf{b}_2+A\mathbf{x}_{+}=A\mathbf{x}_{+}+c_1A\chi_1+c_2A\chi_2+c_3A\chi_3\\
\mathbf{x}=\mathbf{x}_{+}+c_1\chi_1+c_2\chi_2+c_3\chi_3
}
$$

이제 2×2 때와 같이 변수 $\beta_i$를 선언한다. $\beta_i$는 $\mathbf{b}_{i,f}$의 아래부터 3개의 원소를 원소로 가지는 벡터이다. 그러면 $\beta_i$는 아래와 같은 관계를 만족한다.

$$
\begin{pmatrix}b_{27}\\b_{28}\\b_{29}\end{pmatrix}=c_1\beta_1+c_2\beta_2+c_3\beta_3\\
=\begin{pmatrix}\beta_1&\beta_2&\beta_3\end{pmatrix}\begin{pmatrix}c_1\\c_2\\c_3\end{pmatrix}=B\mathbf{c}
$$

이를 만족하는 $\mathbf{c}$를 찾아서 아래의 식에 대입하면 해가 나온다.

$$
\mathbf{x}=\mathbf{x}_{+}+X\mathbf{c}(X=\begin{pmatrix}\chi_1&\chi_2&\chi_3\end{pmatrix})
$$

#### 4) 검산

이제 위의 식을 실제 값으로 바꾸어 계산해보겠다.

$$
\displaylines{
B=\begin{pmatrix}\beta_1&\beta_2&\beta_3\end{pmatrix}=\begin{pmatrix}0&1&1\\1&1&1\\1&1&0\end{pmatrix}\\
\begin{pmatrix}b_{27}\\b_{28}\\b_{29}\end{pmatrix}=B\mathbf{c}=\begin{pmatrix}c_2+c_3\\c_1+c_2+c_3\\c_1+c_2\end{pmatrix}\\
B^{-1}=\begin{pmatrix}1&1&0\\1&1&1\\0&1&1\end{pmatrix}\\
\therefore\mathbf{c}=B^{-1}\begin{pmatrix}b_{27}\\b_{28}\\b_{29}\end{pmatrix}=\begin{pmatrix}b_{27}+b_{28}\\b_{27}+b_{28}+b_{29}\\b_{28}+b_{29}\end{pmatrix}\\
\mathbf{x}=\mathbf{x}_{+}+X\mathbf{c}=\begin{pmatrix}0\\0\\0\\b_{01}\\b_{02}\\b_{03}\\\sum_{\mathbf{b}_0} m(1,2,4)\\\sum_{\mathbf{b}_0} m(1,2,3,5)\\\sum_{\mathbf{b}_0} m(2,3,6)\end{pmatrix}+\begin{pmatrix}b_{27}+b_{28}\\b_{27}+b_{28}+b_{29}\\b_{28}+b_{29}\\b_{29}\\b_{28}\\b_{27}\\b_{27}+b_{29}\\0\\b_{27}+b_{29}\end{pmatrix}\\
=\begin{pmatrix}0\\0\\0\\b_{1}\\b_{2}\\b_{3}\\\sum_{\mathbf{b}} m(1,2,4)\\\sum_{\mathbf{b}} m(1,2,3,5)\\\sum_{\mathbf{b}} m(2,3,6)\end{pmatrix}+\begin{pmatrix}\sum_{\mathbf{b}} m(1,3,6,7,8)\\\sum_{\mathbf{b}} m(5,7,8,9)\\\sum_{\mathbf{b}} m(1,3,4,8,9)\\\sum_{\mathbf{b}} m(1,3,5,6,9)\\\sum_{\mathbf{b}} m(4,5,6,8)\\\sum_{\mathbf{b}} m(1,3,4,5,7)\\\sum_{\mathbf{b}} m(4,6,7,9)\\0\\\sum_{\mathbf{b}} m(4,6,7,9)\end{pmatrix}=\begin{pmatrix}\sum_{\mathbf{b}} m(1,3,6,7,8)\\\sum_{\mathbf{b}} m(5,6,7,9)\\\sum_{\mathbf{b}} m(1,3,4,8,9)\\\sum_{\mathbf{b}} m(3,5,6,9)\\\sum_{\mathbf{b}} m(2,4,5,6,8)\\\sum_{\mathbf{b}} m(1,4,5,7)\\\sum_{\mathbf{b}} m(1,2,6,7,9)\\\sum_{\mathbf{b}} m(1,2,3,5)\\\sum_{\mathbf{b}} m(2,3,4,7,9)\end{pmatrix}\\
=\begin{pmatrix}1&0&1&0&0&1&1&1&0\\0&0&0&0&1&0&1&1&1\\1&0&1&1&0&0&0&1&1\\0&0&1&0&1&1&0&0&1\\0&0&0&1&1&1&0&1&0\\1&0&0&1&1&0&1&0&0\\1&0&0&0&0&1&1&0&1\\1&0&1&0&1&0&0&0&0\\0&0&1&1&0&0&1&0&1\end{pmatrix}\mathbf{b}
}
$$

$\mathbb{Z}_2$ 위에서의 $A$의 역행렬은 존재하며, 그 값은 위 행렬의 값과 같다.
