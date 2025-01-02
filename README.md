# Olist 이커머스 데이터 분석 프로젝트
## 목차

- [개요](#개요)
- [데이터 설명](#데이터-설명)
- [분석 내용](#분석-내용)
  - [KPI 1. 고객 이탈률 분석](#kpi-1-고객-이탈률-분석)   
  - [KPI 2. RFM 고객세분화 분석](#kpi-2-rfm-고객세분화-분석)
  - [KPI 3. 정시 배송 비율 분석](#kpi-3-정시-배송-비율-분석)
  - [결론(비즈니스 전략 제안)](#결론비즈니스-전략-제안)
- [회고 및 개선점](#회고-및-개선점)


## 개요
- 주제
    - 이커머스 데이터 분석을 통한 3가지 핵심 성과 지표(KPI) 도출 및 비즈니스 전략 수립
- 배경
    - 브라질 이커머스 시장은 규모가 커지고 있으며 남미 이커머스 시장에서 브라질은 점유율이 높지만 Olist는 재구매 유도가 잘 되지 않고 있음
- 목표
    - Olist의 재구매 고객, 충성 고객 비율을 높이고 정시 배송을 확보하여 브라질 이커머스 시장에서의 입지 강화
- 비즈니스 전략 세가지 제안
  1. 재구매 고객 확보 전략
  2. 충성 고객 확보 전략
  3. 정시 배송 확대 전략
- 역할
    - RFM분석 및 비즈니스 전략 도출
    - 기획서 및 보고서 작성
    - 발표자료 제작 및 발표

## 데이터 설명
- 데이콘 「KPI 도출 비즈니스 전략 아이디어 경진대회」에서 제공하는 데이터를 활용
- 대회와 프로젝트 진행시기가 맞지 않아 대회 참가는 하지 않음
### 데이터 셋 정보

|데이터 세트 이름	|설명	|shape |
|-|-|-|
|Customers	|고객과 관련된 정보	|(87955, 5)|
|Locations	|지역과 관련된 정보	|(1000163, 5)|
|Order_items	|주문 아이템과 관련된 정보	|(100557, 6)|
|Orders	|주문과 관련된 정보	|(87955, 7)|
|Payments	|결제와 관련된 정보	|(91971, 5)|
|Products	|제품과 관련된 정보	|(29471, 6)|
|Reviews	|리뷰와 관련된 정보	|(87873, 5)|
|Sellers	|판매자와 관련된 정보	|(2763, 4)|


### 사용 라이브러리

|전처리	|모델링	|시각화|
|-|-|-|
|numpy	|sklearn	|seaborn|
|pandas	|optuna	|matplotlib|
|	|	|tableau|
|	| |folium|


## 분석 내용
### ERD
이미지 올리기


### 주요 EDA
이미지 올리기


### KPI 선정
1. 고객 이탈률
    - 고객 이탈률은 기업이 고객 관계를 관리하고 비즈니스 성과를 극대화하기 위한 중요한 지표
    - 고객 이탈률을 지속적으로 모니터링하고 이를 낮추기 위한 전략을 수립함으로써 Olist의 장기적인 성장을 도모하기 위하여 선정
2. RFM 스코어
    - RFM 분석은 전자상거래 플랫폼에서 고객의 가치를 극대화하는 데 도움이 되는 지표이므로 선정
3. 정시 배송 비율
    - 정시 배송 비율은 고객 만족도와 신뢰성을 높이고 운영 효율성을 평가하며 비용절감과 경쟁력 강화를 위한 중요한 지표
    - Olist의 전반적인 성과와 성장에 긍정적인 영향을 미칠 수 있는 지표이므로 선정

### KPI 1. 고객 이탈률 분석
✔️ 지표 정의
- 기존 고객 이탈률 계산식
```
당월 고객 이탈 수 / 월 초 고객 수 X 100
```
- 새로운 고객 이탈률 정의
```
고객 이탈 = 1회 구매한 고객이 2회 이상 구매하지 않은 경우
```
- 현재 보유하고 있는 데이터로는 월마다 이탈하거나 가입하는 고객 수를 파악할 수 없으므로 새로운 정의의 고객 이탈률을 사용
  
✔️ 분석 진행 방식 및 결과
- 고객 이탈률에 영향을 미치는 컬럼을 찾기 위한 모델링 진행
  - Optuna를 활용하여 RandomForest의 최적의 파라미터 탐색
  - 최적의 파라미터를 통해 RandomForest 모델링 진행 후 feature importance 확인
  - feature importance 확인 결과 (이미지 첨부)
    - 금액적인 측면, 고객의 경도, 판매 품목의 상위 카테고리, 위치 좌표에 관한 컬럼이 고객 이탈에 영향을 끼치는 것을 확인
      
✔️ 인사이트 도출
  - 판매 가격
    - 1000헤알 미만의 분포가 대부분 (1000헤알=한화 약 27만원 정도)
  - 상위 카테고리
    - 대부분의 이탈 고객이 1000헤알 미만의 낮은 금액대를 소비하는 경향을 보임
    - 판매 가격 1000헤알 미만에서의 카테고리 중 상위 5개 카테고리를 중점으로 비즈니스 전략을 수립할 수 있음
  - 고객 경도
    - 주문 날짜로부터 실제 배송받은 기간의 지역 분포 확인 결과 배송 빠르기에 관계없이 남쪽과 남동쪽 지역 위주로 배송되는 것을 확인
    - 남동쪽 지역을 기준으로 배송과 관련된 비즈니스 전략을 수립할 수 있음
  - 판매자의 위치
    - 상위 카테고리 5개의 판매자 위치 확인 결과 고객 경도 분석과 마찬가지로 남동쪽 지역을 중심으로 비즈니스 전략 수립 가능


### KPI 2. RFM 고객세분화 분석
✔️ 지표 정의
- R(Recency)
  - 고객이 마지막으로 구매한 시점을 나타내는 지표로 고객이 최근에 구매한 경우 가까운 시일 내에 다시 구매할 가능성이 높다고 판단
- F(Frequency)
  - 고객이 얼마나 자주 구매했는지 빈도를 측정하는 지표로 구매 빈도가 높은 고객은 충성도가 높고 향후에도 계속 구매할 가능성이 큼
- M(Monetary)
  - 고객이 얼마나 많은 금액을 지출했는지 나타내는 지표로 많은 금액을 지출한 고객은 기업에 큰 가치를 제공한다고 볼 수 있음


✔️ 분석 진행 방식
- R, F, M 지표 모두 한쪽으로 치우친 형태이므로 로그변환과 MinMaxScaling을 통해 표준화 진행  
- F지표의 경우 고객의 구매 빈도가 1인 경우가 대다수이기 때문에 1회 구매 고객(RM 스코어)과 재구매 고객(2회 이상 구매, RFM 스코어)을 분류하여 각각의 고객 세분화 분석 진행  


✔️ RM 스코어
- 1회 구매 고객을 대상으로 고객세분화 분석 진행
- F지표의 값이 모두 1이기 때문에 R, M지표에 대해서 점수를 산정하여 고객 등급 세분화 진행
- Qcut을 이용하여 4개의 범주로 R과 M지표 범주화
- R, M지표 범주화에 따라 고객 등급 세분화

|R-score|범위||M-score|범위|
|-|-|-|-|-|
|4|0~111일 이내||1|0~63.75헤알|
|3|112~206일 이내||2|63.76~113.68헤알|
|2|207~332일 이내||3|113.69~213.55헤알|
|1|333~664일 이내||4|213.56~9051.20헤알|

  - 이미지 첨부
  - 집중 관리 고객을 핵심 그룹으로 선정

✔️ RFM 스코어
- 재구매 고객(2회 이상 고객)을 대상으로 고객세분화 분석 진행
- Frequency값이 2이상인 경우에 대해 고객 등급 세분화 진행
- K-means 클러스터링
    - 엘보우 메소드를 활용하여 최적의 클러스터(4)를 구한 후 CV(변동계수)와 가중치 계산
- F지표는 구매횟수에 따라 4등분, R, M지표는 8등분하여 RFM 스코어 계산
    - R, M지표는 비교적 고르게 분포한 것으로 보이나 F지표는 재구매 빈도가 너무 낮아 Frequency지표는 임의로 4등분하여 진행

|Frequency 지표|구분|
|-|-|
|2회 구매|1|
|3,4회 구매|2|
|5,6,7,8회 구매|3|
|9회 이상 구매|4|

- 상위 5%, 25%, 60%, 100%에 따라 4개 등급으로 세분화하고, 1회 구매 고객과 최초 가입 고객을 포함하여 총 6개의 최종 등급 선정
  
|퍼센트(%)|등급명|
|-|-|
|상위 5%|Red Diamond|
|상위 25%|Blue Diamond|
|상위 60%|Pink Diamond|
|상위 100%|Yellow Diamond|
|1회 구매 고객|Diamond|
|최초 가입 고객|Graphite|

- ANOVA 분석 검증
    - K-Means 클러스터링을 사용해 분류한 고객 등급이 적절하게 분류되었는지 확인하였으며 세 지표 모두 F분포는 충분히 크고 p-value는 충분히 작으므로 적절하게 분류되었음을 확인



### KPI 3. 정시 배송 비율 분석
✔️ 지표 정의
- 정시 배송
```
실제 배송 날짜가 기대 배송 날짜보다 빠르거나 같은 경우
```

- 정시 배송 비율
```
(정시 배송된 주문 수 / 전체 주문 수) x 100%
```

✔️ 분석 진행 방식
- 파생 변수 추가
    - 제품 준비 기간, 실제 배송 기간, 예상 배송 기간, 실제 수령 기간
- 위치 및 판매자 데이터를 병합하는 방식으로 '판매자 경위도' 컬럼 추가
- 정시 배송 성공, 실패 확률 확인 및 원인 분석
- 주, 지역별 정시 배송 비율 시각화 및 인사이트 도출

✔️ 분석 결과
```
정시 배송 성공 확률 = 92.2%
정시 배송 실패 확률 = 7.8%
```
- 정시 배송 실패 확률은 낮지만 6885건으로 무시할 수 없는 수치라고 판단
- 정시 배송에 실패한 건에 대한 분석 결과 배송 문제가 98.9%, 제품 준비 문제가 10.1%임을 확인
  - 배송 문제가 발생한 경우(제품 준비 기간보다 실제 배송 기간이 긴 경우)는 정시 배송 실패의 89.9%를 차지하므로 실제 배송 기간에 초점을 맞추어 그 기간을 줄이는 것이 적합하다고 판단
- 지역별 구매자, 판매자 분포 확인
    - 구매자 : 북부를 제외한 전 지역에 고르게 분포되어 있으며 특히 남동부에 집중 분포
    - 판매자 : 남동부에 집중 분포
- 주, 지역별 평균 배송기간과 배송량 또한 남동부로 갈수록 배송기간이 짧고 배송량이 많아지며 정시 배송 실패의 경우 또한 남동부에 밀집되어 있음
- 남동부의 구매자, 판매자 분포 확인
    - 구매자 : 남동부 전역에 비교적 골고루 분포
    - 판매자 : 주로 상파울루주, 리우데자네이루주에 분포
- ➡️남동부는 브라질에서도 정시 배송 실패 비율이 높은 지역이므로 남동부에 물류 센터를 신설함으로써 정시 배송 비율 개선
- 남동부 물류 센터 위치 선정
    - 정시 배송에 실패한 구매자와 판매자의 위치 중간값 계산
    - 정시 배송에 실패한 구매자와 판매자의 중간 지점에 물류 센터가 위치할 경우 정시 배송 비율이 상승할 것으로 예측

### 결론(비즈니스 전략 제안)
1. 재구매 고객 확보 전략
   - 등급 업그레이드
   - 고객맞춤형 재구매 유도 메일 발송
   - 챌린지 이벤트 개최
2. 충성 고객 확보 전략
   - 배송일 지정 서비스
   - 무료 교환, 반품
   - 고객 커뮤니티 게시판 운영
3. 정시 배송 확대 전략
   - 남동부 물류 센터 신설

  
## 회고 및 개선점
### 회고
- KPI 수립과 인사이트 도출을 직접 해볼 수 있었다. 다른 나라의 데이터이다보니 정보 수집과 인사이트 도출 과정에서 어려운 점도 있었지만 해결책을 찾기 위해 더 많이 고민하고 공부하는 계기가 되었다.
- 의사소통 능력을 향상시켰다. 팀원들과 의견 교류도 잘 되었고 프로젝트 진행 과정 중 크고 작은 부분에서 다양한 아이디어가 필요했는데 적극적으로 아이디어를 냈고 제안한 아이디어가 채택이 되기도 하면서 프로젝트에 대한 주인의식을 높일 수 있었다.
- 설득력을 높이기 위한 충분한 근거를 찾고 토론하면서 발전적인 방향으로 프로젝트를 진행할 수 있었다.

### 개선점
- Olist의 최근 데이터를 확보한다면 데이터 분석 결과와의 유사점, 차이점 등을 분석하고 최근 경향에 맞는 비즈니스 전략을 도출할 수 있을 것이다.
- 비즈니스 전략 제안 파트에서 남동부 위주로 전략을 제안했지만, 다른 지역의 고객을 확보하기 위한 전략도 세워볼 수 있을 것이다.
- 각 KPI에 대한 구체적인 수치를 정하고 목표에 도달하기 위한 분석을 진행할 수 있을 것이다.
