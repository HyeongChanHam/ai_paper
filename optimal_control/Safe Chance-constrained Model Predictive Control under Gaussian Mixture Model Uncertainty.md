K Ren, C Chen, H Sung, H Ahn, I Michell, M Kamgarpour

# 내 생각

앞의 두 논문들은 open-loop이지만, 이 논문은 closed-loop로 MPC를 적용함.

---


# 배경
- 자율주행, 로봇같은 자율 시스템은 dynamic한 환경에서 동작할 때 어려움을 겪는다.
	- 예를 들어, 다른 자동차나 사람의 불확실한 미래 위치를 피하면서 trajectory를 계획해야 한다.
- 다른 장애물의 미래 움직임은 주로 multi-modal이다.
	- 근처의 차량은 직진할 수도 있고, 회전할 수도 있음.
- 이런 multi-modal behavior를 감안하면서 안전한 trajectory를 계획하는 것은 어렵지만 중요하다.
- 우리는 multi-modal uncertainty 분포가 있을 때, 안전하고 연산가능한 trajectory planning 방식을 제안하는 것을 목표로 함.

# 관련 연구
- Risk-constrained trajectory planning
	- 불확실한 환경에서 안전을 보장하기 위해 주로 쓰임.
	- 모든 불확실한 곳을 제한하는 것에 비해서 이 방법의 장점
		- 주어진 risk metric에 대해 특정 threshold까지 constraint violation이 버틸 수 있게 함.
		- chance constraint는 이러한 risk metric중에 하나임.
			- constraint violation의 확률을 제한함.
			- 단점: constraint violation의 잠재적인 심각성을 모름.
			- 그래서 CVaR 생김
		- conditional value-at-risk (CVaR)
			- constraint violation의 기대값을 설명함.
		- 두 metric 다 1) constraints가 linear하고, 2) uncertainty가 Gaussian일 때 tractable하다.

- Trajectory prediction model은 주로 GMM으로 모델링됨.
	- 그래서 우리도 GMM uncertainty에서 안전한 trajectory planning 하는 것에 집중함.
	1) GMM uncertainty의 각 mode의 moments를 알고 있고, constraints가 선형일 때
		- chance-constrained 문제는 풀린다.
		- 근데 CVaR-constrained 문제는 iterative cutting-plane으로 풀린다.
	- 근데 GMM의 정확한 분포는 실제로 알기 어렵고 데이터만 알 수 있음.
	2) uncertainty의 분포에 대해 사전 지식이 없을 때
		- CVaR-constrained 문제는 sample average approximation으로 풀리지만, 이론적인 근사를 보장하지 않음.
		- Wasserstein distance ambiguity set으로 distributionally robust approach를 사용해서 trajectory planning을 풀 수도 있음.
	3) uncertainty에 대해 일부만 알 때 (multi-modal이고, mode의 개수만 알 때)
		- scenario approach로 가능
		- moment concentration bound를 써서 risk constraint를 robustify 함.
		- 근데 trajectory planning의 관점에서, 두 연구 다 open-loop framework이다.


- 자율주행의 trajectory planning에서,
- 빠르게 변하고 불확실성이 높은 환경에서, 매우 조심스럽고, 행동이 불가능한 문제는 자주 관찰된다.
	- Model Predictive Control (MPC)는 trajectory planning 문제를 receding-horizon manner로 푼다.
	- sensor가 정확해지고, forecasting model이 실시간으로 발전해서, 이제는 ego vehicle의 trajectory를 매번 update되는 관측값과 prediction을 통해 closed-loop 방식으로 planning할 수 있다.
	- 이전에 (S Nair)가 GMM uncertainty에서 MPC를 만들어서 시뮬레이션에서 효율이 좋은걸 보임.
		- 근데 guarantee를 증명하지 않음.


- MPC의 안전성을 저해하는 주된 요인: recursive feasibility problem
	- 매 timestep마다 feasible control input이 존재해야만 한다.
	- 그런 solution이 없으면 safety constraint를 위반할 가능성이 있음.
	- 이런 특징은 MPC의 deterministic setting에서 많이 연구되었음.
	- stochastic MPC 에서는
		- direct recursive feasibility assumption을 만들었다.
	- 반면에, uncertainty가 bounded support를 가질 때, 다른 연구들은 recursive feasibility를 제공하거나, safe control invariant set을 식별하는 것에 집중했다.
	- 하지만, 그런 set을 정의하는 것은 unbounded GMM uncertainty에서 risk-constrained trajectory planning을 하기에는 어렵다.

# 이 논문
- GMM uncertainty에서 Chance-constrained로 Model Predictive Control하는 것 제안.
	- provable safety guarantee도 보여줌.
- 움직이는 장애물의 미래 행동을 예측하는 불확실성을 고려함
	- 여러 mode가 있음 (직진 or 회전 등)
- multi-modal uncertainty 분포를 다루기 위해 3가지 MPC를 제안함.
	1) nominal chance-constrained planning
	2) robust chance-constrained planning
	3) contingency planning
- 위 3개의 planner로부터 생성된 closed-loop trajectory는 안전하다는 것을 증명함.
	- 특히, robust chance-constrained planner는 prediction uncertainty의 전파에 대한 특정 가정이 있을 때 recursively feasible 하다.
	- 그리고 contingency planner는 nominal planner에 비해 덜 보수적인 closed-loop trajectory를 만들어낸다.

# 결과


---

# Problem Formulation

- 목표: 최적의 EV state trajectory를 찾는 것
	- 특정 cost function (연료 소모, 타겟과의 거리)을 최소화해야 함
	- planning horizon 전체동안 다른 OV들과의 collision-free한 path를 보장해야 함
- EV 모델링은 deterministic linear time-varying system으로 함.
- $$x_{t+1}=A_tx_t+B_tu_t$$
- 도로 규칙(차선 안에 있고, 속도 지키고)을 준수하기 위해, state랑 control input은 time-invariant convex set 안에 제한됨.

## A. Selection of Risk-constrained Formulation
- GMM uncertainty를 다루기 위해
	- 안전을 보장하기 위해 risk-constraind formulation을 고려함.
## Chance-constrained Planning

# Chance-Constrained MPC
## A. Nominal Planning
## B. Robust Planning for Recursive Feasibility
## C. Contingency Planning for Reducing Conservativeness
