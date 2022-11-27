* #### Neural CF (Neural Collaborative Filtering)

  * Neural CF는 추천 시스템의 **협업 필터링에 딥러닝을 적용한 오픈소스 소프트웨어**이다.

    * [추천 시스템](https://ko.wikipedia.org/wiki/%EC%B6%94%EC%B2%9C_%EC%8B%9C%EC%8A%A4%ED%85%9C) : 정보 필터링 (IF) 기술의 일종으로, 특정 사용자가 관심을 가질만한 정보를 추천하는 시스템이다.

    * [협업 필터링(Collaborative Filtering)](https://developers.google.com/machine-learning/recommendation/collaborative/basics)

      * 많은 사용자들로부터 얻은 기호정보에 따라 사용자들의 관심사들을 자동적으로 예측하게 해주는 방법이다. 
      * 근본적으로 사용자들의 과거의 경향이 미래에서도 그대로 유지 될 것이라는 전제를 기반으로한 선형 함수로 구현된 모델이다. 

    * 기존의 선형 함수로 구현된 협업 필터링에 Deep Neural Networks를 적용하여 선형 함수와 비선형 함수로 구현된 오픈소스 소프트웨어이다.

       



* #### Neural CF의 장점

  * 다른 추천 시스템 오픈소스 중 FM모델을 이용한 오픈소스들은 선형 모델로, 단순히 선형 데이터만을 비교하기 때문에 사용자와 데이터 간의 상호작용(잠재요인)을 분석하는 데에 한계를 가지고 있다. 그에 반해 Neural CF모델은 비선형 모델을 이용하여 사용자와 상호작용하는 **데이터의 복잡한 구조까지 분석**할 수 있다.
    * FM모델 :  Factorization Machines의 약자로 선형 데이터 셋을 계산하는 추천 모델이다.
    * FM을 이용한 오픈소스 소프트웨어로는 DeepCTR, xlearn, fastFM 등이 있다.



* #### Neural CF의 구조

  

  ![Neural_CF](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221117215924420.png)

  

  

  

  * **Input Layer**: 입력으로 들어갈 유저의 데이터(User)와 유저가 구매한 데이터(Item)
  * **Neural CF Layers**: Neural CF 모델
  * **Output Layer**: 추천 선호도에 따른 점수(Score)



* #### Neural CF의 라이선스

  Neural CF는 Apache 2.0 라이선스를 적용하고 있다.

  

  <img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221117220004777.png" alt="image-20221117220004777" style="zoom: 67%;" />

  

  * **Apache 2.0** 
    *  소스 코드 공개의 의무가 없고, 2차 라이선스와 변형물의 특허 출원이 가능하다. 
    * 라이선스 적용 시 아파치 재단의 이름과 라이선스의 내용을 명시해야 하며, 아파치 라이선스 2.0이 적용된 소스 코드를 수정했을 경우 외부에 그 사실을 밝혀야 한다.

