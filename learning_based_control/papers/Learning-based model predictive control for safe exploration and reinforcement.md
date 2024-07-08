# 배경
- RL이 많은 복잡한 unknown environment의 어려운 문제를 해결함.

# 문제
- 기존의 RL은 학습하면서 safety를 보장하지 않음.

# 이 논문
- 학습하는 동안 high-probability safety guarantees를 주는 learning-based MPC를 제안.
- RL task를 state, control constraints 하에 풀 수 있는 safe learning-based MPC framework 제안.
- method: learning-based MPC의 safety feature를 model-based RL과 결합.
# Experiment
- inverted pendulum system, simulated cart-pole system

# result
- 주어진 task를 학습하며 system의 safety를 보장할 수 있게 됨.