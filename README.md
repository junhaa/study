

![image-20221126110745578](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126110745578.png)





---

# Introduction



### 1. Purpose of Document

---

 사람들은 매일 아침 점심 저녁을 먹지만 매끼 같은 음식을 먹지 않는다. 또한 매끼 같은 음식을 먹고 싶지도 않다. 메뉴 선택은 알게 모르게 고민거리가 되기도 한다. 반복되는 일상 속에서 식사시간이 되면 항상 드는 생각인 “오늘 뭐를 먹을까? “를 주제로 삼게 되었다. 코로나 19로 인해 우리 일상에 더 가까워진 배달 문화는 우리 식탁에 더욱 많은 종류의 음식을 배달 가능하게 했다. 배달 음식점의 증가는 종류의 다양화로 이어졌고 이전에 치킨, 피자, 짜장면 같았던 대표적인 배달 음식에서 찜닭, 아귀찜, 쌀국수, 샌드위치, 커피 같은 새로운 배달음식들이 생겨났다. 수 많은 배달 음식 중에서 그날그날 먹고 싶은 음식을 바로 고르기 힘들 때가 있다. 이점에 착안하여 '**momuk**'이라는 **메뉴 추천 서비스**를 개발하게 되었다. 본 문서는 소비자 개인에 맞춰 배달 메뉴를 추천해주는 서비스 'momuk'에 대해 다루어 보려고 한다. 



### 2. Methodology, Tools, and Approach

---

 본 문서는 DFD를 이용하여 'momuk' 내의 데이터 흐름을 분석하고, 사용된 오픈소스 소프트웨어의 정보를 제공한다.

'momuk'에 사용된 오픈소스는 다음과 같다.

|      오픈소스      |                             역할                             |
| :----------------: | :----------------------------------------------------------: |
|    **Flutter**     |     오픈 소스 크로스 플랫폼 GUI 애플리케이션 프레임워크      |
|     **Django**     |                    웹 서버 구축 오픈소스                     |
|   **Neural CF**    |       딥 러닝을 이용한 추천 시스템 오픈소스 소프트웨어       |
| **Naver 지도 API** |            Naver 지도 데이터를 이용하기 위한 API             |
|    **Leaflet**     |          자바 스크립트 오픈소스 웹 매핑 라이브러리           |
|     **Saleor**     |                 전자 상거래를 위한 오픈소스                  |
|      **OSRM**      | OpenStreetMap의 데이터를 사용하도록 설계된 오픈 소스 라우터  |
|     **Docker**     | 워크플로우 가속화, 애플리케이션 스택 및 구축 환경 지원 오픈소스 |
|     **SQLite**     |                  데이터베이스 관리 오픈소스                  |





---

# Design Overview



### 유사 서비스 분석

![image-20221127133430219](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221127133430219.png)

* **배달의 민족, 요기요, 쿠팡이츠 등 배달 앱**
  * 위치기반 서비스이면서 O2O서비스인 위의 앱들을 가장 참고하여 만들었다.
    * O2O서비스 : 온라인에서 소비자를 모아 오프라인의 판매처로 연결해 준다는 것



* **AI 소믈리에**
  * 와인 추천 어플이지만 소비자의 기호에 맞춰 와인을 추천해 준다. 
  * 와인의 종류와 맛이 다양하고 소믈리에라는 직업이 존재할 정도로 와인에 대한 이해가 필요한 분야이기에 서비스가 생긴 것 같다.



* **요고모고**
  * 최근 섭취한 음식 세가지를 입력하면 그 성분을 분석해 과잉된 영양소를 배제하고 부족한 영양소는 보충하는 등, 영양소 균형을 위한 음식을 추천해주는 서비스이다. 
  * 이용자가 못 먹는 음식을 입력하면 해당 음식은 추천 메뉴에서 제외하며 요일별 스트레스 지수, 피로도, 면역력 등을 종합 검토해, 이용자가 실제로 적합한 점심메뉴를 선택해 준다.



### 'momuk'의 배경 정보

위의 유사 서비스 분석을 기반으로 다음과 같이 'momuk'을 개발할 예정이다.

