## 프로젝트 2

대학원 시절에 Wafer 데이터를 활용해서 프로젝트를 진행한적이 있으며 데이터에 대해서 간단한 실험만 진행 했으며 더 프로젝트를 진행 하고 싶어 토이 프로젝트를 하게 됨
출처 : https://aiegg-travel.tistory.com/entry/Kaggle-WM-Dataset%EC%9C%BC%EB%A1%9C-Wafer-Map-Defect-Pattern-%EB%B6%84%EB%A5%98-%EC%8B%A4%EC%8A%B5 이 사이트를 확인하고 코드를 돌려 보았음('A Voting Ensemble Classifier for Wafer Map Defect Patterns Identification in Semiconductor Manufacturing'논문에서 나오는 방법론 인거 같다)
1. 케글에서 데이터를 가져와서 블로그를 통해서 간단한 데이터 분석과 다양한 특징 추출 방법을 통해 데이터 전처리를 진행 하였고 multi=types feature를 추출해서 실습을 진행함 -> One-Vs-One svm 모델을 적용하여 test 데이터에서 weighted f1-score 72을 기록함
2. 다른 모델을 사용하여 성능 올리거나 다른 특징 추츨 기법을 사용해서 성능평가



🔥🔥🔥 CNN모델을 사용해서 학습한 결과 test 데이터에서 f1-score 75(몇번 과정에서 85까지 도출해봄)를 달성함 

🔥🔥🔥 오토인코더 모델을 사용하여 latent 변수를 만들어 -- > 기존의 사용했던 svm or cnn 모델을 사용하요 평가할 예정
    
