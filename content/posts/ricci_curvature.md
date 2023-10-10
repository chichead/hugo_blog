---
title: '✏️ Ricci Curvature로 Over-squashing 해결하기'
date: '2023-04-30'
categories: ['GNN', 'Math', 'Deep Learning']
showToc: true
ShowBreadCrumbs: true
comments : true
---

GNN(Graph Neural Network)은 점점 더 발전하고 있는 딥러닝 분야다. 이미 다양한 영역에서 그 잠재력과 영향력을 선보이고 있는 상황. GNN이 풀어야 하는 질문들은 여전히 많이 있지만, 그중에서도 가장 대표적인 challenge 2개가 있다. 바로 over-smoothing과 over-squashing인데, over-smoothing은 여러 개의 graph convolution layer를 쌓게 될 경우, 노드의 정보가 유사해지는 경향이 있어 GNN 모델의 학습 능력이 떨어지는 문제를 말한다. over-squashing는 노드의 정보가 과도하게 압축되면서 정보가 손상되는 문제이다. 

이번 포스트에서는 over-squashing 문제를 해결하기 위한 방법 중 하나인 ricci curvature에 대해 정리해 본다. 참고한 논문 등은 아래 Reference에 정리하였다.


## 🐣 Over-squashing

요즘 사용하는 대부분의 GNN은 message-passing(메시지 전달)이 기본 프레임워크라고 할 수 있다. MPNN(Message Passing Neural Network)는 한 노드의 정보를 그 노드의 이웃들의 정보를 이용해서 업데이트하는 구조를 가지고 있다. 이런 정보는 노드와 노드를 연결해주는 엣지를 따라 흘러갈텐데, 특정 상황에서는 제대로된 메시지 전달이 되지 않는 경향이 있다. 노드와 노드 사이의 거리가 증가함에 따라 두 노드 사이의 통계적 dependency가 감소하는 이른바 Long Range Dependency(LRD, 장거리 의존성) 문제가 있는 경우가 그렇고, 먼 거리의 노드가 기하급수적으로 많이 발생하는 그래프 구조를 가지고 있을 경우도 그렇다.

이웃 노드가 아닌 멀리 떨어진 노드에서 오는 메시지는 고정된 크기의 벡터로 압축 과정을 거치는데, 위와 같은 상황에서는 메시지 정보가 과도하게 스쿼싱되는 현상, 즉 over-squashing 현상이 발생한다. 그리고 이 over-squashing 현상을 유발하는 그래프의 구조적인 특성을 bottleneck(병목)이라고 한다. 출퇴근길 병목현상으로 꽉 막혀서 교통흐름이 원할하지 않은 상황을 생각하시면 될 것이다.

![](/images/graph.webp)

그래프의 bottleneck을 제대로 이해하기 위해선 그래프를 기하학적으로 살펴볼 필요가 있다. 그래프의 기하학적 구조에 따라 message receptive field의 증가 형태가 달라질 테니까. 가령 그리드 형태의 그래프라면 노드의 receptive field가 산술급수적으로 늘어날 것이다. 왜냐하면 이웃 노드의 갯수가 다 동일하니까! 반면 트리 구조의 그래프라면 어떨까? 이 경우엔 노드의 정보가 기하급수적으로 늘어난다. 그렇게 되면 over-squashing이 발생할 가능성이 커질 것이다.

## 🐣 Ricci Curvature

그래프를 기하학적으로 이해하기 위해, 우리가 선택한 도구는 바로 리치 곡률(Ricci Curvature)이다. 리치 곡률은 간단히 말해서 두 점에서 뻗어 나오는 직선이 수렴하는지(곡률이 >0), 평행하게 유지되는지(=0), 발산하는지(<0)를 확인할 수 있는 개념이다. 이 리치 곡률 개념을 그래프에 적용해 보겠다. 

![](/images/manifold_curvature.webp)

그래프에서 리치 곡률은 노드 p와 q에서 나오는 엣지(검은색)에 의해 형성된 로컬 구조를 살펴보면 알 수 있다. 아래 그림을 보면 spherical geometry에서는 그 구조가 삼각형(3-clique)을 형성하는 경향이 있다는 걸 확인할 수 있다. 반면 euclidean geometry에서는 평행을 유지한다. 그래프로 보면 1hop을 유지하며 사각형(4-cycle)을 형성한다. 마지막으로 hyperbolic geometry에서는 엣지가 벌어진다. 그래프로 보면 트리와 같은 구조가 될 것이다.

![](/images/graph_curvature.webp)

