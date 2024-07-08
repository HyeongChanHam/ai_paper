J Schulman, F Wolski, P Dhariwal, A Radford, O Klimov
OpenAI
2017



# 배경

강화학습을
- 주어진 데이터를 가지고,
- 현재 policy를 최대한 큰 step만큼 빠르게 향상시키면서,
- 그렇다고 성능이 발산해버릴 정도로 너무 큰 step으로 업데이트 하는 것은 억제

# 이 논문
- TRPO의 업데이트 크기를 clip하여 정책을 조금씩만 업데이트 하는 방법
	- 원래 TRPO에서는 penalty term으로 KL Divergence를 사용했는데, 이거를 clipping으로 바꿈
- TRPO를 실용적으로 발전시킴
	- second-order가 first-order로 계산이 가능해져서 실용적이게 됨.



# 결과 
- performance와 complexity의 벨런스가 잘 잡혀있음