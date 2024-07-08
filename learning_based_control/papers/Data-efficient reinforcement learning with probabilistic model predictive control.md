
# 배경
 - Trial-and-error 기반 RL 잘함
 
# 문제
- 대다수의 RL은 engineered feature, 또는 환경과의 수많은 interaction에 의존한다.
- 현실에서는 수많은 interaction은 실용적이지 못함. agent 망가지고 환경 손상됨.
# 이 논문
- system interaction을 줄이고, constraints를 다루기 위해, MPC 기반의 model-based RL 제안함.
- 확률적 transition model을 Gaussian Process로 학습해서 model uncertainty를 long term prediction에 통합하고 모델 에러를 줄임.
- MPC로 long term cost 기댓값을 줄이는 control sequences를 찾음.
# experiment
- cart-pole swing-up, double-pendulum swing-up

# 장점
- data efficiency가 좋음. learning rate가 좋음.