# 주제 : 따릉이 대여량에 영향을 미치는 요인 분석과 요인에 따른 대여량 예측


## 분석 배경 및 목적
<img src ="https://user-images.githubusercontent.com/105912035/169677587-86dcf3b8-d80f-44c4-9119-5b3093cbdeee.png" width="400" heigth="400"/> <img src ="https://user-images.githubusercontent.com/105912035/169677588-0e91b296-5dad-448a-ab6f-8677d0a9cbb7.png" width="400" heigth="400"/> 
<img src ="https://user-images.githubusercontent.com/105912035/169680407-0d42c94c-a957-4f02-bd7f-f8a5c485c3bb.png" width="800" heigth="400"/> <br>

코로나 19가 확산하면서, 서울시민들은 사람들이 많이 몰리는 버스나 지하철 이용을 피하기위해 비대면 교통수단인 따릉이를 즐겨 찾으면서 사용자가 급증한것으로 보인다.<br>   
따릉이 사용이 급증함에 따라서, 서울시도 자전거수와 대여소를 대폭 늘렸다고 한다. 하지만 실제로 서울과학기술대학교 근처에 있는 따릉이 대여소를 찾아가보면, 대여 가능한 따릉이가 없을때가 있었다.<br>
이와 같이 대여소와 자전거를 확충함에도 몇몇 대여소들은 자전거의 부족함을 확인할 수 있다. 그리하여 따릉이 대여량에 영향을 미치는 요인들이 무엇인지 궁금하게 되었다.<br>

<img src ="https://user-images.githubusercontent.com/105912035/169682242-4521b6f0-6355-4f21-a627-48f1091f4a17.png" width="600" heigth="600"/>
<기사출처> https://www.news1.kr/articles/?4579584 <br>
공공자전거 무인대여시스템은 2015년 서울시에서 시작되어 대전(타슈), 세종(뉴 어울링), 창원(누비자), 광주(타랑께)에 설치되면서, 점차 전국적으로 확대되고있다. 위에 기사에 따르면 22년 초, 부산 기장군에서 처음으로 공공자전거 무인대여시스템을 도입했다.<br> 이처럼 부산 이외의 다른 도시에도 공공자전거 무인대여시스템을 도입할 경우, 우리가 파악한 요인들을 통해 대여량 예측을 하여 대여소 설치에 근거자료로 도움이 될 수 있을 것 같다고 생각하여 모델을 만들어보기로 하였다.  


## 데이터획득
#### 변수 설정
서울시 행정동 426곳 중 따릉이 대여소가 존재하는 416곳의 토지특성인 시설물 수와 대중교통(버스,지하철)이용에 따른 하루 평균 승객 승하차 수, 그리고 지역특성인 하루 평균 연령대별 생활인구와 성별 전체 생활인구의 데이터를 통해 행정동별 한개 대여소의 하루 평균 대여량을 예측하겠다.

- 따릉이 대여소와 대여/반납
  - (3개 feature : return_day, rent_day, rental_stop)
- 토지특성인 시설물 수
  - (12개 feature : large_store(대규모점포), company, hospital, restaurant, park, cafe, house, bank, car, school, bus_stop, subway_station)
- 대중교통(버스, 지하철)이용에 따른 승객 승하차 수
  - (4개 feature : bus_geton_people, bus_getoff_people, subway_geton_people, subway_getoff_people)
- 지역특성인 하루 평균 연령대별/성별 생활인구
  - (18개 feature : M_child, M_teenager, M_20, M_30, M_40, M_50, M_60, M_over70, W_child, W_teenager, W_20, W_30, W_40, W_50, W_60, W_over70, M_sum, W_sum)

총 37개의 feature가 있다.

