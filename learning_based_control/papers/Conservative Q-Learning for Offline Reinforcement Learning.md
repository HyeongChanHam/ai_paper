 - Offline RL 시작 baseline으로 좋음

A Kumar, A Zhou, G Tucker, S Levine
UC Berkeley
2020 NIPS

# 배경
- 강화학습에서 이전에 모았던 방대한 데이터셋을 활용하는 것은 중요하다.
- offline-RL은 환경과 interaction 없이 이전에 모았던 static dataset으로 효과적으로 정책을 학습한다.


# 문제
- 근데 보통의 off-policy RL 방식은 value를 overestimation해서 실패한다.
	- 복잡하고 multi-modal인 데이터 분포일 때
	- 데이터셋이랑 학습된 정책이랑 distributional shift 때문에 실패함.

# 이 논문
- Conservative Q-Learning (CQL) 제안.
	- conservative Q-function을 사용해서
		- Q-function의 하한선 아래의 정책의 가치 기대값은 참값이다.
- CQL은 현재 정책의 가치의 하한선을 제공함.
- 기존의 Bellman error objective를 간단한 Q-learning regularizer로 강화한다.
	- 기존의 deep Q-learning이랑 actor-critic 구현체에 구현하기 직관적임.

# experiments
- discrete, continuous control domain에서 기존의 offline-RL보다 잘함.
	- 2-5배 더 높은 최종 리턴을 얻음


---

# 배경
- 강화학습으로 많은 진보가 있었다.
- 근데 online-RL은 현실에 어려움이 있음
	- 실제 환경과 상호작용이 필요한데, 이는 비용이 들고 위험함.
	- 온라인으로 수집할 수 있는 데이터 양은 지도학습에서 사용되는 오프라인 데이터셋보다 상당히 적음.
	- 강화학습에서 이전에 모았던 방대한 데이터셋을 활용하는 것은 중요하다.
- offline-RL이 나옴.
	- offline-RL은 환경과 interaction 없이 이전에 모았던 static dataset으로 효과적으로 정책을 학습한다.
	- 실제 세계와 상호작용이 없기 때문에 비용이 없고 안전함.
	- 대량으로 데이터 수집 가능하다.


# 문제
- 근데 보통의 off-policy RL 방식은 value를 overestimation해서 실패한다.
	- 복잡하고 multi-modal인 데이터 분포일 때
	- 데이터셋을 수집한 정책이랑 학습된 정책이랑 distributional shift 때문에 실패함.
		- 강화학습은 주로 TD 방식을 기반으로 가치함수 업데이트 함.
		- 이때 다음 타음 스텝의 행동을 선택하기 위해 argmax 연산을 사용함.
		- 이렇게 선택된 행동은 unseen action일 수도 있음.
		- 온라인 강화학습은 중간에 학습된 정책과 환경 간의 상호작용을 통해 다양한 데이터를 수집할 수 있지만, 오프라인 강화학습은 그렇지 못해서 argmax 연산에 의해 unsceen action을 과대평가(overestimation) 할 수 있음.
	- 즉 기존의 value-based off-policy 강화학습을 offline에서 직접 활용하면 일반적으로 out-of-distribution action(=unseen actions)에서의 부트스트래핑 문제 및 오버피팅과 관련된 문제로 인해 성능이 저조함.
	- 이는 잘못된 낙관적(optimistic) 가치함수의 추정으로 나타남.

# 가설
- 가치함수의 보수적인 추정을 학습할 수 있다면, Q값의 overestimation 문제를 해결할 수 있을까?

# 이 논문
- Conservative Q-Learning (CQL) 제안.
	- conservative Q-function을 사용해서
		- Q-function의 하한선 아래의 정책의 가치 기대값은 참값이다.
- CQL은 현재 정책의 가치의 하한선을 제공함.
- 기존의 Bellman error objective를 간단한 Q-learning regularizer로 강화한다.
	- 기존의 deep Q-learning이랑 actor-critic 구현체에 구현하기 직관적임.

# experiments
- discrete, continuous control domain에서 기존의 offline-RL보다 잘함.
	- 2-5배 더 높은 최종 리턴을 얻음