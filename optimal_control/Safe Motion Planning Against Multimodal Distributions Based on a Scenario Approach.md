# 배경

# 문제

# 이 논문
- Autonomous Vehicle에서 safety를 보장할 수 있는 motion planning 알고리즘을 디자인함.
- uncertainties의 multimodal distribution을 고려함.
	- 예를 들어, 주위 vehicles의 미래 경로의 불확시한 예측은 discrete decision을 반영함. (직진하거나 회전하거나...)
- 우리는 scenario-based approach를 만듬
	- 연산량이 효율적임.
	- multimodal distribution에서 quantifiable 할 수 있는 수의 샘플이 있을 때 높은 confidence로 motion planning 문제를 해결할 수 있음
	- 두 단계로 진행
		1) sample들을 별개의 cluster로 분리
		2) 각 cluster마다 bounding polytope를 계산
	- 이렇게 해서 motion planning 문제 -> polytope를 이용한 mixed-integer 문제로 변환.

# 실험
- nuScenes dataset

# 결과
- multimodal uncertainty에서 높은 확률로 안전을 보장할 수 있음
- 연산량이 효율적임
- 다른 scenario approach보다 덜 보수적임.


---
# 배경
- 불확실한 환경에서 안전한 motion planning 하는 것은 중요함.
	- 바람같은 역학모델 변수가 이런 불확실성을 위해 많이 연구되어왔음.
	- 근데 불확실한 **장애물** 위치같은 불확실성도 중요함.
- uncertainties의 **multimodal distribution**을 고려해야 함
	- 예를 들어, 주위 vehicles의 미래 경로의 불확시한 예측은 discrete decision을 반영함. (직진하거나 회전하거나...)
# 문제
- 이전 방법들은
	- 불확실성을 Gaussian distribution으로 가정하거나,
	- 가정 없이 유한한 샘플의 수로 분포를 표현했었다.
- unimodal distribution에 비해, multimodal distribution을 Stochastic control하는거는 어려움
	- 만약 그냥 gaussian distribution이면
		- chance constraint는 quantile function인 convex constraint로 표현 가능.
	- 하지만 두 gaussian 분포의 혼합이라면?
		- quantile function의 closed form이 존재하지 않기 때문에 convex reformulation이 불가능하다..!
- 그래서 우리가 알기로 multimodal distribution에서 일반적인 chance-constrained 문제를 푼 사례가 없음.
- 어디(Chance constrained programs with mixture distributions)에서는 Gaussian 혼합모델의 chance-constrained 문제를 풀기 위해 **branch-and-bound** 접근을 했었는데,
	- 이거는 large problem에서는 써먹기 어렵고, 실시간성 구현하기 어렵다.

- 기존의 scenario approach를 multimodal인 uncertainty에서 motion planning 문제에 적용하면 
	- conservative하고, 
	- 연산량이 많음

# 이 논문
- AV가 multimodal distribution으로 예측할 때, safety를 보장할 수 있는 motion planning 알고리즘을 디자인함.

- motion planning 문제 -> chance-constrained 문제로 만듬
- 우리는 scenario-based approach를 만듬
	- 연산량이 효율적임.
	- multimodal distribution에서 quantifiable 할 수 있는 수의 샘플이 있을 때 높은 confidence로 motion planning 문제를 해결할 수 있음
	- 두 단계로 진행
		1) sample들을 별개의 cluster로 분리
		2) 각 cluster마다 bounding polytope를 계산
		-> 이러면 chance-constrained 문제를 deterministic 문제로 바꿀 수 있고, 그러면 샘플 수 N에 상관없게 된다.
	- 이렇게 해서 motion planning 문제 -> polytope를 이용한 mixed-integer 문제로 변환.
		- high-confidence solution을 얻을 수 있다.

# 실험
- nuScenes dataset

# 결과
- multimodal uncertainty에서 높은 확률로 안전을 보장할 수 있음
- 연산량이 효율적임
- 다른 scenario approach보다 덜 보수적임.



---

# Problem Setup

문제
- planning horizon 길이 $T$ 에서 input sequence $u\in \mathcal{U}$ 를 아래 조건에 맞추어 계산
$$min_{\mathbf{u}\in \mathbf{\mathcal{U}}} J(\mathbf{u}) $$
$$s.t. \;\;\;\;x_{t+1} = f_t(x_t, u_t), t\in \mathbb{z}_{0:T-1},$$
$$y_t = h_t(x_t), t\in \mathbb{z}_{1:T},$$
$$Pr(\land^T_{t=1} \land^O_{o=1}y_t\notin \mathcal{O}(X_{t,o}))\geq1-\epsilon. $$

- 여기서 $J:\mathcal{U} \to\mathbb{R}$는 convex objective function이다.
	- Ego vehicle을 goal state로 유도함.
- 두 번째 식은 Ego vehicle의 discrete-time dynamical 모델임
- 세 번째 식은 output임.
- 네 번째 식은 chance constraint임
	- 충돌 회피를 encode함.
	- $X_{t,o}\in\mathbb{R}^{nX}$ 는 other vehicle $o$가 시간 $t$ 에서 예측된 state이다.
	- $\mathcal{O}(X_{t,o})\subset\mathbb{R}^{n_y}$ 는 Ego vehicle의 output $y_t$가 피해야 하는 obstacle 집합이다.
	- $X_{t,o}$가 random variable이니까 non-collision 확률을 $1-\epsilon$ 보다 작지 않다고 두었음. ($\epsilon \in (0,1)$은 risk level을 나타냄)
	- Ego vehicle 주위의 other vehicle의 수는 $O$임.


