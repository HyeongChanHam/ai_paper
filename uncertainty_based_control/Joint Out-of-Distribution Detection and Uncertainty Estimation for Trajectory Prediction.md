J Wiederer, J Schmidt, U Kressel, K Dietmayer, V Belagiannis
IROS 2023

# 이 논문
![[Pasted image 20240708165743.png]]

1. scene encoder가 매 agent i 마다 latent representation $h_i=f_e(X),\mathcal{I})$를 계산한다.
	1. X에서 agent간의 social interaction을 담고
	2. I에서 scene context를 담는다.
2. trajectory prediction decoder가 모든 agent encodings $\{h_i\}_{i=1}^N$에 대해 미래 state $y_i$의 확률 분포 $p(y_i|\{h_i\}_{i=1}^N)=g_p(\{h_i\}_{i=1}^N)$ 를 구한다.
동시에 OOD detection, uncertainty estimation도 같이 한다.
3. $g_{ood}$: OOD detection
	- latent feature vector $h_i$에 대해 scalar-valued score인 $\hat{\alpha}_i=g_{ood}(h_i)$를 구한다.
		- scene이 ID인지 OOD인지 판별!
	- Gaussian mixture model을 사용함.
1. $g_u$: Uncertainty estimation
	- 예측한 trajectory의 uncertainty를 측정함.


## Model Architecture
### Scene Encoder
- scene encoder $f_e$로 HiVT처럼 local encoder를 쓴다.
- Input
	- 모든 agent들의 observed states: $X=\{x_i\}_{i=1}^N$
	- contextual information $\mathcal{I}$
		- L개의 lane vector $V=\{v_l\}_{l=1}^L$
		- $v_l\in \mathbb{R}^o$ 는 o개의 lane feature를 포함함.
		1. translation invariant한 scene representation을 생성.
			- 이를 통해 과거의 state vector랑 HD map에서의 lane segment들을 포함한 scene element가 vector representation으로 변환됨.
			- 그리고 distance 정보를 유지하기 위해 element 간의 상대 position vector도 같이 씀.
		2. 그리고 각 agent마다 encoder는 spatio-temporal feature $h_i=f_e(x_{n\in \mathcal{N}_i}, v_{m\in\mathcal{N}_i})$ 를 뽑는다.
			- x: agent의 observed state sequence
			- v: local neighborhood $\mathcal{N}_i$에서의 lanes
				- 해당 에이전트의 반지름 r인 원을 기준으로 주위를 정의.

### Multi-modal Trajectory Prediction Decoder
- 여러 trajectory mode를 포함하기 위해 타겟 분포 $p(y_i|\{h_i\}_{i=1}^N)$은 mixture density distribution을 따른다고 가정함.
	- 각 density component k는 K개의 가능한 미래 trajectory중 하나를 나타냄
	- bi-variate Gaussian distribution의 sequence로 표현됨.
	- 이 density component들은 mixing coefficients $\pi_{i,k}$로 결합됨.
	- 이 distribution parameter들은 trajectory prediction decoder $g_p$를 거쳐서 나온다.
		- 이 decoder는 global message passing network랑 aggregation network로 이루어져 있다.
		- 그리고 MLP도 각 distribution parameter마다 있다.
		- mixing coefficient 구하는 법
			- unnormalized coefficient를 먼저 예측하고, softmax를 거친다.

### Out-of-Distribution Detection
- OOD scenario는 드물고 대부분 학습하는동안 접근하기 어렵다.
- 우리는 OOD detection을 one-class classification 문제로 봄
	- ID sample만 학습에 쓸 수 있다.
- OOD detector를 latent representation space에 적용해서 ID랑 OOD 구별하는 데에 씀.
- 마지막으로, parametric probability distribution의 parameter를 추정함.
	- 이는 학습한 feature vector 의 ID를 나타내고,
	- testing 하는 동안에 낮은 density 영역의 OOD sample을 식별한다.
- **이 OOD detection module은 latent feature의 확률 분포가 mixture of multivariate Gaussian distribution을 따른다고 가정함.**
	- 그래서 우린 Gaussian mixture model을 정의함.
	- 이를 latent GMM (lGMM)이라 부른다.
	- 추론하는 동안 lGMM은 OOD score를 추론한다.
	$$\hat{\alpha_i}=-log\;{q(h_i)}=-log(\sum_{i=1}^C\pi_c\mathcal{N}(h_i|\mu_c, \Sigma_c))$$
		- Gaussian mixture distribution의 negative log-likelihood임.
			- ID scenario는 낮음
			- OOD scenario는 높은 negative log-likelihood를 가짐
				- NLL이 높다 = likelihood가 낮다
