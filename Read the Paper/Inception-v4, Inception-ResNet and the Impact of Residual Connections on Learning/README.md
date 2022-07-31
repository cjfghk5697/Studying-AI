# Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning
## [Link](https://arxiv.org/abs/1602.07261?context=cs)

## 1) 요약
 We give clear empirical evidence that training with residual connections accelerates the training of Inception networks signnificantly. We also present several new streamlined architectures for both residual.
 
 ## 2) 평가
 - Novel Idea => Normal
 - Good Motivation => Good
 - Easy to Read => Good
 - Repoducible => --
 - Convincing Results => Good
 
## 논문 요약

### 1. Introduction
  2015 ILSVRC에서 resdual connection은 State of Art를 달성했다. 그래서 저자는 기존 Incepton Network와 residual connection을 결합하여 어떤 결과를 내는지 실험해보고자 한다.
  저자들은 이런 의문을 품고 Residual connetion과 Inception architecture를 합쳐서 테스트를 했다.
  
  Inception 구조는 기본적으로 매우 깊다. 이것을 개선하기 위해 Inception의 filter contatenation 단계를 Residual 구조로 대체했다. 
  교체함으로써 연산 효율성을 보존하면서 residual 접근의 이점을 거둘 수 있었다.

  이외에도 Inception 그 자체로 더 깊게 넓어지는 효율적인 방안을 연구했다. 이 방안을 적용한 모델이 바로 Inception-V4이다. 전 버전인 Inception-V3보다
  Inception 모듈을 추가하면서 더욱 심플하게 구조를 바꾸었다.
  
  실제로 Inception-V4는 성능향상이 있었다. Inception-ResNet-V2와 비슷한 결과를 냈다고 저자는 말한다.
  
### 2. Architectural Choices
  - Pure Inception Blocks
    옛 Inception model은 분산 학습을 했다. 그러나 Inception 구조는 높은 tunable을 가졌는데, 이것은 다양한 레이어들 안에 필터들의 수들을
    바꿀 수 있다는 것을 의미한다. 이를 통해 학습 속도를 최적화해왔었다. 그러나 TenseorFlow 도입으로(텐서플로우는 분산학습을 지원한다.) replica(복제본)의 분할 없이 
    모델을 학습시킬수 있게되었다. 이는 역전파에 의해 메모리 최적화가 가능하고 tensor 수를 줄여 구조적인 연산과 기울기 연산에 필요한 텐서들이 무엇인지
    신중하게 결정했다. 구조를 바꾸고 선택하는 다양한 실험 결과로 Inception-V4는 필요없는 baggage(단점)를 버리고 grid size에 대한 Inception 구조를 획일화시켰다.
    
    **'V'표시는 padding='valid'**
    
