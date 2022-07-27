[Link](https://arxiv.org/abs/1608.06993)

# Densely Connected Convolutional Networks

## 1) 간단한 요약
 Our proposed DenseNet architecture explicitly differen between information that is added to the network and information that is preserved. DenseNeets may be good feature extractors for various computer vision tasks that build on convolutional features.
 
## 2) 평가
1. Novel Idea => Good
2. Good Motivation => Normal
3. Easy to Read => Good
4. Reproducible => --
5. Convincing Results => Good

## 논문 읽기

### 1. Introduction
기존 CNN 구조들은 점점 deep하게 층수가 늘어났다. 결국 이러한 구조로 인해 새로운 문제가 발생했다. 그것은 역전파 되면서 Vanishing 
Gradient 문제가 발생했다. 그래서  Stochastic depth와 같은 구조를 이용해 이 문제를 해결했다.
![image](https://user-images.githubusercontent.com/80466735/181252901-14032f15-ca3f-4c83-99ee-6f4362b2965b.png)
ResNet이 가장 대표적이다. 논문 저자들은 이러한 방법을 좀 더 개선하거나 다양한 방법으로 Vanshing Gradient 문제를 해결하고자했다.
결국 더욱 효율적으로 해결할 수 있는 기존보다 더 단순한 연결된 pattern을 만들었다고 한다. 이러한 구조는
information 흐름을 높이고 network를 통해 gradient 문제를 해결해서 train을 더 쉽게 하도록했다고 한다.
이뿐만이 아니라 dense layer 구조는 regularizing effect를 준다고한다.



### 2. 특징
![image](https://user-images.githubusercontent.com/80466735/181254916-c3083497-e391-4348-96c2-3d28aa7aeacc.png)

* Dense connectivity
 DenseNet은 layer 사이에 정보 흐름을 중가 시키기에 모든 레이어들 사이에 direct connection이 되어 있다. 
 그래서 위에 있는 DenseNet 구조 이미지를 본다면 선으로 layer들이 연결되어있는 것을 볼수있다.
 
 * Composite function
 ![image](https://user-images.githubusercontent.com/80466735/181255050-95a6a63c-2ca6-40ac-a8c7-aad5919c7b70.png)
위 공식을 설명하자면 위에 Dense connectivity을 위해 출력값 x를 H에 넣어 xl을 만드는 것이다. 여기서 H를 composite function인데
H는 [BN => ReLu => 3*3 convolution]으로 구성되어있다.

* Pooling Layers
![제목 없음](https://user-images.githubusercontent.com/80466735/181257304-6d905ef7-c672-408b-b7f9-9640fd5e5e9e.png)

특성맵의 크기가 바뀌면 Dense connectivity가 불가능하다. down sampling은 피할수가 없어서 결국 저자는 down sampling  기능을 dense block 사이에
 Transition layer를 넣었다. Transition layer는 convolution-pooling 계층으로 구성되어 있다. 저자는 BN-> 1*1 conv -> 2*2 pooling layer로 구성했다고 한다.

* Growth rate
이전 레이어로부터 계속 입력을 받기에 개수가 계속 많아진다. 그래서 Growth rate를 이용해 비율을 설명하는 것이다. Growth Rate는 k로 표현한다.
실제로 k0+k*(l-1)이 input feature map인데 k의 수는 계속 늘어난다. 즉 k의 수가 크다면 입력 개수가 많다는 것이다.

근데 기존 network에도 비슷한 개념이 있다. 그러나 기존 Network와 DenseNet과 큰 차이는 DenseNet은 narrow layer를 가진다는 차이가 있다고 한다.
small growth rate가 state of the art를 도달했다고 저자는 설명했다. 그 이유는 각 모든 layer가 block 안에 이전 feature_map을 입력 값으로 넣기 때문이라고한다.
이를 네트워크의 collective knowledge(아마 집단지성..)를 이용해서 라고 했다.
또한 k(growth rate)가 각 layer에 얼마나 얼마나 globla state에 기여했는지 알려준다고 한다. global state는 network 내 어디든 접근 할 수 있다.
기존 모델과 다르게 이것은 layer에 복사할 필요가 없다고 한다.

* Bottleneck layers
![image](https://user-images.githubusercontent.com/80466735/181261858-325a8313-ee17-4af0-9a38-a6705fd775ca.png)
기존 layer와 달리 bottleneck 구조를 채택하여 input feature map의 number를 줄이고 연산은 효율적으로 한다.

* Compression
DenseNet은 Compactness한 모델이다. 우리는 transition layer에 feature map 수를 줄였다. dense block은 m개의 feature map이 추출되면
우리는 output을 transition layer에서 feature map 개수를 줄이거나 늘릴 수있다. 이를 θm으로 표현한다. θ는 0 초과 1이하의 값을 가진다.
θ은 더욱 Compression 하게 해준다. 0<1은 DenseNet-C로 부른다. 실험에서는 0를 0.5로 조정했다.
또한 transition layer, bottleneck 모두 0<1 이라면 DenseNet-BC라고 한다.


### 3. 실험
![image](https://user-images.githubusercontent.com/80466735/181263693-12dc0fc7-1ab6-4667-ad4a-25ce38210187.png)

(C는 CIFAR dataset, SVHN은 The Street View House Numbers dataset이다.)
이미지에서 bold된 것은 surpass한 결과 이고 best result는 파란색을 되어있다. +가 붙어 있는 것은 augmentation(mirroring, translation) 기법을 이용한 것이다.
+가 없는 것은 drop out만을 사용했다.

ResNet보다 적은 parameter로 낮은 error를 기록하고 large margin에 의해 좋은 결과를 보인다

### 4. Discussion
* Model Compactness
모든 layer로 부터 접근 가능하기에 feature map 학습이 좋았다. 이는 network로부터 feature reuse를 증진하고 model을 더욱 compact하게했다
* Implict Depp supervision
짧은 연결에서 loss function 덕에 추가적인 spervisionable을 일으켰다. 이덕에 정확도를 더 높이는 결과를 얻었다. 하지만 loss function, gradient는 낮은 complicated를 가지고 있다.
* Stochastic vs deterministic connection
 Stochastic depth는 기존 ResNet에서 쓰듯 랜덤하게 drop한다. 기존 모델들은 pooling layer를 drop에서 제외했다. 하지만 DenseNet은 pooling layer도 drop하는 작은 가능성을 두고 있다.

### 5. 결론
DenseNet 구조는 overfitting 과 degradation을 막았다. 게다가 DenseNet은 fewer parameter와 less computation으로 state of art에 도달하였다.







