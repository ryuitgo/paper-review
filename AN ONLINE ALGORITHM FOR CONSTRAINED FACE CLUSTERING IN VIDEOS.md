# AN ONLINE ALGORITHM FOR CONSTRAINED FACE CLUSTERING IN VIDEOS
Prakhar Kulshreshtha, Tanaya Guha

Indian Institute of Technology, Kanpur

# 소개

비디오에 등장하는 얼굴을 클러스터링 하는것은 비디오 summarization, indexing, retrieval, character analysis 등의 영역에 효과적인 솔루션이 될 수 있다. 하지만 비디오에 등장하는 얼굴들은 pose, expression, occlusion, apperance 의 다양함 때문에 클러스터링 하기가 쉽지 않다.

클러스터링은 데이터를 처리하는 과정에 따라 두 가지로 분류하면 offline과 online 방법이 존재한다.

---

* offline clustering : 모든 데이터가 주어져서 한 번에 클러스터링 한다.

* online clustering : 모든 데이터가 한 번에 주어지지 않고 sequantial하게 입력이 되고, 이를 그때 그때 클러스터링해서 클러스터의 상태를 업데이트 한다.

---

# 논문이 제기하는 문제는? 
 
다수의 실제 시나리오에서는, 모든 데이터가 한번에 주어지지 않거나 데이터의 양이 많아서 한번에 처리하기 어렵기 때문에 offline 방식보다는 online clustering이 적합하다. 하지만 ~ 이유 때문에 기존의 online clustering은 offline clustering에 비해 성능이 낮다. 

# 논문에서는 문제를 어떻게 해결했는가?

이 논문은 성능이 좋은 online clustering 알고리듬을 제시한다. 

