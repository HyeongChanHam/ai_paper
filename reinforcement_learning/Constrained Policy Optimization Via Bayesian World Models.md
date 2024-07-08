Y. As, ..., Andreas Krause
ETH Zurich
ICLR 2022

# 배경
- 고위험의 현실세계에 RL을 배포할 때 sample efficiency랑 safety를 향상시키는 것은 중요하다.


# 이 논문
- LAMBDA 제안
	- 사람이 정한 safety constraints를 준수하기 위해 Bayesian model based policy optimization 알고리즘임.
		- policy optimization 하는데 model-based 접근
		- constrained MDP로 모델링해서 안전에 중요한 태스크 하기 위함.
	- 우리는 Bayesian world model을 씀
		- 이 Bayesian world model의 결과 uncertainty를 통해서
			- task 목적의 낙관적인 상한을 최대화하고,
			- safety constraint의 비관적인 상한도 최대화 하는데 썻다.
	- Policy search 하는 데에 Augmented Lagrangian 방법을 씀
		- optimistic/pessimistic bound로 constrained optimization problem을 품.

# 실험
- Safety-Gym benchmark

# 결과
- 다른 방법에 비해
	- sample efficiency
	- safety
	- task-solving
	- 이 좋다.


---

# 배경
- 고위험의 현실세계에 RL을 배포할 때 sample efficiency랑 safety를 향상시키는 것은 중요하다.
	- 그래서 조심스럽게 행동하고 주위의 안전을 스스로 확신할 수 있도록 효과적이고 안전하게 학습할 수 있는 에이전트를 연구한다.

- 강화학습에서 safety critical environment를 모델링 하는 보편적인 페러다임: Constrained MDP (CMDP)
	- CMDP: reward 뿐만 아니라 추가적인 cost function을 도입
		- 안전하지 않은 state-action pair를 나타내는 cost
		- 이 cost를 지켜서 위험한 사건이 일어날 가능성을 지킨다.
	- tabular case에서 CMDP는 linear program(LP)로 풀린다.
	- tabular가 아닌 문제를 풀기 위해 기존의 방법들은 model-free 방식을 사용해왔음.
		- 이 방법들은 제약조건을 만족하는 정책으로 수렴한다고 함.
		- 근데, unconstrained MDP인 model-free 접근법들처럼, 얘네들은 sample 을 많이 뽑아야 함.
			- 환경이랑 상호작용 하는데 그게 잠재적으로 위험할 수도 있음.
	- 그래서 이 sample efficiency를 해결하기 위해서 대안으로 model-based RL을 씀.
		- 그중 Bayesian을 써서 추정 모델의 불확실성을 정량화한 연구가 있음
			- exploration을 guide할 수 있다.
		- unconstrained 에서 Bayesian은 많이 연구되었지만, constrained에서는 아직 많이 연구되지 않았음.

# 이 논문
- LAMBDA 제안
	- 사람이 정한 safety constraints를 준수하기 위해 Bayesian model based policy optimization 알고리즘임.
		- policy optimization 하는데 model-based 접근
		- constrained MDP로 모델링해서 안전에 중요한 태스크 하기 위함.
	- 우리는 Bayesian world model을 씀
		- 이 Bayesian world model의 결과 uncertainty를 통해서
			- task 목적의 낙관적인 상한을 최대화하고,
			- safety constraint의 비관적인 상한도 최대화 하는데 썻다.
	- Policy search 하는 데에 Augmented Lagrangian 방법을 씀
		- optimistic/pessimistic bound로 constrained optimization problem을 품.
	- 실제 환경 대신 model이 만든 trajectory를 통해서 reward 뿐만 아니라 unsafe한 것도 경험을 해서 safe policy를 학습한다.

# Contribution
- world model을 통해 1) task랑 2) safety objective 둘 다 기울기 역전파를 통해 constrained optimization이 가능함을 보임. (-> Augmented Lagrangian을 사용함.)
- optimism for exploration이랑 pessimism for safety 사이에 trade-off 를 위해 probabilistic world model을 사용함.

# 실험
- Safety-Gym benchmark

# 결과
- 다른 방법에 비해
	- sample efficiency
	- safety
	- task-solving
	- 이 좋다.



# Related Work
## safety 관련해서 다양한 정의
1) reversible MDP
	- safety를 state 간에 reachability로 정의함.
	- backup policy를 이용해 safety를 보장함.
2) robust MDP
	- worst-case transition model이 있는 상황에서 성능을 최대화하는 것
3) non-tabular CMDP
	- safety requirements를 사전에 threshold로 정해 지켜져야 하는 cost function으로 하는 것으로 정의.

- 이 논문에서는 3) non-tabular CMDP 문제임.
- 기존 방법들은 model-free 방식이랑 function approximator를 써서 constrained policy optimization을 풀었다.
	- model-based 로 하면 sample efficiency를 높일 수 있다는 것을 여기서 보일 것임.

## Safe model-based RL
- Bayesian modeling으로 저차원의 연속 컨트롤 문제에 적용한 방법: Gaussian Process (GP)로 모델 학습함.
	- (F Berkenkamp, A Krause)는 GP로 Lyapunov function 주위에 confidence interval을 만듬
		- 이걸 써서 정책이 항상 Lyapunov-stable region of attraction 안에 있게 최적화함.
	- (Koller, ...)같은 애들은 GP 모델로 MPC 안에서 행동의 안전을 보증함.
	- (Liu, ...)는 MPC를 아래 방법 써서 고차원의 연속 제어 문제에 적용함.
		- Neural Networks
		- Cross Entropy Method (CEM)
		- rejection sampling
		- 이거 3개 써서 안전한 행동들의 리턴 기대값을 최대화하도록 함.
		- 근데, 단점으로 짧은 horizon에서 online으로 planning 하고 critics도 안쓰면 행동이 근시안적이게 될 수 있음.
	- (Zanger, ...)는 NN이랑 constrained model-based policy optimization을 함.
		- 근데 optimistic-pessimistic framework에서 모델 불확실성은 쓰지 않음.
		- 대신 policy optimization에서 에러가 많을 수 있는 model prediction의 영향을 제한함.
		- 정확한 모델 prediction을 쓰면 policy learning 에 도움 될 수 있지만,
		- 이 방법은 그 정확한 예측이 드물 때 epistemic uncertainty의 장점을 고려하지 않음