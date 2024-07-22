Min Wen @ upenn 박사때(now Google), Ufuk Topcu 우푸크 @ ut austin
Topcu 교수가 UT austin 오기 전에 pennsylvania 있을 때 지도교수.
NIPS 2018

# 배경
- safe reinforcement learning 문제
	- constraints = 유한 길이의 trajectories에서 expected cost로 정의


# 이 논문
- Constrained Cross-Entropy-based method 제안.
	- constraint를 만족하는 성능을 명시적으로 추적하고, 안전이 중요한 응용에 적합하다.
- 이 알고리즘의 점근적인 행동은 ordinary differential equation으로 표현될 수 있다.
	- 이 알고리즘이 수렴하기 위해 이 differential equation의 충분조건을 보여줌.

# 실험

# 결과
- 제안한 알고리즘은 초기 정책의 feasibility에 대한 가정 없이도 feasible policy를 학습할 수 있다.
	- non-Markovian인 목적함수나 constraint 함수이더라도 가능!

---

# 배경
- safe reinforcement learning 문제
	- constraints = 유한 길이의 trajectories에서 expected cost로 정의

- constrained optimal control problem
	- dynamical system model, 연속된 state, actions, 목적함수, constraint function이 주어졌을 때,
		- 목적함수를 최대화 하는 동시에 constraint를 만족하는 controller를 찾아라!

# 문제
- control community에서 수십년간 다뤄왔지만 여전히 실용적인 문제에서는 문제가 있다.
- 어려움으로 예를 들자면,
	- 장애물을 피하면서 cost-efficient 한 방법으로 목적지에 도달하는 nonholonomic mobile robot을 생각해보자.
		- obstacle-free한 state space는 주로 nonconvex이다.
		- dynamical system model의 방정식은 주로 매우 비선형적이다.
		- constraint 함수랑 cost 함수는 convex가 아니거나, 미분이 불가능할 수도 있다.
		- 게다가 관측불가능한 숨겨진 변수들이 있을 수 있는데 이거때문에 transition이랑 cost가 non-Markovian이게 된다.
	- 이 모든 어려움들을 봤을 때, 우리는 적어도 feasible하고 cost objective를 가능한 많이 향상시킬 수 있는 정책을 계산할 필요가 있다.

- control task를 reward나 cost function으로 encoding을 해서 RL은 많이 성공했음.
- 대부분의 RL은 unconstrained 문제를 푼다.
- **근데 constrained 최적 제어 문제를 unconstrained 문제로 바꾸는건 쉽지 않다.**
	- **objective 최적화랑 constraint 충족 간의 비대칭성 때문임.**
- 주로 목적함수 최적화 하면서 local optimal인 정책을 찾는 것은 납득 가능하다.
- 근데, safe 조건이나 다른 것들을 constraints로 encode할 때, 이 constraint를 조금이라도 위반하면 중대한 결과로 이어지게 됨.


# 관련 연구
- 기존에 policy gradient 기반의 safe RL 방식들은 그들이 출력하는 정책의 strict feasibility를 guarantee할 수 없다.
	- feasible한 초기 정책으로 초기화가 되었더라도 보장할 수 없다..!
	- infeasible한 초기 정책으로 초기화되면 주로 수렴하는 동안 하나의 feasible policy를 찾는 것도 불가능하다.

- Constrained Policy Optimization (CPO)
	- neural network같은 고차원의 policy class를 다룰 수 있음
	- 만약 처음에 feasible solution에서 시작했다면 이 feasibility를 유지할 수 있다!
	- 단점
		- feasibility는 실제로 학습하는 동안 거의 보장되지 않았음.
			- gradient랑 Hessian matrix estimation의 에러 때문으로 보임.

- cross-entropy (CE) concept 기반의 stochastic optimization 방법
	- 

