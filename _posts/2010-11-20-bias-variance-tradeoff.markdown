---
layout: post
title: "Bias-Variance Tradeoff: 경험에서 배울 때 주의사항"
categories: thought
noindex: true
---

기계학습, 통계적 추론(Statistical Inference)을 공부하다 보면 언젠가는 Bias-Variance Tradeoff라는 개념을 만나게 된다. 사실, 아무리 기를 써도 피할 수 없다 :) Bias의 사전적 의미는 "편이", "선입견", "편견", "성향", "치우침", Variance는 "변화", "편차", "분산"이다. 기계학습의 문맥에서 이들의 의미는 '학습 모형이 입력 데이터에 얼마나 의존하는가'라고 이해하면 쉬울 것 같다. Bias가 높다 / 낮다는 말의 의미를 혼동하기 쉬운데, 내가 찾아낸 헷갈리지 않는 설명은 이렇다.

Bias, 즉 선입관이 크면, (좋게 말해서) 줏대가 있고 (나쁘게 말해서) 고집이 세기 때문에 새로운 경험을 해도 거기에 크게 휘둘리지 않는다. 평소 믿음과 다른 결과가 관찰되더라도 한두 번 갖고는 콧방귀도 안 뀌며 생각의 일관성을 중시한다. (High Bias, Low Variance) 반대로 선입관이 작으면, (좋게 말하면) 사고가 유연하고 (나쁘게 말하면) 귀가 얇기 때문에 개별 경험이나 관찰 결과에 크게 의존한다. 새로운 사실이 발견되면 최대한 그걸 받아들이려고 하는 것이다. 그래서 어떤 경험을 했느냐에 따라서 최종 형태가 왔다갔다한다. (High Variance, Low Bias)

일란성 쌍둥이가 경제적 능력과 가풍이 크게 다른 두 집안에 입양되었다고 하자. 20년 후 그들은 서로 얼마나 달라져 있을까? 유전적으로 타고난 Bias가 높다면 환경의 차이에도 불구하고 교육으로 학습된 개인차는 상대적으로 작을 것이며, Variance가 크다면 둘은 정말 완전히 다른 사람이 되어 있을 것이다. (수식을 안 적으려고 나름대로 비유한 것이다. Bias와 Variance 용어의 정의가 궁금한 사람은 따로 책을 찾아보도록 하자.)

### 이 개념이 기계학습에서 왜 중요할까?

기계학습이 다루는 중요한 문제 중 하나는 귀납적인 알고리즘을 이용해서 데이터를 잘 분류하는 모형을 찾는 것이다. 그런데 이런저런 제약이 많아서 데이터 전체를 살펴볼 수는 없고 일부만 샘플링해서 학습해야 한다. 모형마다 표현할 수 있는 능력이 다르기 때문에 문제의 복잡도에 따라서 적당한 모형을 고르는 것이 중요하다. 닭 잡는 데 소 잡는 칼을 쓰는 것도, 바늘 들고 소 잡겠다고 설치는 것도 모두 현명한 일이 아니므로. 위키피디아에서 가져온 아래 그림에서 녹색 곡선이 소잡는 칼인데, 과욕이 불러온 참사를 목격할 수 있다. 반대로, 만약 이런 데이터를 곡선이 아닌 직선으로 분류하려고 하면 어떤 일이 생길지를 생각해보자. 그게 바로 바늘이다. 검은선처럼 적당한 도구를 찾아야 한다.

<a title="By Chabacano (Own work) [GFDL (http://www.gnu.org/copyleft/fdl.html) or CC BY-SA 4.0-3.0-2.5-2.0-1.0 (http://creativecommons.org/licenses/by-sa/4.0-3.0-2.5-2.0-1.0)], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File%3AOverfitting.svg"><img width="256" alt="Overfitting" src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Overfitting.svg/256px-Overfitting.svg.png"/></a>

세상일이 복잡다단한데도 자기만의 잣대로 너무 단순하게 해석해버리는 사람을 우리는 순진(naive)하다고 한다. 그런 사람이 내놓는 결론은 정확도가 낮아서 신뢰하기 힘들다. 반면, 자기 경험에 지나치게 생각이 맞춰진(overfitting) 사람의 의견은 그 경험과 조금만 상황이 달라져도 신뢰하기 어렵다. 일부 샘플을 과신해서 그 하나하나에 다 맞추려다 보니 보편성을 잃어버리는 것이다. 그렇다고 다시 변화에 보수적인 태도를 취하면 유연성을 잃어버린다. 경험에서 배우는 게 적고 스스로의 능력에 한계를 지우게 된다. 그 결과, 순진해진다.

이렇게 하나를 추구하면 다른 하나를 희생해야 하기 때문에 Bias와 Variance는 서로 트레이드오프(tradeoff) 관계에 있다고 한다. 결국 답은 이 둘의 합이 최소가 되도록 모델링을 잘 해야 한다는 것인데, 통계학에는 이 Bias와 Variance의 합을 일컫는 용어가 이미 있다. 바로 위험(risk)이다. Bias, Variance, Risk 이런 개념들을 창안한 학자들이 이름 지을 때는 경험을 통해서 일상용어 중 적당한 것을 가져다 붙인 것이겠지만 뒤늦게 공부하는 후학으로서는 거꾸로 이런 학술용어의 의미를 일상에서 곱씹어 보게 된다.

경험으로부터 어떻게 배워야 할까? 기계학습에는 이런 위험을 최소화하는 수학적 방법이 있다지만, 우리 삶에서 위험을 최소화하는 알고리즘은 어디에 있을까? 아니, 있기는 할까?

### 참고자료

- Introduction to Information Retrieval, Christopher D. Manning, Prabhakar Raghavan, Hinrich Schütze, 2008
- All of Statistics: A Concise Course in Statistical Inference, 1ed, Larry Wasserman, 2004