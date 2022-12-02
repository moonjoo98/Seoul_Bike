# DATA

### 1. (외부데이터) 서울시 공공 데이터 따릉이 데이터활용

##### 사용 데이터 : 서울시 공공대여소 정보, 서울시 공공자전거 이용정보(일별)
- OPENAPI, CSV파일로 데이터를 가져와서 전처리 진행

### 전처리
- 월별 데이터 마다 encoding이 달라서 python에서 open되는 encoding으로 변환 (utf-8, cp949)
- 대여일자 및 여러 컬럼들이 "'2018-01-03'" | "2020-03-21" 이런식으로 일부는 따옴표를 포함하지 않고 일부는 따옴표가 포함된 경우가 있음 -> strip으로 양옆 따옴표 제거
- 대여일자에 일부 데이터가 20? 203뒤 등 의미를 알수 없는 문자가 섞여 있어서 제거
- 연령대 코드가 2018년 데이터는 10대,20대 이런식으로 라벨링 돼있는데 2019~21년 데이터를 보면 AGE_001,AGE_002로 표시됨. -> 2018년 데이터 포맷을 2019~21년 데이터 포맷으로 변경



-
