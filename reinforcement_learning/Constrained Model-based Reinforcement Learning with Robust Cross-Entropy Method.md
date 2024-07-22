Z Liu, H Zhou, B Chen, S Zhong, M Hebert, D Zhao
CMU


- Constrained Model-based Reinforcement Learning via Robust Planning
	- ICML 2022


# 배경
- safe reinforcement learning
	- 희박한 constraint 위반 시그널이 있는 문제


# 이 논문
- 상당히 적은 수의 violation budget이 주어졌을 때
	- 환경의 system dynamics랑 constraints를 모르는데
	- 환경을 효과적으로 explore할 수 있게 만듬
	- model-based approach

- neural network 앙상블 모델 사용
	- prediction uncertainty를 측정하기 위해!
- MPC를 기본 control framework로 사용함.

- constraints간에 model unceratinty를 고려하면서 control sequence를 최적화하기 위한 robust cross-entropy 방법을 제안함.

# 결과
- baseline에 비해 더 안전하게 태스크를 완료하게 됨
- 다른 constrained model-free RL 방식과 비교했을 때 훨씬 더 나은 sample efficiency를 달성함.

---

# 배경
- safe reinforcement learning
	- 희박한 constraint 위반 시그널이 있는 문제

- 학습할 때, 에이전트가 위험한 state로 빠지는 것을 막기 어려움.
- 그래서 constrained, 아니면 안전한 RL 연구하는게 중요함.

- 일부 연구가 이를 prior knowledge를 알고 있는 상태에서 풀지만, 이는 현실적이지 못함.
- system dynamics랑 명확한 constraint function 없이 safety constraints를 어떻게 강제할까??
	- 예를 들어 Safety Gym
		- 로봇은 목적지로 navigate하면서 위험한 지역은 피해야 한다.
		- 환경의 dynamics model은 모르고
		- 로봇은 constraint 위반할 때 indicator signal만 받게 됨.
		- observation은 센서 데이터여서 observation space를 constraint violation으로 매핑해서 표현하기 어려움...
	- 이런 문제 풀려고하는데 현재 문제점
		1) model-free constrain RL 방법들은 sample efficient하지 못하다.
			- 학습하기 위해 많은 수의 unsafe data를 모아야 되고, 이를 위해서 계속 safety constraints를 위반해야 한다.
			- 최종 policy 가 제약조건을 보장하기 어렵다.
		2) task objective랑 safety object가 서로 상충됨
			- 기존의 reward 최적화 기준을 보상이랑 제약조건 위반 cost로 변환하는 방법의 경우, policy 최적화 과정을 방해함
		3) black-box constraint 함수랑 unknown environment dynamics model이 문제를 최적화하기 어렵게 만든다.
			- 특히 observation space가 고차원일 때!
- 대부분의 기존의 model-based constrained RL은
	- known prior dynamics model을 가정하거나,
	- constraint function의 알려진 구조를 가정함. (특정 형태의 공식으로 표현되게)


# 관련 연구
- CPO

# 이 논문
- 상당히 적은 수의 violation budget이 주어졌을 때
	- 환경의 system dynamics랑 constraints를 모르는데
	- 환경을 효과적으로 explore할 수 있게 만듬
	- model-based approach

- neural network 앙상블 모델 사용
	- prediction uncertainty를 측정하기 위해!
- MPC를 기본 control framework로 사용함.

- constraints간에 model unceratinty를 고려하면서 control sequence를 최적화하기 위한 robust cross-entropy 방법을 제안함.


- Contribution
	1) continuous state, action 공간에서 constrained model-based RL 을 제안함.
		- 거의 최적의 성능과 거의 0에 가까운 constraint violation rate를 달성함.
		- system dynamics랑 constraint function에 대한 추가적인 가정 없이 CMDP 문제를 만듬.
			- 원래는 제한된 unsafe sample이랑 sparse constraint violation 표시 시그널에서 모아진 데이터로부터 학습해야 함.
	2) Robust cross-entropy (RCE) 최적화 방법 제안
		- uncertainty-aware dynamics model에서 동작
			- unsafe behavior로 이어질 수 있는 dynamics prediction error를 다룸
		- 이 RCE 방법이 다른 model-free나 model-based 방법에 비해 sample efficiency도 좋고 성능도 좋다!


# 결과
- baseline에 비해 더 안전하게 태스크를 완료하게 됨
- 다른 constrained model-free RL 방식과 비교했을 때 훨씬 더 나은 sample efficiency를 달성함.

---

# CCEM (Wen & Topcu, 2018)과 차이점
- 이거랑 비슷하긴 한데 2가지 측면에서 다른다.
	1) 우리는 worst-case cost를 고려하고 feasible set을 고르기 위해 최대 cost를 최소화하는 것을 목적으로 두었지만, CCEM은 기댓값을 계산하였음.
	2) CCEM의 주된 application은 model-free RL에서 policy parameter를 최적화하는 것임. 근데 우리는 model-based RL setting에서 planning horizon 내에 action sequence를 바로 최적화한다.


