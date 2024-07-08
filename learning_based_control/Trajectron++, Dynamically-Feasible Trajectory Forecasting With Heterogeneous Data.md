ECCV 2020


# 배경
- 로봇 주행을 위해 human motion에 대해 이해하는 것은 중요하다.
- 그래서 multi-agent behavior prediction이 중요해짐

# 문제
- 기존의 많은 방법들은 dynamic constraints를 고려하지 않음
- map 같은 환경 정보를 반영하지 않음

# 이 논문
- Trajectron++ 제안
	- modular, graph-structured recurrent model
	- 다양한 agent의 수의 trajectory를 예측함.
	- agent의 dynamics와 heterogeneous data (semantic maps)도 같이 고려함.
	- ego-agent의 motion plan에 조건부로 예측을 할 수도 있음.



---


# 4. Trajectron++
## Scene Representation
## Modeling Agent History
## Encoding Agent Interactions
## Incorporating Heterogeneous Data
## Encoding Future Ego-Agent Motion Plans
## Explicitly Accounting for Multimodality
- CVAE latent variable 써서 multimodality를 다룰 수 있음.
- discrete Categorical latent variable $z\in Z$ 를 써서 타겟 $p(y|x)$ 분포를 만들어 냄.
- $z$는 고차원의 latent behavior를 encode하고, $p(y|x)=\sum_{z\in Z} p_\psi (y|x,z)p_\theta (z|x)$로 표현되게 함.
- $|Z|=25$ 이고, $\psi, \theta$ 는 neural net 가중치이다.
- z가 discrete여서 해석하는 데에도 도움됨
	- trajectory를 샘플링 해서 어떤 high-level behavior가 각 z에 해당이 되는지 시각화 해볼 수 있음.
## Producing Dynamically-Feasible Trajectories
- latent variable $z$ 를 얻고 난 뒤, 이거랑 backbone representation vector $e_x$를 decoder (GRU)에 넣는다.
- 각 GRU cell은 bivariate Gaussian distribution의 parameter를 출력함.


- 기존의 방법들이 바로 output position을 출력했던 것과 달리, 우리는 그 trajectory sample이 동적으로 feasible하다는 것을 보장할 수 있음.

## Output Configuration
- Trajectory++는 여러 출력을 만들 수 있다.
1) Most Likely (ML)
2) $z_{mode}$
3) Full
4) Distribution
	- discrete latent variable과 Gaussian output structure를 쓰기 때문에, $p(y|x)=\sum_{z\in Z} p_\psi (y|x,z)p_\theta (z|x)$ 를 계산해서 결과 분포를 제공할 수 있음.

## Training the Model
- InfoVAE objective function 사용
- 