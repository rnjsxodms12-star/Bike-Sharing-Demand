# Bike-Sharing-Demand
# Data Source: https://www.kaggle.com/c/bike-sharing-demand/data
Public Leaderboard RMSLE: 0.48725  
Baseline 대비 약 14% 개선

First Submission: 0.56722 [Baseline (AutoGluon default)]  
Best Score: 0.48725

Windspeed 측정 오류 보정 가설 실험	0.50060	(Data Insight)

🚲 Bike Sharing Demand Prediction (with AutoGluon)
워싱턴 D.C.의 자전거 대여 이력 데이터를 분석하여 시간별 수요를 예측하는 프로젝트입니다. 본 프로젝트는 데이터의 물리적 특성을 이해하고, 생성형 AI와의 협업을 통해 이상치 수사 및 정제에 집중하여 모델의 신뢰성을 확보하는 데 목적이 있습니다.

🕵️ 수사 개요 (Project Overview)
목표: 매 시간(Hourly) 자전거 총 대여 수(count) 예측

데이터 구성:

훈련 데이터 (Train): 매월 1일 ~ 19일 (대여량 정보 포함)

테스트 데이터 (Test): 매월 20일 ~ 월말 (대여량 예측 대상)

평가 지표: RMSLE (Root Mean Squared Logarithmic Error)

실제값과 예측값의 '비율'에 집중하며, 과소평가(Underestimation)에 더 큰 페널티를 부여하는 지표입니다.

🛠️ 주요 수사 기법 (Key Methodologies)
1. 풍속(Windspeed) 데이터 이상치 보정
문제점: 데이터 탐색 결과, 풍속이 0이거나 10 미만인 데이터가 비정상적으로 많이 분포하는 것을 확인했습니다.

해결 방안:

풍속 10 이상의 정상 데이터를 기준으로 평균값을 산출.

10 미만의 불확실한 단서들을 전체 평균값으로 일괄 보정하여 모델의 혼란을 방지했습니다.

2. 피처 엔지니어링 (Feature Engineering)
datetime 컬럼을 분해하여 Hour(시간), Month(월), Weekday(요일) 정보를 추출했습니다.

**Workingday(평일 여부)**에 따른 시간대별 대여 패턴 차이를 분석하여 모델의 주요 단서로 활용했습니다.

3. 머신러닝 모델링 (AutoGluon)
AutoGluon의 best_quality 프리셋을 사용하여 600초간 다수의 모델을 앙상블 학습시켰습니다.

수동적인 하이퍼파라미터 튜닝 대신, 모델 간의 가중치 최적화를 통해 강력한 예측 성능을 도출했습니다.

📂 저장소 파일 안내 (File Manifest)
통합노트북.ipynb:

데이터 로드부터 전처리, AutoGluon 학습 및 최종 제출 파일 생성까지 포함된 메인 수사 기록입니다.

BSD발표.ipynb:

대회의 목표, 평가 방식, 핵심 시각화를 직관적으로 정리한 발표용 요약 보고서입니다.

BSD발표 추가자료코드.ipynb:

데이터 행/열 구성 확인 및 날짜 범위 검토 등 기초 수사 자료를 담고 있습니다.

BSD0210발표순서 및 흐름.txt:

생성형 AI를 활용한 자료 조사부터 최종 분석까지의 논리적 수사 경로를 정리한 시나리오입니다.

📊 수사 결과 요약 (Insights)
시간대별 패턴: 평일 출퇴근 시간대(8시, 17~18시)에 수요가 집중되는 명확한 패턴을 확인했습니다.

데이터 정제의 가치: 풍속 이상치를 보정함으로써 모델이 노이즈에 휘둘리지 않고 보다 안정적인 예측을 수행할 수 있는 환경을 조성했습니다.

🚀 시작하기 (How to Run)
본 저장소를 클론(Clone)합니다.

data/ 폴더 내에 Kaggle에서 제공하는 train.csv, test.csv, sampleSubmisson.csv를 배치합니다.

통합노트북.ipynb를 실행하여 데이터 전처리 및 모델 학습을 진행합니다.

Lessons Learned
1. 형태변환 시키고 나서 마지막에 shape로 형태 확인하기.
2. 생성형ai가 그래프 설명해준다고 곧이 곧대로 믿지말고 직접 그래프 그려서 확인하기
3. 이유없이 의미없는 그래프 그리지말기
4. eda 조금 더 신경 써주기
5. automl은 가능하면 기존 베이스 코드랑 선발 비교용으로 한번, 그리고 마지막에만 쓰기.
6. 기회가 되면 pca나 k-means 같은 것으로 대용량 다뤄보기