- 

## 학습
1. trajectory prediction network를 학습
2. scene encoder는 freeze한 상태로, 나머지 2개의 module을 학습한다.


# Experimental Setup
## Dataset
- Shifts dataset
	- trajectory prediction 에서 OOD detection, uncertainty estimation을 다루는 유일한 데이터셋임.
	- ID training set: 388,406 sequences / Moscow (비 안옴)
	- 9,569 / 36,605 validation, 9,939 / 36,804 test (ID / OOD)
	- 3개의 dev랑 3개의 eval set을 만들 수 있음.
		- ID / OOD / Full
	- OOD는 비 오거나 눈 내리고 Ann-Arbor, Tel Aviv에서 얻음.

	- 5Hz로 획득
	- 객체
		- pedestrians / vehicle
		- 매 timestemp마다 position, velocity vector로 표현
		- vehicle은 acceleration, yaw angle도 있음
	- HD-map도 있음.
	- 10초 녹화는 5초 observation이랑 5초 prediction으로 나뉨.

## Evaluation Protocol
- 3개 있다.\
	1. trajectory prediction
	2. OOD detection
	3. uncertainty estimation

1. Trajectory prediction
	- minADE: minimum Average Displacement Error
	- minFDE: minimum Final Displacement Error
	- weighted ADE
		- $wADE(y,\hat{y})=\sum^K_{k=1}\pi_k \cdot ADE(y,\hat{y_k})$ 
			- $\pi_k$: mixture coefficient
			- K개의 mode간에 평균을 낸다.
			- 이게 우리의 main metric임!
		- $wFDE$도 비슷함.
	- 근데, 기존의 메트릭 단점으로, predicted distribution을 포함하지 않고, mode-collapse 문제를 겪는다.
		- 이 두 문제를 해결하기 위해 Negative Log-Likelihood score를 사용함.
			- NLL: 예측한 Gaussian mixture distribution 하에 GT trajectory의 likelihood를 계산.

2. OOD Detection
	- AUROC: Area Under the Receiver Operating Characteristic curve
		- ROC curve는 예측한 OOD score의 threshold에 대해서
			- false positive에 대한 true positive의 비율을 그린다.
		- ROC의 아래 영역(AUROC)은 OOD score가 OOD sample을 탐지할 수 있는지를 평가한다.
		- 이상적인 classifier는 100% AUROC가 나온다.
		- random classifier는 50% AUROC가 나온다.
3. Uncertainty estimation
	- R-AUC: area under the Retention curve
		- retention curve는 $wADE$같은 error metric으로부터 uncertainty $\hat{e}$와 $e$ 사이의 agreement를 측정한다.


## Implementation Details
- agent state: $s_i^t\in \mathbb{R}^7$ => x, y 좌표에서 벡터화된 위치, 속도, 가속도, class 정보 (vehicle or ped.)
	- pedestrian은 가속도 0으로 뒀음.
- lane vector: $v_l\in \mathbb{R}^{10}=> x,y 좌표에서 벡터화된 centerline segment랑 context feature 집합들 (HD map에서 가져옴)
	- speed limit, lane availability vector => traffic light state에서 가져옴
	- lane priority
- receptive field의 반지름 r=50 meter

### 학습
1. prediction loss $L_p$를 최적화
	- 64 epoch
	- init learning rate: $1\times 10^{-4}$
	- batch size: 48
2. regression loss $L_u$로 $E_{reg}$의 weight 학습
	- 100 epoch
	- learning rate: $1\times 10^{-3}$
	- batch size: 1024
	- regression target $e_i=log(wADE(y_i,\hat{y_i}))$

- 두 단계 모두 AdamW optimizer 사용
- cosine annealing learning rate scheduler 사용


- lGMM의 parameter는 EM-Algorithm으로 최대 100 iteration 학습함
	- 초기화는 k-means algorithm으로 함.
		- K=6 mixture component를 씀.
			- HP search 해보니까 제일 나음