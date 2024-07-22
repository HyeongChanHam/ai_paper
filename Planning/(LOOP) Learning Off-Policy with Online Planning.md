Harshit Sikchi, Wenxuan Zhou, David Held
CMU Robotics Institute (RI)
CoRL 2022

# 배경
- 데이터가 적고 위험에 민감한 도메인에서 강화학습은 성능이 좋고 유연한 배포 정책을 필요로 한다.
	- 배포하는 동안 쉽게 constraint를 포함할 수 있게!
- 이런 정책의 분류중 하나로 semi-parametric H-step lookahead policy가 있다.
	- terminal value 함수와 함께 고정된 길이의 horizon에서 dynamics model에 대해 trajectory optimization을 써서 action을 선택함.


# 이 논문
- model-free off-policy 알고리즘에서 학습된 모델과 terminal value function과 함께 새로운 종류의 H-step lookahead를 조사함
- Learning Off-Policy with Online Planning (LOOP)


---

# 배경
- Off-policy RL은 sample 효율이 좋고 다른 source로부터의 data를 활용할 수 있어서 robotic application에 자주 활용되었다.
- Model-free off-policy 알고리즘은 replay buffer에서 transotion을 뽑는다.
	- 그리고 value function을 학습한다.
	- 그리고 policy를 이 value function에 따라 업데이트한다. (SAC 방식임)
	- 그래서 policy의 성능은 value function의 estimation에 따라 크게 달라진다.

- 하지만, off-policy data로부터 정확한 value function을 학습하는 것은어렵다. (특히 Deep RL에서)
	- 왜냐하면 
		- overestimation bias
		- delusional bias
		- rank loss
		- instability
		- divergence
		- 때문
- continuous control에서 model-free off-policy 알고리즘의 다른 단점
	- policy는 feed forward Neural network로 parametrized되는데,
	- 배포하는 동안 flexibility가 부족하다.



- model-based RL에서 이전 연구들은 dynamics model을 써서 다른 방향으로 off-policy 알고리즘을 향상시키는 연구를 해왔다.
- 그중 하나의 방법으로,
- H-step lookahead 정책을 사용하는 방법이 있음
	- 매 timestep마다, H-step lookahead 저액은 dynamics 모델을 H-step만큼 현재 상태부터 미래로 rollout한다.
		- return이 가장 높은 action sequence를 찾기 위해서임.
	- 이 trajectory 최적화 과정동안에 rollout 끝에 terminal value function이 붙는다.
		- 고정된 horizon 너머의 return 추정값을 제공하기 위해!

- 이런 방식의 online planning은 우리에게 어느정도의 설명가능성을 준다.
	- fully parametric method에서는 못함
	- 우리가 배포하는 동안 constraint를 같이 고려할 수 있게 만들어준다.

- 




- 데이터가 적고 위험에 민감한 도메인에서 강화학습은 성능이 좋고 유연한 배포 정책을 필요로 한다.
	- 배포하는 동안 쉽게 constraint를 포함할 수 있게!
- 이런 정책의 분류중 하나로 semi-parametric H-step lookahead policy가 있다.
	- terminal value 함수와 함께 고정된 길이의 horizon에서 dynamics model에 대해 trajectory optimization을 써서 action을 선택함.

# 이전 연구



# 이 논문
- model-free off-policy 알고리즘에서 학습된 모델과 terminal value function과 함께 새로운 종류의 H-step lookahead를 조사함
- Learning Off-Policy with Online Planning (LOOP)