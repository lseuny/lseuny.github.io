---
layout: post
title: "나의 첫 클로저 프로그램"
categories: thought
noindex: true
---

요즘 나의 [버킷테스트](http://www.4four.us/article/2013/03/bucket-test)는 [클로저(Clojure)](http://ko.wikipedia.org/wiki/클로저_(프로그래밍_언어))다. [『해커와 화가』](http://www.4four.us/article/2007/08/hackers-and-painters) 이후로 리스프(Lisp)에 대한 호기심은 항상 있었지만, 번번이 실용성의 문턱을 넘지 못했다. 클로저는 리스프라는 언어의 강점을 유지하면서도 자바 플랫폼과의 호환성을 지니기 때문에 매력적으로 느껴졌다.

리스프를 배우고 싶은 이유는 "코드와 데이터의 형식이 같으며, 매크로(Macro)를 이용해 프로그램을 짜는 프로그램을 만든다"는 말의 의미를 실제로 느껴보고 싶었기 때문이다. 에릭 레이몬드 정도 되는 사람의 [추천사](http://en.wikiquote.org/wiki/Eric_S._Raymond)에도 귀가 팔랑팔랑.

아직은 매크로는 커녕 기본적인 구문도 책 찾아보면서 겨우 문법에 맞춰 타이핑하는 수준이지만, 그래도 책만 읽기보다는 뭔가를 만들면서 배우는 게 낫겠다 싶어서 간단한 프로젝트를 구상했다. 이름 하여 ptimeline. Personalized Timeline의 줄임말로서, 트윗을 추천해주는 프로그램이다. 전체적인 그림은 아래와 같다.

- imatestrobot(아임 어 테스트 로봇)이라는 비밀 계정을 만든다. 내가 지정한 사용자들을 팔로우하면서 그들의 트윗을 수집하기 위한 계정이다.
- ptimeline 프로그램은 트위터 API를 통해서 iamatestrobot이 수집한 트윗을 읽고, 이중에서 추천하는 트윗을 RT한다. (비밀 계정이기 때문에 무엇을 RT하는지는 기본적으로 비공개다.)
- 내 진짜 계정으로 이 로봇을 팔로우하면, 나만의 트윗 추천자가 생기는 것이다.

찾아보니 트위터 API를 클로저로 포장한 [twitter-api](https://github.com/adamwynne/twitter-api)가 있다. 이걸 쓰면 아래의 코드처럼 내 계정의 타임라인을 읽거나, 특정 트윗을 리트윗할 수 있다.

    (defn home-timeline [name]
      (:body (statuses-home-timeline :oauth-creds my-creds
                                     :params {:screen-name name, :count 200})))
    
    (defn retweet [x]
      (statuses-retweet-id :oauth-creds my-creds
                           :params {:id (:id x)}))

메인 함수는 아래와 같다. 추천할 트윗 목록을 얻은 뒤 그중 아직 리트윗하지 않은 것들만 골라서 리트윗한다.

    (defn main []
      (dorun
        (for [x (sort-by :id (recommended-tweet))
              :when (= false (:retweeted x))]
          (retweet x))))

대망의 추천 함수는 매우 간단하다. RT가 2번 이상 되었고, 10자보다 긴 트윗이면 무조건 추천한다.

    (defn recommended-tweet []
      (for [x (map #(get % :retweeted_status %) (home-timeline "imatestrobot"))
            :when (and (< 1 (:retweet_count x))
                       (< 10 (count (:text x))))]
        x))

프로그래밍 언어를 글로만 배우다 보니 그냥 마구잡이로 되는 대로 짜게 된다. 그래도 일단 부딪히고 좋은 예제 코드 찾아보고 하면 나아지겠지.