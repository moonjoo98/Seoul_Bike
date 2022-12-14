# 따릉이문주

### 1. 인사이트
- 요일, 계절 변수(범주형 변수) - 단 시계열 모델에는 넣지 못할거같고.. 부스팅 계열에 넣어야 할 것 같다.
요일, 계절 변수를 추가.one-hot-encoding을 통해 추가해주자. ++ 원핫 인코딩 시 요일이 7개면 -1로 6개만 넣기

##### Information
자전거 이용 인구 1340만 명 - 그 중 330만 명은 매일 자전거 이용
연 1회 이상 이용하는 사람이 전체의 35.2%
월 1회 이상 이용자가 33.5%
주 1회 이상 이용자가 25.6%
매일 이용하는 사람이 8.3%

##### 이용시간, 거리
절반 이상(56.4%)이 출퇴근 시간대에 집중
71%가 4km 이내 단거리 이용자
57%가 20분 이내 이용

##### 계절, 날씨
자전거 타기 좋은 계절인 봄‧가을철에 이용률이 가장 높음
여름철에 비해 겨울철에 이용건수가 크게 줄어드는 양상을 보임 -> 더위보다는 추위가 따릉이 이용에 더 영향을 줌.(손시렵잖아)

##### 더위보다 추위에 민감하고 비오는 출‧퇴근길엔 이용량 감소. 
##### 봄~가을철에 비해 기온이 영하권으로 떨어지는 겨울철에 이용건수가 일 2만 건 이하로 크게 감소
##### 특히 비 내리는 출‧퇴근시간대에 이용량이 급격히 감소

##### 요일
주말에도 주중과 이용량에서 큰 차이를 보이지 않았다

### 2. (외부데이터) 서울시 공공 데이터 따릉이 데이터활용

##### 사용 데이터 : 서울시 공공대여소 정보, 서울시 공공자전거 이용정보(일별)
- OPENAPI, CSV파일로 데이터를 가져와서 전처리 진행

### 전처리
- 월별 데이터 마다 encoding이 달라서 python에서 open되는 encoding으로 변환 (utf-8, cp949)
- 대여일자 및 여러 컬럼들이 "'2018-01-03'" | "2020-03-21" 이런식으로 일부는 따옴표를 포함하지 않고 일부는 따옴표가 포함된 경우가 있음 
-> strip으로 양옆 따옴표 제거
- 대여일자에 일부 데이터가 20? 203뒤 등 의미를 알수 없는 문자가 섞여 있어서 제거
- 연령대 코드가 2018년 데이터는 10대,20대 이런식으로 라벨링 돼있는데 2019 데이터를 보면 AGE_001,AGE_002로 표시됨. </br>
-> 2018년 데이터 포맷을 2019~21년 데이터 포맷으로 변경
- 대여소 정보 데이터와 이용정보 데이터를 대여소번호를 key값으로 merge

### 3. Feature (정상성, 차분)
##### Stationary time series / 정상성
stationarity, 데이터가 정상성을 가진다는 의미는 데이터의 평균과 분산이 안정되어 있어 분석하기 쉽다는 의미이다. 그러므로 통상적으로 평균이 일정하지 않으면 차분을 취하고, 분산이 일정하지 않으면 변환을 취한다.
##### VAR 등 모델에 적용하기 위해서는 시계열 변수가 모두 stationay 상태이어야 한다. 
-> p-value값을 비교해보면서 차분을 취하고 staionarity상태가 되도록 할 필요가 있음. 이 후 예측을 진행한 뒤에는 차분(diffencing)에 대한 모델에 대한 것이라는 점을 유념해야 하며 차분을 더하여 우리가 예측해야 할 값으로 만들어줘야함.

##### 날짜 encoding
https://github.com/erykml/nvidia_articles/blob/main/three_approaches_to_encoding_time_information_as_features_for_ml_models.ipynb