# 질문
- constraint satisfaction의 우선순위를 명시적으로 다루는 강화학습 알고리즘을 만들 수 있나?
	- 초기 정책이 feasible하고 항상 추정된 gradient 방향으로 feasible policy를 찾을 수 있다고 가정하기 보다는,
	- initial policy는 not feasible하거나, 이전에 feasible policy를 본적이 없는 경우를 다룰 것이다!

# 이 논문
- Constrained Cross-Entropy-based method 제안.
	- constraint를 만족하는 성능을 명시적으로 추적하고, 안전이 중요한 응용에 적합하다.

- 기본적인 framework는 기존의 CE method와 동일함.
	- 매 iteration마다 policy의 분포에서 샘플링을 하고, 몇몇 elite sample policy를 고른다.
	- 그리고 그것들을 가지고 policy distribution 을 update한다.
- (policy gradient method처럼) constraints를 목적함수의 extra term으로 보는게 아니라,
- constraint value를 가지고 **policy를 정렬**하는데 사용한다!
- 만약 feasible한 sample policy가 부족하다면,
	- 가장 좋은 constraint performance를 하는 것을 elite sample policy로 고른다.
- 만약 주어진 sample policy의 proportion이 feasible하다면
	- 가장 좋은 objective value를 가지는 feasible sample policy를 elite sample policy로 고른다.
- 최적화를 feasible policy로 초기화하지 않고,
	- 이 방법은 objective function이랑 constraint function 둘다 향상시킨다. (constraint에 우선순위를 두고!)


- 우리 방식은 black-box 최적화로 사용될 수 있음.
	- 최적화 목적이나 제약함수를 encoding하는 reward나 cost function이 있다고 가정하는것도 없음!
- 사실, 이 방식은 아무 유한한 horizon 문제에는 다 적용될 수 있다!
	- objective, constraint function이 어떤 trajectory의 distribution에 대해 평균 성능으로 표현되는거라면..!
	- 예를 들어, constraint function은 agent가 주어진 뭔가를 만족하는 확률일 수 있다.
		- 만약 주어진 태스크를 만족한다는 것이 N-step trajectory 안에 결정될 수 있다면..!
			- 최적화의 목적은 agent가 목적 state에 도달하는데 걸리는 step 수의 기대값일 수도 있고,
			- 아니면 agent가 출발지로부터 떠나온 최대 거리의 기대값일수도 있고,
			- 아니면 전체 trajectory에 대해 agent와 장애물 사이의 최소 거리의 기대값일 수도 있고...

- 이 알고리즘의 점근적인 행동은 ordinary differential equation으로 표현될 수 있다.
	- 이 알고리즘이 수렴하기 위해 이 differential equation의 충분조건을 보여줌.

- Contributions
	1) conti- state, action space에서 동작하는 model-free constrained RL 알고리즘을 제안함.
	2) 우리 알고리즘의 점근적인 행동은 ordinary differential equation으로 표현될 수 있다.
		- objective에 대해 쉽게 해석이 가능함.
	3) 이 ODE가 우리 알고리즘의 수렴을 보장하기 위한 충분조건을 제시


# 실험
- local sensor만 갖고있는 mobile robot navigation task
	-  $\mathcal{G}$ : compact goal region
	-  $\mathcal{B}$ : non-overlapping compact bad region
	- transition function: deterministic
	- 로봇은 local sensing model을 써서 $\mathcal{B}$나 $\mathcal{G}$가 근처에 있는지, 혹은 $\mathcal{G}$의 중심의 방향이 local coordinate에 있는지 관측한다.

- CCE랑 비교한 방법들
	- TRPO
		- SOTA unconstrained RL 알고리즘.
	- CPO
		- TRPO의 variant

- 세팅
	- policy
		- FC network w/ 2 hidden layers w/ 30 nodes
	- Trajectory length: N=30
	- rllab에서 실험함.


# 결과
- 제안한 알고리즘은 초기 정책의 feasibility에 대한 가정 없이도 feasible policy를 학습할 수 있다.
	- non-Markovian인 목적함수나 constraint 함수이더라도 가능!
