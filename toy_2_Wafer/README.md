## 토이 프로젝트 Wafer_bin_map_data

대학원 시절에 Wafer 데이터를 활용해서 프로젝트를 진행한적이 있으며 데이터에 대해서 간단한 실험만 진행 했으며 더 프로젝트를 진행 하고 싶어 토이 프로젝트를 하게 됨
출처 : https://aiegg-travel.tistory.com/entry/Kaggle-WM-Dataset%EC%9C%BC%EB%A1%9C-Wafer-Map-Defect-Pattern-%EB%B6%84%EB%A5%98-%EC%8B%A4%EC%8A%B5 이 사이트를 확인하고 코드를 돌려 보았음('A Voting Ensemble Classifier for Wafer Map Defect Patterns Identification in Semiconductor Manufacturing'논문에서 나오는 방법론 인거 같다)
1. 케글에서 데이터를 가져와서 블로그를 통해서 간단한 데이터 분석과 다양한 특징 추출 방법을 통해 데이터 전처리를 진행 하였고 multi=types feature를 추출해서 실습을 진행함 -> One-Vs-One svm 모델을 적용하여 test 데이터에서 **weighted f1-score 72**을 기록함
2. 다른 모델을 사용하여 성능 올리거나 다른 특징 추츨 기법을 사용해서 성능평가

## 간단 정리

🔥🔥🔥 CNN모델을 사용해서 학습한 결과 test 데이터에서 **weighted f1-score 85**를 달성함 

🔥🔥🔥 오토인코더 모델을 사용하여 latent 변수를 만들어 -- > 기존의 사용했던 svm or cnn 모델을 사용하요 평가할 예정

📌📌 CNN 기반 분류 모델과 오토인코더(AutoEncoder, AE)를 함께 실험하여 웨이퍼 맵 패턴 분류 성능 개선 가능성을 검토하였다. 하지만 아래와 같은 한계가 확인되었다.
1. 인코더에서 생성되는 잠재변수 z를 추출하여 CNN 모델로 분류를 하면 성능이 좋을까? 라는 생각으로 한번 시도 해봄  --> 그 결과, CNN 기반의 직접 분류보다 latent 기반 분류 성능이 더 낮게 나타남(너무 1차원적 생각 이였음)
2. 잠재변수를 다시 CNN의 입력으로 사용하는 접근은, 목적과 과정이 중복되어 실질적 이득이 크지 않다는 점에서 비효율적일 수 있다.

✅ latent z만 사용하는 대신, **원본 이미지와 재구성 이미지의 차이(Residual map)를 추가 입력으로 활용 이를 통해 결함이나 패턴의 위치 정보가 더 드러나 분류 성능에 도움을 줄 수도 있고, 필요하다면 데이터 증강용으로도 활용할 수 있다.
즉, 재구성 오차를 단순한 평가 지표로만 쓰지 않고, feature나 학습 데이터 확장의 도구로 재해석할 여지가 있다.(개인적인 생각..)
