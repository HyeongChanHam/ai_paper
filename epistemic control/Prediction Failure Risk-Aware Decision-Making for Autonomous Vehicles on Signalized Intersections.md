K Yang, ... H Wang
tsinghua univ.
IEEE TITS 2023


# 배경
- 자율주행이 주변 애들의 미래 행동을 예측하기 위해 Motion prediction module은 중요하다.
- 근데 이 prediction module이 실패하면 그 뒤의 planner가 안전하지 않은 결정을 내리게 되어 실패할 수 있다.
- 현재, 딥러닝 기술은 성능이 좋아서 prediction에 자주 쓰인다.


# 문제
- 하지만, 그런 딥러닝 모델은 long-tail driving scenario에서 실패할 수 있다.
	- 학습 데이터가 불충분하거나 없을때
	- prediction model의 epistemic uncertainty로 나타내어진다.

# 이 논문
- risk-aware decision-making (RADM) framework 제안
	- 불충분한 데이터로 prediction model을 학습해서 생기는 epistemic uncertainty를 다루는 프레임워크
	1) multi-agent prediction network랑 epistemic uncertainty quantification 같이 함
	2) 이 네트워크는 근처 road users의 과거 state들이랑 map 정보랑 traffic lights를 입력으로 받는다.
	3) 그리고 RADM은 model predictive control 기술을 써서
		- multi-agent prediction 결과를 처리하고
		- prediction model의 epistemic uncertainty도 고려한다.

# 실험
- 만든 prediction model은 실제 driving dataset에서 검증함
- RADM은 실제 주행 log에서 얻은 replay data에서 평가되었고, 
- SUMO 시뮬레이터 써서 검증함.
	- pedestrian이랑 non-motorized vehicles이 신호위반하면서 교차로를 건너는 상황


# 결과
- RADM은 driving risk를 줄일 수 있다.
- driving safety를 향상시킬 수 있다.


---


# 배경
- 자율주행이 주변 애들의 미래 행동을 예측하기 위해 Motion prediction module은 중요하다.
- 근데 이 prediction module이 실패하면 그 뒤의 planner가 안전하지 않은 결정을 내리게 되어 실패할 수 있다.
- 현재, 딥러닝 기술은 성능이 좋아서 prediction에 자주 쓰인다.


# 문제
- 하지만, 그런 딥러닝 모델은 long-tail driving scenario에서 실패할 수 있다.
	- 학습 데이터가 불충분하거나 없을때
	- prediction model의 epistemic uncertainty로 나타내어진다.


- 신호 위반 사고는 prediction model 학습하기 위한 작은 데이터셋을 모으기 위해 시간이 더 걸리기 때문에,
	- 신호위반 하는 road user는 부정확하게 예측할 가능성이 있다.

# 이 논문
- risk-aware decision-making (RADM) framework 제안
	- 불충분한 데이터로 prediction model을 학습해서 생기는 epistemic uncertainty를 다루는 프레임워크
	1) multi-agent prediction network랑 epistemic uncertainty quantification 같이 함
	2) 이 네트워크는 근처 road users의 과거 state들이랑 map 정보랑 traffic lights를 입력으로 받는다.
	3) 그리고 RADM은 model predictive control 기술을 써서
		- multi-agent prediction 결과를 처리하고
		- prediction model의 epistemic uncertainty도 고려한다.

# 실험
- 만든 prediction model은 실제 driving dataset에서 검증함
- RADM은 실제 주행 log에서 얻은 replay data에서 평가되었고, 
- SUMO 시뮬레이터 써서 검증함.
	- pedestrian이랑 non-motorized vehicles이 신호위반하면서 교차로를 건너는 상황


# 결과
- RADM은 driving risk를 줄일 수 있다.
- driving safety를 향상시킬 수 있다.


# Contribution
1) 교통신호를 고려한 multi-agent prediction network를 제안함.
	- epistemic uncertainty도 나옴
	- 해당 prediction failure risk도 실행하는 동안 측정됨
2) risk-aware decision-making (RADM) 방법 제안함
	- MPC 기반임
	- prediction model의 epistemic uncertainty 고려함
3) 실제 현실의 driving log-replay랑 SUMO 시뮬레이션 실험으로 검증함.


---

# 관련 연구
## 1) Multi-Agent Motion Prediction
- Graph NN 기반이 효과가 좋다.
- 근데 대부분은 traffic light의 영향을 고려 안함.
	- signalized intersection에서는 중요함.
- 딥러닝 기반 prediction model은 long-tail driving scenario에서는 실패할 수 있다. (학습 데이터가 부족한 경우)
- decision-making의 관점에서, 만약 모델이 prediction 결과의 위험성에 대해 충분한 단서를 주지 않는다면 안전하지 않은 결정을 내리게 될 것이다.


## 2) Epistemic Uncertainty Quantification
- epistemic uncertainty estimation은 불충분하고 학습때 보지 못한 데이터로 인해 일반화가 부족한 것을 탐지할 수 있다.

- Bayesian neural network 기반 방법
- single-pass uncertainty estimation 기반 방법
- ensemble 기반 방법

## 3) Decision-Making Considering Prediction Uncertainty
- Autonomous driving을 여러 subtask로 나눔
	- 연산량을 줄이고 well decision-making transparency를 줌

(1) classical methods
(2) learning-based methods
 - reinforcement learning, deep learning
(3) game theoretic methods


- model predictive control (MPC)로 충돌이랑 kino-dynamics 제약을 명확하게 정의함
- (X Tang)
	- 주위 road user의 예측 불확실성을 고려하기 위해,
	- 주위 차량의 미래 위치의 불확실성이 처리되고 extended Kalman filter로 전파되었다.
	- potential field랑 같이 합쳐진 MPC도 사용되어서 prediction uncertainty를 다룸.
	- 근데 이 방법은 고정된 parameter를 사용함.
	- 매우 보수적인 plan이 됨
- (다른 방법)
	- probabilistic prediction 기술을 쓰는 것
	- decision-making system이 probabilistic prediction 결과에 기반으로 planning 하는 것.
	- Branch MPC 방식이랑 Stochastic MPC 방식을 포함함
	- 단점
		- 이 방법들은 pretrained prediction model에 의존함.
		- prediction model의 epistemic uncertainty를 고려하지 않음


---
![[Pasted image 20240722162014.png]]
# 2. Multi-Agent Prediction Network with Epistemic Uncertainty Quantification
- SOTA motion prediction model인 HEAT 모델 씀
	- traffic light states를 같이 고려하게 변경
	- deep ensemble 기술을 써서 epistemic uncertainty를 계산함.

## A. Prediction Model Formulation

## B. Epistemic Uncertainty Quantification and Failure Risk Estimation

# 3. Prediction Failure Risk-Aware Decision-Making\
## A. Vehicle Kinematics Model
## B. Objective Function
## C. Collision Avoidance Constraints Considering the Prediction Failure Risk
## D. Traffic Rule and Input Constraints