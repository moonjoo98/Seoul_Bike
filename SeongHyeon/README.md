### 주의사항

1. gpu로 돌릴 경우 할당되는 gpu가 계속적으로 변동되기 때문에 **cpu로 돌려야 일정한 결과값**이 나옴
2. 추가적으로 **HP-filter에 활용되는 lambda값에 따라 결과값이 상이**해지기 때문에 최적의 lambda를 찾아야함(현재는 6000이 최적)
3. **모든 피처는 정규화(10이상의 값은 활용하지 않는 것)** 한 상태로 학습을 해야 결과값이 좋음
4. 결과를 제출하기 전 **지역별 최대, 최소, 평균 등을 확인해 best output과 비교**하면서 제출하는 것을 권장

---

### requirement.txt(버전 단일화)

pytorch--1.9.0\
python--3.7.6(코랩 버전 그대로 활용하면 됨)

---

### 사용 가능한 모델 정리

1. GNN
2. transform + self-supervising learning(contrastive learning)
3. LSTM 혹은 CNN와 같은 기본 모델
[https://dacon.io/competitions/official/235720/codeshare/2563?page=4&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/2563?page=4&dtype=recent)
4. Causal Padding을 활용한 CNN1d모델
    [https://dacon.io/competitions/official/235720/codeshare/4088?page=1&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/4088?page=1&dtype=recent)
    
5. H2o AutoML 활용
[https://dacon.io/competitions/official/235720/codeshare/2914?page=1&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/2914?page=1&dtype=recent)

---

### 앞으로 진행할 것
#### 4개로 약 30개의 피처를 예측하는 것은 힘듦

- **날짜**와 같이 **주어진 피처를 이용해 카테고리 피처 추가로 생성**한 다음 **정제된 피처(추가한 피처)를 활용해 피처 예측** 하는 것 중요?!
- 또한 예측해야하는 피처 수가 **30개는 너무 많기 때문에 줄이는 것이 효율적**일 것?!

#### 예측해야하는 피처 수 줄이는 방법

- 수치형 데이터 - **차원축소, 필터링, 클러스터링**
- 범주형 데이터 - **여러 범주를 하나의 범주로 만들기(ex: 성별 + 연령대 → 성별_연령대)**

---

### 계절성을 제거해야할까?
데이터 유형을 기반으로 시계열을 고정 및 비고정의 2가지 주요 유형으로 분류할 수 있습니다. 고정 데이터에는 시계열의 추세, 계절성, 주기적 및 불규칙성 구성 요소가 없습니다. 비정상 데이터에는 이러한 구성 요소의 일부 또는 전부가 포함됩니다. 실제 시나리오에서 대부분의 시계열 데이터는 고정적이지 않지만 데이터를 고정하고 시계열 예측 모델 개발에 사용할 수 있도록 추세, 계절성, 주기 및 불규칙 구성 요소를 제거해야 합니다.