#### 데이터 획득 경로(독립변수)
지하철역 위치 : https://data.seoul.go.kr/dataList/OA-21232/S/1/datasetView.do  
지하철 승하차 인원 : https://data.seoul.go.kr/dataList/OA-21224/S/1/datasetView.do (21.7 데이터)  
버스정류장 위치 : https://data.seoul.go.kr/dataList/OA-15067/S/1/datasetView.do (22.3.29 데이터)  
버스 승하차 인원 : https://data.seoul.go.kr/dataList/OA-21225/S/1/datasetView.do (21.7 데이터)  
은행 : https://data.seoul.go.kr/dataList/10129/S/2/datasetView.do (21.7 데이터)  
회사 : https://data.seoul.go.kr/dataList/103/S/2/datasetView.do (19년 데이터)   
병원 : https://www.data.go.kr/data/15069540/fileData.do (19년 데이터)   
학교 : https://www.data.go.kr/data/15049381/fileData.do (21.9 데이터)  
자동차 등록현황 : https://data.seoul.go.kr/dataList/OA-21236/F/1/datasetView.do (21.12 데이터)  
연령대별 생활인구 : http://data.seoul.go.kr/dataList/OA-14991/S/1/datasetView.do (21.7 데이터)   
음식점 : https://data.seoul.go.kr/dataList/10154/S/2/datasetView.do (20년 데이터)   
카페 : https://www.data.go.kr/data/15083033/fileData.do (22.3 데이터)  
아파트 수(주택수) : https://data.seoul.go.kr/dataList/10585/S/2/datasetView.do (20년 데이터)  
대규모 점포수 :　https://data.seoul.go.kr/dataList/OA-16096/S/1/datasetView.do (22년 데이터)  
공원 존재여부 : https://data.seoul.go.kr/dataList/OA-394/S/1/datasetView.do (21.5 데이터)  
#### 데이터 획득 경로(종속변수)
따릉이 대여이력 : https://data.seoul.go.kr/dataList/OA-15182/F/1/datasetView.do# (21.7 데이터 사용)  
  
---> 여러 데이터를 모으다보니 데이터 시점이 맞지 않는 경우가 발생하는 한계가 있었다. 데이터의 시점을 최대한 21년 7월로 통일시켰다. 

- 데이터 획득과정 코드
  - 행정동별_반납건수_대여건수_대여소수.ipynb  
  - large_com_hos_food_park_cafe_house_data_processing.ipynb  
  - bank_car_people_data_processing.ipynb  
  - school_data_processing.ipynb  
  - 행정동별_버스_승하차인원.ipynb  
  - 행정동별_지하철역_승하차인원.ipynb  
  - data_combining.ipynb  
## 데이터 분석
### EDA
<img src ="https://user-images.githubusercontent.com/105912035/169684355-b0530afe-7a5c-4ad3-a2cf-5049fa9db64b.png" width="600" heigth="600"/>
<img src ="https://user-images.githubusercontent.com/105912035/169684671-47a416cb-759a-4c60-bb4e-a64b11db7f12.png" width="600" heigth="600"/>
<img src ="https://user-images.githubusercontent.com/105912035/169684437-af583857-cbac-4136-9809-dda8d952c4ca.png" width="600" heigth="600"/>
<img src ="https://user-images.githubusercontent.com/105912035/169684443-20befd5b-0913-4308-b2fe-7e09c4ed6e24.png" width="600" heigth="600"/>

- 시설물 feature가 min이 0인 것을 보아 특정 행정동에는 시설물 수가 없을 수도 있음을 확인할 수 있다.  
- 생활인구 feature 와 다른 시설물 feature간에 수치가 차이가 너무 크기에 분석을 진행할 때는 scaling이 필요함을 알 수 있다.  
- 지하철과 버스 승/하차 승객 수를 비교해보면 지하철을 이용하는 사람들이 훨씬 많음을 알 수 있다.  

