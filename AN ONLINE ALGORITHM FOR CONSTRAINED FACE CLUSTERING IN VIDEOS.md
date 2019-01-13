AN ONLINE ALGORITHM FOR CONSTRAINED FACE CLUSTERING IN VIDEOS

# Introduction 
우리는 비디오에 등장하는 얼굴들을 클러스터링 하는것에 흥미를 가졌다. 
왜냐하면 이것은 video summarization, indexing, retrieval, character analysis 등의 영역에 효과적인 솔루션이 될 수 있기 때문이다.
하지만 비디오에 등장하는 얼굴들은 scale, pose, illumination, expression, apperance, occlusion 과 같은 상황 때문에 클러스터링 하기가 어렵다.

클러스터링을 잘하기 위해 여러가지(...) 노력이 있었다. 하지만 무엇보다 성능에 중요한것은 효과적으로 대표 얼굴을 나타내는 것이고, CNN을 이용한 Deep feature가
등장하면서부터 눈에 띄게 성능이 향상되었다.

데이터를 처리하는 방법에 따라 두 가지로 분류하면 offline과 online 방법이 존재한다.
offline은 모든 데이터를 한번에 처리한다.
online은 모든 데이터를 한번에 알 수 없는 상황에서 sequantial 입력되는 데이터를 그때그때 처리한다.
online은 offline에 비해 한번에 주어지는 정보가 적고, 매번 클러스터 정보를 업데이트 하기 때문에 offline에 비해 더 어렵다.
따라서 현재 높은 성능을 자랑하는 대부분의 방법들은 offline 방법이다. 하지만 online 방법은 전체 데이터를 한번에 볼 수 없는 경우에 매우 유용하다.

우리는 이 논문에서 성능이 offline보다 좋은 online 방법을 소개한다.

# Proposed approach
1. Face track creation
2. Online face clustering 


실험 및 결과
![an online algorithm for constrained face clustering in videos-1](https://user-images.githubusercontent.com/23207379/51081658-5b5deb80-1738-11e9-828e-c0d2cf87584c.png)

# Conclusion 
우리는 offline,online에서 가장 성능이 좋은 online clusteirng 방법을 제시했다. 
우리의 online 방법은 강인한 대표 얼굴을 위해 CNN feature를 사용했고, 몇가지의 spatio-temporal 제약을 두어 sequantial하게 들어오는 얼굴들을 처리했다.
나아갈 점으로는 클러스터의 대표 얼굴을 선정하는 방법으로 centroid가 아닌 multivariate gausian을 사용하는 것이 있겠다.
