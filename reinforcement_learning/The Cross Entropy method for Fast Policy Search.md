S Mannor, R Rubinstein, Y Gat
ICML 2003

# 배경

# 이 논문
- policy space에서 최적화하는 것을 기반으로 MDP를 학습하는 프레임워크 제안.
	- 상대적으로 느린 gradient-based 최적화 방식 대신,
	- 우리는 fast Cross Entropy 방식을 사용함.



---

# 방법
- 핵심: 안좋은 에피소드는 버리고 좋은 에피소드에서 학습하는 것!
1) 현재 모델과 환경으로 N번의 에피소드를 돌린다.
2) 매 에피소드마다 총 보상을 계산하고, 보상의 경계를 정한다.
	1) 보통 모든 보상의 백분위수를 사용함.
3) 경계 밖의 에피소드는 모두 버린다.
4) 남은 에피소드를 기반으로 학습하고, 결과를 비교한다.
5) 만족스러운 결과가 나올 때까지 1~4단계를 반복.

- 백분위수 이상의 좋은 에피소드들만 남겨서 그걸 가지고 action 값에 대해 CrossEntropyLoss를 적용한다.
	- policy gradient method.

# 이론적 배경
- importance sampling theorem에 기초를 두고 있다.
- Importance sampling 수식
- $$\mathbb{E}_{x-p(x)}[H(x)]=\int_x p(x)H(x)dx=\int_x q(x)\frac{p(x)}{q(x)}H(x)dx=\mathbb{E}_{x-q(x)}[\frac{p(x)}{q(x)}H(x)]$$
	- $H(x)$: policy $x$에 의해 얻어진 보상 값
	- $p(x)$: 모든 가능한 정책의 분포
- 모든 정책들을 뒤져가며 보상을 최대화 하는 것이 아니라, 두 확률분포의 거리를 줄이기를 반복하면서 $\frac{p(x)H(x)}{q(x)}$를 근사하는 방법을 찾고자 함. 두 확률분포 간 거리는 KL divergence로 계산.
- $$KL(p_1(x) ||p_2(x))=\mathbb{E}_{x-p_1(x)}log\frac{p_1(x)}{p_2(x)}=\mathbb{E}_{x-p_1(x)}[logp_1(x)]-\mathbb{E}_{x-p_1(x)}[logp_2(x)]$$

# 실험
- cartpole vs frozenlake
	- cartpole에서는 잘 되는데 frozenlake에서는 잘 안됨
	- 이유
		- cartpole은 한 번 움직이면 보상을 줌. (오래 버티는 것이 목표)
		- frozenlake는 각 스텝마다 보상을 주지 않고, 목표 지점에 가야 1점을 주고, 중간중간에 함정도 있다.
			- 백분위수 기준으로 좋은 학습 데이터를 뽑을 수가 없다.
				- 개선의 여지가 없다...!
- Cross-Entropy method의 한계
	- 학습을 위해서, 에피소드는 유한해야 하고, 가급적이면 짧아야 한다.
	- 좋은 에피소드와 나쁜 에피소드를 분리할 수 있을 정도로 학습 에피소드들의 총 보상은 충분한 분산을 가져야 한다.
	- 에이전트가 성공했는지 실패했는지 중간 지시자가 없다.
