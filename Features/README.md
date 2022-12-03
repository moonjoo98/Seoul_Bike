### 정제 데이터 구글 드라이브 링크
https://drive.google.com/drive/folders/1wgFq6fSEGJ4KnDz_hQ0jlyPkQHT_g3vy?usp=sharing

##### 파생변수 설명 및 코드 업로드
**FORMAT**
- 파생변수 설명 - 사용DATA

### 날짜 Feature(문주_일별_따릉이데이터.csv에 day_name 컬럽 추가)


### 따릉이데이터 Features

##### 일별 자치구별 이동거리 평균 - 문주_일별_따릉이데이터.CSV 
join_df.groupby(['대여일자','자치구'])['이동거리(M)'].mean().unstack()

##### 일별 자치구별 따릉이 이용횟수 - 문주_일별_따릉이데이터.CSV 
join_df.groupby(['대여일자','자치구'])['대여소번호'].count().unstack()