[\<Understanding Over-squashing and Bottlenecks on Graphs via Curvature\>](https://arxiv.org/abs/2111.14522) 논문에서 저자들은 이 리치 곡률을 그래프에 적용한 balanced forman curvature를 이용해 input 그래프의 feature를 계산해 낸다. 그리고 그 곡률이 마이너스인 엣지들은 over-squashing을 일으키는 bottleneck 현상을 일으킨다고 이야기한다.

## 🐣 Balanced Forman Curvature

그래프에서 forman curvature는 다음과 같이 표현할 수 있다다.

$$ 
𝐹(i,j) := 4 - d_i - d_j + 3|♯_{\ ∆}(i, j)|
$$

여기서 $d_i$와 $d_j$는 노드 $i, j$의 degree다. $|♯_{\ ∆}(i, j)|$ 는 노드 $i$와 노드 $j$가 함께 이루는 삼각형(3-clique)의 수를 뜻한다. 참고로 clique는 서로 다른 두 노드가 반드시 하나의 엣지로 연결된 그래프를 뜻하는 완전 그래프(complete graph)를 의미한다. 3-clique는 노드 3개가 이루는 완전 그래프, 즉 삼각형 구조를 뜻한다. 위 논문의 저자들은 이 forman curvature를 바탕으로 balanced forman curvature를 제시하는데, 이 곡률을 수식으로 표현하면 다음과 같다.

$$
Ric(i, j) := \frac {2} {d_i} + \frac {2} {d_j} - 2 + 2 \frac {|♯_{∆}(i, j)|} {max(d_i, d_j)}  + \frac {|♯_{∆}(i, j)|} {min(d_i, d_j)} +  \frac {(γ_{max})^{-1}} {max(d_i, d_j)} (|♯_{□}^{i}| + |♯_{□}^{j}| ) 
$$  

balanced forman curvature에서는 Sepiral geometry의 특성을 확인할 수 있는 삼각형의 갯수와, Euclidean geometry의 특성을 확인할 수 있는 사각형의 갯수를 이용해 그 곡률을 계산한다. 바로 $♯_{∆}(i, j)$와 $♯_{□}(i, j)$, 그리고 $γ_{max}$를 통해서. $♯_{∆}(i, j)$는 위에서 이야기한 것처럼 노드 $i$와 노드 $j$가 함께 이루는 삼각형(3-clique)의 갯수를 의미한다. $♯_{□}^{i}(i, j)$ 는 노드 $i$와 노드 $j$가 함께 만드는 사각형(4-cycle)에서 노드 $i$의 이웃 노드의 갯수이다. 다만 이 사각형은 내부 대각선(diagonal inside)이 없어야 한다. 마지막 $γ_{max}$는 공통 노드를 공유하는 사각형(4-cycles)의 최대 갯수를 의미한다. 

실제 그래프에서 어떻게 적용할 수 있는지 예를 들어서 설명해보겠다. 아래 그래프에서 노드 0과 노드 1 사이의 엣지에 대해 balanced forman curvature를 구해보겠다.

![](/images/bfc1.png)

먼저 $♯_{∆}(0, 1)$부터. 위에서 설명했던 것처럼 노드 0과 노드 1이 함께 이루는 삼각형의 갯수를 세면 된다. 노드 0과 1은 노드 6과 함께 삼각형을 이룬다. 즉, $♯_{∆}(0, 1)$은 1이다. 그 다음은 이제 사각형의 갯수를 셀 차례. 먼저 $♯_{□}^{0}(0, 1)$ 부터 계산해보겠다. 노드 0과 노드 1이 만드는 사각형(4-cycles)은 \{0, 2, 5 ,1\}과 \{0, 3, 5, 1\} 이렇게 2개가 있다. \{0, 4, 6, 1\}의 경우에는 사각형이긴 하지만 내부 대각선 요소가 있기 때문에 제외된다. 사각형 2개에서 노드 0의 이웃은 \{2, 3\} 이렇게 2개다. 즉 $♯_{□}^{0}(0, 1)$는 2. 마찬가지로 $♯_{□}^{1}(0, 1)$을 계산하면 노드 1의 이웃 노드는 \{5\} 하나 있기 때문에 1이다. 

자 마지막으로 $γ_{max}$를 구하면 된다. $γ_{max}$는 공통 노드를 공유하는 사각형의 최대 갯수를 의미한다. 노드 0과 노드 1이 구성하는 사각형 2개의 공통 노드는 5. 그리고 그 공통 노드 5를 통과하는 사각형의 최대 갯수는 2개다. 즉 $γ_{max}$는 2다. 이제 이 숫자를 balanced forman curvature에 대입하면 끝! d_0는 5이고, d_1은 3이니까, 이제 계산해 보겠다.

$$
Ric(i, j) = \frac {2} {5} + \frac {2}{3} -2 + 2 \frac{1}{5} + \frac{1}{3} + \frac {(2)^{-1}}{5} (2 + 1) = 0.10 > 0
$$

계산해보니 balanced forman curvature는 0.1로 0보다 큰 숫자가 나왔다. 이번엔 또 다른 형태의 그래프에 대해서 곡률을 계산해본다.

![](/images/bfc2.png)

이번 그래프에서는 노드 0과 노드 1이 이루는 삼각형과 사각형이 존재하지 않는다. 이 경우 balanced forman curvature를 계산해보면 음수가 나온다.

$$
Ric(i, j) = \frac {2} {4} + \frac {2}{3} -2 + 2 \frac{0}{4} + \frac{0}{3} + \frac {(0)^{-1}}{4} (0 + 0) = -0.83 < 0
$$

이렇게 음의 곡률을 가진 edged들은 over-squashing을 일으키는 그래프 bottleneck 현상을 일으킨다. 특히 -2보다 작은 곡률을 가진 엣지가 bottleneck 현상을 발생시킨다.


## 🐣 Stochastic Discrete Ricci Flow

![](/images/sdrf.webp)

위와 같은 과정을 거친다면 input graph에서 over-squashing을 일으킬 가능성이 높은 엣지들을 골라낼 수 있게된다. 그리고 그래프에서 가장 큰 음의 곡률을 가진 엣지 주변에 국부적으로 엣지를 추가한다면 bottlneck 현상을 완화시킬수도 있다. 이러한 graph rewiring 방법을 stochastic discrete ricci flow라고 한다. 이 과정을 거치면 원래 그래프보다 훨씬 더 message passing을 잘 하게 되고, 그렇게 되면 GNN 성능이 향상될 수 있다. 

## 📚 Reference

1. [J. Topping, F. Di Giovanni et al., Understanding over-squashing and bottlenecks on graphs via curvature (2022)](https://arxiv.org/abs/2111.14522)
2. [M. Bronstein, Over-squashing, Bottlenecks, and Graph Ricci curvature](https://towardsdatascience.com/over-squashing-bottlenecks-and-graph-ricci-curvature-c238b7169e16)