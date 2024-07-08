![[Pasted image 20240702182803.png]]

# 분류

## Safe Learning Control Approaches
### 1. Learning Uncertain Dynamics to Safely Improve Performances
	1) Integrating machine learning and adaptive control
		1) Learning nonparametric unknown dynamics with machine learning models
		2) Cautious adaptation with probabilistic model learning
		3) Memorizing experience with deep architectures
	2) Learning-based robust control
		1) Using a Gaussian process dynamics model for linear robust control
		2) Exploiting feedback linearization for robust learning-based tracking
	3) Reducing conservatism in robust model predictive control with learning and adaptation
		1) Robust adaptive model predictive control
		2) Learning-based robust model predictive control
	4) Safe model-based reinforcement learning with a priori dynamics
### 2. Encouraging Safety and Robustness in Reinforcement Learning
	1) Safe exploration and optimization
		1) Safe exploration
		2) Safe optimization
		3) Learning a safety critic
	2) Risk-averse reinforcement learning and uncertainty-aware reinforcement learning
	3) Constrained Markov decision processes and reinforcement learning
		1) Lagrangian methods in reinforcement learning optimization
		2) A Lyapunov approach to safe reinforcement learning
		3) Learning backward value functions
	4) Robust Markov decision processes and reinforcement learning
		1) Robustness through adversarial training
		2) Robustness through domain randomization

### 3. Certifying Learning-Based Control Under Dynamic Uncertainty
	1) Stability certification
		1) Lipschitz-based safety certification for deep neural network-based learning controllers
		2) Learning regions of attraction for safety certification
	2) Constraint set certification
		1) Control barrier functions
		2) Hamilton-Jacobi reachability analysis
		3) Predictive safety filters



---

## Safe Learning Control Approaches
### 1. Learning Uncertain Dynamics to Safely Improve Performances
0) 
	 - robot dynamics의 priori model을 사용함.
	 - data에서 uncertain dynamics를 학습해서 성능 향상.
	 - safety는 standard control-theoretic framework로 보장됨. 
	 
1) Integrating machine learning and adaptive control
	1) Learning nonparametric unknown dynamics with machine learning models
	2) Cautious adaptation with probabilistic model learning
	3) Memorizing experience with deep architectures
2) Learning-based robust control
	1) Using a Gaussian process dynamics model for linear robust control
	2) Exploiting feedback linearization for robust learning-based tracking
3) Reducing conservatism in robust model predictive control with learning and adaptation
	1) Robust adaptive model predictive control: parametric uncertainty로 adapt한다.
		- [[Learning model predictive control for iterative tasks. a data-driven control framework]]
	2) Learning-based robust model predictive control: unknown dynamics f를 학습함.
		- [[Data-efficient reinforcement learning with probabilistic model predictive control]]
		- [[Learning-based model predictive control for safe exploration and reinforcement]]
4) Safe model-based reinforcement learning with a priori dynamics
	- [[Safe Model-based Reinforcement Learning with Stability Guarantees]]
### 2. Encouraging Safety and Robustness in Reinforcement Learning
0) 
	- priori robot model이나 safety constraints에 대한 지식이 없는 경우
	- hard safety guarantees를 주는 것 보다는, safe robot operation을 권장함. (위험한 action을 penalize하는 식으로.)
1) Safe exploration and optimization
	1) Safe exploration
	2) Safe optimization
		- [[Safe exploration in finite Markov decision processes with Gaussian processes]]
		- [[Safe exploration for optimization with Gaussian processes]]
		- [[Bayesian optimization with safety constraints safe and automatic parameter tuning in robotics]]
	3) Learning a safety critic
		- [[Recovery RL Safe reinforcement learning with learned recovery zones]]
		- [[Conservative Q-Learning for Offline Reinforcement Learning]]
2) Risk-averse reinforcement learning and uncertainty-aware reinforcement learning
3) Constrained Markov decision processes and reinforcement learning
	1) Lagrangian methods in reinforcement learning optimization
		- [[ai_paper/learning_based_control/papers/Constrained Policy Optimization]]
	2) A Lyapunov approach to safe reinforcement learning
	3) Learning backward value functions
1) Robust Markov decision processes and reinforcement learning
	1) Robustness through adversarial training
		- [[Active domain randomization]]
	2) Robustness through domain randomization

### 3. Certifying Learning-Based Control Under Dynamic Uncertainty
0) 
	-  목적: 원래 safety constraints를 고려 안하는 learning-based controller에게 safety certificate를 제공하는 것.
	- learning controller의 출력을 우리가 알고있는 safe backup controller를 사용해서  constraint를 주거나, 아니면 stability나 constraint satisfaction을 만족하도록 바꾼다.
1) Stability certification
	1) Lipschitz-based safety certification for deep neural network-based learning controllers
	2) Learning regions of attraction for safety certification
		- [[The Lyapunov neural network: Adaptive stability certification for safe learning of dynamical systems]]
1) Constraint set certification
	1) Control barrier functions
		- [[End-to-end safe reinforcement learning through barrier functions for safety-critical continuous control tasks]]
	2) Hamilton-Jacobi reachability analysis
	3) Predictive safety filters


