---
layout: page
group: "Chapter. 5"
title: "5. Finite-Horizon MDP (FH-MDP)"
---

- 이제 현실적으로 좀 써먹을 수 있는 MDP 문제를 다루어보도록 하자.
    - 가장 먼저 FH-MDP라 불리우는 *finite-horizon MDP* 문제를 살펴보도록 한다.
    - 이건 실제로도 FH-MDP 라고 많이 부른다.
- *fininte-horizon* 모델은 간단히 이야기하자면 확률 프로세스를 \\(H\\) 스텝까지만 진행하는 모델이라고 생각하면 된다.
    - 즉, 종료 시점이 초기부터 지정되어 있다.
    - 예를 들어 '총 스텝은 10이다.' 등으로 \\(H\\) 가 지정되어 있는 모델이다.
    - 물론 이후에 이와는 대조적인 *infinite-horizon MDP* 모델도 설명할 것이다.
- *finite-horizon MDP* 는 다음 4가지 요소로 이루어져 있다.

$$FHMDP\;(X, A, P, R)$$

- 각각의 의미는 다음과 같다.
    - \\(X\\) : (유한) 상태 집합
    - \\(A\\) : (유한) 액션 \\( A(x)\\) 의 집합
    - \\(P\\) : 전이 확률 테이블
    - \\(R\\) : 보상(reward) 함수 ( \\(X \times A(x) \rightarrow \mathbb{R}\\) )
- 가끔 *discount factor* 를 요소 중 하나로 포함하여 5가지 요소라고 말하기도 한다. ( \\( FHMDP\;(X, A, P, R, \gamma) \\) )
    - 혹은 이것 대신 스텝 \\(S\\) 를 포함하기도 한다. 아니면 둘 다 포함하여 6가지 요소라고도 한다.
- 참고로 보상 값 계산은 다음과 같이 이루어진다.

$$R(x, a) = \sum_y p_{xy}^{a}R(x, a, y)$$

- 액션이 선택되는 경우 그 액션을 인해 전이되는 모든 경우에 대해 보상의 기대값(expectation)을 사용한다.
    - 즉, 액션을 취한 위 어떤 상태로 전이되는가도 보상 식에 포함된다.
    - 그리고 이 전이는 확률적으로 선택된다고 이미 이야기했다.
    - 아주 간단한 모델에서는 액션을 선택할 때 전이되는 상태가 오로지 한개인 경우도 있다. (사실 이런 경우가 많다.)
- 확률 전이 테이블은 각 액션에 따라 정의된다.

$$p_{xy}^{a} = p(y|x, a)$$

$$p^{a} = \left[\begin{array}{ccc} p_{00} & ... & p_{0n} \\ ... & ... & ... \\ p_{n0} & ... & p_{nn} \end{array}\right]$$

- 이제 가능한 모든 policy \\(\pi\\) ( \\(\pi \in \Pi\\) ) 중 \\(\max\_{\pi} V\_H^{\pi}(x)\\) 를 만족하는 \\(\pi\\) 를 찾아보자.
- 모든 \\(x \in X\\) 에 대해 \\(V_{n}^{\*}(x)\\) 는 다음을 만족한다.

$$V_{n}^{*}(x) = \max_{a \in A(x)}\left\{\bar{R}(x, a) + \gamma \sum_{y \in X} p_{xy}^{a} V_{n-1}^{*}(y)\right\}$$

- 단, \\(n=\{1,2,...H\}\\) 이고 \\(V\_0^*(x)=0, \forall x \in X\\) 이다.
- 증명은 생략한다. (증명식이 있긴 한데 이해하기 어렵네. 그럼 생략이지.)
- 이 식을 **Bellman's optimality equation** 이라고 한다.
- 수식의 형태는 앞서 살펴본 DP 식과 거의 유사하다.
- MDP 에서 사용되는 식 중 가장 중요한 식으로, 이 식을 이해하면 기본은 다 이해한 것이다.

## [Example] Simple MDP
- 그냥 끝내기는 섭섭하니 이 쯤에서 예제를 보도록 하자. 
    - 물론 이후에 여러 예제를 살펴볼 계획이지만 여기서는 일단 초간단 예제를 통해 감을 익히자.
- 다음과 같은 조건을 가지고 *finite-horizon MDP* 를 확인해 보도록 한다.

$$\left|\begin{array}{cc} x_0 & x_1 \\ x_2 & x_3 \end{array}\right|$$

