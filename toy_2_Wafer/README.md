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

✔✔✔ 
  1. 초기 가설
    +오토인코더를 통해 입력 데이터를 압축 → latent vector 추출
    +latent vector를 CNN 입력으로 사용하면 특징이 잘 요약되어 분류 성능이 향상될 것이라 기대
  
  2. 검증 결과
    +latent vector 기반 CNN은 기존 CNN보다 성능이 낮게 나타남
  3. 개선 전략
     +정상 데이터만 학습하는 오토인코더 기반 이상 탐지로 문제 재정의
     +정상 패턴 학습 후, 정상과 다른 나머지 패턴을 이상(anomaly)으로 정의하고 이를 기반으로 이상 탐지 문제로 적용하여 모델 성능 평가

이상 탐지 적용
  +정상 데이터: 147,431개 (failureNum=8, none) 정상 데이터 학습/테스트: 117944/29487
  +비정상 데이터: 25,519개 (failureNum=0~7, 패턴 8종)
  +이상 탐지 평가(정상/비정상 데이터의 재구성 오차 계산) 25,519+29,487 =55,006
  +결과: PR AUC 0.7897, ROC AUC 0.7809
  +최적 임계값에서 Precision 0.73, Recall 0.69, F1 0.71


🔥 지식 증류(Teacher–Student) 실험 위 한계를 극복하고 경량 모델을 만들기 위해 Teacher–Student Distillation을 도입 - 학습 과정에서 모델 경량화 및 성능 유지 아이디어인 지식 증류(Knowledge Distillation)를 접했고, 이를 직접 구현해보기로 함.
Teacher 모델 - Weighted F1 ≈ 0.84, Student 모델 Weighted F1 ≈ 0.61 (생각보다 차이가 많이 나서 당황)

Teacher 대비 파라미터 수가 크게 줄어든 영향과 distillation 세팅(α, temperature 등) 정확한 이유는 모르지만 성능이 더 낮게 기록

Student가 Teacher의 soft 정보를 충분히 흡수하지 못한 상태이므로..α/temperature를 조정하거나 Student 용량을 조금 늘리는 방법을 생각 
| 모델                  | 크기 | 속도 | 성능            |
| ------------------- | -- | -- | ------------- |
| Teacher             | 큼  | 느림 | 높음            |
| Student (distilled) | 작음 | 빠름 | teacher랑 비슷해짐 |

참고: https://blog.naver.com/bang_chuchu/224073466089
