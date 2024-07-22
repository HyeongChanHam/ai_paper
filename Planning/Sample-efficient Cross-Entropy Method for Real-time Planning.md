C Pinneri, S Sawant, S. Blaes, J. Achterhold, J Stuckler, M. Rolinek, G. Martius
Max Plank
CoRL 2021

# 배경
- model-based RL에서 Cross-Entropy Method (CEM)같은 Trajectory optimizer는 고차원의 control task나 sparse-reward 환경에서도 설득력있는 결과를 낼 수 있다.

# 문제
- 걔네들의 sample 비효율성 때문에 실시간 planning과 control에 사용되는데 어려웠다.
# 이 논문
- 빠른 planning을 위해 향상된 버전의 CEM 제시
	- temporally-correlated action이랑 memory를 포함한 새로운 addition
	- 2.7~22배 덜 샘플링한다.
	- 성능도 1.2~10배 더 높다.

---

# 배경
- 고차원 시스템에서 Model-based RL은 trajectory optimize를 하기 위해 population-based 알고리즘을 많이 썼다.
- Sampling-based 방법도 cost function이 미분 불가능할 때 자주 쓰였다.
- 이러한 방법들의 중요한 매력은 몇몇 중요한 요소에 있다.
	- black-box function을 최적화 할 수 있다
	- robustness가 높다
		- hyperparameter tuning 에 민감하지 않다.
	- gradient 정보가 필요하지 않다.
	- local optima에 빠질 가능성이 적다.
- Cross-Entropy Method (CEM)이 1990년대에 처음 소개되었다.
	- stochastic하고
	- derivate-free하고
	- global optimization technique이다.
	- 근데 최근에 model-based RL 커뮤니티에서 관심을 얻었음
	- trajectory 최적화 하는데 CEM은 유망한 metaheuristic이다.
		- 학습된 모델에서도 잘 동작함
		- model-free RL에 비해 비슷하거나 더 좋은 성능을 냄.

- model-based RL에서 Cross-Entropy Method (CEM)같은 Trajectory optimizer는 고차원의 control task나 sparse-reward 환경에서도 설득력있는 결과를 낼 수 있다.

# 문제
- 근데 문제는 population-based optimizer들은 태생적인 문제가 있었다.
	- 실시간 planning이랑 control하기 어렵다.
	- 학습된 모델에 대해서도말이다.
	- 연산량이 너무 높다...!
- CEM같은 heuristics는 objective function을 최소화 하기 위해 많은 수의 샘플이 필요하다.
	- 
- 
- 걔네들의 sample 비효율성 때문에 실시간 planning과 control에 사용되는데 어려웠다.


# Motivation
- CEM 같은 zeroth-order optimizer를 가지고 실시간 플래닝을 할 수 있을까?

# 이 논문
- 빠른 planning을 위해 향상된 버전의 CEM 제시
	- temporally-correlated action이랑 memory를 포함한 새로운 addition
	- 2.7~22배 덜 샘플링한다.
	- 성능도 1.2~10배 더 높다.

- Contributions
	- iCEM 제시
		- CEM보다 더 빠르고 sample-efficient하고 성능이 좋다.
		- MBRL이 시뮬레이션이랑 현실에서 하는 것의 갭을 줄일 수 있다.
	- CEM에 비해 향상된 요소를 자세하게 분석함
	- MuJoCo simulator에서 테스트함.
		- 90% 성공률과 13.7배 낮은 샘플 수를 사용
		- CEM에 비해 4배 더 높은 평균 성능을 달성


# 실험
- model error를 최소화하기 위해 ground truth dynamics에서 실험함.
- 그리고 PlaNet에서 학습된 모델에다가 붙였을 떄도 실험함.

---

# 관련 연구
- 많은 Model-based RL이랑 motion planning 연구에서,
- gradient descent 안쓰고도 system을 control 할 수 있다는 것을 보였다.
- Evolution Strategies (ES)가 RL에서 주목을 받았다.
	- 이 덕분에 population-based 방법이 policy gradient 나 supportive guidance 방법의 대안으로 나왔다.
- sampling 기반의 방법도 Model-predictive control (MPC)에 사용되었다.
	- Model predictive Path Integral (MPPI) control


- Cross-Entropy Method (CEM)도 direct policy optimization이랑 planning에 사용되었다.
	- 빠르게 random tree를 탐색하는 성능 향상시키기 위해


- (Duan): CEM이 다른 정교한 Covariance Matrix Adaptation ES (CMA-ES)에 비해 더 좋다고 말함.
	- CMA-ES는 연산량이 엄청 큼
	- 반면에 CEM은 planning horizon에 독립적으로 샘플링됨
		- 오직 diagonal covariance matrix만 필요함.

- 우리 연구는 CEM을 실시간 결정을 할  수 있게 만드는 것!



# The Cross-Entropy Method
- derivative-free 최적화 방법
- 처음에 Rubinstein 이 제안
	- cross-entropy 측정법을 사용해서 rare-event의 확률을 측정하는 importance sampling 절차로 소개함.

- CEM은 cost function f(x)를 최소화하는 Evolution Strategy로 볼 수 있다.
- individual들은 population/distribution에서 샘플링 되고, f(x)로 계산된다.
- 그러고 나서, 이 cost function 값에 따라서 정렬하고, 일정 량의 elite 후보들을 선택한다.

- 이 elite-set으로 다음 iteration의 population의 parameter를 정할 것이다.
	- 보통 population은 Gaussian Distribution (평균, 분산)으로 모델링 된다.
- 이 평균과 분산을 elite-set에 fitting해서 샘플링 분포가 낮은 cost에 집중하게 된다.
- 이런 selection 절차를 몇 번 반복하고 나면 local optimum이나 global optimum에 가까운 x를 찾게 된다.
- iterative procedure 때문에 evaluate하는 샘플의 수가 너무 많아서 f(x) 비용이 크다면 실행하는 데에 오래 걸리게 된다.

# Standard modifications of CEM for model-predictive control: CEM_MPC
- MPC 세팅에서 매 timestep마다 CEM을 써서 action sequence에 대해 h-step planning 문제를 최적화 한다.

- 또다른 버전: CEM iteration 사이에 distribution을 refitting 하는 데에 momentum term을 이용함
	- 왜냐하면 elite-set은 수가 적은데 sampling distribution의 많은 parameter를 estimate해야 하기 때문에!
	- $\mu^{i+1}_t=\alpha \mu^i_t + (1-\alpha)\mu^{elite-set_i}$ 
	- 이 방법을 CEM_MPC라고 부름

- 또또다른 버전 (PETS 방법)
	- Sampling distribution도 바뀜
	- truncation bounds를 action-range에 match하기 세팅하는 것 대신,
	- truncation은 항상 $2\sigma$로 고정
	- 이거를 $CEM_{PETS}$라고 부름