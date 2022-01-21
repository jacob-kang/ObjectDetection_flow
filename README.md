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