### 4. Model
![image](https://user-images.githubusercontent.com/103553532/205504220-8b9ad042-5012-4698-8f4c-ec3bd054c0b4.png)

##### darts 
- darts 는 시계열을 쉽게 조작하고 예측할 수 있는 Python 라이브러리입니다. 여기에는 ARIMA와 같은 고전 모델부터 심층 신경망에 이르기까지 다양한 모델이 포함되어 있습니다. 모든 모델은 scikit-learn과 유사한 fit()및 기능 을 사용하여 동일한 방식으로 사용할 수 있습니다 . predict()또한 이 라이브러리를 사용하면 모델을 쉽게 백테스트하고 여러 모델의 예측을 결합하고 외부 데이터를 고려할 수 있습니다. Darts는 단변량 및 다변량 시계열과 모델을 모두 지원합니다. ML 기반 모델은 여러 시계열이 포함된 잠재적으로 큰 데이터 세트에서 훈련될 수 있으며 일부 모델은 확률적 예측을 위한 풍부한 지원을 제공합니다.
- 코드 주소 https://github.com/unit8co/darts
- 논문 주소 https://www.jmlr.org/papers/volume23/21-1177/21-1177.pdf
##### LTSF-Linear(darts)

##### 설명
선형: 단층 선형 모델이지만 트랜스포머보다 성능이 뛰어납니다.
NLinear: 데이터 세트에 분포 이동이 있을 때 Linear의 성능을 향상시키기 위해 NLinear는 먼저 시퀀스의 마지막 값으로 입력을 뺍니다. 그런 다음 입력은 선형 레이어를 통과하고 최종 예측을 하기 전에 뺀 부분이 다시 추가됩니다. NLinear의 뺄셈과 덧셈은 입력 시퀀스에 대한 간단한 정규화입니다.
DLinear: Autoformer 및 FEDformer에서 사용되는 분해 방식을 선형 레이어와 조합한 것입니다. 먼저 이동 평균 커널과 나머지(계절) 구성 요소에 의해 원시 데이터 입력을 추세 구성 요소로 분해합니다. 그런 다음 두 개의 단일 레이어 선형 레이어를 각 구성 요소에 적용하고 두 기능을 합산하여 최종 예측을 얻습니다. 추세를 명시적으로 처리함으로써 DLinear는 데이터에 명확한 추세가 있을 때 바닐라 선형의 성능을 향상시킵니다.
LTSF-Linear는 단순하지만 다음과 같은 몇 가지 매력적인 특징이 있습니다.

##### 장점
최대 신호 통과 경로 길이: 경로가 짧을수록 종속성이 더 잘 캡처되므로 LTSF-Linear는 단거리 및 장거리 시간 관계를 모두 캡처할 수 있습니다.
고효율: 각 브랜치에 하나의 선형 레이어만 있기 때문에 기존 트랜스포머보다 메모리 비용이 훨씬 적고 매개변수가 적으며 추론 속도가 빠릅니다.
해석 가능성: 학습 후 가중치를 시각화하여 예측 값에 대한 통찰력을 얻을 수 있습니다.
사용하기 쉬움: LTSF-Linear는 모델 하이퍼 매개변수를 튜닝하지 않고도 쉽게 얻을 수 있습니다.
https://github.com/cure-lab/ltsf-linear

##### Latent ODE
Neural ODEs(2018)에 기반한 시계열 생성 모형.

##### Overview
Latent ODEs with ODE-RNN은 Time-series의 hidden state간 변화를 continuous-depth로 모델링한다. 이를 토대로 주어진 sample의 continuous dynamics와 latent trajectory를 학습할 수 있다. 본 연구는 제안한 방법론을 통해 continuous latent trajectory를 학습해, 기존 RNN보다 irregular sampled time-series에 대해 prediction, extrapolation에서 높은 성능을 보여준다.
1) 과제에서 주어진 시계열 샘플의 time-gaps가 중요하거나, missing value가 많을 때
2) 과제에서 주어진 데이터 샘플들의 latent function이 초기 조건(first hidden state in time-series)을 바탕으로 deterministic할 때(time-invariant할 때)
3) 과제의 목표가 Physics와 같이 주어진 샘플의 내재한 dynamics를 학습하는 것일 때
##### 4) 과제의 목표가 긴 시점의 time-series를 extrapolation일 때 <- 우리 task

http://dsba.korea.ac.kr/seminar/?mod=document&uid=2568
