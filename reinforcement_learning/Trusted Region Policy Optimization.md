J Schulman, S Levine, P Abbeel, ...
ICML 2015

---

# 배경
- 기존에 policy improvement 하는 방법
	- 모든 $(s,a)$ 순서쌍에 대한 정책의 행동가치함수 $Q_\pi(s,a)$를 계산
	- 이를 policy improvement에 사용
$$\pi_{new}(s)=argmax_{a\in \mathcal{A}} Q_{\pi_{old}}(s,a), \forall s\in\mathcal{S} $$
- 이걸로 정책 개선하면 매 업데이트마다 정책의 성능이 동일하거나 더 좋아지는 monotonic improvement가 보장되고, 업데이트를 반복하면 결국은 최적의 정책에 수렴한다.


# 문제
- 상태 공간과 행동 공간이 large scale이거나 continuous하다면?
	- 모든 순서쌍 $(s,a)$를 고려하기 어려움
	- argmax를 구할 수 없음
	- 그래서 위에 식을 사용할 수 없음...
	- 대신 function approximation을 사용하여 행동가치함수 Q를 추정하거나 정책을 모델링한다.
	- 근데, 이런 approx.는 추정에 대한 오차를 발생시켜서 **monotonic improvement를 보장할 수 없음.**


# 이 논문
- policy improvement 단계에서 어떻게 하면 정책을 **monotonically improve**할 수 있을까에 대한 논문.
- 위 문제에 대해,
	- large scale MDP에서도 정책을 monotonically improve할 수 있는 방법을 제안함.
- 어떻게?