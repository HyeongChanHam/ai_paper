
# 배경
- aleatoric, epistemic uncertainty를 추정하는 거 중요해짐

# 기존의 문제
- 기존에는 dropout이나 ensemble 방식으로 다른 submodel에서 probability prediction을 sampling 해서 uncertainty를 측정함.
	-> inference time이 느림.
- 최근에는 NN으로 probability prediction의 prior distribution의 parameter를 바로 예측해서 이러한 단점을 해결함.
	 -> 근데, in-distribution data에 대한 임의의 목표 parameter를 정의해야 함
	 -> 그리고 학습할 때 Out-of-distribution data도 알아야 되어서 비현실적임.

# 이 논문 해결책
- Posterior Network (PostNete) 제안
	- Normalizing Flow를 써서 어떠한 input 에 대해서든 predicted probabiliy의 개별 closed-form posterior distribution을 예측함.

# 결과
- PostNet으로 학습한 posterior distribution은 학습할 때 OOD data 없이도 ID, OOD 의 uncertainty를 정확하게 반영함.
- 

---

# 배경
- aleatoric, epistemic uncertainty를 추정하는 거 중요해짐
- 기존의 NN은 학습한 데이터랑 다르더라도 over-confident 한 경향이 있음.
- uncertainty
	- aleatoric uncertainty: data uncertainty -> 줄일 수 없음.
	- epistemic uncertainty: knowledge uncertainty -> 못 본 데이터에 대한 지식이 부족해서 생기는거라서 줄일 수 있음.
# 기존의 문제
- 기존에는 dropout이나 ensemble 방식으로 다른 submodel에서 probability prediction을 sampling 해서 uncertainty를 측정함.
	- Bayesian NN은 weights에서 distribution을 학습함.
	- Ensemble은 submodel의 collection을 사용해서 그 예측결과를 합치고 그걸로 class probability distribution의 mean, variance같은 statistics를 추정한다.
		- 하지만 예측에 대한 implicit distribution을 묘사하고,
		- 추론할 때 sampling cost가 크다.
	-> inference time이 느림.
- 최근에는 NN으로 probability prediction의 prior distribution의 parameter를 바로 예측해서 이러한 단점을 해결함.
	 -> 근데, in-distribution data에 대한 임의의 목표 parameter를 정의해야 함
	 -> 그리고 학습할 때 Out-of-distribution data도 알아야 되어서 비현실적임.

# motivation / insight
- 일반적으로 이 모델들은 추론할 때 비슷한 OOD 를 탐지하기 위해 학습할 때 ID와 OOD 둘 다 쓴다.
- 학습할 때 OOD data에 접근 못하게 하면 기존 방법들은 성능 저하가 일어난다.
	- 관측한 데이터와 멀리 떨어진 샘플에 대해 자신감을 가지게 됨.

# 이 논문 해결책
- Posterior Network (PostNete) 제안
	- OOD sample에는 높은 epistemic uncertiainty를 부여하고, single class의 관측된 데이터 근처의 지역에는 낮은 overall uncertainty를 부여함.
	- 그리고 다른 클래스에서 관측된 데이터 근처에는 높은 aleatoric, 낮은 low epistemic uncertainty를 부여.
	- Normalizing Flow를 써서 어떠한 input 에 대해서든 predicted probabiliy의 개별 closed-form posterior distribution을 예측함.
		- latent space에서 Dirichlet parameter의 분포를 학습하기 위해 normalizing flow를 사용함.
		- 직관적으로, Dirichlet parameters가 각 클래스의 관측 수에 대응되는 것처럼,
		- 개별 클래스의 밀도가 그 클래스의 학습 데이터의 수에 통합되도록 강제함.
	- 학습에 OOD sample 필요없음.
	- 임의로 target prior distribution 특정지을 필요 없음
	- test time에 sampling 할 필요 없음.

# 결과
- PostNet으로 학습한 posterior distribution은 학습할 때 OOD data 없이도 ID, OOD 의 uncertainty를 정확하게 반영함.


# Posterior Network
두 종류의 uncertainty
- aleatoric uncertainty: class prediction에 대한 불확실성
	- $y^{(i)}=\{1,...,C\}$ 
- epistemic uncertainty: categorical distribution prediction에 대한 불확실성
	- $p^{(i)}=[p_1^{(i)}, ..., p_C^{(i)}]$ 
- 두 개를 동시에 모델링하고 싶으면 categorical distribution prediction $p^{(i)}$의 epistemic distribution $q^{(i)}$를 사용
	- $p^{(i)} {~} q^{(i)}$
	- epistemic distribution은 자연스럽게 class prediction의 aleatoric distribution의 추정을 따른다.

- evidential deep learning 에서 모델의 출력 $f_\theta (x^{(i)})=\alpha^{(i)}$ 이고, $q^{(i)}=Dir(\alpha^{(i)})$이다.
- epistemic distribution: $q^{(i)}=Dir(\alpha^{(i)})$
- aleatoric distribution: $p_c^{(i)}=\frac{\alpha_c}{\alpha_0} { } with  \alpha_0=\sum_{c=1}^{C}\alpha_c$ 
- class prediction: $y^{(i)}=argmax [p_1, ..., p_C]$

- concentration parameter $\alpha_c^{(i)}$는 클래스 c에서 관측된 샘플의 개수로 해석될 수 있다. -> epistemic uncertainty를 나타내기에 좋다!
	- 이걸 학습하기 위해 [[Prior Networks]]는 학습에 OOD sample을 사용했고, ID랑 OOD data의 target value를 다르게 정의했음.