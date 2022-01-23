# Catching_deep_learning_history
이 Read Me는 Object detection의 흐름을 잡기위해 정리해둔것이다.  
위에서 부터 아래로 관련 연구들과 순서들이고, 논문 원본 해석 + 논문 리뷰 정리 + 자의적 해석으로 오역/잘못이해 된 부분이 있을수있다.  
_Made by Jacob Jisu Kang._

---

CNN이 세상에 알려지며 object classificaiton과 object detection의 관심이 늘어남.  
하지만 CNN을 사용하여 object detection을 하는것은 여전히 어려움.  
</br>

* ### Efficient Graph-Based Image Segmentation  
_Felzenszwalb, Pedro F., and Daniel P. Huttenlocher. "Efficient graph-based image segmentation." International journal of computer vision 59.2 (2004): 167-181._  
</br>
이 논문은 그래프 이론을 바탕으로 Image segmentaion을 하는방법을 고안함.  
방법은 상당히 간단한데, 인접한 두개 영역의 유사도의 최소값과 멀리있는 두개의 영역의 유사도의 최대값을 비교하는것임.  
![image](https://user-images.githubusercontent.com/88817336/150452600-b13486d4-6faf-4c40-90d5-a0de06d48134.png)
</br>
![image](https://user-images.githubusercontent.com/88817336/150452711-35bbc2b3-c1c1-4e04-86a0-c2283b612e13.png)
</br>
위의 사진처럼 인접한 두개 영역의 유사도는 같아서 최소값또한 높은데, 그럼에도 불구하고 멀리있는 영역의 유사도의 최대값보다 작다면, 같은영역으로 판별하는것이다.  
간단한 알고리즘으로  
![image](https://user-images.githubusercontent.com/88817336/150453015-354f0261-4915-49d8-9d3b-58518cfee509.png)  
위와 같이 잘 판별해 낸다.  

---

* ### Selective Search for Object Recognition  
_Uijlings, Jasper RR, et al. "Selective search for object recognition." International journal of computer vision 104.2 (2013): 154-171._  
</br>
이 논문은 Object detection에 많은 영향을 미친 논문인데,  
그 당시에 Sliding window와 Selective search 2개가 detection부분에서 붐이었다.  
![image](https://user-images.githubusercontent.com/88817336/150456270-11a80118-6af4-466b-b9fd-56d917833611.png)  
</br>
위 사진처럼 (b)는 같은 질감이지만 다른 색상으로 구분하고, (c)는 같은 색상이지만 다른 질감으로 구분하며, (d)는 색상, 질감이 전부 다르지만 자동차에 있는 일부분이므로 자동차로 구분된다. (타이어빼고 차체만 자동차라 할수는 없으니) (a)에 관한 얘기는 책상의 범위를 어디까지 볼것인지에 대해 얘기한다. 그릇하고 그릇안에 있는 숟가락(?)같은것도 책상을 segmentation할때 포함해야하는지 말아야 하는지에 대해..(사실 이부분은 왜있는지 잘모르겠음)  
Selective search는 _Efficient Graph-Based Image Segmentation_ 를 기초로 사용되는 기술이다.  
문제는 (b),(c)처럼 저런 부분에서는 단순히 색상이나 혹은 단순한 방법으로 detection하기가 쉽지않다는것임. 따라서 이 논문에서 Selective search를 고안함.  
방법은 아래와 같은데  
1. 우선 _Efficient Graph-Based Image Segmentation_ 를 사용하여 2000개의 초기영역을 추출  
2. 탐욕 알고리즘을 사용하여, 여러 추출한 영역중 유사도를 추출하여 가장 비슷한 영역을 골라 통합시킴.  
3. 통합된 영역을 바탕으로 후보영역을 만들어 냄.  

![image](https://user-images.githubusercontent.com/88817336/150485838-6f4b3e02-fee9-458c-8fd8-0320ec2532dd.png)

---

* ### R-CNN   
_Girshick, Ross, et al. "Rich feature hierarchies for accurate object detection and semantic segmentation." Proceedings of the IEEE conference on computer vision and pattern recognition. 2014._  
#### Contribution 1  
R-CNN은 Region proposal + CNN이라 R-CNN이다. Object detection을 사용할때 물체가 있을법한 위치에 CNN을 돌리는방법.  
R-CNN은 입력 영상에 Selective search를 이용하여 Region proposal을 하고, 그 후보들에 전부다 CNN을 돌려 결과를 내는방법.  
![image](https://user-images.githubusercontent.com/88817336/150487156-0c8c25ec-3a99-43e2-a4e1-16f57d9bc95c.png)  
위의 그림에서 사진을 넣고 전부다 CNN을 돌려 나온 결과로 SVM은 Classification을 하고, Bbox regression을해 박스위치를 수정한다.  
![image](https://user-images.githubusercontent.com/88817336/150487408-c44fbaf7-d151-4134-9fa8-c9848f3d40c5.png)

---

* ### SPP-Net  
_He, Kaiming, et al. "Spatial pyramid pooling in deep convolutional networks for visual recognition." IEEE transactions on pattern analysis and machine intelligence 37.9 (2015): 1904-1916._  
#### Contribution 1  
R-CNN에서는 몇가지 문제가 있는데, 제일 큰 문제는 Input size가 고정이 되어있다는것이다.  
그 이유는 어떠한 이미지를 집어넣고 CNN을 통해 featrue extract를 하고 그 결과들을 바탕으로 마지막에 fully connected layer(fc layer)에 전달하게되는데 (이건 그냥 간단히 ANN임.) fully connected layer부분에는 input size와 outp size가 고정되어있음.  
![image](https://user-images.githubusercontent.com/88817336/150489131-93bdb07b-e732-43aa-9bf3-4b2e3df5fc1a.png)  

즉, 이게 시사하는 바는 fc layer에 전달할때만 잘 전달하면 되는것이지 이미지의 input size를 맞추기 위해 절삭하거나 비율을 꾸겨 가며 왜곡된 이미지를 넣을 필요는 없다는 것임.  
</br>
SPP-Net은 fc layer이전에 spartial pyramid pooling을 추가하여 고정된 size를 확보하여 fc layer의 input에 문제가 없도록 하게하는것임.  
![image](https://user-images.githubusercontent.com/88817336/150489203-6ca68824-77c3-439e-85eb-b5ddb939fe58.png)  
고정된 크기의 사이즈로 만들기 위해 사이즈에 맞춰서 그리드를 나눠 Max pooling을 진행함. 그 과정은 아래의 그림과 같다.  
![image](https://user-images.githubusercontent.com/88817336/150489245-9cb4f476-c613-4918-a7a5-dc74a2883d4e.png)  

#### Contribution 2  
R-CNN은 Region proposal을 하여 2000개의 위치를 proposal받고, 그 각 위치에 CNN을 돌리게된다. 문제는 2000번의 CNN을 하기때문에 test에도 엄청나게 느린속도와 많은 메모리를 소요하게됨.  
SPP-Net은 이부분에 집중하여 Input 이미지에 CNN을 돌리고 각 추출된 feature들에 대해 region proposal을 하게된다.   
추출된 feature들은 사이즈가 작고 가벼우므로 region proposal의 속도가 빠르다.  
SPP-Net은 기존 R-CNN의 2000번의 CNN에 비해 1번의 CNN만 돌려 속도를 높혔다.  
![image](https://user-images.githubusercontent.com/88817336/150490393-3806d83e-76cb-47f7-b5b3-864728ebf0aa.png)  

---

* ### Fast R-CNN  
_Girshick, Ross. "Fast r-cnn." Proceedings of the IEEE international conference on computer vision. 2015._  
이름도 유명한 Fast R-CNN이다. 좀 이해하기가 어렵고 상당히... 어렵다.  
#### Contribution 1  
![image](https://user-images.githubusercontent.com/88817336/150491907-a70dd7b5-49c1-4894-95d7-b7251e336b47.png)  
Fast R-CNN은 SPP-Net과 똑같이 Input 이미지에 바로 CNN을 돌리는것은 똑같다.  
차이점은 SPP-Net에서는 일부 feature에서의 Region proposal이 이상한 결과를 가져오게된다.  
따라서 좀더 확실하게 바로 input 이미지를 바로 Region proposal을 돌리고 그 결과를 feature에 투영하여 찾는방법을 고안한것이다.  
ROI (=Region proposal을 통해 나온 각 영역들)을 통해 유연하게 fc layer로 넘겨주는게 가능해진것이다.  
방법은 Input 이미지를 한쪽에서는 Region proposal을 돌리고, 나머지 하나는 CNN으로 돌린다.  
CNN으로 나온 feature에 Region proposal에서 나온 결과 위치를 투영시킨다.  
![image](https://user-images.githubusercontent.com/88817336/150492142-3a2de41a-5ebf-4da2-9955-cc94743a78b0.png)  
위의 이미지를 예시로 들면, 강아지 위치는 왼쪽 아래인데 그 위치를 그대로 feature 사이즈에 맞춰서 featrue에서 그 위치를 잘라 Max pooling을 하여 2x2의 사이즈를 확보한다.  
이런식으로 하게되면 fc layer에 들어가는 input을 조정할수 있다.  
</br>
#### Contribution 2  
SPP-Net의 문제점은 multi-stage pipeline이라는것이다. 따라서 결과를 여러 과정을 거쳐서 Real-time으로서 무겁다.  
SPP-Net은 CNN이후 SVM과 bbox regression를 이용하는데, 이것들을 통채로 한꺼번에 학습이 되지않는다. CNN이후 하나는 SVM 하나는 regression으로 서로 다른모델이기때문.  
Fast R-CNN은 그 모든것들을 multitask loss로 한꺼번에 뭉쳐서 사용하였다. ROI영역을 feature영역에 투영하여 동일한 데이터로 각각 softmax, regression에 들어가므로 연산을 공유해서 end-to-end로서 학습이 가능하다는 의미.     
![image](https://user-images.githubusercontent.com/88817336/150493542-1ca033e9-bf57-40c3-90b5-9fb1e46dcf87.png)  
위 이미지는 Classification과 localization의 loss를 합친것이고, 학습이 가능하여 약간의 정확도와 빠른속도를 확보하였다.  

---

* ### Faster R-CNN  
_Ren, Shaoqing, et al. "Faster r-cnn: Towards real-time object detection with region proposal networks." Advances in neural information processing systems 28 (2015): 91-99._  
이 논문에서부터는 짜잘한 테크닉이 많이나오는데, 분명 중요하지만 이해하기가 상당히 어렵다.  
Faster R-CNN은 Fast R-CNN의 일부 문제들을 다른시선으로 바라본것같다.  
#### Contribution 1  
이전 Fast R-CNN의 문제점은 Region proposal을 사용할때 Selective search를 사용했는데, 이것은 하나의 모듈로서 사용하였고, CPU에서 돌아가므로 속도가 상당히 느렸다. 프로세싱 타임 2.3초중 2초가 Region proposal에 사용되었다고 한다. 그래서 Bottle neck(병목현상)이 발생하여 비효율적이었다.  
이부분에 Faster R-CNN에서는 Region proposal을 외부 모듈에서 사용하는것이 아니라 하나의 Region proposal을 위한 Network를 만들었고(이하 RPN), 이것을 통해 GPU에 연산을 시켜 빠른 연산속도를 이뤄냈다.  
![image](https://user-images.githubusercontent.com/88817336/150667961-425c1000-2ca9-46a0-9040-a21ffd68d74e.png)  
위의 사진으로 구조를 볼수있는데, Fast R-CNN이랑 RPN의 유무 빼고 모두 같다.  
RPN의 결과와 feature의 결과를 투영시켜 똑같이 결과를 내는대에는 똑같다.  
#### Contribution 2  
RPN에서 Region proposal의 방법이 기존의 Selective search랑 방법이 다른데, CNN에서 나온 feature의 수만큼 다양한 Anchor box를 만들어 (feature는 원본이미지의 압축버전이라고 생각하면 편하다)
![image](https://user-images.githubusercontent.com/88817336/150667909-75660e76-74f5-444d-9dec-b81eb4f5fcbe.png)  
다양한 크기의 물체를 찾을수 있도록 했다. 8x8 의 feature가 만들어졌다면, 각 1개의 feature마다 9개의 Anchor를 넣어 다양한 크기의 물체를 찾으며 feature를 여러개 선택하여 큰 물체도 찾을수있다.  
![image](https://user-images.githubusercontent.com/88817336/150667939-c37f4fc5-7668-4443-91e5-c0d7ebad1be6.png)  

---






