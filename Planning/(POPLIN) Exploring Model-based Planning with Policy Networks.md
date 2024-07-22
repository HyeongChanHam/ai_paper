Tingwu Wang, Jimmy Ba
U Toronto, Vector Institute

# 배경
- model-predictive control이나 online planning과 함께 Model-based RL 하니까 locomotion control task에서 sample efficiency랑 성능 오르는 것도 좋았다.

# 문제
- 그럼에도 불구하고, 기존의 planning method는 action space에서 랜덤하게 생성된 candidate sequence로부터 검색을 하는데, 이 방법은 복잡한 고차원의 환경에서는 비효율적이다.

# 이 논문
- 새로운 Model-based RL 알고리즘 제안
- model-based policy planning (POPLIN)
	- policy network를 online planning과 함께 합쳤다.
- 우리는 매 time step마다 action planning하는 것을 neural network를 이용해서 최적화 하는 문제로 봄

- policy network에서 초기화된 action sequence에 대해 최적화하는 것을 실험해봄
- policy network의 parameter에 관해 바로 online 최적화 하는 것도 실험해봄

# 결과
- POPLIN이 MuJoCo에서 성능 좋았음
	- 3배 sample efficient하다
- 우리 알고리즘의 효율성을 설명하기 위해
	- parameter space의 optimization surface가 action space보다 더 부드럽다는 것을 보였다.
- 그리고 distilled dpolicy network가 특정 환경에서는 test time에 expansive MPC 없이도 더 효과적으로 적용될 수 있다는 것을 보였다.

---

# 배경
- model-based RL은 세상의 dynamics를 환경과 반복적으로 interaction 하면서 학습한다.
- agent는 latent dynamic를 통해 online planning 할 수 있고,
- imaginary data로 상호작용 할 수 있고,
- dynamics를 통해 컨트롤러를 최적화 할 수 있다.
	- sample efficiency가 좋아진다!
- 하지만, 현실에서 MBRL은 복잡도가 높아지면서 scaling이 잘 되지 않는다.
- 그리고 dynamics의 modeling error가 시간이 지나면서 누적이 되어서 성능을 제한한다.
- 그래서 최근의 강화학습들은 다 model-free에서 발전되었다.


- Deep Learning이 발전하면서, NN 기반의 dynamics를 써서 MBRL하는 연구가 나왔다.
- 그들 중에 Model-predictive control (MPC)를 쓰는 random shooting (RS) 알고리즘이 robust하고 scalability하다고 한다.

- Shooting algorithm
	- agent는 랜덤하게 action sequence를 만들어낸다.
	- dynamics를 써서 미래 state를 예측한다.
	- 가장 기대 보상이 높은 sequence의 첫 번째 action을 취한다.

- 하지만, RS는 model-free controller에 비해 asymptotic performance가 더 안좋다.
- 그리고 PETS 저자들이 말하길, RS의 성능은 latent dynamics의 성능에 직접적으로 영향을 받는다고 한다.
	- 그래서 걔들은 model의 불확실성을 잡기 위해 probabilistic ensemble을 제안함.
	- 근데 높은 차원에서는 그리 효율적이지 못함.

- model-predictive control이나 online planning과 함께 Model-based RL 하니까 locomotion control task에서 sample efficiency랑 성능 오르는 것도 좋았다.

# 문제
- 그럼에도 불구하고, 기존의 planning method는 action space에서 랜덤하게 생성된 candidate sequence로부터 검색을 하는데, 이 방법은 복잡한 고차원의 환경에서는 비효율적이다.

# 이 논문
- 새로운 Model-based RL 알고리즘 제안
- model-based policy planning (POPLIN)
	- policy network를 online planning과 함께 합쳤다.
- 우리는 매 time step마다 action planning하는 것을 neural network를 이용해서 최적화 하는 문제로 봄

- policy network에서 초기화된 action sequence에 대해 최적화하는 것을 실험해봄
- policy network의 parameter에 관해 바로 online 최적화 하는 것도 실험해봄

- MPC를 쓰는 sota MBRL 알고리즘은 실시간으로 동작 못한다.
- 그래서 MPC 없이 빠르게 control하는 policy network distillation schemes를 실험함.

- Contribution
	- dynamics를 모르는 고차원의 locomotion control 문제에서 MPC의 proposal을 만들어내기 위해 policy network를 적용함
	- planning을 neural network로 최적화하는 것으로 정의했다. 그리고 policy planning을 parameter space에서 하는 것을 제안함.
	- planned trajectory에서 policy network distillation을 탐색함. distilled policy network 혼자서도 online planning 비용 없이도 높은 성능 달성함.

# 결과
- POPLIN이 MuJoCo에서 성능 좋았음
	- 3배 sample efficient하다
- 우리 알고리즘의 효율성을 설명하기 위해
	- parameter space의 optimization surface가 action space보다 더 부드럽다는 것을 보였다.
- 그리고 distilled dpolicy network가 특정 환경에서는 test time에 expansive MPC 없이도 더 효과적으로 적용될 수 있다는 것을 보였다.

---

# 관련 연구
- Dyna: 실제 환경에서 샘플링 하고, 환경의 학습된 모델에서 컨트롤러를 최적화 함.
- PILCO: Gaussian Process로 dynamics를 모델링함, surrogate expected reward를 최적화 함
	- 간단한 환경에서는 잘 풀리지만, 차원의 저주 문제를 겪는다. (고차원에서는 잘 안됨)
- GPS: Guided Policy Seaerch
	- iLQG를 local controller로 사용
	- knowledge를 policy newral network로 distill했다.
- SVG: stochastic value gradient를 사용함
	- stochastic policy network는 off-policy 데이터로 학습된 dynamics network를 통해 back propagation을 해서 최적화 할 수 있다.
- PETS: 최근 random shooting 방법이 robustness랑 effectiveness를 보여주었다.


---

# 3. Background
## 3.1. Reinforcement Learning
- 
## 3.2. Random Shooting Algorithm and PETS
- 우리가 제안한 알고리즘은 random shooting 알고리즘 기반이다.
	- 이전에 생성한 실제 trajectory로부터 데이터셋을 모은다.
	- 에이전트는 NN의 ensemble을 학습한다.
	- Planning에서, 에이전트는 랜덤하게 K개의 action sequence를 생성한다.
		- 각 action sequence는 planning horizon $\tau$ 만큼 control signal을 포함하고 있다.
		- 그중 현재 dynamics network에서 기대 보상이 가장 큰 action sequence를 선택한다.
		- model predictive control 알고리즘에서 RS는 첫 번째 액션만 실행하고, 다시 planning한다.
	- PETS에서는 CEM을 사용함
		- 최근의 CEM iteration에서 가장 좋았던 sequence들 근처에 있는 것들을 다시 샘플한다.
- 

# 4. Model-Based Policy Planning
- 2개가 있음
	- POPLIN-A: model-based policy planning in action space
	- POPLIN-P: model-based policy planning in parameter space

## 4.1. Model-based Policy Planning in Action Space
- policy network를 좋은 initial action 분포를 생성하는 데에 사용한다.
- 먼저 policy network가 expected trajectory들에다가 action sequence를 생성하고 나서
- candidate action에다가 Gaussian noise를 추가한다.
- CEM을 써서 noise distribution의 평균이랑 표준편차를 fine tune 한다.



## 4.2. Model-based Policy Planning in Parameter Space

## 4.3. Model-predictive Control and Policy Control

## 4.4. Policy Distillation Schemes