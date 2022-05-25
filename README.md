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
따라서 따릉이 대여량 예측을 통해 공공자전거 미설치 지역의 대여소 입지 설정에 활용하고, 대여량에 영향을 미치는 요인을 분석하여 대여소 부족으로 인한 공공자전거 대여소 혼잡도를 해결하고자 한다.

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
도로명주소 데이터, 좌표계 확인 및 변환 : https://anweh.tistory.com/53  
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
  - 따릉이대여소_행정동명_행정동코드.ipynb   
  - large_com_hos_food_park_cafe_house_data_processing.ipynb  
  - bank_car_people_data_processing.ipynb  
  - school_data_processing.ipynb  
  - 행정동별_버스_승하차인원.ipynb  
  - 행정동별_지하철역_승하차인원.ipynb  
  - data_combining.ipynb  
## 데이터 분석
### EDA
<img src ="https://user-images.githubusercontent.com/105912035/170056907-3d43c8ff-859f-41c1-8b5d-232f9f0eaa5d.png" width="600" heigth="600"/>
<img src ="https://user-images.githubusercontent.com/105912035/170057071-73c1e30c-7665-45ff-95fb-e5195179c69f.png" width="600" heigth="600"/>
<img src ="https://user-images.githubusercontent.com/105912035/170057433-0d2bb104-6763-461f-b102-1a6c1789bb2c.png" width="600" heigth="600"/>
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
- 각 feature들의 histogram은 다음과 같으며, data들의 분포를 확인해본 결과, 대부분 평균에 몰려있음을 확인했다.