![image](https://user-images.githubusercontent.com/80466735/182019778-9715738c-d23b-4093-8a94-35e01926263d.png)

*Fig.3 구조 개요*
    
![image](https://user-images.githubusercontent.com/80466735/182019785-c7e8f3d9-ac35-48f7-ba6e-903fd0669706.png)

*Fig.4 Inception-V4와 Inception-Resnet-v2의 입력부문 Fig 3의 stem 부분이다.*
    
![image](https://user-images.githubusercontent.com/80466735/182019811-a1379e76-ac92-4634-9a91-1fd7f5267495.png)
    
*Fig.5 Inception-v4의 block(그리드 사이즈가 35x35) FInception-A이다*

![image](https://user-images.githubusercontent.com/80466735/182019856-ddf39fa7-b91c-424b-a8cb-4339f87aab2a.png)

*Fig.6 Inceptin-v4의 블럭 (그리드 사이즈 17x17) Inception-B이다.*
    
![image](https://user-images.githubusercontent.com/80466735/182019893-c325de3b-4224-4134-8bdb-2cb3390d6f7b.png)

*Fig.7 Inceptin-v4의 블럭 (그리드 사이즈 8x8) Inception-C이다.*

![image](https://user-images.githubusercontent.com/80466735/182019968-df0a3b2a-4dcf-4f8d-9310-fadc2d49eca3.png)

*Fig.8 Inceptin-v4에서 grid size를 35x35에서 17x17로 줄일 때 사용하는 reduction module. Reduction-A이다.*

![image](https://user-images.githubusercontent.com/80466735/182020033-fd313939-fcbd-4c40-a575-23ca03e95bab.png)

*Fig.9 Inception-V4에서 grid size를 17x17에서 8x8로 줄알 때 사용, Reduction-B에 해당*
    
### - Residual Inception Blocks
  Residual Version은 기존 Inception보다 가벼운 block을 사용한다. 각 Inception block은(activation 없이 1x1 convolution) filter-expansion layer을 사용한다.
  이는 Inception block이 1x1이기에 dimensionality reduction을 보완하기 위함이다.
  input 깊이를 맞추기 전에 filter의 차원을 높였다. 그래서 Inception block의 차원 감소로 보상이 필요하다. 이논문에서는 2가지 버전을 제시한다.
  Inception-v3 연산량인 Inception-Resnet-V1과 Inception-V4에 맞춘 Inception-ResNet-v2가 있다.
  
![image](https://user-images.githubusercontent.com/80466735/182020354-117ddb41-d615-4d23-af8c-4b87839189ea.png)

*Fig.10 Inception-ResNet-v1과 Inception-ResNet-v2의 전체 구조 개요.*

![image](https://user-images.githubusercontent.com/80466735/182020380-7f1a0111-1b0c-4521-afe6-23d9482a1218.png)

*Fig.11 Inception-ResNet-v1 입력부분, stem이다.*

![image](https://user-images.githubusercontent.com/80466735/182020415-5a502489-6ced-4afe-8d43-5696501fafab.png)

*Fig.12 Inception-ResNet-v1(grid size 35x35) Inception-ResNet-A이다.*

![image](https://user-images.githubusercontent.com/80466735/182020427-6577e24d-5f8b-43a1-817d-8df706507218.png)

*Fig.13 Inception-ResNet-v1(grid size 17x17) Inception-ResNet-B이다.*

![image](https://user-images.githubusercontent.com/80466735/182020465-6f5ba151-e624-4420-aeb8-58cd650b7eb7.png)

*Fig.14 Inception-ResNet-v1(grid size 17x17-> 8x8) reduction module, Reduction-B이다.*

![image](https://user-images.githubusercontent.com/80466735/182020483-d9e117a7-5d3e-4ff9-bd27-837143b79f20.png)

*Fig.15 Inception-ResNet-v1(grid size 8x8) Inception-ResNet-C이다.*

![image](https://user-images.githubusercontent.com/80466735/182020495-7526bfe3-c922-4217-8a92-8c111eb036b6.png)

*Fig.16 Inception-ResNet-v2(grid size 35x35) Inception-ResNet-A이다.*

![image](https://user-images.githubusercontent.com/80466735/182020509-cf16f804-d11b-4ae5-8483-8511dba20dc3.png)

*Fig.17 Inception-ResNet-v2(grid size 17x17) Inception-ResNet-B이다.*

![image](https://user-images.githubusercontent.com/80466735/182020518-f4a705aa-1107-436a-ac45-eb07333d9f02.png)

*Fig.18 Inception-ResNet-v2(grid size 17x17 -> 8x8) reduction module이며, Reduction-B이다.*

![image](https://user-images.githubusercontent.com/80466735/182020547-d678e6f6-c947-42cf-966f-6d5409a797aa.png)

*Fig.19 Inception-ResNet-v2(grid size 8x8) Inception-ResNet-C이다.*

Residul과 non-residual에는 Inception 사이 작은 기술력 차이가 있다. 원래 기존 layer에만 BN을 썼지만 summation에는 BN이 없다.
그 이유는 single GPU에서도 학습 가능한 모델을 유지하고 싶어서다. 만약 BN이 계속 있다면 너문 많은 연산이 필요하고 Inception block이 늘어난다는 trade-off가 있다.

저자는 자원의 효율성을 좋게 하고싶기에 trade-off가 불필요하다고 생각한다.

### - Scaling of the Residuals
  filter의 수가 1000개를 넘으면 학습 초기에 죽어버린다. 이는 lr, BN을 추가해도 결과는 똑같다. 저자는 안정화 하기위해 Scaling Down 하는 방법을 발견했다.
  하나는 Warm-up을 사용해 초반에는 낮은 lr로 학습하다가 두 번째부터 lr를 높이는 거다.
  
### 3. 학습 및 학습 결과
- momentum(decay 0.9)
- RMSProp(decay 0.9, E 1.0)
- LR( start 0.045 2번의 epoch마다 0.94 곱함)

![image](https://user-images.githubusercontent.com/80466735/182021250-b3a84ebc-a66d-4418-ad51-0d48889d22d4.png)

*Fig.20 Inception-v3와 Inception-ResNet-v1 top-5 비교, Inception-v3보다 좀더 성능이 좋은 걸로 나온다*

![image](https://user-images.githubusercontent.com/80466735/182021273-4542d338-920d-4f33-a5fc-9aa42a934cf8.png)

*Fig.21 Inception-v3와 Inception-ResNet-v1 top-1 비교, Inception-v3보다 좀더 성능이 안좋게 나온다*

![image](https://user-images.githubusercontent.com/80466735/182021291-37ce431f-777f-4446-aafb-3591a96a4f8e.png)

*Fig.22 Inception-v4와 Inception-ResNet-v2 top-5 비교, Inception-v4보다 좀더 성능이 좋게 나온다*

![image](https://user-images.githubusercontent.com/80466735/182021315-9ac039d9-9b90-496d-9e83-5830d36ed453.png)

*Fig.23 Inception-v4와 Inception-ResNet-v2 top-1 비교, Inception-v4보다 좀더 성능이 좋게 나온다*

![image](https://user-images.githubusercontent.com/80466735/182021326-600eb5b3-9292-4c90-a4e8-36a76aa2bb19.png)

*Fig.24 모든 모델 top-5 비교, Residual이 더 바르고 모델 크기에 따라 결과가 다르다*

![image](https://user-images.githubusercontent.com/80466735/182021347-2638dd9d-41a5-440f-9907-9fda06be9afa.png)

*Fig.25 모든 모델 top-1 비교, Residual이 더 바르고 모델 크기에 따라 결과가 다르다*


### 4. 결론

Inception Architecture 속도를 높이기 위해 Residual connection을 추가했다. 또한 우리 모델들이 이전 모델보다 뛰어난 것을 볼 수 있었다.