![image](https://user-images.githubusercontent.com/23207379/51081843-0ec8df00-173d-11e9-8873-07f3f8389fe9.png)

1. Face track creation

* Shot boundary detection

  shot이란 연속으로 촬영된 프레임으로 정의하고 video를 shot level로 나눈다. 그러기 위해 shot boundary를 detection하는데, 
  연속된 두 프레임간에 픽셀들의 평균의 차가 threshold보다 높으면 shot boundary로 간주한다.
  
* Face detection and Feature extraction

  shot boundary를 찾은 후엔 해당 shot에 존재하는 모든 얼굴을 찾고, 얼굴의 feature를 뽑는다.
  
* Face track creation

  ![image](https://user-images.githubusercontent.com/23207379/51081834-e7721200-173c-11e9-984f-db7baa3f2624.png)
  
  연속된 프레임에서 두 얼굴 영역의 겹치는 정도가 t1이상이고, 그 두 얼굴 영역의 face feature간의 distance가 t2보다 작으면
  같은 face track으로 간주한다.
  
2. Online face clustering 

  최종적으로 우리가 하고자 하는 것은 각 shot에서 생성한 face track들을 이미 생성된 클러스터에 포함시키거나, 
  처음 등장한 얼굴이라면 새로운 클러스터로 형성하는 것이다.
  
  temporal constraint : 한 시점에서 겹치는 face track은 같은 ID가 될 수 없다.
  
  similarity matrix : face track과 기존 클러스터간의 거리를 계산
  
  face track 클러스터간의 거리 : face track내의 모든 얼굴에 대해서 클러스터의 center와의 거리를 구하고 그 거리들을 평균
  
  클러스터의 center : 해당 클러스터에 존재하는 모든 얼굴들의 feature의 평균을 사용한다. 

# 문제를 해결한 결과는?

![an online algorithm for constrained face clustering in videos-1](https://user-images.githubusercontent.com/23207379/51081658-5b5deb80-1738-11e9-828e-c0d2cf87584c.png)

![image](https://user-images.githubusercontent.com/23207379/51081694-08386880-1739-11e9-852f-11cff87f593a.png)

# Review 
이 논문은 offline 클러스터링보다 성능이 낮은 기존 online 클러스터링과 다르게 offline보다 성능이 높은 online 클러스터링 방법을 제시하고있다. robust한 얼굴 feature를 위해 CNN Deep feature를 사용했고, 몇가지 spatio-temporal trick을 사용해서 효과적으로 face track을 만들고 클러스터링 했다. 

하지만 클러스터링 성능의 비교대상에 내가 리서치한 성능 좋은 알고리듬(DBSCAN, Rank-Order ...)들이 빠져있고 비교적 단순한 Kmeans, GMM을 사용하고 있다는 점에서 과연 논문에서 제시한 방법이 state of the art가 맞는지 의문이다. 또한, 클러스터의 center를 선정하는데 있어 단순히 face feature들의 평균을 구하는것, face track과 클러스터의 유사도를 구하는데 단순히 거리들 평균을 구하는 것을 보고 과연 이 기본적인 처리 방식을 이용해서 Real-World Video에 등장하는 많은 face variation을 감당해 낼 수 있는지 의문이 들었다. 




# Introduction 
우리는 비디오에 등장하는 얼굴들을 클러스터링 하는것에 흥미를 가졌다. 왜냐하면 이것은 video summarization, indexing, retrieval, character analysis 등의 영역에 효과적인 솔루션이 될 수 있기 때문이다. 

클러스터링을 잘하기 위해 여러가지(...) 노력이 있었다. 하지만 무엇보다 성능에 중요한것은 효과적으로 대표 얼굴을 나타내는 것이고, CNN을 이용한 Deep feature가 등장하면서부터 눈에 띄게 성능이 향상되었다. 클러스터링은 데이터를 처리하는 방법에 따라 두 가지로 분류하면 offline과 online 방법이 존재한다.

* offline은 모든 데이터를 한번에 처리한다.

* online은 모든 데이터를 한번에 알 수 없는 상황에서 sequantial 입력되는 데이터를 그때그때 처리한다.

online은 offline에 비해 한번에 주어지는 정보가 적고, 매번 클러스터 정보를 업데이트 하기 때문에 offline에 비해 더 어렵다. 따라서 현재 높은 성능을 자랑하는 대부분의 방법들은 offline 방법이다. 하지만 online 방법은 전체 데이터를 한번에 볼 수 없는 경우에 매우 유용하다. 우리는 이 논문에서 성능이 offline보다 좋은 online 방법을 소개한다.

# Proposed approach

  ![image](https://user-images.githubusercontent.com/23207379/51081843-0ec8df00-173d-11e9-8873-07f3f8389fe9.png)

1. Face track creation

* Shot boundary detection

  shot이란 연속으로 촬영된 프레임으로 정의하고 video를 shot level로 나눈다. 그러기 위해 shot boundary를 detection하는데, 
  연속된 두 프레임간에 픽셀들의 평균의 차가 threshold보다 높으면 shot boundary로 간주한다.
  
* Face detection and Feature extraction

  shot boundary를 찾은 후엔 해당 shot에 존재하는 모든 얼굴을 찾고, 얼굴의 feature를 뽑는다.
  
* Face track creation

  ![image](https://user-images.githubusercontent.com/23207379/51081834-e7721200-173c-11e9-984f-db7baa3f2624.png)
  
  연속된 프레임에서 두 얼굴 영역의 겹치는 정도가 t1이상이고, 그 두 얼굴 영역의 face feature간의 distance가 t2보다 작으면
  같은 face track으로 간주한다.
  
2. Online face clustering 

  최종적으로 우리가 하고자 하는 것은 각 shot에서 생성한 face track들을 이미 생성된 클러스터에 포함시키거나, 
  처음 등장한 얼굴이라면 새로운 클러스터로 형성하는 것이다.
  
  temporal constraint : 한 시점에서 겹치는 face track은 같은 ID가 될 수 없다.
  
  similarity matrix : face track과 기존 클러스터간의 거리를 계산
  
  face track 클러스터간의 거리 : face track내의 모든 얼굴에 대해서 클러스터의 center와의 거리를 구하고 그 거리들을 평균
  
  클러스터의 center : 해당 클러스터에 존재하는 모든 얼굴들의 feature의 평균을 사용한다. 
   

# Performance evaluation

![an online algorithm for constrained face clustering in videos-1](https://user-images.githubusercontent.com/23207379/51081658-5b5deb80-1738-11e9-828e-c0d2cf87584c.png)

![image](https://user-images.githubusercontent.com/23207379/51081694-08386880-1739-11e9-852f-11cff87f593a.png)

# Review 
이 논문은 offline 클러스터링보다 성능이 낮은 기존 online 클러스터링과 다르게 offline보다 성능이 높은 online 클러스터링 방법을 제시하고있다. robust한 얼굴 feature를 위해 CNN Deep feature를 사용했고, 몇가지 spatio-temporal trick을 사용해서 효과적으로 face track을 만들고 클러스터링 했다. 

하지만 클러스터링 성능의 비교대상에 내가 리서치한 성능 좋은 알고리듬(DBSCAN, Rank-Order ...)들이 빠져있고 비교적 단순한 Kmeans, GMM을 사용하고 있다는 점에서 과연 논문에서 제시한 방법이 state of the art가 맞는지 의문이다. 또한, 클러스터의 center를 선정하는데 있어 단순히 face feature들의 평균을 구하는것, face track과 클러스터의 유사도를 구하는데 단순히 거리들 평균을 구하는 것을 보고 과연 이 기본적인 처리 방식을 이용해서 Real-World Video에 등장하는 많은 face variation을 감당해 낼 수 있는지 의문이 들었다. 
