J Achiam, D Held, A Tamar, P Abbeel

# 배경
- 최근 딥러닝을 통해서 NN policy가 고차원의 컨트롤 태스크를 잘 수행하게 되었다.


- 강화학습은 시행착오를 통해 학습한다.
- 최근 deep RL은 학습하는 동안 **어떤 행동이든 탐색 가능하다고 가정**한다.
- 근데 현실에서는 에이전트가 완전 자유롭게 하지는 못할 것이다.
	- RL agent에게 **safe exploration**은 중요하다!
	- 이 safety를 확보하는 자연스러운 방법은 **constraint**를 통한 것이다.

# Related Works
- 강화학습에 constraint를 쓰는 것은 Constrained MDP임.
	- agent가 auxiliary cost의 기댓값을 충족시켜야 함.
	- model 을 알고 있는 유한한 CMDP에서 최적 정책은 Linear Programming으로 얻을 수 있음.
	- 근데, high-dimensional control에서는 안됨...ㅠ

- 강화학습 할 때, reward 함수만으로 정책 학습하는 것 보다, constraints도 같이 써서 학습하는게 더 편할 것이다.
	- 예를 들어, 사람 주변에 물리적으로 상호작용 하는거면 안전 제약을 만족해야 할 것임.

- 오늘날 policy search 알고리즘은 고차원의 control task 잘 풀고 있음.
	- CMDP에서 policy search하기 위한 heuristic 알고리즘도 있음.
	- primal-dual 방식 기반의 접근도 constraint를 만족하는 정책으로 수렴한다고 함.
# 문제
- 최근 policy search 알고리즘들은 고차원 제어가 가능하게 되었지만, constrained setting은 아직 고려하지 않고 있음.
- **학습하는 동안 모든 정책이 constraint를 만족할 것임을 보장**하는 **continuous** CMDP에서의 policy search 방식은 없다.

# 이 논문
- Constrained Policy Optimization (CPO) 제안
	- 매 iteration마다 near-constraint를 만족함을 보장하는 constrained 강화학습 하는 방법
	- 학습하는 전체 기간동안 정책 행동에 대해 보장할 수 있게 만들었음.


# 실험
- 고차원의 simulated robot locomotion task
	- 보상을 최대화
	- constraint도 동시에 만족해야 함
1) Humanoid-Circle
2) Point-Gather