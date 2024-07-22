L Zheng
HKUST
IEEE IV 2024

# 배경
- dense traffic scenario에서 높은 성능을 유지하면서 안전을 보장하는 거 중요하다.

# 이 논문
- 연산 효율성이 좋은 spatiotemporal receding horizon control (ST-RHC) 제안
	- 안전하고, 
	- dynamically feasible하고,
	- energy-efficient한
	- trajectory를 control space에서 생성함.
- proactive interaction을 고려한 embodied spatiotemporal safety barrier module을 고안함.
	- 다른 vehicle의 trajectory를 예측하는데 발생하는 부정확성을 완화하기 위해!
- motion planning이랑 control 문제를 constrained nonlinear optimization 문제로 구성함.
	- multiple shooting을 다루는 기존의 optimization solver를 같이 활용할 수 있다.


# 실험
- systhetic
- real-world dataset
- in dense traffic
- 

# 결과
- dense traffic에서 여러 task들이 높은 성능과 안전을 보장한 채 실시간으로 가능함.

---

# 배경
- 자율주행은 지난 몇십년동안 학계랑 산업계에서 상당한 발전을 이뤄왔다.
- 근데, 이 차량들의 안전을 확신하는 것은 도심 주행환경에 적용하는 데에 엄청 중요하다.
- dense traffic 상황에서 가장 큰 문제중에 하나는
	- 안전 조건을 충족하면서 높은 성능을 내는 것이다!
- 자율주행 자동차가 이 두 목적을 효과적으로 달성하도록 설계된 것은 피할 수 없다.
- 하지만, 안전이 중요한 자율주행 자동차에게 동적인 주행환경에서 다른 자동차들과 안전하고 효과적이게 상호작용 하는 것은 중요한 문제다.
	- 예를 들어,
	- dense traffic한 상황에서 overtaking maneuver를 하는 Adaptive cruise control을 하는 동안,
	- Ego vehicle은 높은 성능을 달성하기 위해 stable하고 정확한 주행 속도를 유지해야 한다.
	- 그리고, 느린 앞차를 추월한 뒤에는 원래 차선으로 복귀해서 driving consistency랑 safety compliance를 확신해야 한다.
- dense traffic scenario에서 높은 성능을 유지하면서 안전을 보장하는 거 중요하다.



- 이런 dynamic하고 복잡한 주행 시나리오에서, 사람 운전자는 예측불가능한 multi-modal driving 행동을 보일 수 있다.
	- 가속 / 감속하거나, 차선을 바꾸는 것
	- planning이나 control 하는데 정확하게 예측하기 어려움
- 충돌을 피하기 위해 EV는 주위의 HV의 잠재적인 예측 에러를 같이 고려해야 하고, 미리 뒤얽힌 상태와 입력 제약사항을 고려해서 local trajectory를 재계획해야 한다.


- 하지만, 이 제약사항들은 복잡도가 너무 높아서 연산 문제가 있다.
	- 실시간으로 잘 하는데 어려움을 줌
- objective function 만드는 데에 안전이랑 성능 사이 균형을 맞추는 데에도 어려움을 준다.



- 일반적으로 motion planning과 control은 순차적으로 실행될 수 있다.
- 계층적인 구조에서, 
	- high-level planner: task requirements를 충족하는 feasible trajectory를 계획함
	- low-level feedback: 그것을 실행함
- 그런데 안전하고, feasible하고, 에너지 효율적인 trajectory를 dense traffice에서 모든 동적 차량을 다 고려하면서 생성하는건 연산량이 너무 크다.

	- closed-loop controller랑 같이 informed rapidly-exploring random tree 방법이 나옴
		- sample efficiency를 높이고,
		- invalid한 reference trajectory를 repair한다.
	- 위협으로부터 순간적인 반응성을 실현하기 위해 자율주행에서 같은 안전의 해석을 공유하는 최적화 기반의 보완적인 planner와 controller가 나왔다.
	- 하지만 이 방법들은 replanning하는 동안 연산량이 너무 많다.

- 이 문제를 해결하는 방법
	- 안전하고 feasible한 EV의 trajectory를 생성하기 위해 longitudinal, lateral motion을 분리하는 것
		- 이 연구들은 motion 문제를 두 개의 독립인 1차원 움직임으로 분리했고,
		- 연산량을 개선시킴.
		- 하지만, longitudinal이랑 lateral 움직임 사이에 coordination이 없어서 안전 문제가 생김
		- 특히, EV가 dense traffic scenario에서 갑자기 차선을 변경하면서 다른 차량들과 충돌을 피하려고 할 때 중요하다.
	- 대안으로, 몇몇 연구들은 path-valocity를 분리해서 motion planning 문제를 path-time (s x t) 공간에서의 문제로 간략화 했다.
		- 이러한 기술은 
			- 정적인 장애물을 피하는 planning과
			- 동적인 장애물을 피하기 위한 경로를 따라 속도를 최적화하는 것을
			- 포함하고 있다.
		- 하지만, reference path는 속도 최적화를 제한한다
			- dense traffic에서 생성된 trajectory의 퀄리티에 영향을 줌.
		- 이 문제를 해결하기 위해, 상태랑 constrol 제약사항을 충족시키는 piecewise polynomial이 쉽게 최적화된다.
			- 그래서 빠른 재계획을 실현시킨다.
		- 이러한 연구들에서, planner는 smooth하고 feasible한 경로를 생성하기 위해 bicycle kinematic model의 differential flatness 특성에 의존한다.
		- kinematic model에 기반한 polynomial trajectory를 생성하는 것은 feasible하긴 하지만, nonholonomic 제약사항이 있는 nonlinear dynamic EV에게 actuator potential을 충분히 활용하지 못한다.
			- 그래서 control policy가 sub-optimal이게 된다.

