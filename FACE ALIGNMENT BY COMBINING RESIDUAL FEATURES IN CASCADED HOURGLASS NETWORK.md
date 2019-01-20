# FACE ALIGNMENT BY COMBINING RESIDUAL FEATURES IN CASCADED HOURGLASS NETWORK

Weiliang Chen, Qiang Zhou and Roland Hu

College of Information Science and Electronic Engineering, Zhejiang University
Hangzhou, 310027, P. R. China

# 소개

Face alignment란 얼굴 이미지 위에 미리 정의된 눈,코,입,외곽선의 위치를 estimation하는 것입니다. 최근에는 Fully Connected Network (FCN) 를 사용한 픽셀 레벨의 밀도 추정 방식을 많이 사용합니다. 즉, 데이터로부터 변수가 가질 수 있는 모든 밀도(확률)를 추정하는 것입니다. FCN에서는 이를 위해 kernel function을 이용합니다. kernel function으로 ground truth값과, 예측된 밀도 값을 나타내고 두 값의 difference(loss)가 최소화(Optimization)되게 학습합니다. 이때, 두 값의 loss를 구하기 위해 cross entropy loss또는 L2 norm loss를 사용합니다. 

# 논문이 제기하는 문제는? 
  
Face alignment를 CNN으로 학습하는 과정에서 예측된 alignment 결과와 ground truth를 kernel function으로 나타냅니다. 하지만 kernel function을 이용해서 cross-entropy나 L2 norm으로 loss를 계산하는 것이 face alignment의 실제 location error를 정확히 반영하지 못한다고 주장합니다.

---

![image](https://user-images.githubusercontent.com/23207379/51083635-ccb19480-1760-11e9-91af-737b98b45dac.png)

---

![image](https://user-images.githubusercontent.com/23207379/51083649-f965ac00-1760-11e9-8578-679a4c535c91.png)  

---

# 논문에서는 문제를 어떻게 해결했는가?

학습 과정에서 사용하는 loss에 실제 alignment location error를 추가했습니다.

---

네트워크 구조 

![image](https://user-images.githubusercontent.com/23207379/51083157-a982e700-1758-11e9-99bb-37a1d500e543.png)

---

(1) hourglass network : 다운 샘플링과 업 샘플링을 거쳐 입력 이미지의 해상도와 같은 heatmap을 출력 

(2) residual feature : hourglass network로 부터 얻은 heatmap의 alignment error를 estimation하기 위해 사용

---

![image](https://user-images.githubusercontent.com/23207379/51130190-c8c06800-186f-11e9-8163-f4de28af70bf.png)

---

objective function 

처음에는 (5)만 이용해서 학습, 수렴한 후에는 (6) [ residual features & cross entropy loss] 으로 바꿔서 학습

![image](https://user-images.githubusercontent.com/23207379/51130107-90b92500-186f-11e9-93d2-bd56c90ee9f5.png)

---

# 문제를 해결한 결과는?

---

### Data Set

![image](https://user-images.githubusercontent.com/23207379/51131859-2c4c9480-1874-11e9-850b-99b4bff05372.png)

---

### Our Model

* CHN, RF-CHN : training subset of the 300-W dataset

---

![image](https://user-images.githubusercontent.com/23207379/51439493-41a34200-1cfe-11e9-9586-cddfbc050f22.png)

---

![image](https://user-images.githubusercontent.com/23207379/51131499-42a62080-1873-11e9-84ae-085513d8e589.png)

---
