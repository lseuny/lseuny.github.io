---
layout: post
title: "왜 분산을 구할 때 차이의 제곱을 쓰는가?"
categories: research
noindex: true
---

회사에서 점심을 먹던 중 분산을 구할 때 왜 데이터 값과 평균의 차이의 -절대값이 아니라- 제곱을 쓰냐는 얘기가 나왔다. 호기심이 생겨서 찾아보다가 이런 내용을 발견했다.

> what is the degree of variability of these data? Intuitively, the best measuring index for it is the one which is minimized (or maximized) down to the limit in this context. The context is "around the arithmetic mean". Then st. deviation is the best choice in this sense. If the context were "around the median" then mean deviation would be the best choice, because median is the locus of minimal sum of absolute deviations from it., from [Mean absolute deviation vs. standard deviation](http://stats.stackexchange.com/questions/81986/mean-absolute-deviation-vs-standard-deviation)

주어진 데이터를 대표하는 값을 뽑아야 한다고 생각해보자. 그 대표값은 어떤 조건을 만족해야 할까? 그 값을 중간이라고 생각했을 때, 전체 데이터의 퍼진 정도(Spread, Dispersion, Variability)가 최소가 되는 지점을 찾으면 되지 않을까? 퍼진 정도를 측정하는 방법은 여러 가지가 있다. 우리가 아는 분산은 개별 값과 대표값의 차이의 제곱의 평균이다. 반드시 제곱을 취해야 하는 건 아니고, 그냥 차이의 절대값을 취한 평균은 [Average absolute deviation](http://en.wikipedia.org/wiki/Absolute_deviation#Mean_absolute_deviation_.28MAD.29_.28about_mean.29)이라고 부른다. 우리가 아는 분산을 최소화하는 대표값이 바로 평균이고, Average absolute deviation을 최소화하는 대표값은 중앙값(Median)[이라고 한다](http://en.wikipedia.org/wiki/Central_tendency). 이런 관점에서 보면 왜 분산을 구할 때 제곱을 하는지를 묻기 전에 왜 대표값으로 평균을 쓰는지를 먼저 물어야 한다. 그 대답은 평균이 분산으로 표현되는 퍼진 정도를 최소화하는 값이기 때문에 대표로 적당하다는 것이다.

제곱을 쓰면 절대값을 쓸 때보다 어떤 유용함이 있다는 설명도 좋지만, 당연히 이렇게 될 수밖에 없는 어떤 당위성을 발견했다는 느낌이다. 이런 관계가 성립한다면, 평균을 쓸 때는 퍼진 정도를 측정하기 위해 분산 혹은 표준편차를 이용하고 중앙값을 쓸 때는 Average absolute deviation을 이용하는 게 자연스럽게 느껴진다.

> The mean and the standard deviation of a set of data are descriptive statistics usually reported together. In a certain sense, the standard deviation is a "natural" measure of statistical dispersion if the center of the data is measured about the mean. This is because the standard deviation from the mean is smaller than from any other point., from [위키피디아의 Standard deviation 설명](http://en.wikipedia.org/wiki/Standard_deviation) 중