# 배경

# 이 논문
- Gaussian mixture model (GMM) uncertainty가 있는 곳에서 안전하게 trajectory planning하는 것을 다룬다.
- 주위 장애물의 불확실한 상태의 multimodal behavior를 모델링하기 위해 GMM을 사용함.
- 그러고, chance-constrained trajectory planning problem을 위해서 mixed-integer conic approximation을 씀
	- deterministic linear system이랑 polyhedral obstacle을 같이 씀

- GMM moments가 유한한 sample로 추정되었다면,
	- 바람직한 confidence에 chance constraint를 보장하기 위해 tight concentration bound를 만든다.
- constraint violation의 양을 제한하기 위해, Conditional Value-at-Risk (CVaR)접근을 사용함.
	- 알고있는, 추정된 GMM에 대해 tractable approximation을 유도해낸다.

# 결과
- 우리 방법을 최신 trajectory prediction, dataset에서 검증함.


---

# 배경
- 자율주행 분야에서, 불확실한 환경에서 안전하게 작동하는 것은 중요하다.

# 관련 연구
- 불확실한 환경에서 안전을 보장하기 위해 chance-constrained program을 주로 씀.
	- 불확실한 곳을 싹 다 constraint 걸어버리는건 conservative한 결정임.
	- 그와 달리, chance-constrained 방식은 작은 constraint 위반 확률을 감안한다.
- CCP는 non-convex chance constraints때문에 다루기 힘들지만,
- Gaussian 분포처럼 특정 클래스의 확률 분포에서는 쉽게 다뤄질 수 있다.
	- 근데 실제로 정확한 gaussian moments는 알기 어려움.
	- 그래서 추정값 (센서 관측값)을 사용.
- uncertainty 분포가 gaussian이 아니라면,
	- 상태 확률 분포의 moment의 constraints를 확률을 mapping 함.


 - 기존의 scenario approach는 각 mode마다 distribution sample을 clustering 하고, scenario-based로 접근하는 방법을 사용함.
	 - 이를 통해 유한한 수의 샘플에 대해 안전을 보장할 수 있음.
	 - scenario-based 방식은 실제 분포에 대한 prior knowledge를 포함하지 않음.
	 - Trajectron++같은 애들은 GMM으로 미래 vehicle의 위치 분포를 표현함.
	- 그래서 우리도 GMM 상에서 안전하게 trajectory planning 하는 것을 만듬.


- GMM uncertainty 에서 chance-constraint를 쓰면 deterministic second-order cone constraint로 표현할 수 있음.
	- unimodal Gaussian distribution에서 chance-constraint 쓰는 거랑 비슷.
	- GMM으로 chance constraint 하는걸 trajectory planning에 적용한 것은 우리가 처음이다!


# 이 논문
- 불확실한 환경에서 안전하게 trajectory planning 하는 것을 다룸.
- Gaussian mixture model (GMM) uncertainty가 있는 곳에서 안전하게 trajectory planning하는 것을 다룬다.
- 주위 장애물의 불확실한 상태의 multimodal behavior를 모델링하기 위해 GMM을 사용함.
- 그러고, chance-constrained trajectory planning problem을 위해서 mixed-integer conic approximation을 씀
	- deterministic linear system이랑 polyhedral obstacle을 같이 씀

- GMM moments가 유한한 sample로 추정되었다면,
	- 바람직한 confidence에 chance constraint를 보장하기 위해 tight concentration bound를 만든다.
- constraint vioration 하는 심각도를 조절하기 위해 chance constraint랑 비슷한 conditional value-at-risk (CVaR)를 사용함.
- constraint violation의 양을 제한하기 위해, Conditional Value-at-Risk (CVaR)접근을 사용함.
	- 사전에 알고있거나 추정된 GMM에 대해 tractable approximation을 유도해낸다.

# Experiments
- 우리 방법을 최신 trajectory prediction, dataset에서 검증함.
- nuScenes dataset

# 결과
- 불확실한 분포를 GMM으로 모델링하면 unimodal Gaussian으로 모델링하는 것에 비해 움직임이 덜 보수적이다.
- CVaR-constrained planner는 constraint violation하는 양을 제한하면서 높은 확률로 안전을 보장한다.

---

# Chance Constraint With GMM Uncertainty
- other vehicle의 불확실한 미래 위치 때문에 우리는 constraint를 위반할 확률을 미리 정한 threshold $\epsilon\in (0,0.5)$ 가 되게 한다.
- Chance constraint
$$\mathbb{P}_{\delta~p_*}(C(x,\delta)\leq0)\geq1-\epsilon$$
$C(x,\delta)\leq0$: constraint를 만족한다.
- $x$: ego vehicle의 state를 encoding한 것
- $\delta$: distribution $p_*$를 따르는 불확실한 파라매터

## 1. Linear Constraint
- $\delta\in\mathbb{R}^n$ 이 다른 vehicle의 불확실한 edge 위치를 encode한다고 하자.
- $\delta$의 차원은 $n=n_x+1$이다.
- 내 차 EV가 다른 차의 edge에서 멀어지는 것은 linear constraint로 표현될 수 있다.
	- $\delta^T \tilde{x}\leq 0$  
	- 2차원 공간에서 ($n_x=2$)는 다음과 같다.
	- ![[Pasted image 20240711190523.png]]
	- 
- Assumption 1: constraint 함수는 선형 곱의 형태이다.
	- $C(x,\delta)=\delta^T \tilde{x}\in\mathbb{R}$

