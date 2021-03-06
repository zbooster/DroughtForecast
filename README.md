# DroughtForecast ML (머신러닝 기반 가뭄 예측 알고리즘)



## Introduction
---
머신러닝 기반 직업 추천 시스템 

- 경북-충남-강원지역의 최근 30년치 종관기상관측 데이터를 통합, 변형하여 가뭄 예측 모델링을 제시


## contents
---
- 1. 프로젝트 기획 의도
- 2. 데이터 개요
- 3. 데이터 전처리 및 베이스라인 선정
- 4. 모델링
- 5. 모델 분석 및 조정
- 6. 모델 설명
---

## 1. 프로젝트 기획 의도

---
전 세계 곳곳에서 기후변화로 불거진 이상기후 현상이 잦아지고 있다. 세계기상기구 
(WMO)는 이상기후가 더 잦아지고 그 피해 규모는 커질 것이라고 경고했다. 그 중에서도 
가뭄은 단기적 피해 뿐만 아니라 짧게는 몇 주에서 길게는 수년동안 농업, 생활, 사회 그
리고 경제 등으로 그 영향이 광범위하고, 막대한 피해를 발생시킨다. 따라서 이상기후에 
의한 가뭄을 대비하고 피해 규모를 최소화하기 위한 대책 마련이 필요하다.

---
## 2. 데이터 개요
---
- 2 - 1  데이터 출처 : 기상자료 개방포털 (data.kma.go.kr)의 종관기상관측 데이터.

기상자료 개방 포털은 정부에서 운영하는 데이터 플랫폼으로서 국가기상기후 자료의 통합관리 및 서비스 선진화를 목표로 2015년 신설되었으며, 기상청에서 생산하거나 취득하는 관측 자료, 예보 자료 등 다양한 데이터를 통합적으로 관리하며, 품질관리를 통해 고품질의 데이터를 생산, 제공하고 있으며, 또한 통계처리를 통한 유용한 기후정보와 함께 기후데이터의 기록을 보존하는 국가기록보관소의 역할을 수행하고 있다

- 2 - 2  데이터 내용: 경북-충남-강원 지역의 근 30년치 종관기상관측(ASOS) 데이터이며 60가지의 특성 중 본 프로젝트에서 사용한 특성은 8가지(평균기온	최저기온 최고기온 기압 강수량 풍속 이슬점 상대습도)로 하였다.

- 2 - 3 모델링의 방향성 : spi지수(가뭄지수)라는 새로운 특성을 만들고 spi지수에 따라 0또는 1의 target값을 반환하기로 한다
시작 모델은 Rf로 한다
---

## 3. 데이터 전처리 및 베이스라인 선정
---
- 3 - 1  결측치 처리 : 결측치를 확인하기 위하여 isna() 함수를 사용해 결측치를 확인한 결과 결측 데이터가 특성별로 100개 내외의 수준이였다. 한 특성의 데이터량이 290000개 정도에 이르는 점과 , 날씨의 연속성을 감안하여 소수의 결측 데이터를 전일자의 데이터로 처리하였고. 강수량 데이터의 경우 비가 내리지않는날은 0으로 처리됨을 확인하여 0으로 처리하였다.
- 3 - 2   SPI지수 추가 : SPi지수는 6개월간의 표준강수지수 이기 때문에 
