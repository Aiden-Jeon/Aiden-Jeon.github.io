---
title: Introduction to XAI
comment: true
categories: [XAI]
toc: true
date: 2021-03-31
author: Jongseob Jeon
---


## Introduction to XAI(eXplainable AI)

최근 많은 산업에서 딥러닝을 적용을 시도하고 있습니다. 하지만 모델의 결과가 어떤 과정을 통해서 그런 예측을 했는지 해석하기 어려운 '블랙박스' 문제는 딥러닝 적용을 blocking하는 큰 요인입니다.

![그림-1](/imgs/xai/xai-1.png)

예를 들어 신규 대출을 받으려는 고객이 있습니다. 은행에서는 이 고객이 대출을 잘 갚을 수 있을지에 대해 심사를 하고 대출을 승인을 결정합니다. 이 은행에서는 딥러닝 모델을 이용해 고객들의 대출 심사를 합니다. 모델은 이 고객에게 대출을 거절했습니다. 고객과 대출 담당자는 대출이 거절된 이유가 고객의 나이, 직업 등 어떤 이유로 대출이 거절 되었는지 알고 싶습니다. 여기서 문제가 발생합니다. 우리는 딥러닝 모델의 결정을 해석할 수가 없습니다. 이러한 해석을 할 수 없다는 단점은 여러 산업에서 딥러닝 모델의 적용을 망설이게 합니다.

## Interpretable vs Accurate Trade-off
![그림-2](/imgs/xai/xai-2.png)

그럼 이런 의문이 들 수 도 있습니다. 해석이 힘든 딥러닝 모델 대신 해석하기 쉬운 선형회귀분석 같은 모델을 사용하면 되지 않을까? 하지만 복잡한 모델과 간단한 모델, 이  두 모델 사이에는 Trade-off 가 있습니다. 해석이 쉬운 모델들은 정확도가 떨어지며, 높은 정확도를 보이는 모델은 해석이 어렵습니다. 

그럼 해석하기도 쉽고 정확도도 높은 모델을 만들면 되지 않을까요? 방법은 두가지가 있습니다. 해석이 쉬운 모델의 정확도를 높이는 방법과 정확도가 높은 모델의 해석 가능성을 높이는 방법입니다. 그리고 최근 논문들을 보면 2번의 방법을 연구하는 추세입니다.

![그림-3](/imgs/xai/xai-3.png)

1. 해석이 쉬운 모델의 정확도를 높이는 방법

![그림-4](/imgs/xai/xai-4.png)

2. 정확도가 높은 모델의 해석 가능성을 높이는 방법

## 대리 분석 (Surrogate Analysis)
Surrogate Analysis를 우리 말로 하면 대리 분석입니다. 즉, 해석이 어려운 모델을 바로 해석하지 않고 해석 가능한 대리 모델을 만들고 이를 이용해 모델의 예측 결과를 해석합니다.

### 글로벌 대리 분석 (Global Surrogate Analysis)
글로벌 대리 분석이란 전체 학습 데이터를 사용해 블랙박스 함수 f를 따라하는 유사함수 g를 만들고 g를 해석 가능하도록 변조하는 방법을 말합니다.

#### 글로벌 대리 분석 수행 과정
1. 데이터 집합 X를 선택합니다. 이 때 집합은 학습 데이터 전체 또는 일부입니다.
2. 선택한 데이터 집합 X에 대해 블랙박스 모델 f의 예측 결과를 구합니다. 이 집합과 예측 값은 해석가능한 모델을 fit 시키기 위한 데이터셋으로 사용됩니다.
3. 해석 가능한 모델(g)을 고릅니다. 
4. 해석 가능한 모델을 2번에서 만든 데이터셋을 이용해 학습합니다.
5. 데이터 X에 대하여 모델 f가 예측한 결과(2)와 모델 g의 예측 결과를 비교하면서 두 모델이 최대한 유사한 결과를 내도록 튜닝합니다.
6. 설명 가능한 모델 g을 이용해 블랙박스 모델(f)을 해석합니다.


