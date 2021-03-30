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

사실 백준 [14939번 - 불 끄기](http://noj.am/14939) 문제를 보고 생각난거 적는 사이트입니다. 논문 아님

## 서론

### Lights Out 퍼즐은 무엇인가

처음으로 만들어진 Lights Out 퍼즐은 5×5 보드 위에 각 칸마다 전구와 스위치가 하나씩 존재하고, 스위치를 누르면 해당 칸의 전구와 가로세로 방향으로 인접한 전구가 토글된다는 규칙이 있는 퍼즐이다. 퍼즐을 처음 시작하면 랜덤하게 켜진 전구와 꺼진 전구가 분포하며 위의 규칙대로 스위치를 눌러서 보드 위의 모든 불을 끄는 퍼즐이다.

### Lights Out 퍼즐의 일반적인 해결법

#### Chasing Light 방법을 사용한 해법

일반적인 해결법으로는 Chasing Light 방법이라는 그리디한 방법을 사용한다. 이 방법은 아래와 같은 과정으로 진행한다.

1. 가장 윗줄부터 켜진 불을 끈다. 불을 끌 때에는 해당 칸의 아래 스위치를 토글하여 불을 끈다.
1. 이 방법을 가장 윗줄부터 차례로 진행하면 가장 아랫줄에만 켜진 전구가 남는다.
1. 가장 아랫줄에 남아있는 전구의 형태에 따라 가장 윗줄의 몇 개의 전구를 킨다.
1. 가장 윗줄부터 1번~2번의 방법을 적용하면 남아있는 모든 전구가 꺼진다.

#### 행렬을 사용한 해법

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
\mathbf{b}+A\mathbf{x}=0\newline\matrix{A}=\begin{pmatrix}Z&I&O&O&O\\I&Z&I&O&O\\O&I&Z&I&O\\O&O&I&Z&I\\O&O&O&I&Z\end{pmatrix}\newline Z=\begin{pmatrix}1&1&0&0&0\\1&1&1&0&0\\0&1&1&1&0\\0&0&1&1&1\\0&0&0&1&1\end{pmatrix}
$$

그리고 $\mathbf{b}$와 $\mathbf{x}$ 모두 $\mathbb{Z}_2^{25}$ 위의 벡터이므로 $A$는 $\mathbb{Z}_2^{25\times 25}$ 상의 행렬일 것이다. 그러면 다음의 식을 만족한다.

$$
\mathbf{b}+A\mathbf{x}=\mathbf{b}+A\mathbf{x}-2A\mathbf{x}=\mathbf{b}-A\mathbf{x}=0\newline\mathbf{b}=A\mathbf{x}
$$

이제 $\mathbb{Z}_2^{25\times 25}$ 상에서의 $A$의 역행렬을 구하면 $\mathbf{x}=A^{-1}\mathbf{b}$의 식을 사용하여 어떤 스위치를 누르면 퍼즐을 해결할 수 있는지 구할 수 있다.

이제 여기서 한 가지의 의문점이 생긴다. 과연 $A$의 역행렬이 존재할까? Lights Out 퍼즐의 관점에서 다시 써보자면, 어떤 상태의 보드에 대해서도 해가 존재할까?

### Lights Out 퍼즐은 아무 때나 해결할 수 있는가...를 알아보기 전에

$n\times m$ 보드에 대해 역행렬(정사각행렬이 아닌 경우 유사역행렬)의 존재성을 찾는 것은 곧 $(nm)×(nm)$ 크기의 행렬의 역행렬이 존재하는 것인지 찾는 것이기 때문에 매우 힘들다. 당장 10×10 보드라면 100×100 크기의 행렬의 역행렬의 존재 가능성을 판별해야 한다. 정사각행렬의 역행렬을 판별하는 과정은 [위키피디아](https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations#Matrix_algebra)에 따르면 일반적인 방법이 $O(n^3)$, 속도를 극대화한 알고리즘의 경우 $O(n^{2.373})$의 시간 복잡도를 가진다고 한다. 그렇다면 10×10 보드라면 $O((10\times 10)^3)$의 시간 복잡도를 필요로 한다. $n\times m$ 보드에 대하여는 $O((nm)^3)$의 시간 복잡도를 필요로 하게 된다. 따라서 문제를 쪼개서 필요한 연산을 줄일 필요가 있다. 이 방법을 찾는 것이 이 문서의 목표이다.
