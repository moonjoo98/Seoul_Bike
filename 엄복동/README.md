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

### 4개로 약 30개의 피처를 예측하는 것은 힘듦

날짜와 같이 주어진 피처를 이용해 카테고리 피처를 추가로 만든 다음 정제된 피처(추가한 피처)를 활용해 피처 예측을 하는 것이 중요해보임\
또한 예측해야하는 피처 수가 30개는 너무 많기 때문에 줄이는 것이 효율적일 것으로 보임

### 예측해야하는 피처 수 줄이는 방법

- 수치형 데이터 - 차원축소, 필터링, 클러스터링
- 범주형 데이터 - 여러 범주를 하나의 범주로 만들기(ex: 성별 + 연령대 → 성별_연령대)