## 2. Chance and CVaR Constraints Under GMM
- $z\in \mathbb{R}^n$: $\delta$랑 같은 차원의 random variable
- K mode의 GMM
- $$p_*(z)=\sum^K_{k=1}\pi_kp_k(z), \;\;\;\sum^K_{k=1}\pi_k=1$$
- 이 GMM을 고려할 때, 위의 chance constraint는 아래와 같이 decomposed됨.
- $$\mathbb{P}_{\delta~p_*}(\delta^T\tilde{x}\leq0)\geq1-\epsilon_k, \;\;\;\;\forall k\in \mathbb{z}_{1:K}$$
- $$\sum^K_{k=1}\pi_k\epsilon_k=\epsilon$$
- 그리고, chance constraint는 심각한 위반이랑 가벼운 constraint 위반이랑 구별하지 못하기 때문에, 우리는 risk-aware approximation도 고려함. (CVaR로!)
- for the $k^{th}$ mode,
- $$CVaR_{\epsilon_k}(\delta^T \tilde{x}):=inf_{\alpha>0}\{\frac{1}{\epsilon_k}\mathbb{E}[(\delta^T\tilde{x}+\alpha)_+]-\alpha\}\leq0$$
- CVaR: $\epsilon$-worst constraint 값 들에서 constraint violation 기댓값을 측정한다.
	- CVaR constraint는 chance constraint의 보수적인 근사다.

- 다음으로, risk-aware 근사를 위해서 어째저째 할거다...

## 3. Deterministic Reformulation & Moment Robustification
- assumption 1이랑 GMM uncertain param.으로 우리는 chance / CVaR constraint를 결정론적으로 정할 수 있다.
LEMMA 1: GMM의 moments $(\mu_k, \sum_k)$를 모든 k에 대해 알고 있을 때,
			chance constraint랑 CVaR approximation은 아래의 second-order cone constraint로 똑같이 재구성될 수 있다.
			$$\Gamma_k\sqrt{\tilde{x}^T\Sigma_k\tilde{x}}+\mu_k^T\tilde{x}\leq0, \;\;\; \forall k\in\mathbb{z}_{1:K}$$
- $\Gamma_k$의 값
	- chance constraint: $\Gamma_k=\Psi^{-1}(1-\epsilon_k)$  
	- CVaR approximation: $\Gamma_k=\Phi(\Psi_{-1}(1-\epsilon_k))/\epsilon_k$ 
	- 

- 근데, 정확한 GMM moments $(\mu_k, \Sigma_k)$는 모른다.
	- 그래서 이거를 $\delta$의 $N_s$개의 샘플에서 추정하는 것도 고려함!

Remark 1: sample $\{\delta_1, ..., \delta_{N_s}\}$ 가 주어졌을 때, GMM의 각 mode마다 $(\mu_k, \Sigma_k)$를 추정하기 위해서,
			각 샘플에 속하는 mode를 결정한다.
-  expectation maximization method같은 방법으로 데이터를 K개의 mode로 나누고 각 샘플에 어떤 mode가 속하는지 결정할 수 있다.
- Assumption 2: sample $\{\delta_1, ..., \delta_{N_s}\}$ 에 대해서, 각 샘플이 속해있는 k랑, GMM의 각 mode인 $\pi_k$는 모든 k에 대해 알 수 있다.


Theorem 1: Gaussian의 각 mode의 moments가 $N_k$ 샘플로부터 추정되었을 때,
			만약 아래의 부등식이 성립한다면, 그럼 위의 constraint 식이 $1-2\beta$의 확률로 성립한다. ($\beta\in(0,1)$은 safety tolerance이다.)


- 이전의 연구 (V Lefkopoulous)는 평균 추정 오차를 강건하게 하기 위해 위에 8a 식을 사용했다.
- 하지만 그 값은 trajectory planning problem에서 나타나는 linear constraint 구조를 고려하지 않았다.
- 우리가 관찰하기로는, 7번 식에서 더 타이트한 강건함을 제공하는거같다.
- 이제 다음으로 실제 trajectory planning 예시에서, 
	- 강건해진 chance / CVaR constraint planner가 8번 식의 bound 안에서 infeasible solution을 만들어내고, 새로운 bound 7번식은 feasible solution을 가능하게 한다.
# Chance-Constrained Trajectory Planning
- trajectory planning 시나리오를 고려
	- 목적이 cost function (연료나 목적지 까지의 거리)을 최소화하는 동시에 다른 차량과의 충돌을 planning horizon동안 피하는 것.
- Ego vehicle을 linear discrete-time system으로 고려함.
- $$x_{t+1}=A_tx_t+B_tu_t$$
- $A_t, B_t$: system의 dynamics matrices
- control의 입력 $u_t$: time-invariant convex set $\mathcal{U}$ 

- Other Vehicle의 불확실한 행동을 고려할 때, trajectory planning 문제를 finite-horizon CCP로 구성하기
- $$min_u f(x_0, u)$$
- $$s.t. \mathbb{P}_{\delta^t_{ij}-p^t_{ij*}}(\land^T_{t=1}\land^J_{j=1}\lor^{I_j}_{i=1}{\delta^t_{ij}}^T \tilde{x}_t\leq0)\geq1-\epsilon$$
- uncertain parameter $\delta^t_{ij}$는 시간 t에 j번째 OV의 i번째 면(face)이다.
- 이 constraint로 EV가 OV의 한쪽 면에서부터 멀어지게 한다.
- 이 chance constraint의 합집합 교집합의 다루기 힘든 점을 해결하기 위해, 우리는 Big-M method랑 Boole's inequality를 써서 보수적으로 근사한다.
## Deterministic Reformulation & Moment Robustification