* Flutter로 **ios와 Android** **크로스 플랫폼으로 개발**을 하게 된다. 

  * django 오픈소스로 웹 서버를 구축한다. 

    * 사용자가 정보 요청 시 db에 저장되어 있는 정보를 불러와 사용자에게 전송하는 역할을 한다. 

  * 데이터베이스 관리 오픈소스인 SQLite 로 데이터베이스를 관리를 하게 된다. 

    *  딥러닝을 할 유저 구매 데이터 등을 관리하는 역할을 한다. 

  * 시중에 나와 있는 여러 배달 앱들의 주문 기록과 리뷰를 Neural Collaborative Filtering (Neural CF)으로 딥러닝한다. 

  * 받아온 유저 데이터와 유저 구매 데이터를 Neural CF에 입력하고 음식에 대한 추천 점수를 반환해 전달한다. 

    * 자신이 가장 자주 주문했던 음식을 추천하고, 리뷰에 적은 평점에 근거하여 음식을 추천한다. 다른 사용자들의 데이터들도 종합하여 사용자에게 적합한 색다른 음식을 추천해 준다.

      

* 위치기반 오픈소스 OSRM과 네이버지도 API, Leaflet을 이용하여 **배달서비스도 제공**한다. 

  * 네이버지도의 기능을 통해 위치기반으로 작동을 하며 네이버지도에 등록된 상점들과 연동이 된다. 

  * 웹 매핑 라이브러리 오픈소스인 Leaflet를 통해 추천받은 가게를 지도에 나타나게 한다.

  *  OSRM을 통해 유저와 가게 좌표 정보를 네트워크에 스냅하여 가장 가까운 일치 항목을 반환하는 방식으로 배달서비스를 하게 된다.

    

* 기본적인 E-커머스의 기능인 **결제, 장바구니, 주문과 더불어 관리가 용이한 대쉬보드**까지 제공한다.





---

# Logical Architecture



### 1. Flutter

---



### 2. Django

---



### 3. Neural CF

---

