Beomjun Kim, Heejin Ahn
American Control Conference (ACC) 2023

# 배경
- AV는 다양한 환경에서 동작해야 함.
- 현재 환경을 인식하는건 종종 어려움.
	- traffic sign에 적힌 speed limit을 지키고 멈춰야 한다.

	
# 문제
- 근데 neural net 같은 perception module은 이 traffic sign을 정확히 읽는다고 보장할 수 없음.
- perception module은 uncertain output을 냄.

# 이 논문
- 가능한 mode에 따라 constraints를 만족하도록 높은 확률로 보장할 수 있는 chance-constrained control 구현을 함.
	- 이를 위해 베이즈 룰과 샘플링 기반으로 각 모드의 확률을 계산하기 위해 방법을 제시함.

# 결과
- 우리 방법은 새로운 상황에서 constraints를 만족할 수 있음.
	- perception module 학습 때 쓰이지 않은 상황에 가능
- 한정된 데이터로 인한 에러를 설명하기 위해 우리는 높은 확률로 constraint를 만족함을 보장할 수 있는 robust formulation을 제안함.


---

# 배경
- AV는 다양한 환경에서 동작해야 함.
- 현재 환경을 인식하는건 종종 어려움.
	- traffic sign에 적힌 speed limit을 지키고 멈춰야 한다.

	
# 문제
- 근데 neural net 같은 perception module은 이 traffic sign을 정확히 읽는다고 보장할 수 없음.
- perception module은 uncertain output을 냄.

# 이 논문
- 가능한 mode에 따라 constraints를 만족하도록 높은 확률로 보장할 수 있는 chance-constrained control 구현을 함.
	- 이를 위해 베이즈 룰과 샘플링 기반으로 각 모드의 확률을 계산하기 위해 방법을 제시함.

# 결과
- 우리 방법은 새로운 상황에서 constraints를 만족할 수 있음.
	- perception module 학습 때 쓰이지 않은 상황에 가능
- 한정된 데이터로 인한 에러를 설명하기 위해 우리는 높은 확률로 constraint를 만족함을 보장할 수 있는 robust formulation을 제안함.