![image](https://user-images.githubusercontent.com/105912035/169685745-65b58b0d-688b-4f61-84cd-1afcb3efba10.png)
- 연령대별 생활인구 변수끼리의 상관계수가 0.7 이상으로 상관관계가 매우 높은 것을 확인할 수 있다.<br>
회귀분석에서 독립변수들 사이의 상관관계가 높으면, 성능에 악영향을 미칠 수 있다고 판단하여 연령대별 생활인구와 성별 전체 생활인구 feature는 제외시키고 회귀분석을 진행하겠습니다.<br>
### 회귀분석 - 전처리
대여량을 예측할 것이므로 rent_day(대여량)를 타겟으로 지정하고 <br> target과 관련없는 변수인 dong_name(행정동 이름), code_dong(행정동코드), 반납량(return_day), 대여소 수(rental_stop), bus_geton_people(하루 평균 버스 승차 승객 수), subway_geton_people(하루 평균 지하철 승차 승객 수),<br> 그리고 상관관계분석을 진행하며 상관관계가 높았던 연령대별 생활인구 변수들을 제외시킨 dataframe을 생성합니다.  
  
Scaler로 StandardScaler를 사용하였다.
### 회귀분석 - 모델링
사이킷런의 Ride, Lasso, LinearRegression 의 best model을 선정할려고 한다.
Ridge, Lasso 은 Gridsearch 와 Kfold를 이용하여 각 모델별 best model을 선정한 후,   LinearRegression 모델을 포함한 3개의 예측모델 중 최고 성능을 보이는 모델을 선정.

평가지표로는 R-squared와 MAE로 선정했다. 대여량을 예측하는 것이기 때문에 MAE가 적당한 평가지표라고 생각했다. Validation 시 평가지표로는 best R-squared를 가진 모델을 best모델이라고 생각했다.

#### Ridge
[Validation]  
Ridge모델의 validation 결과로 alpha:100 일때, R-squared:0.0058로 가장 높아 Ridge(alpha:100)을 best_model로 선정하였다.  
[Test]  
Ridge(alpha:100)로 test셋에 대하여 검증했을 때, R-squared:-0.0398, MAE:19.967로 안좋은 모델 성능을 보였다.  
성능 향상을 기대하며, Wrapper Method와 PCA기법을 사용하여 다시 test셋에 대하여 성능 검증을 실시했다.  
- Wraaper 사용 시: R-squared:-0.0282, MAE:19.863  
- PCA 사용 시: R-squared:-0.035, MAE:19.859
-------> 모델 향상을 위해 여러가지 방법을 사용해보았지만, 의미있는 모델을 생성하기 힘들다고 판단다.

#### Lasso  
[Validation]  
Lasso모델의 validation 결과로 alpha:0.1 일때, R-squared:0.0058로 가장 높아 Lasso(alpha:0.1)을 best_model로 선정하였다.   
[Test]   
Lasso(alpha:0.1)로 test셋에 대하여 검증했을 때, R-squared:-0.0238, MAE:18.0328로 안좋은 모델 성능을 보였다.  

#### Linear
[Test]   
Lasso(alpha:0.1)로 test셋에 대하여 검증했을 때, R-squared:-0.14457, MAE:16.9705로 안좋은 모델 성능을 보였다.  

### 회귀 분석 - 결론 
**모든 예측모델의 설명계수가 음수(-)값을 가진다. --> 예측모델 만들기 실패.. 해석할 수 있는 의미있는 모델을 만들지 못하였다.**

### Clustering - 전처리
target값인 rent_day, return_day를 제외한 총 35개의 변수를 사용하여 clustering을 진행하기로 했다.
  
Scaler로 StandardScaler를 사용하였다.

### Clustering - 모델링
사이킷런의 KMeans, Agglomerative, DBSCAN를 사용하여, 평가지표로 Silhouette score를 통해 군집화가 잘된 모델을 선정하려고한다. 

#### Kmeans
[Silhouette_score]  
silhouette score는 cluster수: 2(약 0.36)>3(약 0.32)>5>4 순으로 나왔다.

#### Agglomerative
[Silhouette_score]  
silhouette score는 cluster수: 2(약 0.35)>3(약 0.34)>5>4 순으로 나왔다.

#### DBSCAN
[Silhouette_score]  
silhouette score는 eps=3.9일 때, silhouette score가 0.382로 나왔다. cluster가 나뉘지는 않고 noise만 검출해내기 때문에 저희 주제에 알맞지 않은 방법임을 알 수 있습니다.


### Clustering - 모델 선정
Kmeans, Agglomerative 중 cluster 수가 2일 때를 분석하는 것 보다, cluster 수가 3일 때 각 클러스터 별 특징을 파악하는게 더 의미있을거라 생각하였다. 그리하여 kmeans와 agglomerative 각각 cluster 수 3일 때, silhouette score를 비교해보았을 때, agglomerative가 0.34(>0.32)로 더 높게 나와 agglomerative를 최종 클러스터링 모델로 선정하였다. 


### Clustering - cluster별 특징
clustering을 하였을 때 3개의 cluster들이 각각 다른 feature 특징을 갖고 있음을 확인 할 수 있다.  
각 cluster의 feature값 분포를 보고 라벨링을 해줬다.  
- cluster 0 ==> 전반적인 인프라가 상대적으로 적은 지역  
- cluster 1 ==> 어린이 유동인구가 많고 회사가 많은 지역
- cluster 2 ==> 전반적인 인프라가 상대적으로 잘 갖춰진 지역  
<br>또한, pca를 사용하여 시각화를 해본 결과, clustering이 꽤 잘 된 것을 확인할 수 있다.  
하지만 target 값(rent_day)은 평균과 분포가 비슷함을 확인할 수 있는데 이는 한 대여소당 감당해야 하는 대여량이 비슷함을 암시한다.  
즉, 군집의 특성과 상관없이 하루 평균 대여량은 비슷하다는 것인데 유독 모든 feature에서 낮은 값을 가진 cluster0에서 이상치가 많다는 사실을 확인할 수 있다.  
이를 통해 인프라가 상대적으로 발달되어있지 않은 행정동의 대여소들의 혼잡도가 잘 관리되지 않는다는 사실을 발견했고.
따라서 이러한 이상치에 해당하는 동네의 특징을 확인하고, 대여소 확장을 제안하겠다.

### Clustering - Outlier 특징 파악 
반납과 대여에 대한 outlier을 spotfire를 이용하여 확인해보니 같은 행정동임을 확인할 수 있었다.  
이는 '대여소당_하루평균_대여건수'와 '대여소당_하루평균_반납건수'가 상관계수 1로 높은 상관관계를 갖고 있기 때문에 발생한 현상으로 추측된다.  

따라서 우리는 '전반적인 인프라가 상대적으로 적은 지역'의 대여에 대한 outlier를 파악하여 outlier 값들과 outlier가 아닌 값들에 대한 차이를 분석하고자 한다.

  
outlier가 아닌 값들에 대한 feature 평균값과 outlier들의 feature 값을 비교하여, 많이 차이나는 feature를 통해 rent_day(대여량)이 차이나는 이유에 대하여 해석해보겠다.  

- 가양2동 : 특별히 차이가 나는 feature들은 평균보다 작은 편이나 한강을 끼고 있어 feature의 값들에 비해 대여량이 많은 것으로 추정
- 갈현2동 : 학교가 많고, 유동인구가 많고, 주거단지가 많은 것으로 보아 주거 및 생활인구의 영향으로 대여량이 많은 것으로 보임
- 답십리1동 : 많은 feature에서 평균과의 큰 차이를 보여 다양한 feature들의 영향으로 대여량이 많음
- 반포본동 : 대규모 점포많고 버스 하차인원이 많으며(지하철도 많음) 한강을 끼고 있어 생활인구 대비 방문인구의 수로 인해 대여량이 많은 것으로 추정
- 보라매동 : 모든 feature가 평균값과 비슷하나 한 대여소에서의 대여량이 많기 때문에 대여소 보충 및 확장을 통해 대여량 분산이 필요할 듯
- 숭인1동 : 많은 feature에서 평균에 비해 작은 값을 가지나 큰 규모의 공원을 끼고 있어 대여량이 많은 것으로 추정
- 신림동 : 생활인구, 카페, 음식점, 버스 하차인원이 평균에 비해 높은 값을 가지고 서울대학교를 끼고 있어 대학의 영향이 많은 것으로 추정
- 응봉동 : 모든 featurer가 평균보다 작은 값이며 버스와 지하철 하차 인원이 적은 곳으로 목적지보다는 중랑천에서 한강으로 이어진 자전거도로가 영향을 미쳤을 것이라 추정
- 자양1동 : 유동인구, 병원, 음식점수가 많으며 지리적으로 동서울터미널과 건국대가 인접해 있어 자전거로 이동하는 사람이 많을 것으로 추정
- 정릉4동 : 모든 feature가 평균값과 비슷하나 한 대여소에서의 대여량이 많기 때문에 대여소 보충 및 확장을 통해 대여량 분산이 필요할 듯

### Clustering - 결론
서울시는 대부분의 대여소를 혼잡하지 않은 상태로 잘 유지하고 있다고 판단했다.  
하지만 이상치 분석 결과를 미루어 보아 특정 대규모 시설(공원, 대학 등)을 보유하거나 근처에 한강이 있는 행정동의 대여소들에 대한 혼잡도 관리는 잘 이루어지지 않은 것으로 보인다.  
  
**따라서 대규모 시설을 보유한 행정동의 대여소들에 대한 더욱 세심한 관리와 대여소 확충이 필요하다고 결론지었다.**

## 기대 효과
**행정동의 특징에 따른 공공자전거 대여소에 대한 효율적 관리를 통해 관리 비용을 줄이고 그 효과를 증가 시킬 수 있을 것으로 기대할 수 있다.**

## 한계점
1. 독립변수와 종속변수와의 상관성 부족으로 인해 대여량 예측 모델 생성에 실패하였다. -> 상관성, 인과성을 갖는 변수에 대해 탐색이 필요했다.
2. 자료 수집의 문제로 데이터 별 시점이 완벽하게 동일하지 않았다. 
