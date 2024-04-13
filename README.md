# LG Aimers 4기 Hackathon

## 대회 개요
* 주최/주관 | LG AI Research + LG 전자
* 기간 | 2024.02.01 ~ 2024.02.26(예선/온라인), 2024.04.06 ~ 2024.04.07(본선/오프라인)
* 주제 | MQL 데이터 기반 B2B 영업기회 창출 예측 모델 개발
* 평가 지표 | F1-score
* 규모 | 총 1000여명 참가

## 결과
* 예선 public 57위 private 9위 / 844팀 중
* 본선 public 13위 private 21위 / 34팀 중

## 대회 접근법
처음으로 참여하는 해커톤이었는데, 팀원들과 열심히 진행하여 예선 private 9등으로 본선까지 진출하게 되었습니다.
데이터는 다양한 고객 정보와 타겟 값인 영업 전환 여부로 이루어져 있었습니다. 
numerical feature 보다는 categorical feature 들이 더 많았고 대부분 High Cardinality 특성을 띄고 있습니다.

예선에서는 categorical feature 대부분이 자연어로 이루어져 있었고, 같은 의미이지만 다르게 표현된 값들이 많았기 때문에 이러한 값들을 통일시키는 방식으로 전처리를 진행했습니다.
예선 초반에 몇 개의 변수들만 간단한 전처리를 진행하고 categorical 이진 분류에 특화되어 있는 CatBoost 분류 모델을 활용하여 74점이라는 점수를 기록했습니다.
이후 다른 변수들까지 모두 더욱 꼼꼼한 전처리를 진행하고 점수를 살펴보았지만 76점 이상을 기록하지 못했고, 오히려 점수가 떨어지는 모습도 관찰했습니다.
결국 예선 마감날에는 팀원들과 상의 후, 가장 전처리를 많이 수행하지 않았지만 점수가 가장 높은 코드 파일을 등록했습니다.

public 점수가 57위였기 때문에, 본선은 당연히 진출하지 못할 것이라고 예상하고 팀원들과 수고했다는 말만 아쉽게 나누고 있던 와중에 private 점수가 공개되었습니다.
저희 팀은 private 9위로 순위가 대폭 올라가 있었습니다.
순위가 올라간 원인으로는 train 데이터의 target 값의 불균형과, overfitting을 피해야 했던 F1-score 평가 지표로 인해 
가장 전처리가 되어 있지 않은 코드를 제출함으로써 overfitting을 피해 private 점수가 올라간 것이라고 추측했습니다.

저희는 overfitting을 피하는 것이 private에서 가장 점수를 높일 수 있다는 가정을 본선에서도 이어갔습니다.
새로운 변수들을 다양하게 만들어내는 것보다, 변수들의 의미와 데이터를 최대한 보존하면서 학습에 방해되는 요소들만 제거하는 방향으로 전처리를 진행했습니다.
또한 텍스트로 이루어진 lead_message 컬럼에서는 영업 전환 여부에 따른 키워드 추출을 통해, positive/negative로 새롭게 라벨링하는 기법을 사용했습니다.
예선에서는 public 점수가 아무리 높아도 private에서 뒤집히는 상황을 겪었기 때문에, 13위 정도의 코드를 제출했습니다.

하지만 본선 private은 예선과 다르게 순위가 많이 떨어지면서 결국 발표 평가에는 들지 못했습니다.


## 배운점
첫 해커톤이었기 때문에, 상위권 팀들의 발표 평가가 더욱 기대됐었고 더욱 집중해서 발표를 들었습니다.
발표를 듣고 저희가 놓쳤던 부분들과 새롭게 알게된 것들이 많았습니다.

1. 데이터 불균형 처리
   train data에서는 target 값인 영업 전환 여부가 11:1 정도로 불균형이 매우 심했었습니다. 따라서 상위권 팀들은 오버 샘플링과 언더 샘플링 등을 진행했는데 결과적으로는 점수가 더 높은 언더 샘플링을 공통적으로 진행했었습니다. (언더 샘플링: 불균형한 데이터 셋에서 높은 비율을 차지하던 클래스의 데이터 수를 줄임으로써 데이터 불균형을 해소)
2. validation data 구성
   저희 팀은 예선 때 진행한 그대로 random sampling을 통해서 가장 간단한 방법으로 validation data를 구성하여 모델 점수를 측정했습니다. 하지만 데이터 불균형이 심하기 때문에 이러한 방법은 정확하게 작용하지 않을 가능성이 큽니다. 본선 때는 다행이 제출 수가 많기 때문에 바로 점수를 확인할 수 있었지만, 다음 대회에서는 validation data 까지 신경써서 이용해야겠다고 다짐했습니다.
3. feature importance
   이번 대회에서 상위권 팀의 공통점은 feature importance 평가를 통해 customer_idx와 lead_owner의 중요도가 높다는 것을 확인하고 이를 이용해 피처 엔지니어링과 모델링을 진행한 것이었습니다. 저희 팀도 예선 때는 각 피처들의 중요도를 확인해보았지만 이를 활용하여 큰 점수를 얻지 못해 본선 때는 이를 간과했던 것 같습니다.
4. 자연어 처리
   최종 3등한 팀은 정형 데이터를 자연어로 접근하였습니다. 각 피처들을 이용해 다양한 문장을 만들어 Bert 모델의 input으로 활용했습니다. 피처를 자연어로 이용하면서, 데이터의 불균형을 해결하고 xsbert로 가장 작은 모델임에도 불구하고 견고하게 학습을 진행했습니다. 이를 통해 정형 데이터를 자연어 처리로 이용할 수 있는 새로운 시각을 얻었습니다.

비록 수상은 못했지만 1박 2일 동안 AI 모델 개발에 대한 시각을 넓힐 수 있는 알찬 경험이었습니다.
다음 기수에는 꼭 수상을 목표로 열심히 공부해보겠습니다 :)