- 어떤 공간에 위와 같은 4개의 상태가 존재한다고 하자.
- 이 때 주어진 환경은 다음과 같다.

- - -

- \\( X = \\{x\_0, x\_1, x\_2, x\_3\\} \\)
- \\( A = \\{right(r), left(l), down(d), up(u), stay(s) \\} \\)
- \\( A(x\_0) = \\{r, d\\}, A(x\_1) = \\{l, d\\}, A(x\_2) = \\{r, u\\}, A(x_3) = \\{s\\}\\)

- - -

- 원래 수식대로라면 스텝 \\( t\\) 에 따라 액션이 달라질 수 있지만 그러면 문제가 너무 복잡해진다.
    - 따라서 여기서는 \\( t\\) 에 상관없이 한 상태에서 취할수 있는 액션은 모두 동일하다고 가정한다. (사실 이게 마코프 모델이다.)
        - \\( \\{[s,a]\_0, [s,a]\_1,...,[s,a]\_t\\} \equiv \\{[s,a]\_{1...t}\\} \\)

- 이 때의 보상 테이블도 정의해보자. (그리고 이것도 최대한 단순하게 정의하자.)

$$R(x, a, y) = \left\{\begin{array}{cc} 2 & x \neq y \;\&\; y \neq x_3 \\
                                        20 & x \neq y \;\&\; y=x_3 \\
                                        0 & x = y \end{array}\right.$$
                                        
- 딱 느낌이 오는데 여기서는 최종 목표가 \\( x_3\\) 에 도착하는 것이라는 것을 알 수 있다.

- 확률 전이 테이블도 정의하자. 당연히 초 간단 모델이다.

$$P = \left\{\begin{array}{ccc} p_{x_0,x_1}^{r}=1 & p_{x_0,x_2}^{d}=1 & p_{x_2,x_3}^{r}=1 \\
                                  p_{x_1,x_0}^{l}=1 & p_{x_1,x_3}^{d}=1 & p_{x_3,x_3}^{s}=1 \end{array}\right.$$
                                  
- 계산을 진행해보자. 우선 \\(H=1\\) 인 상황에서의 값 함수 \\( V\\) 를 살펴보자.
    - 즉, \\(H=1\\) 이라는 것은 오로지 1번만 이동이 가능한 경우이다.
    - 이 때 모든 상태에 대해 \\(V\\) 를 확인 가능하다. (시작 위치가 주어지지 않았으므로)

$$V_1(x_0) = \max_{a\in A(x_0)} \left(\sum_{y \in X} p_{x_0 y}^{a}(R(x_0, a, y) + V_0^*(y)) \right) = \max_{a\in A(x_0)}\left(\sum_{y \in X} p_{x_0 y}^{a}(R(x_0, a, y)\right)$$

- 이를 각각의 상태에 따라 전개하면 다음과 같다. 일단 \\(x_0\\) 부터 보자.

$$V_1(x_0) = max_{a\in A(x_0)}\left(p_{x_0x_1}^r R(x_0, r, x_1), p_{x_0x_2}^d R(x_0, d, x_2)\right) = max \left(2, 2\right) =  2$$

- 다른 위치에서의 값도 살펴보자.

$$V_1^*(x_2) = 20$$

$$V_1^*(x_3) = 0$$

$$V_1^*(x_1) = max_{a\in A(x_1)}\left(p_{x_1x_0}R(x_1, l, x_0), p_{x_1x_3}R(x_1, d, x_3)\right) = max \left(2, 20 \right) = 20$$

- 지금까지 1-step 에 관한 내용을 자세히 살펴보았다.
    - 이걸 확장에서 더 큰 스텝(step)의 값도 만들어 낼 수 있다.
    - 다음 스텝(step)을 확인하기 전에 \\(H=1\\) 에서의 *policy* \\(\pi^{\*}\\) 를 결정해보자.
    
$$\pi_{1}^{*} = \{ \pi_0(x_0) = r, \pi_0(x_1)=d, \pi_0(x_2) = r, \pi_0(x_3)=s\}$$

- - -

- 이제 \\(H=2\\) 인 모델을 고려한다.

$$V_2^*(x_0) = max_{a\in A(x_0)} \left(p_{x_0x_1}[R(x_0, r, x_1)+V_1^*(x_1)], p_{x_0x_2}[R(x_0, d, x_2)+V_1^*(x_2)]\right) = max\left(2+20, 2+20\right) = 22$$

- \\(DP\\) 를 이해하고 있다면 별로 어려운 식이 아님을 알 수 있다.
- 이제 *policy* \\(\pi\\) 를 결정해보자.
    - 일단 위에서 구한 식은 \\(x\_0\\) 이므로 우선 \\(x\_0\\) 의 액션을 결정해본다.
    
$$\pi_0(x_0) = r$$

- 그럼 \\(\pi\_1(x\_0)\\) 를 결정할 수 있을까?
    - 현재 \\(t\\) 값에 따라 상태의 액션이 바뀌는 모델이 아니기 때문에 그냥 \\(\pi\_1(x\_0)=r\\) 을 사용해도 된다.
        - 이를 *stationary* 모델이라고 한다.
        - 하지만 보다 근본적으로, 이 모델에서는 \\(\pi\_1\\) 인 경우 \\(x\_0\\) 상태에 머무를 수 없다.
    - 따라서 실제로는 정의를 하지 않아도 문제는 없다.
    
- 참고로 \\(t\\) 값에 따라 상태의 액션 선택이 바뀌는가 아닌가에 따라 모델을 다르게 고려한다.
- 그리고 이 둘을 구분짓는 것이 중요하다.
    - 상태 \\(x\\) 에서 \\(\pi\_t(x) = \pi\_{t'}(x)\\) 인 조건을 만족하는 *policy* 를 *stationary policy* 라고 부른다.
    - 상태 \\(x\\) 에서 \\(\pi\_t(x) \neq \pi\_{t'}(x)\\) 인 조건을 만족하는 *policy* 를 *non-stationary policy* 라고 부른다.
    - 당연히 *non-stationary policy* 가 계산량이 훨씬 작고, 따라서 자주 사용되는 모델이다.
    
- - -

- 지금까지 \\(V\_H^{\pi}\\) 값을 최대로 만드는 방법을 살펴보았다.
- 이 알고리즘은 매우 간단한데, \\(V\_0^\*(x)=0\\) 에서 시작하여 \\(V\_1^\*(x)\\) , \\(V\_2^\*(x)\\) 순서로 확장, 반복하여 \\(V\_H^\*\\) 를 구하는 방식을 사용한다.
    - 결국은 \\(V_H\\) 를 최대화하는 과정을 스텝 별로 찾은 뒤에, 얻어진 \\(V\\) 를 활용하여, 상태에서 사용할 액션을 선택하여 최종 \\(\pi^\*\\) 를 얻게된다.

- 이제 \\(\pi\\) 의 관점에서 \\(\pi\\) 에 대해 최적의 \\(\pi^\*\\) 를 구할 수 있는 수식을 직접적으로 표현해보자.

$$\pi_{H-n}^*(x) = {\arg\max}_{a\in A(x)}\left(\bar{R}(x, a) + \sum_{y}p_{xy}V_{n-1}^*(y)\right)$$

- \\(\pi\\) 에 붙은 \\(H-n\\) 의 의미를 잘 생각해야 한다.
    - 전체 step 은 \\(H\\) 이다. 
    
$$\pi_{H-1}^*(x) = \arg\max(\bar{R}+\sum p_{xy} V_{0}^*(y))$$

$$\pi_{H-2}^*(x) = \arg\max(\bar{R}+\sum p_{xy} V_{1}^*(y))$$

$$...$$

$$\pi_{0}^*(x) = \arg\max(\bar{R}+\sum p_{xy} V_{H-1}^*(y))$$

- 이 때의 시퀀스가 최적의 \\(\pi^\*\\) 가 된다.

$$\pi^* = \left\{\pi_0^*, \pi_1^*,...\pi_{H-1}^*\right\}$$

- 이렇게 얻어진 \\(\pi^\*\\) 는 다음의 식을 만족하게 된다.

$$\pi^* \in {\arg\max}_{\pi \in \Pi}V_{H}^{\pi}(x), \forall x \in X$$

- 사실은 귀납법을 이용하여 증명해야 하지만 우리가 이런거 다 따지는 사람들이 아니다.
    - 참고로 *fixed point theorem* 를 이용하여 증명한다.

- 이 식을 모두 *Bellman's Equation* 이라고 한다. 최종 정리하면,

$$V_H^{*}(x) = max_{a \in A(x)}\left(\bar{R}(x, a) + \gamma\sum_y p_{xy}^aV_{H-1}^*(y)\right)$$

$$\pi_{H-n}^*(x) \in {\arg\max}_{a \in A(x)}\left(\bar{R}(x, a)+ \gamma\sum_y p_{xy}^aV_{n-1}^*(y)\right)$$


