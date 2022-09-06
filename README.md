![image](https://user-images.githubusercontent.com/100823210/188532428-32e0dbf6-294e-4d7f-8dff-f580db189e60.png)


# [ML] 머신러닝을 이용한 가뭄 예측

## 프로젝트 개요
전 세계 곳곳에서 기후변화로 불거진 이상기후 현상이 잦아지고 있습니다. 세계기상기구(WMO)는 이상기후가 더 잦아지고 그 피해 규모는 커질 것이라고 경고했고, 그 중 가뭄은 단기적 피해 뿐만 아니라 짧게는 몇 주에서 길게는 수년동안 농업, 생활, 사회 그리고 경제 등으로 그 영향이 광범위하고, 막대한 피해를 발생시킵니다. 따라서 이상기후에 의한 가뭄을 미리 대비하고자 경북-충남-강원지역의 가뭄에 대한 예측을 시도해보았습니다.

> [선거에 묻힌 봄 가뭄... 타들어가는 농심](https://n.news.naver.com/mnews/article/016/0001997952?sid=100)

> [가뭄,홍수 기상이변에 전세계 농산물값 '들썩'](https://www.nongmin.com/news/NEWS/ECO/WLD/349928/view)

## 수행 결과
(2022년 6월 10일 기준) 모델을 검증하기 위하여 기상청에서 발표한 가뭄지수(표준강수지수)를 검증 기준으로으로 사용하였고 아래 그림에서 약한 가뭄(SPI6 < -1)는 짙은색으로 표시하였습니다. 5월 데이터를 검증해본 결과, 24개 관측소기준, 4개 관측소를 제외한 나머지 20개 관측소와 같은 결과를 보여줬으며(정확도 83.3%), 현재 진행중인 6월의 경우, 3개의 관측소를 제외한 21개 관측소에서 약한 가뭄이상이 예측되고 있습니다.

![2022년 5월 예측](https://user-images.githubusercontent.com/100823210/188533003-c1114c48-4723-4eec-8bee-3f8a844c63e9.png)

![2022년 6월 예측](https://user-images.githubusercontent.com/100823210/188533057-b2d7e670-787b-4a4c-b401-acc643db74c7.png)

## 데이터 개요
### 출처

기상자료 개방포털 (data.kma.go.kr)의 종관기상관측 데이터.

기상자료 개방 포털은 정부에서 운영하는 데이터 플랫폼으로서 국가기상기후 자료의 통합관리 및 서비스 선진화를 목표로 2015년 신설되었으며, 기상청에서 생산하거나 취득하는 관측 자료, 예보 자료 등 다양한 데이터를 통합적으로 관리하며, 품질관리를 통해 고품질의 데이터를 생산, 제공하고 있으며, 또한 통계처리를 통한 유용한 기후정보와 함께 기후데이터의 기록을 보존하는 국가기록보관소의 역할을 수행하고 있다

### 지역선정이유
기획단계에서 참고했던 기사가 패스트푸트점에서 감자튀김이 부족하다는 기사였기 때문에 봄 감자의 주 산지인 강원-경북-충남으로 결정했으며, 해당 지역의 종관기상관측소(ASOS) 24개의 데이터를 수집하였다.

### X Features
경북-충남-강원 지역의 근 30년치 종관기상관측(ASOS) 데이터이며 60가지의 특성 중 본 프로젝트에서 사용한 특성은 8가지(평균기온,최저기온,최고기온,기압,강수량,풍속,이슬점,상대습도)로 하였다.

### Y Label
기상청에서 발표한 가뭄지수(표준강수지수)를 약간가뭄(-1 이하)를 가뭄으로 특정하여 사용 하였다.

![표준강수지수](https://user-images.githubusercontent.com/100823210/188548951-596e33ac-809f-4f7c-8719-9b890e8e4a8e.png)

## 데이터 전처리
### 결측치
- 강수량의 경우, 비가 오지 않은날이 Null값으로 처리되어 있어 0으로 채웠다.
- 나머지 Feature의 경우, Feature별로 결측치가 100개 내외수준이며, 이는 전체 데이터 29만건 대비 미미한 수준(0.03%)이므로 날씨의 연속성을 고려하여 전일자 데이터로 채웠다.
![결측치 처리](https://user-images.githubusercontent.com/100823210/188536084-aba333f7-622c-41a9-8449-28437009d9a9.png)
### 기간보정
기상청에서 제공하는 데이터는 일간 발표되는 데이터로 우리가 예측하려는 월(Month)과는 차이가 있어 지점별로 나누에 다음과 같이 연산하였다.
- X Features: 일별 데이터를 월 평균으로 변환 후 Y Lable(SPI6)의 기간값에 맞게 다시 6개월 평균으로 계산하였다.
- Y Label: 우리가 사용하려는 SPI6는 6개월이라는 기간이 반영되어있는 데이터를 일별로 제공되고 있다. 따라서, 데이터를 다시 월 평균으로 계산하여 -1 이하일 경우 1, 아닐 경우 0으로 분류하였다.


## 베이스라인
Label로 설정한 SPI6를 약한 가뭄이상(-1 이하)로 0, 1로 분류했을 경우 가뭄아님(0)은 6,609개, 가뭄(1)은 1,136개로 Label을 모드 가뭄아님(0)으로 예측하는 경우 0.85의 정확도(Accuracy)를 가집니다. 따라서 본 프로젝에서는 0.85이상의 정확도를 가지는 모델을 목표로 합니다.

## 모델 선정 및 성능 개선
LinearRegression, DecisionTree, RandomForest, LightGBM 4가지 모델로 예측을 수행했다.

![모델간 성능비교1](https://user-images.githubusercontent.com/100823210/188549990-5d878b31-327a-406f-b0da-ee4855d0e0f3.png)

### Oversampling
imbalanced-learn 패키지의 SMOTE 함수를 이용하여 Oversampling 하였다.(6,195건 -> 10,578건)
Oversampling 이후, Precision은 약간 감소한 반면 Recall이 상승했다. 가뭄예측이 틀리는것 보다. 가뭄을 가뭄이라고 예측할 수 있어야 하기 때문에 Pricision과 Recall 비율이 적절히 높은 모델인 RandomForest를 최종 모델로 선정했다.
![Oversampling](https://user-images.githubusercontent.com/100823210/188553673-0278412d-1de4-457e-b2e2-5e8ae2a0ec48.png)

### 하이퍼 파라미터 튜닝
GridSearchCV를 이용하여 best_estimator값을 중앙값으로 대체하기를 반복하며 가장 높은 수치의 파라미터 값을 찾았다.
![HyperTune](https://user-images.githubusercontent.com/100823210/188553646-5f6bc95a-316e-4fe9-8a88-03fc7d1b0b23.png)

## 가뭄 예측

### 예측하려는 월(Month)의 데이터
우리가 만든 모델은 해당 월의 이전 6개월의 평균 데이터를 넣고 가뭄인지 아닌지 분류하는 모델이다. 따라서, 예측하기 위해서는 과거 5개월의 관측값과 예측하려는 미래의 관측값이 필요하다. 우리는 과거 20년간 월 평균 데이터를 미래의 관측값으로 대체하여 입력하였다.
![image](https://user-images.githubusercontent.com/100823210/188554832-79cddb94-d94a-4c7b-9b41-affcad89db1e.png)


## 결론 및 한계점
### 결론
* 데이터를 어떻게 가공하느냐가 모델링에 많은 영향을 줄 수 있다는 점을 확인할 수 있었다. Nomalization이나 Oversampling을 했을 경우, 우리가 목표했던 바를 좀 더 가까이 다가갈 수 있었다.
* 예측하려는 것이 무엇이 중점이냐에 따라 정확도 이외에 고려할 것이 많다는 점을 확인할 수 있었다. 이번 프로젝트는 가뭄의 예측이였기에 '왜 가뭄을 예측하려는가?'에 대한 답이 중요했으며, 그에 따라 Recall을 모델평가의 중요지표로 삼았다.

### 한계점
* 처음 목표한 것은 증발 수요 가뭄 지수(EDDI)를 이용하여 표준강수지수로 된 가뭄을 예측하려고 했었기 때문에 관측된 강수량으로 EDDI를 계산하여 Feature로 사용하려 했으나, EDDI에 대한 이해부족으로 EDDI를 계산하는 인자값을 Feature로 선정하였다. 
* 가뭄을 분류문제로 풀었는데 회귀문제로 예측 후에 분류하는 것이 좀 더 정확한 예측을 할 수 있지 않았을까 하는 아쉬움이 남았다.

## 참고문헌
* [기계학습을 이용한 효과적인 가뭄예측 성능평가](http://j-kosham.or.kr/upload/pdf/KOSHAM-2021-21-2-195.pdf)
* [기계학습기법을 이용한 부산-울산-경남 지역의 증발수요 가뭄지수 예측](https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=JAKO202125240459718&dbt=NART)