- dynamical model이 시간에 따라 선형적으로 변하는 것을 고려함.
	- 두, 세번째 식에서 $f_t, h_t$는 $x_t, u_t$에 대한 선형 함수이다.
	- $\mathcal{O}(X_{t,o})$는 polytope로 표현된다.
$$\mathcal{O}(X_{t,o}):=\{ y\in \mathbb{R}^{n_y}: A(X_{t,o})y <b(X_{t,o}) \} $$
		- 요거는 nonlinear continuous function A, b의 L halfspace 교점이다. (?)
	- polytopic obstacle avoidance의 constraint를 encode하기 위해서 big-M method를 많이 쓴다.
		- 위 식의 부등식을 만족시키는 indicater로 쓰기 위해 binary variable을 사용함.
		- binary variable $z_{j,t,o}(X_{t,o})\in {0,1}$ 을 사용함.
		- 이걸로 $y_t \notin \mathcal(X_{t,o})$ 이거를 아래처럼 바꿔 표현함.
		$$A_j(X_{t,o})y_t+M(1-z_{j,t,o}(X_{t,o}))\geq b_j(X_{t,o}), \forall j\in \mathbb{z}_{1:L}0$$
			- 여기서 $\sum^L_{j=1} z_{j,t,o}(X_{t,o})\geq 1$ 를 만족.
			- M은 충분히 큰 숫자
			- $A_j, b_j$는 A,b의 j번째 줄
			- $z_{j,t,o}(X_{t,o})=1$ 이면 부등식이 위반되었음을 뜻함. 즉, output $y_t$가 $\mathcal{O}(X_{t,o})$ 밖에 있다는 뜻!
## A. Forecasting
## B. Scenario Approach
- chance-constrained 문제의 흔한 접근법 중 하나임.
- uncertainty distribution을 유한한 샘플의 수로 나타냄
	- 만약 분포가 DNN으로 모델링 되었다면
		- DNN에 query 해서 분포에서 sample을 만들어 냄.

- (G. Calafiore)가 한게 convex 문제만 해결할 수 있어서, (PM Esfahani)는 이거를 mixed integer 문제로 확장했다.
	- 근데 위에 식은 (PM Esfahani) 방식과 맞지 않음
		- 왜냐하면 위에 식에서 binary decision variable이 uncertain parameter $X_{t,o}$에 dependent하기 때문이다.
	- 그래서 우리는 대신 아래 식으로 바꿈.
$$Pr(\land_{j,t,o} A_j(X_{t,o})y_t + M(1-z_{j,t,o})\geq b_j(X_{t,o}))\geq 1-\epsilon$$
		- $z$는 binary variable $z_{j,t,o}\in \{0,1\}$ 의 sequence이다.
			- 그래서 이제 $X_{t,o}$에 독립이게 되었음! (-> 원래 $X_{t,o}$에 종속이던 문제 해결)

- N개의 iid인 샘플 $X_{t,o}^{(i)}$가 주어졌을 때, 위에 constraint에 넣어서 계산함.
	- 그러면 (PM Esfahani)의 결과에 적용할 수 있게 됨.


### Lemma 1 (PM Esfahani)
- risk level $\epsilon\in (0,1)$ 이랑 confidence bound $\beta \in (0,1)$이 주어졌을 때,
- N이 아래의 식을 만족하게 하면 위에 식의 optimal solution이 그 위에 식을 적어도 $1-\beta$의 확률로 만족한다.
$$2^{n_b}\sum^{n_c-1}_{i=0}\binom{N}{i}\epsilon^i(1-\epsilon)^{N-i}\leq\beta$$


## C. Drawbacks of Scenario Problem
- 이렇게 하면 mixed-interger 문제 해법으로 쉽게 풀리긴 하는데 단점이 있음.
	1) mixed-integer constraints의 수가 LTON이다.
		- 문제에 큰 수의 mixed-integer constraints가 포함되어 있다.
		- 만약 예시에서 N=1706이면 푸는데 4초가 걸려서 실시간으로는 못씀
	2) scenario 문제는 데이터가 multimodal distribution이면 **conservative한** 경향이 있음
		- (예시 들었음)
		- uncertain parameter가 multimodal distribution일 경우에 binary variable이 충분하지 못하다면 conservative하게 될 수 있음.
			- (IDEA!) **만약 각 mode의 distribution에 추가적인 binray variables 집합을 더 준다면 conservativeness를 줄일 수 있음.**
			- 이 직관적인 아이디어로 문제 해결하려고 함

# Solution Approach
우리 접근법
1) (Clustering) 샘플들을 K cluster로 나눈다. (K는 분포에서 mode의 수)
	- 분포의 K개 mode를 직접 다뤄서 conservativeness를 줄인다.
2) (Polytopic Approximation) k개의 각 cluster에서 obstacle 집합을 가진 집합 $\mathcal{C}^k$ 를 계산한다.
	- 이를 통해 연산량이 효율적인 선형 계획을 만들고
3) (Mixed-Integer Motion Planning Formulation) 이 $\bigcup^k_{k=1} \mathcal{C}^k$ 를 가지고 deterministic motion planning 문제를 푼다!
	- N 숫자와 독립적이게 mixed-integer motion planning 문제를 푼다.
## Clustering
## Polytopic Approximation
## Mixed-Integer Motion Planning Formulation