* #### Neural CF (Neural Collaborative Filtering)

  * Neural CF는 추천 시스템의 **협업 필터링에 딥러닝을 적용한 오픈소스 소프트웨어**이다.

    * [**추천 시스템**](https://ko.wikipedia.org/wiki/%EC%B6%94%EC%B2%9C_%EC%8B%9C%EC%8A%A4%ED%85%9C) : 정보 필터링 (IF) 기술의 일종으로, 특정 사용자가 관심을 가질만한 정보를 추천하는 시스템이다.

    * [**협업 필터링(Collaborative Filtering**)](https://developers.google.com/machine-learning/recommendation/collaborative/basics)

      * 많은 사용자들로부터 얻은 기호정보에 따라 사용자들의 관심사들을 자동적으로 예측하게 해주는 방법이다. 
      * 근본적으로 사용자들의 과거의 경향이 미래에서도 그대로 유지 될 것이라는 전제를 기반으로한 선형 함수로 구현된 모델이다. 

    * 기존의 선형 함수로 구현된 협업 필터링에 Deep Neural Networks를 적용하여 선형 함수와 비선형 함수로 구현된 오픈소스 소프트웨어이다.

       



* #### Neural CF의 장점

  * 다른 추천 시스템 오픈소스 중 FM모델을 이용한 오픈소스들은 선형 모델로, 단순히 선형 데이터만을 비교하기 때문에 사용자와 데이터 간의 상호작용(잠재요인)을 분석하는 데에 한계를 가지고 있다. 그에 반해 Neural CF모델은 비선형 모델을 이용하여 사용자와 상호작용하는 **데이터의 복잡한 구조까지 분석**할 수 있다.
    * FM모델 :  Factorization Machines의 약자로 선형 데이터 셋을 계산하는 추천 모델이다.
    * FM을 이용한 오픈소스 소프트웨어로는 DeepCTR, xlearn, fastFM 등이 있다.



* #### Neural CF의 구조

  

  ![image-20221117215924420](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221117215924420.png)

  

  * **Input Layer**: 입력으로 들어갈 유저의 데이터(User)와 유저가 구매한 데이터(Item)
  * **Neural CF Layers**: Neural CF 모델
  * **Output Layer**: 추천 선호도에 따른 점수(Score)



* #### Neural CF의 라이선스

  Neural CF는 Apache 2.0 라이선스를 적용하고 있다.

  

  <img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221117220004777.png" alt="image-20221117220004777" style="zoom: 67%;" />

  

  * **Apache 2.0** 
    *  소스 코드 공개의 의무가 없고, 2차 라이선스와 변형물의 특허 출원이 가능하다. 
    * 라이선스 적용 시 아파치 재단의 이름과 라이선스의 내용을 명시해야 하며, 아파치 라이선스 2.0이 적용된 소스 코드를 수정했을 경우 외부에 그 사실을 밝혀야 한다.





### 4. Leaflet

---



<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126151341972.png" alt="image-20221126151341972" style="zoom: 80%;" />



* #### Leaflet

  Leaflet은 가볍고 간단한 **Mapping을 할 수 있는 오픈 소스 자바 스크립트 라이브러리**이다. 

  

* #### Leaflet의 장점

  * 전체 라이브러리가 28K로 가볍다.

    

  * 모든 **주요 데스크톱 및 모바일 플랫폼**에서 효율적으로 작동한다.

    * HTML5와 CSS3를 지원하고 대부분의 모바일 및 데스크톱 플랫폼을 지원한다.

      

  * 많은 **플러그인으로 확장**이 가능하다.

    * [플러그인](https://kathak33.tistory.com/14) : HTML 플러그인이란 웹 브라우저의 표준 기능을 확장해 주는 프로그램을 의미한다. 가장 널리 알려진 플러그인으로는 Java Applet, Flash Player, Pdf Reader 등이 있다. 이러한 플러그인은 object 요소나 embed 요소를 사용하여 HTML 문서에 추가할 수 있다.

      

  * GIS 배경지식이 없는 개발자들도 쉽게 표출이 가능하다.

    * [GIS]( http://www.nsdi.go.kr/lxportal/?menuno=4066) : 인간생활에 필요한 지리정보를 컴퓨터 데이터로 변환하여 효율적으로 활용하기 위한 정보시스템이다. 

      * 모든 지리정보가 수치데이터의 형태로 저장되어 사용자가 원하는 정보를 선택하여 필요한 형식에 맞추어 출력할 수 있다.

      * 다량의 자료를 컴퓨터 기반으로 구축하여 정보를 빠르게 검색, 결합하여 통합 분석 환경을 제공한다.

        

  * 간단하고 **가독성이 좋은 소스 코드**이다.



* #### Leaflet의 단점

  * 다양한 옵션이나 복잡한 지도의 상호작용은 포함되어 있지 않을 수 있다.



* #### Leaflet 라이선스

  Leaflet은 **BSD-2-Clause** 라이선스를 적용하고 있다.

  ![image-20221126152131057](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126152131057.png)

  * **[BSD](https://ko.wikipedia.org/wiki/BSD_%ED%97%88%EA%B0%80%EC%84%9C) (Berkeley Software Distribution)** 
    * BSD 허가서는 자유 소프트웨어 저작권의 한 종류이다. 
    * BSD 라이선스는 아무나 개작할 수 있고, 수정한 것을 제한 없이 배포할 수 있다. 다만 수정 본의 재배포는 의무적인 사항이 아니므로 아무런 제한 없이 누구나 자신의 용도로 사용할 수 있고, 공개하지 않아도 되는 상용 소프트웨어에서도 사용할 수 있다. 
    * 대신 사용자의 사용으로 인해 발생하는 모든 책임은 사용자 본인에게 있다.







### 5. Saleor

---



<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126160929876.png" alt="image-20221126160929876" style="zoom: 50%;" />



* #### Saleor

  * python 및 Django를 기반으로 서비스를 제공 하는 **전자 상거래(E-commerce) 오픈소스**이다.
  * github에서도 별점이 19,000개가 넘는 상당히 인기 있는 프로젝트이다.
  * 배달과 매장이 연동되어 결제 단계를 구축할 때 사용하게 되는 오픈소스이다.



* #### Saleor의 장점

  * **간편한 설치**

    * 개발 환경이 Docker로 설정되어 있기에 개발 환경을 따로 설치할 필요가 없다.

    * 오픈소스를 다운로드 후 실행이 몇 개의 커맨드 실행으로 가능하다.

      1. 프로젝트 다운로드, 도커 빌드

         ```bash
          git clone https://github.com/saleor/saleor-platform.git --recursive --jobs 3
          cd saleor-platform
          docker-compose build
         ```

      2. 프로젝트에 필요한 테이블 생성

         ```bash
         docker-compose run --rm api python3 manage.py migrate
         docker-compose run --rm api python3 manage.py collectstatic --noinput
         ```

      3. 샘플 데이터 생성

         ```bash
         docker-compose run --rm api python3 manage.py populatedb
         ```

      4. 서버 실행

         ```bash
         docker-compose up
         ```

  

  *  **잘 정리된 도큐먼트**

    * 기능별 매뉴얼이 잘 정리되어 있다.

    * 검색도 가능하며 문서를 한글로 변역하면 충분히 볼 수 있을 정도로 잘 정리되어 있다. 

    * 디자인 UI/UX

      * 기존의 이커머스 사이트들은 기능면을 중요시하다 보니 전체적인 디자인과 UI/UX의 퀄리티가 지 않은 경우가 많다.

      * saleor만의 독특한 콘셉트와 디자인 UI/UX는 상당히 퀄리티가 높다는 걸 알 수 있다.

        

  * **버그 대응 유지 보수**

    * saleor의 최신 버전(2022년 11월 기준)은 3.8.0이며 10/28일 업데이트가 되었다.

    * 업데이트가 활발히 이루어지는 프로젝트라는 것을 알 수 있다.

    * 사이트 통계와 제품 관리를 위한 대시보드

      * 대시보드는 사용자와 관리자의 주문, 상품 등록 수정, 삭제, 결제, 통계 등의 기능을 볼 수 있게 하여 꼭 필요한 기능이다.

      * 가독성도 좋아야 하며 다양한 기능이 지원되어야만 한다.

      * saleor의 대시보드 역시 사용자 친화적이며 EC 사이트 운영에 필요한 다양한 기능을 갖추었다.

        

  * **모바일 버전 지원**

    * 최근 이커머스 사이트는 모바일의 접속이 더 많다.

    * saleor의 경우 모바일 접속 시 모바일 버전을 지원하고 있다.

      

  * **Saleor의 특징**

    1. GraphQL API : 단일 요청 등으로 많은 리소스 가져 오기 가능
    2. 다채널 : 가격, 통화, 주식, 제품 등의 채널별 제어가 가능
    3. CMS : 다양한 제품의 카탈로그를 관리할 수 있는 기능
    4. 대시보드 : 사용자 친화적이고, 빠르고 생산적이며 관리 용이
    5. 글로벌 디자인 , 다중 언어, 다중 창고 지원
    6. 주문 : 주문, 발송, 환불을 위한 종합시스템 지원
    7. 장바구니 : 할인 및 프로모션을 완벽하게 제어하는 선지급 및 세금 옵션
    8. 결제 : 유연한 API 아키텍처로 모든 결제 수단 통합 가능
    9. SEO : 더 많은 고객에게 제품이 노출되는 최적화된 SEO
    10. 간편한 클라우드 배포 : Docker를 사용한 배포에 최적화



* #### Saleor 라이선스

  Saleor는 **BSD-3-Clause** 라이선스를 적용하고 있다.

  ![image-20221126152131057](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126152131057.png)







### 6. OSRM (Open Source Routing Machine)

---



<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126183524007.png" alt="image-20221126183524007" style="zoom:80%;" />



* #### OSRM

  * C++로 작성된 고성능 라우팅 엔진으로, **OpenStreetMap 데이터에서 실행**되도록 설계 되었다.
    * **OpenStreetMap** : 누구나 참여할 수 있는 오픈소스 방식의 무료 지도 서비스이다. 
      * ODbL 라이선스가 적용되는 오픈스트리트맵으로 이미지 파일 등을 만들었을 때는, 저작자((c)OpenStreetMap Comtributor) 표기만 한다면 어떤 라이선스로든 배포가 가능하다. 단, 오픈스트리트맵을 활용해 새로운 '데이터베이스'를 만들었거나, 오픈스트리트맵 데이터베이이스를 그대로 활용하지 않고 데이터베이스를 수정한 뒤 수정한 DB로 작업물을 만들었을 경우, 해당 데이터베이스를 ODbL 라이선스로 배포해야 한다.

  

* #### OSRM의 장점

  * OSRM은 **무료 네트워크 서비스**이며, Linux, FreeBSDm Windows 및 Mac OS X 플랫폼을 지원한다.

  * OSRM은 오픈소스이므로 범위 내에서 원하는 로직으로 **customization이 가능**하다.
  * 네이버나 구글지도 API와 달리 OpenStreetMap과 OSRM을 활용하면 **비용이 들지 않는다**.
  * HTTP API, C++ 라이브러리 인터페이스, NodeJs wrapper를 통해 사용할 수 있는 OSRM 서비스는 다음과 같다.
    * **Nearest** - 거리 네트워크에 좌표를 스냅하고, 가장 가까운 일치 항목을 반환
    * **Route** - 좌표 사이에서 가장 빠른 경로를 탐색
    * **Table** - 제공된 모든 좌표 쌍 사이의 가장 빠른 경로의 지속 시간 또는 거리 계산
    * **Match** - 가장 타당한 방법으로 노이즈가 많은 GPS 추적을 도로 네트워크에 스냅
    * **Trip** - 탐욕 알고리즘을 사용하여 TSP(Travelling Salesman Problem)을 해결
      * 탐욕 알고리즘 : 각 단계에서 국소적으로 최적의 선택을 하는 문제 해결 휴리스틱을 따르는 알고리즘이다.
      * TSP : 일련의 지점과 방문해야 하는 위치 사이의 최단 경로를 찾는 임무를 맡은 알고리즘 문제
    * **Tile** - 내부 라우팅 메타데이터가 있는 맵박스 벡터 타일 생성
      * 맵박스 : 맞춤형 디자인 맵을 위한 오픈소스 매핑 플랫폼
      * 벡터 타일 : 점, 선 및 다각형과 같은 지리공간 벡터 데이터를 저장하기 위한 경량 데이터 형식



* #### OSRM의 라이선스

  OSRM은 **BSD-2-Clause** 라이선스를 적용하고 있다.

![image-20221126152131057](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126152131057.png)









### 7.Docker

---



<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221127183803179.png" alt="image-20221127183803179" style="zoom:150%;" />





* #### Docker

  * Docker는 워크플로우를 단순화하고 가속화하는 동시에 개발자가 각 프로젝트에 대해 툴, 애플리케이션 스택 및 구축 환경을 선택하여 자유롭게 혁신할 수 있도록 지원하는 오픈소스 프로젝트이다.
  * 도커 컨테이너 기술은 2013년 오픈소스 도커 엔진으로 출시되었다.



![image-20221127183926285](https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221127183926285.png)



* Docker는 아래의 기능들로 **애플리케이션의 복잡성을 극복**할 수 있다.
  - **Keep It Simple** - 친숙한 CLI 기반 워크플로우는 모든 기술 수준의 개발자가 컨테이너형 응용프로그램을 구축, 공유 및 실행
  - **Move Fast** – 단일 패키지에서 설치하여 몇 분 만에 실행 가능. 개발과 생산 간의 일관성을 보장하면서 로컬에서 코드를 작성하고 테스트함
  - **Collaborate** – 커뮤니티에서 제공되는 인증된 이미지를 프로젝트에 사용 가능. 클라우드 기반 응용프로그램 레지스트리에 푸시하고 팀 구성원과 협업



* #### Docker 라이선스

  Docker는 **Apache 2.0** 라이선스를 적용하고 있다.

<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221117220004777.png" alt="image-20221117220004777" style="zoom: 67%;" />





---

# Data Model





### Database Management System Files - SQLite

---



<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221126130206559.png" alt="image-20221126130206559" style="zoom: 150%;" />





* #### SQLite 

  **SQLite**는 MySQL이나 PostgreSQL과 같은 **데이터베이스 관리 시스템**이지만, 서버가 아니라 응응프로그램에 넣어 사용하는 **비교적 가벼운 데이터베이스**이다. 일반적인 RDBMS에 비해 대규모 작업에는 적합하지 않지만, 중소 규모라면 속도에 손색이 없다. 또 API는 단순히 라이브러리를 호출하는 것만 있으며, 데이터를 저장하는 데 **하나의 파일만을 사용하는 것**이 특징이다. 

  

* #### SQLite의 장점

  * SQLite의 가장 일반적이며 대표적인 사용례는 **전통적인 테이블 지향 관계형 데이터베이스** 역할이다.

    *  SQLite는 트랜잭션과 원자성 동작을 지원하므로 **프로그램 충돌이나 정전이 발생하더라도 데이터베이스가 손상되지 않는다**.

      

  * SQLite에는 전체 텍스트 인덱싱, JSON 데이터 지원 등 고급 데이터베이스에서 볼 수 있는 여러 기능이 있다. 

    * 일반적으로 YAML 또는 XML과 같이 반구조적인 형식으로 저장되는 애플리케이션 데이터는 SQLite 테이블로 저장할 수 있으므로 데이터에 더 쉽게 액세스하고 더 신속하게 처리할 수 있다.

      

  * SQLite는 프로그램의 구성 데이터를 저장하는 **빠르고 강력한 수단**도 제공한다. 

    * 개발자는 YAML과 같은 파일 형식을 파싱하는 대신 SQLite를 이러한 파일의 인터페이스로 사용할 수 있다. 대부분의 경우 이 편이 수작업보다 훨씬 더 빠르다.

    *  SQLite는 메모리 내 데이터로 작업하거나 외부 파일(CSV 파일 등)을 네이티브 데이터베이스 테이블처럼 사용할 수 있으므로 편리한 데이터 쿼리가 가능하다.

      

  * SQLite는 단일 독립형 바이너리이므로 **앱과 함께 배포하고 필요에 따라 앱과 함께 이동**하기도 쉽다. 

    * SQLite에 의해 생성되는 각 데이터베이스 역시 하나의 파일로 구성되며 SQL 명령으로 이 파일을 압축하거나 최적화할 수 있다.

      

  * SQLite용 서드파티 바이너리 확장으로 **더 많은 기능을 추가**할 수 있다. 

    * SQLCipher는 SQLite 데이터베이스 파일에 256비트 AES 암호화를 추가한다. 

    * SQLite-Bloomfilter는 특정 필드의 데이터에서 블룸 필터를 생성할 수 있게 해준다.

      

  * 그 외에도 많은 서드파티 프로젝트가 SQLite를 위한 **부가적인 도구를 제공**한다. 

    * 예를 들어 비주얼 스튜디오 코드 내에서 데이터베이스를 탐색할 수 있게 해주는 비주얼 스튜디오 코드 확장 또는, SQLite용 LiteCLI 인터랙티브 명령줄이 있다. 
    * 그 외에도 깃허브의 추천 SQLite 리소스 목록에서 많은 도구를 찾을 수 있다.



* #### SQLite가 적합하지 않은 경우

  * **SQLite가 지원하지 않는 기능을 사용하는 앱**

    * SQLite는 여러 가지 관계형 데이터베이스 기능을 지원하지 않으며 많은 경우 앞으로도 지원할 예정이 없다. 대체로 코너 케이스에 해당하는 기능이지만, 그 기능이 필요하다면 사용할 수 없다.

      

  * **수평 확장 설계가 필요한 앱** 

    * SQLite 인스턴스는 단일체이며 독립적이고, 인스턴스 간 기본 동기화 기능이 없다.

    * 또한 여러 개를 연합하거나 클러스터를 만들 수 없다. 따라서 수평 확장 설계를 사용하는 애플리케이션에서는 SQLite를 사용할 수 없다.

      

  * **여러 연결에서의 동시 쓰기 작업을 실행하는 앱** 

    * SQLite는 쓰기 작업 시 데이터베이스를 잠그므로 여러 동시 쓰기 작업이 실행되는 앱에서는 성능 문제가 발생할 수 있다. 다만 여러 동시 읽기 작업을 실행하는 앱은 대체로 빠르게 실행된다. 

    * SQLite 3.7.0 이상은 여러 쓰기 작업의 속도를 높이기 위한 로그 선행 기입(Write-Ahead Logging)을 제공하지만 몇 가지 제약이 따른다. 대안으로는 위에 언급한 버클리 DB가 있다.

      

  * **강한 데이터 형식 지정이 필요한 앱** 

    * SQLite의 데이터 형식은 비교적 소수다. 예를 들어 기본 datetime 형식이 없다. 따라서 애플리케이션에서 이러한 형식을 처리해야 한다. 애플리케이션이 아닌 데이터베이스에서 datetime 값을 위한 입력을 정규화하고 제약하고자 한다면 SQLite는 적합하지 않다.

    

* #### SQLite 라이선스

  SQLite는 **퍼블릭 도메인 오픈 소스 소프트웨어**이다. 마음대로 수정해도 되고 영리 목적으로 사용해도 되지만 저작인격권은 지켜주어야 한다

  *  **퍼블릭 도메인(영어: public domain)** 또는 자유 이용 저작물이란 저작권(저작재산권)이 소멸되었거나 저작자가 저작권(저작재산권)을 포기한 저작물을 말한다. 
  *  저작 인격권은 저작자가 저작물을 통해서 가지고 있는 인격적인 이익을 보호하기 위한 권리이다. 쉽게 말해 SQLite를 만든 원작자가 아닌 사람이 자신이 SQLite를 만들었다고 주장할 수 없는 권리이다.







---

# Detailed Design

**'momuk'의 세부 설계와 데이터 이동은 다음 순서대로 이루어진다.**

<img src="https://github.com/hs-2171326-junhakim/study/blob/main/image/image-20221127184140262.png" alt="image-20221127184140262" style="zoom:150%;" />



1. 유저가 어플리케이션 프레임워크인 **Flutter**로 만든 앱으로 위치정보, 계절, 날씨, 나이, 성별같은 유저 데이터를 입력받아 **Django**로 전송한다. 

2. **Django**로 만든 웹서버에서 위치데이터를 제외한 정보를 딥러닝 오픈소스인 **Neural CF**로 전송한다.

3. Neural CF는 데이터베이스에 있는 다른유저들의 구매데이터와 Django로 부터 받아온 유저 데이터를 이용하여 학습한다.

4. 학습을 끝낸 Neural CF는 반환된 점수를 통해 추천음식을 자체 개발 오픈소스에 전달한다. 

   * 위치정보, 계절, 날씨, 나이, 성별등의 데이터를 개별적인 입력 벡터로 변환한 후, 각각의 입력 벡터마다 유저의 구매데이터를 Neural CF의 입력으로 학습시킨다.
   * 이후 반환된 점수들의 평균값을 구한 후, 자체 개발 오픈소스에 추천 음식을 전달한다.

5. 사용되지 않은 유저 위치 데이터는 **네이버 지도** **API**에 보내 근처 가게 데이터를 얻는다.

6.  자체 개발 오픈소스에 추천음식과 가게 데이터를 보내어 추천가게 좌표 및 추천 음식을 받는다.

7. 받아온 추천가게 좌표와 추천음식을 웹 매핑 라이브러리인 **Leaflet**을 사용하여 추천가게가 매핑되어있는 지도를 사용자에게 보여준다. 

8. 마지막으로 전자상거래 오픈소스인 **saleor(세일오어)**를 이용하여 결제를 가능하게 하고 길찾기 알고리즘인 **OSRM**을 사용하여 가게 최단경로를 알려준다.

   
