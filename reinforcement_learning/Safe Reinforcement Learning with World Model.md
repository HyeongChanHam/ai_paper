# 배경
- 실제 세상에 RL 배포하려고 하는데 안전 문제 때문에 실패하곤 함.
- 기존의 Safe RL 방식들은 cost function으로 안전을 보장함.

# 문제
- 이런 기존의 방식들이 vision-only task같은 복잡한 상황에서는 zero-cost performance를 달성하는 데에 실패함.
- 이런 한계는 **1) 모델 부정확성**, **2) 부적절한 샘플 효율** 때문이다.
	- world model을 적용하는게 이 단점들을 해결하는 데에 도움이 된다고 알려짐.

# 이 논문
- SafeDreamer 소개
	- Lagrangian-based 방식을 world model planning process에 도입함.
	- 기존의 Dreamer framework 사용함.

# 실험
- Safety-Gymnasium benchmark

# 결과
- 다양한 태스크(저차원 ~ vision-only input까지)에서 거의 zero-cost performance를 달성함
- performance랑 safety 사이에 균형을 잡는데 효율적임.


---
# 배경
- 실제 세상에 RL 배포하려고 하는데 안전 문제 때문에 실패하곤 함.
- 기존의 Safe RL 방식들은 cost function으로 안전을 보장함.
	- constrained MDP (CMDP)를 정의하고, 잠재적으로 위험한 행동을 정량화하는 cost function을 도입함.
		- agent는 reward를 최대화 하는 동시에, cost를 사전에 정한 threshold 밑으로 유지하도록 한다.
-  하지만, 기존의 Lagrangian-based 방식은 거의 0에 가까운 cost threshold를 만족시키는 조건을 충족시키지 못하거나, 아니면 만족시키더라도 태스크를 못끝냄.
- **반대로, internal dynamics model을 사용하면 agent는 높은 reward랑 거의 0에 가까운 cost를 지키는 agent trajectory를 planning 할 수 있다.** -> dynamics model이랑 planning이 중요하다는 의미!
- 이런 기존의 방식들이 vision-only task같은 복잡한 상황에서는 zero-cost performance를 달성하는 데에 실패함.
	- 여전히 vision-only 자율주행 같이 복잡한 시나리오에서 planning 하기 위해 정확한 dynamics model을 구하는거는 어려움...
		- 그리고 long-horizon planning 하는데 필요한 비용이 많이 들어서 finite horizon 최적화를 하게 되고, 이는 locally optimal하고 불안전한 해를 만들어낸다.

## 메인 질문
- **그러면 long-term reward랑 cost의 균형을 잡기 위해 safety-aware world model을 만들러면 어떻게 하나?**

- 일부 연구에서 world model에 agent의 불편함 레벨을 반영하는 cost를 도입한 사례가 있음.
	- world model로 cost 랑 reward 균형을 잡은 것도 있음.
# 문제
- 하지만, 이 방법들은 zero-cost performance를 달성하는 데에 실패함...ㅠㅠ

	
- 이런 한계는 **1) 모델 부정확성**, **2) 부적절한 샘플 효율** 때문이다.
	- agent의 현재나 미래 state에서 safety를 모델링 하는 것에 대한 부정확함이 있기 때문.

# 이 논문
- SafeDreamer 소개
	- Lagrangian-based 방식을 world model planning process에 도입함.
		- safety planning 이랑 Lagrangian 방식을 world model에서 합침
			- cost model이랑 critics 사이에 error 균형을 잡기 위해!
	- 기존의 Dreamer framework 사용함.

- Contribution
	- Online Safety-Reward Planning algorithm (OSRP)를 제시함.
		- 그리고 vision-only task에서 constraints를 만족하는지 world model의 online planning 을 사용해서 가능성을 입증하였다.
		- Planning 하는데 Constrained Cross-Entropy Method 를 사용함.
	- Lagrangian 방법이랑 safety-reward online and background planning 이랑 world model 안에서 합쳤다.
		- long-term reward랑 cost랑 균형을 맞추기 위해!
		- 이걸로 OSRP-Lag랑 BSRP-Lag 만듬
	- 그래서 SafeDreamer 는 저차원이랑 고차원 태스크 다 다룰 수 있게 되었고, Safety-Gymnasium benchmark에서 거의 zero-cost performance를 달성함.

# 관련 연구

![[Pasted image 20240707104708.png]]

## Safe RL
- 목적: constraints 있는 상황에서 objective를 최적화하는 것을 다룬다.

- [CPO]:([[ai_paper/reinforcement_learning/Constrained Policy Optimization]]) general-purpose policy search algorithm을 제안
	- second-order method때문에 연산량이 높음에도 불구하고, 제약조건을 반복적으로 충족하는데 초점을 둠.
- ...

### 이 논문의 포지션
- 우리 실험이 밝힌 것
	- safe model-free 알고리즘은 환경이랑 상호작용 상당히 많이 해야 함.
	- model-based 알고리즘은 sample efficiency를 효과적으로 높일 수 있다.


## Safe Model-based RL
- Model-based RL 접근으로 더 나은거
	- 환경 dynamics를 모델링 하는 것
	- 이 world model로 action selection이랑 background planning에 활용함.
	- world model을 policy updates에 활용함.
