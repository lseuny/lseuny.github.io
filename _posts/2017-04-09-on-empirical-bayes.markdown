---
layout: post
title: "Empirical Bayes 방법에 대해서"
categories: paper
---

Empirical Bayes(EB) 방법에 대해서 간단히 알아보자. [위키피디아](https://en.wikipedia.org/wiki/Empirical_Bayes_method)에서 찾아보면, 통계적 추론을 할 때 사전확률 분포(Prior Distribution)를 데이터로부터 추정하는 방식이라고 설명한다. 데이터를 보기 전에 사전확률을 고정(Fix)하는 베이지안 방법과 대비된다는 내용이 이어진다. 하지만 이런 설명만으로는 개념을 잡기가 쉽지 않은데, 마침 구체적인 사례와 함께 이 방법론을 설명하는 시리즈 글이 두 개가 있다. 첫 번째는 야구 타자의 타율을 추정하는 문제에 대한 [David Robinson의 글](http://varianceexplained.org/r/empirical_bayes_baseball)이고 두 번째는 전자상거래 사이트에서 상품의 노출 대비 판매율을 추정하는 문제에 관해 이베이 테크 블로그에 올라온 [David Goldberg의 글](http://www.ebaytechblog.com/2015/02/06/a-case-study-in-empirical-bayes/)이다.<!--more-->

이 두 경우 모두 성공 아니면 실패라는 결과가 나오는 실험을 n번 수행한 뒤 그중 k번이 성공했을 때 실제 성공 확률을 추정하는 문제로 일반화된다. 가능도(Likelihood)는 이항분포(Binomial)로 하고, 사전확률은 이항분포의 켤레 사전분포(Conjugate Prior)이기도 한 베타분포(Beta)로 해서 베이즈룰을 적용하면 성공 확률의 사후분포(Posterior)를 얻을 수 있다. 실험 데이터가 충분히 많으면 상관없지만 한두 번밖에 실험해보지 못한 대상(=타자/상품)의 확률 예측은 사전확률에 많이 의존하게 된다. 이 사전확률을 만들 때 EB는 어떤 선험적인 지식이 아니라 이미 관찰된 데이터를 이용한다. 이름에 Empirical(경험적)이라는 단어가 들어간 이유가 여기에 있다.

데이터로부터 사전확률을 어떻게 추정하는지 알아보자. 가장 간단한 방법은 사전확률이 베타분포라고 가정했으므로 관찰된 데이터를 가장 잘 맞추는 [하이퍼 파라미터](https://en.wikipedia.org/wiki/Hyperparameter)(사전분포의 파라미터) α, β를 찾는 것이다. 로빈슨의 글을 보면, 타석이 500 이상인 타자들만 골라서 그들의 타율 분포를 R의 fitdistr 함수에 입력해서 하이퍼 파라미터를 추정했다. 더 정확하게 하려면 타수가 적은 타자들까지 전부 포함하면서 선수들 간의 가중치에 대한 고려도 들어가야 한다. 그 방법으로 beta-binomial 분포를 MLE(Maximum Likelihood Estimate)로 추정하는 코드를 부록에서 제시한다. 골드버그도 상품의 판매 확률의 하이퍼 파라미터를 찾기 위해 beta-binomial 분포를 이용하는데, 추론에 사용하는 함수만 다르다 (optimx 패키지). 공통으로 등장하는 [beta-binomial 분포](https://en.wikipedia.org/wiki/Beta-binomial_distribution)는 베타분포와 이항분포를 아래와 같이 합쳐놓은 분포다. 

$$P(k|n, α, β) = \int L(p|k) \cdot \pi(p|α, β)dp$$

$$L(p|k) = Bin(n, p)$$

$$\pi(p|α, β) = Beta(α, β)$$

성공 확률 p가 고정된 값이 아니라 하이퍼 파라미터 (α, β)를 따르는 랜덤변수가 된다. 성공 확률이 p일 때 n번 실험에서 k번 성공할 가능도를 구하고, 이를 가능한 모든 p값에 대해서 가중평균한다. 즉, 이 문제에서 beta-binomial 분포는 관찰된 결과에 대한 하이퍼 파라미터의 가능도가 된다. 그래서 이를 [Marginal Likelihood](https://en.wikipedia.org/wiki/Marginal_likelihood)라고 부른다. (비록 우리가 지금 베이지안 방식으로 분석하고 있지만) 하이퍼 파라미터의 사전분포는 그냥 유니폼하다고 가정하면 MLE로 (α, β)를 결정할 수 있다.

이렇게 하는 것이 무슨 의미가 있는가? 전체 데이터에서 뽑은 평균적인 성공 확률이 사전확률에 반영되고, 아직 관찰/실험 횟수가 적은 데이터는 평균값에 가깝게 예측하게 된다. 타석에 2번 들어가서 2번 안타를 쳤다고 타율 100%라고 치켜세우지 않고, 2번 노출되었지만 구입이 발생하지 않은 상품이 앞으로도 안 팔릴 거라고 무시하지 않는 것이다.

하지만 여기서 끝이 아니다. 사전확률을 전체 평균이 아니라 개별 혹은 그룹에 대해서 다르게 가져갈 수 있다. 생각해보자. 모든 타자에 대해서 타율이 동일할 것이라고 믿는 것이 합당할까? 모든 상품에 대해서 판매 확률이 같다고 보는 것이 적절할까? 내가 팀의 감독 혹은 전자상거래 사이트의 운영자라면 그렇게 할까?

> When players are better, they are given more chances to bat!, from [Understanding beta binomial regression](http://varianceexplained.org/r/beta_binomial_baseball)

타자가 (신인이 아니라면) 경기에서 타석에 들어선 횟수 자체가 그 타자에 타율에 대한 어떤 정보를 준다. 코치가 괜히 있는 게 아니니까 말이다. 이외에도 로빈슨은 어느손잡이인지, 활동한 연도가 언제인지 같은 정보를 이용해서 각 선수에 대한 하이퍼 파라미터를 다르게 적용하기 위해 beta-binomial regression을 [이용](http://varianceexplained.org/r/hierarchical_bayes_baseball)한다. 골드버그는 [Peer Groups in Empirical Bayes](http://www.ebaytechblog.com/2015/10/28/peer-groups-in-empirical-bayes/)에서 상품의 가격에 따라서 판매 확률의 사전분포를 다르게 설정한 방식을 설명한다. 여기서는 beta-binomial regression이 아니라 (α, β)가 가격에 따라 멱함수를 따른다고 가정하고 파라미터를 추정하는 방식을 썼다. 구체적인 추론 방법보다는 데이터에 의해서 사전확률을 모두 다르게 가져간다는 점에 눈길이 가는 건 예전에 관련된 문제로 고민한 적이 있기 때문일 것이다.

이런 방식으로 사전확률을 결정하는 것이 과연 공정하고 객관적인지는 의문을 가질 수도 있겠지만 실제로 무언가를 예측하고 결정해야 하는 상황에서는 데이터에 근거해서 초기 믿음을 설정하고 다시 데이터에 따라서 믿음을 갱신해나갈 수 있는 좋은 틀이라는 생각이 든다.