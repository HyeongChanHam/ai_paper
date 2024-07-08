CVPR 2020

# 배경

# 문제
- 이전 방법들
	- multimodal regression
	- occupancy maps
	- 1-step stochastic policies

# 이 논문
- trajectory prediction 문제를 다양한 trajectory 집합에서 classification 하는 문제로 봄!
- 이 trajectory 집합은
	- state space에서 원하는 수준의 coverage를 보장하고
	- 물리적으로 불가능한 trajectory를 제거한다.
- agent의 현재 state에서 trajectory set을 동적으로 생성함

---

# Method

- CoverNet input
	- 모든 agent (vehicles, pedestrians, bicyclists) 의 현재와 과거의 states
	- high-definition map
- output
	- 주어진 vehicle의 미래의 multimodal, probabilistic prediction 