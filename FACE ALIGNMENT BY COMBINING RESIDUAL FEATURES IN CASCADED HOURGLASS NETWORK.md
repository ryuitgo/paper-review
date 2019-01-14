# FACE ALIGNMENT BY COMBINING RESIDUAL FEATURES IN CASCADED HOURGLASS NETWORK

Weiliang Chen, Qiang Zhou and Roland Hu

College of Information Science and Electronic Engineering, Zhejiang University
Hangzhou, 310027, P. R. China

# 소개

Face alignment란 얼굴 이미지 위에 미리 정의된 눈,코,입,외곽선의 위치를 estimation하는 것입니다. 최근에는 Fully Connected Network (FCN) 를 사용한 픽셀 레벨의 밀도 추정 방식을 많이 사용합니다. 즉, 데이터로부터 변수가 가질 수 있는 모든 밀도(확률)를 추정하는 것입니다. FCN에서는 이를 위해 kernel function을 이용합니다. kernel function으로 ground truth값과, 예측된 밀도 값을 나타내고 두 값의 difference(loss)가 최소화(Optimization)되게 학습합니다. 이때, 두 값의 loss를 구하기 위해 cross entropy loss또는 L2 norm loss를 사용합니다. 

# 논문이 제기하는 문제는? 
  
Face alignment를 CNN으로 학습하는 과정에서 예측된 alignment 결과와 ground truth를 kernel function으로 나타냅니다. 하지만 kernel function을 이용해서 cross-entropy나 L2 norm으로 loss를 계산하는 것이 face alignment의 실제 location error를 정확히 반영하지 못한다고 주장합니다.

![image](https://user-images.githubusercontent.com/23207379/51083635-ccb19480-1760-11e9-91af-737b98b45dac.png)

![image](https://user-images.githubusercontent.com/23207379/51083649-f965ac00-1760-11e9-8578-679a4c535c91.png)  

특히 저자는 alignment network 학습 과정과 alignment 결과를 테스트 하는 과정에 모순이 있다고 설명합니다.
학습 과정에서는 ~ but 테스트 과정에서는 ~

# 논문에서는 문제를 어떻게 해결했는가?

이처럼 학습 과정에서 최적화 하는 loss가 실제 alignment location error를 정확히 반영하지 못하기 때문에 
논문에서는 학습 과정에서도 alignment error를 반영할 수 있는 loss를 제안합니다.

우선, 네트워크 구조는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/23207379/51083157-a982e700-1758-11e9-99bb-37a1d500e543.png)

이 네트워크는 두 가지 블럭을 형성하는데, (1) hourglass network 와 (2) residual feature 입니다.

(1) hourglass network : 다운 샘플링과 업 샘플링을 거쳐 입력 이미지의 해상도와 같은 heatmap을 출력 

장점 : 

(2) residual feature : hourglass network로 부터 얻은 heatmap의 alignment error를 estimation하기 위해 사용

![image](https://user-images.githubusercontent.com/23207379/51130190-c8c06800-186f-11e9-8163-f4de28af70bf.png)

![image](https://user-images.githubusercontent.com/23207379/51130107-90b92500-186f-11e9-93d2-bd56c90ee9f5.png)

# 문제를 해결한 결과는?

# 결론



---
//# 논문이 제기하는 문제가 과연 문제인가? (생략)
여기서 저자는 FCN 학습과정에서 kernel function을 사용하는것이 alignment 정확도를 손상(harm)시킨다고 주장합니다. 저자에 따르면 학습과정에서의 
목적은 gt값과, 예측된값의 차이를 optimization하는 것인 반면, 테스트과정에서는 alignment의 정확도를 측정하기 위해 유클리디안 거리를 사용한다는 것입니다. 저자는 이런 학습과정과 테스트과정의 모순이 성능 저하를 야기한다고 주장합니다. 
그리고 논문에서 저자는 이런 모순을 완화 시킬 수 있게 학습과정과 테스트과정의 각각 다른 Optimization기준을 결합하는 네트워크 구조를 제시합니다.
