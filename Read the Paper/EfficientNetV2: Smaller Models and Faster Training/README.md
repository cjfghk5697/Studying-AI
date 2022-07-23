# EfficientNetV2: Smaller Models and Faster Training
[arxiv](https://arxiv.org/abs/2104.00298)
## 1) 간단한 요약
This paper introduces EfficientNet V2, a new family of convolutional networks that have faster training speed and better parameter efficiency than previous models
optimized with traing aware NAS and model scaling. To furether spped ip the training, we propose an improved method of progressive learning,
that jointly increase image size and regularization during training.

## 2) 평가
* 1. Novel Idea => Normal
* 2. Good Motivation => Good
* 3. Easy to Read => Good
* 4. Reproducible => --
* 5. convincing results => Good


## 논문 읽기

이 논문은 EfficientNetV2 모델이 나온 논문이다. 이 논문에서는 전 모델인 EfficientNet보다 빠르고 효과적인 모델을 만들었다.
특히 빠른 학습(trainig efficiency)에 집중을 하였다. 최근 나온 많은 모델들(NFNet,ResNet-Rs) 모두 training efficieny를 향상 시키기위해
나온 모델이다.
![image](https://user-images.githubusercontent.com/80466735/180588058-b986f00c-6e75-4c92-a21e-556510832035.png)
그러나 보면 EfficientNetV2는 적은 파라미터로도 비슷한 수준의 성능을 보여준다.

1. Architecture Design
  - 기존 문제 및 효율적인 학습
   기존 모델은 Large image로 인해 느릴 뿐만 아니라 batch size가 낮았다. 그래서 학습속도가 느렸다, 그래서 입력 이미지를 작게하여
   큰 batch 사용과 연산을 줄인다. 근데 작은 이미지 사이즈가 좋은 결과가 안나올 수도 있다고 생각하지만 실제론느 좋은 결과가 나오는 경우도 많음.
   그래서 이미지 사이즈를 조절하는 기법 progressive learning을 제안함.
   
2. Depthwise convolutions are slow in early layers
![image](https://user-images.githubusercontent.com/80466735/180588543-baab5297-fce6-4a8b-814c-1f3e3e228e2c.png)
  - EfficientNet은 extensive depthwise로 인해 utilize model accelerators가 안좋았음. 그래서 depthwise 구조를 대체하였는데
  그것이 MBConv를 Fused-MBConv로 바꾸는 것이었다. 그러므로 depthwise한 부분을 제외했다. 또한 Fused stage 1-3에 적용을 했는데 
  FLOPs 향상을 볼수 있었다. 하지만 모든 스테이지에 적용을한다면 느려졌다. 그래서 적절히 사용하기 위해 NAS(neural architecture search)를 사용하여
  적절히 적용했다.
  
3. 구조
 NAS 사용으로 {MBConv, Fused-MBCon}, number of layers, kernel size{3*3,5*5}, expansion ratio{1,4,6}을 적절히 조절했다. 이뿐만 아니라 필요없는 옵셕을 삭제해서 
 탐색범위를 줄이기도 했다.
 ![image](https://user-images.githubusercontent.com/80466735/180588693-79140c3d-4557-4f84-87e2-53a9c0bf132b.png)
그래서 기존 EfficientNet과 비교되는 4가지의 특징이 있다.
(1) MBConv와 Fused-MBConv 사용
(2) MBConv에 Expansion ration를 줄인다 (overhead 때문)
(3) 3*3의 적은 커넣 사이즈를 사용한다. 작용 커널을 사용하기에 receptive field가 감소하여 더많은 레이어를 사용해 이를 보완한다.
(4) 오버헤드와 파라미터 줄이기위해 stride-1 stage를 제거했다.

4. Motivation
 이미지를 각각 다르게 훈련할 경우 그에 따른 정규화가 필요하다고 생각함. 작은 이미지와 작은 네트워크, 작은 제한이 있어야한다는 거다.
 큰 이미지는 overfitting을 일으키기도 한다. 그래서 RandAug 사용으로 좋은 정확도를 달성했다. 만약 image size가 작으면
 RandAug의 Magnitude 크기도 줄이면 된다.
 ![image](https://user-images.githubusercontent.com/80466735/180588816-33473e69-821a-4e4d-9fa0-81080555080a.png)

5. Regularization
 * Dropout: 무작위로 채널 제거
 * RandAugment: 다양한 데이터 증가(사이즈 조절 같은,,)
 * Mixup: 두 이미지를 적절한 비율로 섞음

6. 결론
결론적으로  progressive learning과 adaptive regularization을 적용하면 다른 모델에 비해 학습 속도와 정확도 상승이 높다.
![image](https://user-images.githubusercontent.com/80466735/180588991-3c97abef-0b36-4dee-8a9c-5515c485fdda.png)
작은 이미지 학습은 좋은 결과를 낸다는 것을 확인할수 있다.


   
   
   