![image](https://user-images.githubusercontent.com/105912035/169684761-ce772689-bdfb-455b-ab46-0dc7378d28f9.png)
![image](https://user-images.githubusercontent.com/105912035/169684765-001fa82a-22b8-416a-8e4a-3949b1184048.png)
![image](https://user-images.githubusercontent.com/105912035/169684781-bf72cc86-030a-444e-93f2-e948cf571ad6.png)
- 결측치 여부를 확인해 본 결과 모든 feature에 대해 없음을 확인했다.

![image](https://user-images.githubusercontent.com/105912035/169685050-e1ba645d-712d-4d1e-967a-5b7d2f8de4f0.png)
![image](https://user-images.githubusercontent.com/105912035/169685057-a14db256-073b-47d7-b292-6060011dbd00.png)
- feature의 type을 확인해본 결과 float와 int로 구성되어있음을 확인했다.

![image](https://user-images.githubusercontent.com/105912035/169685141-03fa6094-9bd5-40f6-8588-dc5b79f2df69.png)
- 연령대별 생활인구 feature를 제외하고 boxplot을 확인해본 결과 행정동별 자동차 등록현황인 car feature가 다른 변수들에 비해 data분포가 큰 것으로 보아 연령대별 생활인구 feature를 제외해도 scaling을 진행해야함을 알 수 있다.

![image](https://user-images.githubusercontent.com/105912035/169685195-28bc524e-2530-492e-9f14-bb2e93893de3.png)
- data들의 분포를 확인해본 결과, 대부분 평균에 몰려있음을 확인했다.

![image](https://user-images.githubusercontent.com/105912035/169685745-65b58b0d-688b-4f61-84cd-1afcb3efba10.png)
- 연령대별 생활인구 변수끼리의 상관계수가 0.7 이상으로 상관관계가 매우 높은 것을 확인할 수 있다.<br>
회귀분석에서 독립변수들 사이의 상관관계가 높으면, 회귀분석의 전체가정인 독립변수들 간에 상관관계가 높으면 안된다는 조건을 위배하기 때문에<br> 
연령대별 생활인구와 성별 전체 생활인구 feature는 제외시키고 회귀분석을 진행하겠습니다.<br>
### 1.회귀분석 - 전처리
대여량을 예측할 것이므로 rent_day(대여량)를 타겟으로 지정하고 <br> target과 관련없는 변수인 dong_name(행정동 이름), code_dong(행정동코드), 반납량(return_day), 대여소 수(rental_stop), bus_geton_people(하루 평균 버스 승차 승객 수), subway_geton_people(하루 평균 지하철 승차 승객 수),<br> 그리고 상관관계분석을 진행하며 상관관계가 높았던 연령대별 생활인구 변수들을 제외시킨 dataframe을 생성합니다.
### 1.회귀분석 - 모델링
Ridge, Lasso 회귀분석 모델은 GridsearchCV 와 Kfold를 이용하여 bset model을 선정한 후, LinearRegression 모델을 포함한 3개의 예측모델 중 최고 성능을 보이는 모델을 선정하려고 한다.

평가지표로는 R-squared와 MAE로 선정했다. 대여량을 예측하는 것이기 때문에 MAE가 적당한 평가지표라고 생각했다. Validation 시 평가지표로는 best R-squared를 가진 모델을 best모델이라고 생각했다.

#### Ridge
[Validation]  
Ridge모델의 validation 결과로 alpha:100 일때, R-squared:0.0058로 가장 높아 Ridge(alpha:100)을 best_model로 선정하였다.  
[Test]  
Ridge(alpha:100)로 test셋에 대하여 검증했을 때, R-squared:-0.0398, MAE:19.967로 안좋은 모델 성능을 보였다.  
성능 향상을 기대하며, Wrapper Method와 PCA기법을 사용하여 다시 test셋에 대하여 성능 검증을 실시했다.  
- Wraaper 사용 시: R-squared:-0.0282, MAE:19.863  
- PCA 사용 시: R-squared:-0.035, MAE:19.859
-------> 모델 향상을 위해 여러가지 방법을 사용해보았지만, 의미있는 모델을 생성하기 힘들다고 판단하였습니다.

#### Lasso  
[Validation]  
Lasso모델의 validation 결과로 alpha:0.1 일때, R-squared:0.0058로 가장 높아 Lasso(alpha:0.1)을 best_model로 선정하였다.   
[Test]   
Lasso(alpha:0.1)로 test셋에 대하여 검증했을 때, R-squared:-0.0238, MAE:18.0328로 안좋은 모델 성능을 보였다.  

#### Linear
[Test]   
Lasso(alpha:0.1)로 test셋에 대하여 검증했을 때, R-squared:-0.14457, MAE:16.9705로 안좋은 모델 성능을 보였다.  

### 1.결론
[결론]  
모든 예측모델의 설명계수가 음수(-)값을 가진다. --> 예측모델 만들기 실패.. 해석할 수 있는 의미있는 모델을 만들지 못하였다.

### 2.Clustering - 전처리

### 2.Clustering - 모델링

### 2.결론
[결론]  



## 기대 효과:
공공자전거가 없는 지역에 새로 설치할 때 동일한 feature들을 이용해 추후 공공자전거 추가 입지 선정에 있어 근거 자료로 활용될 것으로 기대됨.
일부 대여소에서 발생하는 공공자전거 공급부족과 과잉반납과 같은 문제에 대응해 더욱 효율적인 공공자전거 시설 공급에 기여할 것으로 기대된다.
-한계점 및 추후 개선 방안 등:동별로 묶어서 보는거보다, 대여소별로 예측을 할 수 있었으면 더 실용성있는 분석일것같다?
