# FACE ALIGNMENT BY COMBINING RESIDUAL FEATURES IN CASCADED HOURGLASS NETWORK

Weiliang Chen, Qiang Zhou and Roland Hu


College of Information Science and Electronic Engineering, Zhejiang University
Hangzhou, 310027, P. R. China

# Introduction
Face alignment란 얼굴 이미지 위에 미리 정의된 눈,코,입,외곽선의 위치를 estimation하는 것입니다. 
최근에는 Fully Connected Network (FCN) 를 사용한 픽셀 레벨의 밀도 추정 방식을 많이 사용합니다.
이는 다시 말해 데이터로부터 변수가 가질 수 있는 모든 밀도(확률)를 추정하는 것입니다. FCN에서는 이를 위해 kernel function을 이용합니다.
kernel function으로 ground truth값과, 예측된 밀도 값을 나타내고 두 값의 difference(loss)를 최소화(Optimization)되게 학습합니다.
이때, 두 값의 loss를 구하기 위해 cross entropy loss또는 L2 norm loss를 사용합니다. 

여기서 저자는 FCN 학습과정에서 kernel function을 사용하는것이 alignment 정확도를 손상(harm)시킨다고 주장합니다. 저자에 따르면 학습과정에서의 
목적은 gt값과, 예측된값의 차이를 optimization하는것인 반면, 테스트과정에서는 alignment의 정확도를 측정하기 위해 유클리디안 거리를 사용합니다.
저자는 이런 학습과정과 테스트과정의 모순이 성능 저하를 야기한다고 주장합니다. 
그리고 논문에서 저자는 이런 모순을 완화 시킬 수 있게 학습과정과 테스트과정의 각각 다른 Optimization기준을 결합하는 네트워크 구조를 제시합니다. 

# The proposed method

![image](https://user-images.githubusercontent.com/23207379/51083157-a982e700-1758-11e9-99bb-37a1d500e543.png)
위 구조는 stacked hourglass network 와 residual feature 두 블럭으로 구성됩니다.
이미지가 입력되면 landmark의 초기 위치를 나타내기 위해 hourglass network를 거치고, 해상도가 같은 heatmap으로 인코딩됩니다. 이 네트워크의 중간 레이어에는 residual feature를 만들기 위한 작은 서브 네트워크가 있습니다. residual feature는 heatmap의 alignment error를 측정하는데 사용됩니다.
그 결과 heatmap은 matrix를 최적화 하게 학습되고, alignment error를 위한 residual feature는 유클리디안 거리를 이용해서 학습되기 때문에 
기존의 학습과 테스트의 모순을 어느정도 완화시키는 효과를 볼 수 있습니다.

1. The weakness of existing FCN architecture

![image](https://user-images.githubusercontent.com/23207379/51083635-ccb19480-1760-11e9-91af-737b98b45dac.png)

![image](https://user-images.githubusercontent.com/23207379/51083649-f965ac00-1760-11e9-8578-679a4c535c91.png)

2. residual feature


# Review

# Abstract

흔히 Face Landmark Localization으로 알고있는 Face Alignment에 관한 논문입니다.

논문의 저자는 face alignment에서 Fully Convolutional Networks (FCN)의 kernel function을 사용하는 것을 주목했습니다. 

kernel function의 cross entropy와 pixel-wise L2 Loss가 face alignment error를 정확히 반영하지 못한다고 주장합니다.

이 분석을 기반으로 RF-CHN 방법을 고안했고, 이 방법은 훌륭한 성능을 보인다고 주장합니다.


# Introduction

FCN에서는 ground truth probability matrix를 만들기 위해 kernel function을 사용한다. 그리고 이것과 predicted probability matrix 간의

loss 차이를 최소화 되도록 학습한다. 

FCN 학습(training)과정이 predicted matrix와 gt matrix간의 차이를 optimization 하는것이 목적인것과 달리, 

FCN 테스트(testing)에서는 alignment의 quality를 계산하기 위해 유클리디안 거리를 사용한다.

여기서 비롯되는 학습과 테스트에서의 모순이 성능 저하를 야기한다.

이 관찰에 기반해서 우리는 학습과 테스트 각각의 기준을 결합하는 것을 제안했다. 

결국, 학습과 테스트의 모순이 완화됐다. 왜냐하면 heatmap은 matrix difference를 optimizing하게 되고,
alignment residuals는 유클리디언 거리에 의해 학습되기 때문이다. 

# The Proposed Method & Experiment
1. The weakness of existing FCN architecture


2. The cascaded hourglass network with residual features


# Conclusion

우리는 face alignment를 위해 FCN의 kernel function을 사용하는 것에 대한 약점을 연구했고, 

residual features와 cascaded hourglass network를 결합해서 RF-CHN 구조를 제시했다.

그리고 실험에서 우리의 방법이 300-w, Menpo dataset에서 뛰어난 성능을 보였다.