- MPC같은 방법은 safety action 을 생성해서 online planning 의 예로 볼 수 있다.
	- 단점: 근데 이 방법들은 planning 영역이 제한되어 있고, critics가 없어서 근시적인 결정을 내릴 수 있다.
- 최근 연구는 terminal value function을 이 online model planning에 합치려는 방향이다.
	- long term reward를 고려하는 방향임
	- 근데 long-term safety는 다루지 않고 있음.
- 반면, background planning 하는 방식들은 ensemble Gaussian model이랑 safety value function을 써서 PPO랑 SAC 정책을 업데이트 하는데 쓰려고 함.
	- 한계: vision input을 처리하는 태스크에 적용시키는게 한계임.
- 이에 관해, LAMBDA는 Dreamerv1을 Lagrangian이랑 Bayesian 방식 써서 연장함
	- 근데 Dreamerv1에 내재된 한계가 효율을 제한함.
		- disregarding necessary adaptive modifications
		- ubiquitous instabilities
	- 이런 online planning에 관한 문제가 suboptimal한 결과를 만들어냈고, 저차원 문제에 빠졌다. 그래서 world model의 이점을 제대로 활용하지 못함.
- Safe SLAC은 Lagrangian 방법을 SLAC에 합쳐서 vision-only task에서 LAMBDA만큼 성능 내는데에 성공함.
	- 근데 online이나 background planning을 간과해서 safety를 강화하는 데에 world model의 능력을 최대화하지 못함.

# 실험
- Safety-Gymnasium benchmark

# 결과
- 다양한 태스크(저차원 ~ vision-only input까지)에서 거의 zero-cost performance를 달성함
- performance랑 safety 사이에 균형을 잡는데 효율적임.





---


review
3개의 variants
1. constrained cross entropy method (CCEM) 구현, 간단한 MPC 알고리즘으로 high quality trajectory를 filter하고 안전한 trajectory로 수렴하기 위해 action selection parameter를 업데이트 한다.
나머지 둘은 Lagrangian method로 구현함.
2. OSRP-Lag: CCEM이랑 비슷하게, constrained online planning을 위해 PID Lagrangian을 구현함.
3. 마지막으로 BSRP-Lag (augmented Lagrangian)은 policy gradient가 cost TD-lambda estimates를 포함하도록 변경함.
	- 이 버전은 위에 두개랑 달리 online planning 안씀


Safety-Gym의 5개 환경에서 실험함.


약점
1. Safe RL 쪽 관련연구 더 설명해라
2. 여러 방법들 짜집기 했다. (DreamerV3, PID Lagrangian, CCEM, Augmented Lagrangian)
3. 결과 분석 더 해라
	1. 모든 method가 cost에서는 비슷한 성능을 내는 것으로 보임.
	2. 근데 reward는 환경마다 다르게 보임.
		1. 왜 어떤 알고리즘은 특정 태스크에서 reward를 더 받게 되는건지 설명.

답변
2. model-based RL에는 world model을 사용하는 2가지 갈래가 있음
	1. world model을 online planning에 사용
	2. world model로 roll out을 해서 나온 trajectory들을 offline policy optimization에 사용
		1. 이걸 background planning이라 부름
- 우리 주장은, world model로 safe RL에서 zero cost를 이룰 수 있다고 생각함.
	- 이유: online planning이나 background planning으로!
		- 이를 OSRP랑 BSRP-Lag로 입증함.
			- BSRP-Lag: actor에서 생성된 action 분포를 최적화하기 위한 policy optimization의 augmented Lagrangian을 사용함.
			- OSRP: action distribution의 online optimization을 위해 CCEM을 world model이랑 같이 사용함.
		- 근데, cost model의 computational resource랑 error 제약사항 때문에, online planning은 그것의 능력을 long term safety랑 reward 균형 잡는 데에 사용함.
			- 이를 다루기 위해 OSRP-Lag에서 online planning에 Lagrangian이랑 critics를 추가한 것을 보임.

- Lagrangian 방식을 골랐기 때문에 zero cost를 이룬 것이 아니다!
- 중요한 점은 **world model을 이용해서 safety-relevant state를 얼마나 정확하게 모델링 하는가**이다.


 - 지금 constrained policy optimization에서 가장 효과적인 방법
	 - PID Lagrangian
	 - Augmented Lagrangian


- online planning에서, planner는 실시간으로 constrained optimization 문제를 풀어야 한다.
- 우리 알고리즘에서, Lagrangian multiplier의 변화는 환경에서 바로 주는 실제 cost의 variation으로 결정된다.


LAMBDA
- policy optimization하기 위해 bayesian 방식으로 여러 world model parameter를 샘플링하면 point push task처럼 민첩한 task에서 성능 향상 할 수 있다.
	- 왜냐하면 여러 다른 world model에서 학습하는 것을 효과적이게 만들기 때문임.
	- 이는 sim2real에서 부르는 domain randomization과 닮았다.
	- 다른 world model은 다른 physical parameter를 가지게 됨 (차량의 최대 속도나 마찰 같은거)
	- 그래서 이 접근법은 더 robust하게 policy 성능 향상할 수 있음.

W

---

contribution
1. online safety-reward planning
2. integration of Lagrangian methods
3. balancing long-term reward and costs within world models