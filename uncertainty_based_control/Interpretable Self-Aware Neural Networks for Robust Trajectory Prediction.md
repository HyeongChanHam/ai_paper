
CoRL 2022

# 배경
- NN가 많은 도메인에서 predictive model 로 잘 됨

# 문제
- OOD data에서 over-confident되는 문제가 있다.
- 안전이 중요한 어플리케이션을 위해서 epistemic uncertainty를 반드시 정확하게 측정해야 한다.
	- 이를 system self-awareness의 레벨이라고 한다.
- epistemic uncertainty 를 정량화하는데에 주로 학습에 OOD 데이터를 필요로 하거나, 추론과정에 여러 NN Forward pass가 필요하다.
	- 현실의 실시간 동작에 적절하지 않음.
- 기존의 방법들은 측정된 uncertainty의 해석가능성이 부족했다.


# 이 논문
- trajectory prediction setting에서 낮은 차원의, 해석 가능한 latent space에서 epistemic uncertainty를 측정하는 evidential deep learning을 제안.
	- semantic concept 간에 unceratinty의 분포를 해석 가능한 방법 제시
		- past agent behavior
		- road structure
		- social context


---

# 배경
- NN가 많은 도메인에서 predictive model 로 잘 됨

# 문제
- OOD data에서 over-confident되는 문제가 있다.
- 안전이 중요한 어플리케이션을 위해서 epistemic uncertainty를 반드시 정확하게 측정해야 한다.
	- 이를 system self-awareness의 레벨이라고 한다.
- epistemic uncertainty 를 정량화하는데에 주로 학습에 OOD 데이터를 필요로 하거나, 추론과정에 여러 NN Forward pass가 필요하다.
	- 현실의 실시간 동작에 적절하지 않음.
- 기존의 방법들은 측정된 uncertainty의 해석가능성이 부족했다.


- trajectory prediction에서 OOD input을 구별하는 것은 어려움
	- 데이터의 high dimensionality 때문
	- 도로의 모든 상황을 알지 못하기 때문에, OOD dataset을 수작업으로 만들기 어려움.

- challenges
	- 학습하는데 명확한 OOD data가 부족함.
	- 추론할 때 효율성 보장해야 함.
	- uncertainty의 해석가능성
# 이 논문
- trajectory prediction setting에서 낮은 차원의, 해석 가능한 latent space에서 epistemic uncertainty를 측정하는 evidential deep learning을 제안.
	- semantic concept 간에 unceratinty의 분포를 해석 가능한 방법 제시
		- past agent behavior
		- road structure
		- social context

- Evidential deep learning은 epistemic uncertainty를 측정하기 위해 second-order distribution (Dirichlet distribution)의 parameter를 추정한다.
- 학습할 때 OOD data 없이 하기 위해서 normalizing flow를 써서 학습된 Dirichlet distribution을 constrain한다.
	- [[PostNet]]처럼
	- 각 클래스의 밀도가 학습할 때 각 클래스의 샘플 수로 되도록 강제함.


- trajectory prediction에서 human behavior는 discrete mode로 모델링 된다.
	- high-level maneuvers (accelerating, braking, turning)
	- 이 아이디어로 discrete mode를 trajectory의 epistemic uncertainty 측정하는 [[PostNet]]에 쓰겠다!

- 학습된 epistemic uncertainty에서 interpretability를 다루기 위한 아이디어
	- trajectory prediction에서 높은 epistemic uncertainty는 **unfamiliar input behabiors, road structures, or social contexts**에서 오는 것이다!
	- 학습된 epistemic uncertainty를 이 semantic에 분포함.

# Method

Input $x$
- road structure (high-definition (HD) map)
- past trajectory and current state for the agent of interest
- past trajectory information for the surrounding agents

agent의 state
- speed $v$
- acceleration $a$
- heading change rate $h$

trajectory prediction task의 목표
- 미래의 T 시간동안 2D 위치 $y\in\mathbb{R}^{2\times T}$ 


# Discrete mode로 한다는게 이해가 안가는데?? Trajectorn++ 읽고오자!