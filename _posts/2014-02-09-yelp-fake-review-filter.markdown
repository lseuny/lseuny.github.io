---
layout: post
title: "Yelp의 허위 리뷰 필터링 들여다보기"
categories: research
noindex: true
---

[허위 리뷰를 탐지하는 기술](http://www.4four.us/article/2012/04/detecting-opinion-spam)이 가장 간절한 곳은 아마도 실제 사용자 평가에 기반해서 서비스를 제공하는 업체일 것이다. 어뷰징은 이들 서비스의 생명인 신뢰를 깎아내려서 사용자를 모두 내쫓아버린다. 그래서 어뷰징 대응 연구를 하기 가장 좋은 곳은, 현실적인(=금전적인) 필요와 내부에서만 관찰할 수 있는 데이터라는 조건이 만족되는 실제 서비스 업체들이다. 미국에는 [옐프(Yelp)](http://www.yelp.com)라는 서비스가 유명한 듯한데, 흥미롭게도 이 사이트는 내부적으로 허위라고 판단한 리뷰를 외부에 알려준다. 상당한 자신감 없이는 하기 힘든 일이다. AAAI 2013에 [What Yelp Fake Review Filter Might Be Doing?](http://www.aaai.org/ocs/index.php/ICWSM/ICWSM13/paper/view/6006)라는 제목으로, 이 사이트의 필터가 어떻게 동작하는지를 외부 연구자들이 "추측"하는 논문이 발표되었다. 특정 회사의 서비스 내부 알고리즘을 -공개도 아니고- 추측하는 것도 논문 주제가 될 수 있나 보다.<!--more-->

### 실제 세상의 허위 리뷰

예전에 [리뷰의 내용, 특히 등장한 단어에 기반해서 스팸 리뷰 찾는 연구](http://www.4four.us/article/2012/04/deceptive-opinion-spam)를 소개한 적이 있는데, 저자들은 이 방법과 옐프의 로직을 비교해보았다. 옐프에서 필터링된 리뷰는 진짜 가짜라고 치고, 단어 기반 탐지기로 얼마나 정확하게 분류할 수 있는지를 실험한 것이다. 결과는 참패. 단순 정확도 기준으로 60%대에 머무르고 말았다.

재미있는 건 여기부터인데, 왜 잘 안 되는지를 알기 위해 정상 리뷰와 스팸(이라고 옐프가 판단한) 리뷰의 차이를 분석했다. 두 그룹에서 단어의 빈도 분포를 뽑아서 KL Divergence를 계산했다. KL Divergence는 두 분포 간의 유사함/차이를 정량화할 때 쓰는 방법 중 하나다.

이를 통해 알아낸 것은, 기존 연구에서 아마존 메커니컬 터크를 이용해서 작성한 실험용 허위 리뷰는 실제 세상의 것 만큼 정교하지 못하다는 점이다. 도메인 지식 없는 비전문가(?)가 상상력에 의존해서 쓰다 보니 단어의 등장 패턴이나 쓰임새가 아무래도 실제 리뷰와 차이가 나고, 또 정말 잘 써야한다는 동기부여도 약했을 테니까 말이다. 반면, 옐프가 걸러낸 리뷰의 특징은 전체적인 단어 사용은 일반 리뷰와 비슷하지만 특정한 어휘군들의 빈도는 특히 더 높더라 하는 게 저자들의 발견이다.

물론 이게 가짜 리뷰의 특징일 수도 있지만, 옐프가 유독 그런 리뷰를 스팸으로 판정했을 가능성도 있다.

### 역시, 사용자 행위 기반 필터링

내용 기반으로 필터링할 수도 있지만, 리뷰 작성자의 행태 및 작성자 그룹을 분석해서 스팸 리뷰를 찾을 수도 있다. 저자들이 옐프 데이터에서 각각의 사용자가 1) 얼마나 몰아서 한꺼번에 리뷰를 쓰는지 2) 5점 만점에 4점 이상을 준 비율이 얼마나 되는지 3) 쓴 리뷰의 길이가 얼마인지 4) 다른 사람의 평가와 얼마나 유사한지 다른지 5) 작성한 리뷰들이 얼마나 비슷한지 등을 이용해서 판별기를 만들었더니, 내용 기반 분류기를 정확도에서 압도했다고 한다. 구현하기 쉬운데 성능까지 좋다니 운영하는 사이트 내의 허위 리뷰로 골머리를 썩히는 사람들에게는 희소식이다.

이 논문은 무척 실용적인 정보를 주지만, 동시에 한계도 뚜렷하다. 특정한 기업의 서비스 결과를 정답으로 놓고, 그 내부를 추정했다. 그래서 저자들의 발견에더 얻은 통찰이 일반화 가능한 것인지 그냥 그 사이트는 그렇더라 정도에서 그쳐야 할지 알 수가 없다. 독자가 알아서 적당히 받아들여야 할 것이다.