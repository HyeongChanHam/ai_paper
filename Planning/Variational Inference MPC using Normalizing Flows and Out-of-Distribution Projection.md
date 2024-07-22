Thomas Power and Dmitry Berenson

# 배경

# 이 연구
- collision-free navigation을 위한 MPC 를 제안
	- 시작, 목적지, 그리고 환경에 대해 normalizing flow를 학습한 optimal control sequence의 분포를 근사시키는 amortized variational inference를 사용함.
- 이 representation으로 로봇의 dynamics와 복잡한 obstacle geometry를 둘 다 설명할 수 있음.
- 그다음에 이 분포에서 샘플링을 해서 goal-disrected이고 collision-free인 control sequence를 만들어낸다.
	- 이걸로 FlowMPPI를 만들어냄
		- sampling-based MPC method임
- 근데, 이 방법 쓰면서 로봇은 Out-of-distribution 환경에 갈 수도 있음
	- 학습하던 환경이랑 급격하게 다른 곳
	- 그럴 경우 학습된 flow는 low-cost control sequence를 만들어낸다고 믿을 수 없다.
- 우리 방법을 OOD 환경에도 사용할 수 있게 만들기 위해 우리는 우리 MPC 과정의 일부로 환경의 representation으로 projection하는 방법을 제안함.
	- projection을 하면 환경 representation을 더 in-distribution하게 만들면서 trajectory quality를 실제 환경에 더 최적화 시킨다.

# 실험
- simulation
	- 2D double integrator
	- 3D 12DoF underactuated quadrotor

# 결과
- FlowMPPI는 projection이랑 같이 쓸때 SOTA MPC들에 비해 in-distribution이랑 OOD랑 둘다 성능이 좋았음.

---

# 배경
- MPC는 로보틱스 적용에 많이 사용되었음
	- autonomous driving
	- bipedal locomotion
	- manipulation of deformable objects

- nonlinear system에서는 MPC의 sampling based 접근법이 인기 있다.
	- 예를 들어 Cross Entropy Method (CEM), Model Predictive Path Integral Control (MPPI)
	- 왜냐하면
		- 불확실성을 다룰 수 있고
		- dynamics랑 cost function에 대한 assumption을 최소화하고
		- sampling을 병렬화 할 수 있다.

# 문제
- 근데 이 방법들은 low-cost control sequence를 랜덤으로 뽑아내는게 불가능하고, local minima에 빠질 수 있는 상황에서는 어려움을 겪는다.
	- 예를 들어 로봇이 복잡한 환경에서 경로를 찾아야 하는 경우
	- 원인
		- 이 방법들에서 사용되는 sampling 분포는 환경의 기하학적 정보를 모르기 때문.

# 관련 연구
- 이전 연구들은 control과 inference 사이의 duality에 대해 조사했고,
- planning과 control 둘 다 inference 문제로 고려했다.
- 몇몇 연구들은 finite-horizon stochastic optimal control 문제를 Bayesian inference 문제로 보았다.
	- control sequence를 샘플링하기 위한 distribution을 근사하기 위해 variational inference를 쓰는 방법을 제시함.
	- Variational inference를 하기 위해 parametrized distribution을 정해야 한다.
		- 최적화하고 샘플링 하는게 가능해야 하고, 
		- low-cost trajectory의 true distribution의 좋은 근사를 제공하는 데에 유연해야 함
			- 환경에 대해 강한 의존성을 가지고 있고,
			- multimodality도 보인다.
- 더 복잡한 representation이 이 분포를 표현하기 위해 사용되었음에도 불구하고, 
	- 이 분포는 처음에 정보가 없고, deployment 하는 동안 반복적으로 개선되어야 한다.
- 하지만 우리가 제시하는 방법은 normalizing flow를 써서 이 분포를 표현함.
- 그리고 이 모델의 parameter를 데이터로부터 얻는다!


# 이 연구
- collision-free navigation을 위한 MPC 를 제안
	- 시작, 목적지, 그리고 환경에 대해 normalizing flow를 학습한 optimal control sequence의 분포를 근사시키는 amortized variational inference를 사용함.
- 이 representation으로 로봇의 dynamics와 복잡한 obstacle geometry를 둘 다 설명할 수 있음.
- 그다음에 이 분포에서 샘플링을 해서 goal-disrected이고 collision-free인 control sequence를 만들어낸다.
	- 이걸로 FlowMPPI를 만들어냄
		- sampling-based MPC method임
- 근데, 이 방법 쓰면서 로봇은 Out-of-distribution 환경에 갈 수도 있음
	- 학습하던 환경이랑 급격하게 다른 곳
	- 그럴 경우 학습된 flow는 low-cost control sequence를 만들어낸다고 믿을 수 없다.
- 우리 방법을 OOD 환경에도 사용할 수 있게 만들기 위해 우리는 우리 MPC 과정의 일부로 환경의 representation으로 projection하는 방법을 제안함.
	- projection을 하면 환경 representation을 더 in-distribution하게 만들면서 trajectory quality를 실제 환경에 더 최적화 시킨다.

# 실험
- simulation
	- 2D double integrator
	- 3D 12DoF underactuated quadrotor

# 결과
- FlowMPPI는 projection이랑 같이 쓸때 SOTA MPC들에 비해 in-distribution이랑 OOD랑 둘다 성능이 좋았음.



---
# 관련 연구
# A. Planning & Control as Inference
- control이랑 inference 사이의 관계는 오랫동안 확립되어 왔다.
- (Attias): 처음으로 planning을 inference 문제로 만들었고, discrete한 state, action 공간에서 tractable한 inference 알고리즘을 제안함.
- 이후 연구에도 planning을 위한 inference technoques랑 Stochastic Optimal Control (SOC)가 사용되었다.
- sampling based MPC technique에서 잘 쓰이는 2가지 방법
	- MPPI, CEM 둘 다 low-cost control sequence를 만들어내기 위해 importance sampling 방식을 쓴다.
	- SOC의 inference formulation과 강한 연관이 있다.
- 어떤 연구들은 SOC 문제를 Bayesian inference로 고려함.
	- Variational Inference (VI)를 해서 low-cost control sequence의 posterior를 근사함.
	- 이 방법들은 어떻게 variational posterior를 표현하는지에 따라 차이가 있다.
	- VI 방법은 주로 independent Gaussian posterior를 쓴다.
		- mean-field approximation이라고도 불림
	- (OKada and Taniguchi): control sequence를 Gaussian mixture로 표현함
	- (Lambert): particle representation을 사용함
	- 이 representation들은 복잡한 posterior를 표현하는데에 유연성을 주었다.


- 우리 연구는 motion planning을 위해 데이터로부터 sampling distribution을 학습하는 연구와 관련이 있다.
	- (Zhang): 다양한 환경에서 학습된 sampling distribution을 학습함 (근데 환경에는 독립적임.)
	- 또 딴 사람들은 environment, start, goal에 dependent한 sampling distribution을 학습함
	- 이 방법들은 geometric planning에 제한적이지만, 
	- (Li)는 kinodynamic planning 접근법을 제안
		- dynamics에 일관성이 있는 sample state를 사용하는 generator와 discriminator를 학습함
	- (Lai): sampling distribution을 학습하기 위해 diffeomorphism을 사용함.
		- normalizing flow랑 비슷한 모델
	- 우리가 제안하는 모델도 start, goal, environment에 조건적인 샘플을 생성하기 위해 학습할 것이다.
		- 근데 이 연구에서 우리는 offline planning이 아니라 online MPC를 고려하고 있다.
	- 
