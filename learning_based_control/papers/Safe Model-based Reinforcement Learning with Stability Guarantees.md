# 배경
- RL은 실험 데이터로부터 optimal policy를 학습하는 데에 강력함.
# 문제
- optimal policy 찾는데에 대부분 RL은 모든 가능한 action을 탐색하는데 이건 현실에서 매우 위험하다.
- 그래서 안전이 중요한 현실에서 learning algorithm은 거의 안쓰임.
# 이 논문
- stability 보장 측면에서 안전을 고려하는 learning algorithm을 제시.
- Lyapunov stability verification의 control-theoretic 결과를 확장.
- 입증가능한 stability certificates와 함께 고성능 control policy를 얻기 위해 dynamics의 statistical model을  어떻게 사용하는지 보임.
# experiment
- inverted pendulum