- planning이랑 control 문제를 따로 다루는 대신, receding horizon control (RHC) 프레임워크를 써서 planning이랑 control을 하나의 최적화 문제로 통합함.
	- RHC는 system constraints를 자연스럽게 통합하는 general framework를 제공하고,
	- 미래의 사건을 예상하고,
	- objective function에 따라 encoding된 복잡한 task를 달성하는 control action을 취한다.
- 하지만, safety 조건을 만족하면서, 높은 태스크 성능을 달성하는 objective function이랑 constraints를 설계하는 것은 쉽지 않다.
	- safety 조건은 장애물에 대한 distance term을 RHC의 hard constraint로 encoding해서, 장애물과의 충돌을 막는다.
	- 결과적으로, EV는 정확한 dynamic model을 써서 장애물은 피하는 동시에, desired driving task를 달성할 수 있는 동적으로 feasible한 trajectory를 만들어낼 수 있다.
	- 하지만, 이러한 RHC 프레임워크에서 distance requirements로 표현된 반응성있는 안전 제약사항들은 도달 가능한 집합이 장애물과 교차할 때까지 최적화를 가두지 않는다.
	- 그럼에도 불구하고, reactive safety constraint setting에서, EV는 장애물가 가까워질때까지 걔네를 피하려고 액션을 취하지 않아서, 안전 문제를 보장하기 어렵게 한다.

- 충돌을 사전에 방지하기 위해, GPU 병렬 샘플링을 사용하는 sampling-based MPC 기법이 고안됨.
	- 근데 걔네는 MPC 프레임워크에서 동적인 물체를 설명할 수 없었음.
- 이러한 한계를 다루기 위해 control barrier function (CBF) 기반의 discrete nonlinear model predictive control (NMPC)이 제안됨.
	- 느리게 움직이는 상황에서 overtaking에 성공함.
- 그럼에도 불구하고, NMPC에서 최적화 하는건 연산량이 넘 크다
	- Hessian matrix의 역행렬을 풀어야 하기 때문.
	- 그래서 주로 최적화 horizon이 5~10초인 실제의 자율주행에는 적용하기 어려움.
- 효과적으로 최적화하기 위해서, multiplier 방식의 대안이 제안됨
	- 근데 이 방법은 다른 주위의 차량의 속도가 일정하다고 정해버림.
	- 특히, 주위 차량들이 non-deterministic multi-modal 행동을 취할 때 위험하게 됨.

- 주위 차량의 불확실한 행동을 고려했을 때, 연구자들은 EV가 주위의 HV와의 충돌을 피하게 하기 위해 stochastic MPC 방법을 제안함.
	- 이 방법들은 chance-constraint 기반의 safety module을 만들기 위해 HV의 경로의 확률 분포를 예측한다.
	- 추가로, Branch MPC 기술은 trajectory tree를 최적화 해서 HV의 multi-modal 행동을 설명하는데에 사용됨.
	- 이런 방법들이 주위 HV들과 안전하게 상호작용하는 것을 가능하게 만들긴 했는데...
	- 최적의 경로는 보수적인 경향이 있다.
		- 운전 효율성이 저하됨.


- 그래서 이를 극복하기 위해 batch MPC 프레임워크가 제안됨
	- NMPC의 연산 효율성도 개선하고
	- highway scenario에서 다른 차량의 불확실한 multi-modal 행동을 다룸
	- 병렬 경로 최적화를 해서 SV의 multi-modal 행동을 다루고, 사전에 충돌을 예방할 수 있다.
	-  alternating minimization 알고리즘으로 feasible한 경로를 실시간으로 생성할 수 있긴 한데...
	- 단점:
		- 그게 쓰는 trajectory 평가 알고리즘은 candidate trajectory들 간에 빈번하게 바뀔 수 있다.
			- 그러면 난폭운전하게 됨




# 이 논문
- 연산 효율성이 좋은 spatiotemporal receding horizon control (ST-RHC) 제안
	- 안전하고, 
	- dynamically feasible하고,
	- energy-efficient한
	- trajectory를 control space에서 생성함.
- proactive interaction을 고려한 embodied spatiotemporal safety barrier module을 고안함.
	- 다른 vehicle의 trajectory를 예측하는데 발생하는 부정확성을 완화하기 위해!
- motion planning이랑 control 문제를 constrained nonlinear optimization 문제로 구성함.
	- multiple shooting을 다루는 기존의 optimization solver를 같이 활용할 수 있다.


# 실험
- systhetic
- real-world dataset
- in dense traffic
- 

# 결과
- dense traffic에서 여러 task들이 높은 성능과 안전을 보장한 채 실시간으로 가능함.

# Contribution
- ST-RHC 제안: 연산 효율이 좋음
	- multiple shooting method를 활용함
		- 연산 효율적이고
		- numerical stability해서
		- 복잡한 교통 상황에서 실시간으로 정확한 달성할 수 있게 함
- spatiotemporal safety barrier module 고안
	- uncertain Human-driven Vehicle이 있을 때 Ego vehicle에게 proactivity랑 safety를 부여함
		- safety constrains 기반의 barrier function을 고안.
		- 다른 vehicle의 불확실한 운전 행동에서 부정확한 예측의 효과를 완화하기 위해 spatio-temporal 정보를 활용함.
		- 그래서 EV가 덜 보수적인 행동을 하게 함
- 시뮬레이션에서 성능 좋은거 실험함
	- 성능 좋아지고
	- proactive obstacle avoidance (미리 장애물 피할 수 있음)