#### 장점
1. 유연함(Flexible)
    - 모든 해석 가능한 모델에 사용할 수 있다.
    - 모든 블랙박스 모델에 사용할 수 있다.
    - 예를 들어, 선형분석이 편한 이용자와 의사결정나무가 편한 이용자가 있을 때, 한 블랙박스 모델을 선형분석, 의사결정나무 로 대리 분석 모델을 만들 수 있다.
2. 직관적(Intuitive)
    - 쉽게 실행할 수 있다.
    - 쉽게 설명할 수 있다.
3. 평가의 용이
    - R-Squared 척도와 같이 대리분석 모델이 얼마나 블랙박스 모델을 잘 설명했는지 알 수 있다.

#### 단점
1. 모델에 대한 결론을 낼 수는 있지만, 데이터에 대한 결론을 낼 수는 없다.
2. 얼마나 대리 분석 모델을 fitting 시켜야 하는지 알 수 없다.
3. 대리 분석 모델로 선택한 모델의 모든 장점과 단점이 따라온다.

### 로컬 대리 분석 (Local Surrogate Analysis)
글로벌 대리 분석에서 블랙박스 모델을 해석하려는 것과 반대로 로컬 대리 분석은 개별적인 예측을 설명하기 위한 방법입니다. 로컬 대리 분석에는 모델과 관련 없이(Model-Agnostic) 사용 할 수 있는 방법과 특정한 모델에서(model-type-specific) 사용할 수 있는 방법이 있습니다. LIME(Local Interpretable model-agnostic explanations)은 대표적인 모델과 관련 없이 사용할 수 있는 로컬 대리 분석 방법입니다. 특정한 모델에서 사용하는 방법은 DeepLIFT 등이 있습니다.

#### 로컬 대리 분석 방법
1. 블랙박스 모델 예측을 설명하고자 하는 instance (x)를 하나 선택합니다.
2. 데이터를 섞어(perturb) 새로운 데이터 집합(x')을  만듭니다.
3. 생성된 데이터 집합(x')을 해석하려는 모델에 넣어 예측 값f(x')을 계산합니다. 생성된 데이터와 예측 값은 대리 분석 모델의 데이터로 사용됩니다.
4. 정의된 거리에 따라 새로운 데이터의 가중치(w)를 계산합니다.
5. 가중치를 이용해 해석 가능한 모델을 학습합니다. g(f, x', w)
6. 로컬 모델을 이용해 예측을 설명합니다.

#### 장점

1. 설명이 선택적이고 대조적이다.
    - LASSO나 짧은 트리를 이용하면 변수들을 선택할 수 있고 설명을 +,- 로 대조적으로 만들 수 있다.
    - 이는 human-friendly 한 설명을 할 수 있다.
2. fidelity measure
    - 해석 가능한 모델이 블랙박스 모델의 예측과 얼마나 잘 일치하는 지를 평가하는 척도
    - 이를 이용해 블랙박스 예측을 설명하는데 얼마나 신뢰할 수 있는지를 설명할 수 있다.

#### 단점
1. 설명 모델의 복잡도를 사전에 정의해야 한다.
    - Tree의 깊이 등.
2. 설명의 불안정성
    - [논문](https://arxiv.org/pdf/1806.08049.pdf)에서 두 개의 매우 가까운 점에 대한 설명이 시뮬레이션된 환경에서 크게 다르다는 것을 보여주었다.
    - 샘플링 과정을 반복하면 나오는 탐색은 다를 수 있다.
    - 불안정하다는 것은 설명을 신뢰하기 어렵다는 뜻.

## Reference
- XAI 설명 가능한 인공지능, 인공지능을 해부하다
- [https://christophm.github.io/interpretable-ml-book/global.html](https://christophm.github.io/interpretable-ml-book/global.html)
- [https://www.microsoft.com/en-us/research/uploads/prod/2019/05/Explainable-AI-for-Science-and-Medicine-slides.pdf](https://www.microsoft.com/en-us/research/uploads/prod/2019/05/Explainable-AI-for-Science-and-Medicine-slides.pdf)