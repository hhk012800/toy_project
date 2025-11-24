### 프로젝트 배경 
대학원 시절 웨이퍼 맵 데이터를 활용해 간단한 실험을 진행한 경험이 있으며, 이를 확장하고 싶어 개인 토이 프로젝트를 시작했습니다.
아래 블로그 코드를 참고해 Kaggle WM 데이터셋을 가져왔으며 블로그 와 같은 방식으로 다중 특징 추출·전처리를 수행했습니다.
참고: https://aiegg-travel.tistory.com/entry/Kaggle-WM-Dataset%EC%9C%BC%EB%A1%9C-Wafer-Map-Defect-Pattern-%EB%B6%84%EB%A5%98-%EC%8B%A4%EC%8A%B5

기존 실험 요약
Kaggle 데이터 기반 간단한 분석과 다양한 특징 추출 → multi-type feature 조합 후 One-vs-One SVM 학습 → test weighted F1 ≈ 72%.
다른 특징 추출/모델을 탐색하며 성능 향상을 모색

🔥 CNN 직접 학습 시 test weighted F1 ≈ 85% 달성.

🔥 AutoEncoder(AE)로 이미지를 재구성한 뒤, 인코더 잠재벡터 z를 CNN 입력으로 사용했으나 성능이 기대 이하
“잠재벡터를 다시 CNN 입력으로 쓰면 더 좋을까?”라는 가설을 검증했지만 오히려 기존 CNN보다 낮은 성능 - 잠재백터 z만 사용하는 대신 원본 이미지 vs 재구성 이미지 차이(Residual map)를 추가 입력/증강으로 활용하면 결함 위치 정보를 더 잘 드러낼 수 있을 것이라는 아이디어 도출.

🔥 지식 증류(Teacher–Student) 실험 위 한계를 극복하고 경량 모델을 만들기 위해 Teacher–Student Distillation을 도입 - 학습 과정에서 모델 경량화 및 성능 유지 아이디어인 지식 증류(Knowledge Distillation)를 접했고, 이를 직접 구현해보기로 함.
참고: https://blog.naver.com/bang_chuchu/224073466089
