# 배경
- 현실에 RL 적용하는데 safety가 문제가 되고 있음.
- uncertain environment에서 새로운 task 배우려면 탐색 많이 해야 함.
- 근데, safety 관점으로 보면 탐색은 제한된다.
# 이 논문
- Recovery RL: 이 tradeoff 를 다룬다.
1) policy learning 하기 전에 constraint violating zones에 대해 배우기 위해 offline data를 활용.
2) task 성능과 constraint satisfaction 목표를 분리함.
# Experiment
- 많이 함.