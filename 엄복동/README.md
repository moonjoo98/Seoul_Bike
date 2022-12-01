### 주의사항
gpu로 돌릴 경우 할당되는 gpu가 계속적으로 변동되기 때문에 cpu로 돌려야 일정한 결과값이 나옴
추가적으로 HP-filter에 활용되는 lambda값에 따라 결과값이 상이해지기 때문에 최적의 lambda를 찾아야함(현재는 6000이 최적)
결과를 제출하기 전 지역별 최대, 최소, 평균 등을 활용하여 best output과 비교하면서 제출하는 것을 권장

**requirement.txt**
pytorch--1.9.0
python--3.7.6(코랩 버전 그대로 활용하면 됨)


### 사용 가능한 모델 정리

1. GNN
2. transform + self-supervising learning(contrastive learning)
3. LSTM 혹은 CNN와 같은 기본 모델
[https://dacon.io/competitions/official/235720/codeshare/2563?page=4&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/2563?page=4&dtype=recent)
4. Causal Padding을 활용한 CNN1d모델
    [https://dacon.io/competitions/official/235720/codeshare/4088?page=1&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/4088?page=1&dtype=recent)
    
5. H2o AutoML 활용
[https://dacon.io/competitions/official/235720/codeshare/2914?page=1&dtype=recent](https://dacon.io/competitions/official/235720/codeshare/2914?page=1&dtype=recent)




