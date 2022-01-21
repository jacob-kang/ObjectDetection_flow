# Catching_deep_learning_history
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

* ### Selective Search for Object Recognition  
_Uijlings, Jasper RR, et al. "Selective search for object recognition." International journal of computer vision 104.2 (2013): 154-171._  
</br>
이 논문은 Object detection에 많은 영향을 미친 논문인데,  
그 당시에 Sliding window와 Selective search 2개가 detection부분에서 붐이었다.  
![Uploading image.png…]()
</br>
위 사진처럼 (b)는 같은 질감이지만 다른 색상으로 구분하고, (c)는 같은 색상이지만 다른 질감으로 구분하며, (d)는 색상, 질감이 전부 다르지만 자동차에 있는 일부분이므로 자동차로 구분된다. (타이어빼고 차체만 자동차라 할수는 없으니)  

Selective search는 _Efficient Graph-Based Image Segmentation_ 를 기초로 사용되는 기술이다.  
1. 우선 _Efficient Graph-Based Image Segmentation_ 를 사용하여 초기영역을 추출
2. 
