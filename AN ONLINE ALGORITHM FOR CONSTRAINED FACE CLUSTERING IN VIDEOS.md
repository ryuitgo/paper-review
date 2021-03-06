# AN ONLINE ALGORITHM FOR CONSTRAINED FACE CLUSTERING IN VIDEOS

---

Prakhar Kulshreshtha, Tanaya Guha

Indian Institute of Technology, Kanpur

---

# 소개

비디오에 등장하는 얼굴을 클러스터링 하는것은 비디오 summarization, indexing, retrieval, character analysis 등의 분야에 효과적인 솔루션이 될 수 있다. 하지만 비디오에 등장하는 얼굴들은 pose, expression, occlusion, apperance 의 다양함 때문에 클러스터링 하기가 쉽지 않다.


# 제기하는 문제는? 

데이터를 처리하는 방법에 따라 클러스터링을 분류하면 offline clustering 과 online clustering 이 있다.

* offline clustering : 모든 데이터가 주어져서 한 번에 클러스터링 한다.

* online clustering : 모든 데이터가 한 번에 주어지지 않고 sequantial하게 입력되는 데이터를 그때그때 클러스터링해서 클러스터의 상태를 업데이트 한다.

실제 현실에서는 모든 데이터가 한번에 주어지지 않거나 데이터의 양이 많아서 한번에 처리하기 어렵기 때문에 offline 방식보다는 online 방식이 적합하다. 하지만 online은 offline보다 어렵기 때문에 offline 만큼의 성능을 보이기 어렵다.

# 문제를 어떻게 해결했는가?

성능이 좋은 online clustering 알고리듬을 다음과 같이 제시한다. 

### Shot boundary detection

online clustering을 위해서는 클러스터링을 언제 할 것인가에 대한 이슈가 있다. 예를 들어 매 프레임마다 혹은 100프레임마다 한번 클러스터링 한다는 기준이 필요하다. 여기서는 전체 프레임을 shot 단위로 구분한다. shot이란 연속된 프레임간의 변화가 크지 않은 프레임들의 범위로 정의한다. shot 단위로 구분하기 위해 shot boundary를 detection하는데, 연속된 두 프레임간의 픽셀들의 차를 평균한 값이 지정된 threshold보다 높으면 shot boundary로 간주한다.

### Face detection and Feature extraction

프레임에 등장하는 모든 얼굴의 detection결과와 feature(FaceNet)를 뽑는다.

### Face track creation

face track : tracking을 적용해서 shot내에서 tracking된 얼굴들의 집합

얼굴이 연속된 두 프레임에 걸쳐 존재할때, 두 얼굴 영역이 일정 값 이상 겹치고, 그 두 영역의 face feature간의 euclidean distance가 일정 값 보다 작으면 같은 face track으로 간주한다.

![image](https://user-images.githubusercontent.com/23207379/51081834-e7721200-173c-11e9-984f-db7baa3f2624.png)

temporal constraint : 한 시점에서 겹치는 face track은 같은 사람일 수가 없다.

### Online face clustering 

![image](https://user-images.githubusercontent.com/23207379/51081843-0ec8df00-173d-11e9-8873-07f3f8389fe9.png)

shot이 바뀔 때, shot내에 있는 face track들을 이미 생성된 클러스터에 포함시키거나, 처음 등장한 얼굴인 경우 새로운 클러스터로 등록한다.

그러기 위해서는 존재하는 클러스터와 face track간의 가까운 정도를 구할 수 있어야 되는데, 논문에서는 face track내에 존재하는 모든 얼굴 각각에 대해서 클러스터의 center와의 euclidean distance를 구하고 이를 평균한 값으로 클러스터와 face track의 가까운 정도를 나타낸다.

클러스터의 center는 클러스터에 속한 모든 얼굴들의 feature vector를 평균한 vector를 사용하고, 새로운 face track이 클러스터에 추가되면 클러스터의 center를 업데이트 한다.

---

# 결과

![an online algorithm for constrained face clustering in videos-1](https://user-images.githubusercontent.com/23207379/51081658-5b5deb80-1738-11e9-828e-c0d2cf87584c.png)

V measure [0~1] : entropy based measure of cluster homogeneity and completeness

F score [0~1] : geometric mean of precision and recall 

# 정리 
이 논문은 성능이 높은 online 클러스터링 방법을 제안했다. robust한 얼굴 feature를 뽑기 위해 Deep CNN feature를 사용하고, spatio-temporal constraint를 사용해서 효과적으로 face track을 만들고 클러스터링 했다. 

하지만 클러스터링 성능의 비교대상(Kmeans, Gausian Mixture Model)을 살펴 보면 최근에 성능이 좋다고 알려진 알고리듬(DBSCAN, Rank-Order ...)들이 빠져있다는 점에서 논문의 알고리듬 성능이 state of the art가 맞는지 의문이 생긴다. 또한, 클러스터의 center를 단순히 face feature들의 평균으로 정하고, face track과 클러스터의 유사도를 구하는데 단순히 거리들 평균을 구한다는 점에서 새롭게 느껴지는 것이 없었다.
