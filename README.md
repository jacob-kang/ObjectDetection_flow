# The Obeject detection papers
  
__# 영어로 변환중 #__  

__# Translating in English #__  

This Read Me is for catching object detection flow.  
The flow is from top to bottom and it is configured with my paper understanding + some informations from blogs.  
There __may__ be misunderstanding or typo or etc.  
_Made by Jacob Jisu Kang._  

Note. this Read me skips some basic knowledges such as Perceptron, FFNN, Backpropagation etc.  

---  
In 1989, CNN came out in the world. before CNN, People still made lots of effort to solve object classifcaiton, object detection.  
CNN made those hard problems be easy to solve or apporach. __But__ Still hard.  
So There were numerous papers to approach those hard problem with CNN.  
Let's explore it.  
</br>
* ### Efficient Graph-Based Image Segmentation  
_Felzenszwalb, Pedro F., and Daniel P. Huttenlocher. "Efficient graph-based image segmentation." International journal of computer vision 59.2 (2004): 167-181._  
</br>
This paper talks about Image segmentation based on Graph theory.  
The method is quite simple. It is to compare the minimum value of the similarity of two near regions with the maximum value of the similarity of two far regions.  
![image](https://user-images.githubusercontent.com/88817336/150452600-b13486d4-6faf-4c40-90d5-a0de06d48134.png)
</br>
![image](https://user-images.githubusercontent.com/88817336/150452711-35bbc2b3-c1c1-4e04-86a0-c2283b612e13.png)
</br>
Like above picture, The near area similaritys are high because they are literally near. Nevertheless, If near area similaritys are smaller than far area similarity, then it deems same area.  
With this simple algorithm,   
![image](https://user-images.githubusercontent.com/88817336/150453015-354f0261-4915-49d8-9d3b-58518cfee509.png)  
it discerns well like above pircutre.  

---

* ### Selective Search for Object Recognition  
_Uijlings, Jasper RR, et al. "Selective search for object recognition." International journal of computer vision 104.2 (2013): 154-171._  
</br>
This paper hugely affected object detection.  
At the time before the paper, Sliding window (Exhaustive Search) was best in the area of detection.  
But Sliding window a.k.a. Exhaustive Search costs big computation power and takes long time.  
So this paper aimed to make region proposal algorithm be easier and efficiency. 
![image](https://user-images.githubusercontent.com/88817336/150456270-11a80118-6af4-466b-b9fd-56d917833611.png)  
</br>
Like this picture, (b) the cats can be distinguished by colors although it has same texture, (c) chemelon can be distinguished from background by texture, (d) the wheel can be distinguished as a car although it is surrounded by car and it has different color, texture.  
(a) is about what area is table. there are dishes, sppon where is in bowl on table. the question is like "do we detect the table w/out spoon, dishes or not?". (to be honest, I'm not sure I cathced it right. XD)  
Anyway, the author of this paper mentioned that these should be not judged by only one but judged comprehensive things such as arrangement, situation(?).  
So in this paper, the author suggested __Selective search__.  
The procedure is like below,  
1. first of all, by using _Efficient Graph-Based Image Segmentation_, export 2000 initial areas.
2. by using greedy algorithm, combine similar area by checking similarity of area.
3. with combined area, make proposal area.

![image](https://user-images.githubusercontent.com/88817336/150485838-6f4b3e02-fee9-458c-8fd8-0320ec2532dd.png)

---

* ### R-CNN   
_Girshick, Ross, et al. "Rich feature hierarchies for accurate object detection and semantic segmentation." Proceedings of the IEEE conference on computer vision and pattern recognition. 2014._  
To examinate this R-CNN, I wrote down those over papers.  
R-CNN is Regiop proposal + CNN. So it's named as R-CNN.  
Like the name, In this paper, the author suggested a fusion of Region proposal + CNN.  
The procedure is like below.  
1. Do Selective search to input image.  
2. after selective search, There are almost 2000 proposals. and do CNN to the all propsals.  
3. Do SVM for classification and at the same time, Do regression for Bbox.
![image](https://user-images.githubusercontent.com/88817336/150487156-0c8c25ec-3a99-43e2-a4e1-16f57d9bc95c.png)  
![image](https://user-images.githubusercontent.com/88817336/150487408-c44fbaf7-d151-4134-9fa8-c9848f3d40c5.png)  
As you can notice, This costs huge computational power.  

---

* ### SPP-Net  
_He, Kaiming, et al. "Spatial pyramid pooling in deep convolutional networks for visual recognition." IEEE transactions on pattern analysis and machine intelligence 37.9 (2015): 1904-1916._  
  
#### Contribution 1  
There are a few problems on R-CNN, The biggest problem was the input size is fiexed.  
Becuase after CNN, there are features map and those are influenced by input size. The bigger size the more featrues.  
And as we know, after Convolutional layer, there are fully connected layer (fc layer). And those fc layer have to take fixed size.  
So, before this paper, We must keep the specific size for input image.  
What if our image like below?
![image](https://user-images.githubusercontent.com/88817336/150489131-93bdb07b-e732-43aa-9bf3-4b2e3df5fc1a.png)  
Due to these situations, Our model trained something wierd.  
  
In this paper, the author aimed to solve fiex input size problem.  
The method was to insert SPP-Net before fc layer. 
![image](https://user-images.githubusercontent.com/88817336/150489203-6ca68824-77c3-439e-85eb-b5ddb939fe58.png)  
SPP-Net is before fc layer and it makes input feature of SPP-Net have fixed size after Maxpooling.  
![image](https://user-images.githubusercontent.com/88817336/150489245-9cb4f476-c613-4918-a7a5-dc74a2883d4e.png)  
This is the image of SPP-Net. SPP-Net diveds features in to 1,2,4 grids and do maxppoling.  
Then, you can get fixed length vector.  

#### Contribution 2  
R-CNN does CNN to all the proposals after region proposals (Selective search). So it is too slow and consumes big memory.  
The author also focused on this problem.  
So The author suggested that do CNN first not after region proposal.  
This idea is... quite simple. After CNN, There are features. and those are integration of input image. (base on CNN definition)  
So It does CNN only one time. therfore the speed is faster than R-CNN
![image](https://user-images.githubusercontent.com/88817336/150490393-3806d83e-76cb-47f7-b5b3-864728ebf0aa.png)  

---

* ### Fast R-CNN  
_Girshick, Ross. "Fast r-cnn." Proceedings of the IEEE international conference on computer vision. 2015._  
This is the very famous Fast R-CNN. This paper is tough for me...  
  
#### Contribution 1  
Fast R-CNN is like SPP-Net in part of doing CNN first.  
But the problem is sometime SPP-Net proposes wrong regions.  
Because after CNN, the feature contains compressed informations. but sometimes after CNN, There are some useless features.  
So in this paper, The author aim to take the original images and CNN feature at the same time.  
The procedure like below.  
1. Do region proposals to original input image in one side.  
2. And in the other side, Do CNN and get features.
3. After get proposed region, project the region onto features. (So you can use region propsal data, and feature map data.)  
4. Devide selected area into 4 parts and do pooling every parts.  
5. Then you can get fixed length vector for fc layer.  
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

* ### Feature Pyramid Network
_Lin, Tsung-Yi, et al. "Feature pyramid networks for object detection." Proceedings of the IEEE conference on computer vision and pattern recognition. 2017._   
   
Object detection의 큰 문제는 작은물체를 탐지하기 어렵다는것이다. 물론 사람도 당연히 찾기 어렵지만, 생각보다 큰것도 잘 못찾는경우가 많다.  
이를 위해 다양한 연구가 이루어졌는데,   
![image](https://user-images.githubusercontent.com/88817336/151366610-83d001f7-aac4-426a-8e58-e2a48eb711ab.png)   
위의 이미지는 FPN이 이루어지기 전에 이루어졌던 대표적인 방법들이다.   
__(a)__ 같은경우 입력 이미지 자체를 다양한 크기로 resize후 각 이미지에서 물체를 찾는방법이다.  resize를 하게되면 기준 anchor로 sliding window처럼 처음부터 끝까지 찾을때 작은 물체도 잘 찾을수 있게된다. 상당히 직관적이고 간단한방법.  
문제는 여러 사이즈를 만들다보니 많은 memory를 소요하게 되며, sliding window처럼 처음부터 끝까지 훑으므로 여러번의 연산을 해야해서 상당히 큰 computing power가 소요된다. 즉, 비효율적.  
관련 연구로서는 _Zhang, Kaipeng, et al. "Joint face detection and alignment using multitask cascaded convolutional networks." IEEE Signal Processing Letters 23.10 (2016): 1499-1503._ 가 있다.
</br>
__(b)__ 같은경우 우리가 알고있는 일반적인 CNN을 활용한 느낌이다. CNN을 돌려 나온 최종 단계의 feature로 object detection을 수행하는 방법.   
문제점은 CNN은 보통 Convolution -> pooling의 반복으로 점점 width, height는 줄어들면서 channel이 깊어지게 된다.   
이미지를 점점 추상화해서 저장하게되는것인데 그렇게되면 작은 이미지같은경우 짜잘한 노이즈와 같이 소멸될 확률이 높아진다.  
또한, Backbone모델의 영향을 많이 받게되므로, 모델의 중요성이 더 크다고 볼수있다.  
관련 연구로서는 그 이름도 유명한 YOLO다.   
_Redmon, Joseph, et al. "You only look once: Unified, real-time object detection." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016._  
</br>
__(c)__ 같은경우 조금더 머리를 쓴 방법인데, Covolutional layer를 통과하면서 나온 각 feature들마다 predict를 돌리는방법이다.  
즉, 변하는 과정을 모두다 저장하여 그것들에 대해 predict를 하겠다는 뜻.  
SSD는 1 stage detection으로서 상당히 빠르고 좋으며 Yolo보다 더 작은것을 잘찾는다.  
![image](https://user-images.githubusercontent.com/88817336/151375176-5afe5452-6b03-41d2-9235-e0449e8eae61.png)   
(SSD와 YOLO 구조 차이 사진)  
__(d)__ 가 Feature pyramid network의 방법이다.  
(c)의 방법도 있으나, 다른 방법으로 접근한것이 (d)의 방법.  
![image](https://user-images.githubusercontent.com/88817336/151378180-5738b027-d984-4237-afb4-6caeb32740d5.png)  

방법은 간단한데, feature를 추출할수록 일정하게 feature의 사이즈가 줄어드는데, 그 줄어든만큼 feature에 곱해주어 보관한다음, 마지막 feature까지 추출이 되면 마지막 feature도 원래 이미지 크기만큼 업샘플링하여 보관한 사진들을 전부 더한다.   
![image](https://user-images.githubusercontent.com/88817336/151378220-bac2bdee-afbd-4bae-b161-ef957c36388d.png)  
(업샘플링 사진)

그러면 각 feature들의 정보를 전부다 더한 하나의 이미지가 만들어지고 그것에 detection을 돌리는 방법.  
feature의 정보들을 모두다 버리지도 않으면서, 업샘플링을 해서 원래 크기의 이미지를 만들어 detection을 돌리게되어 성능이 잘나온다.  

