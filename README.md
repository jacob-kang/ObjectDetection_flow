# The Obeject detection papers
  
##  영어로 변환중 / Translating in English  

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
![image](https://user-images.githubusercontent.com/88817336/150492142-3a2de41a-5ebf-4da2-9955-cc94743a78b0.png)  
</br>
For instance, let's assume that out input image is a dog.  
First, Do region proposal (Selective search etc.) to the image and the region proposal is the result.  
Second (At the same time), Do CNN with VGG backbone to the image and the features comes out as results.  
Third, You can guess (know?) the area of proposal in featre approximately (in case of just watching). then project the proposal area to feature map.  
Last, Divide projected area into 4 parts and do pooling. Then you get the 2 X 2 size feature!  
</br>

#### Contribution 2  

The problem of SPP-Net is multi-stage pipeline. That means SPP-Net needs to do at least 2 stage compuational operation for making a result. so, This is overheavy.  
SPP-Net does SVM, Bbox regression after CNN. But, unfortunately, These are not trained altogether since SVM, Regression are differnt models after CNN.  
The author focused on this probelm. So he suggested that Fast R-CNN make SVM, Regression be a multitask loss.  
After pooling, The features are used for SVM, Regression at the same time. So it is shared and could be end-to-end learnable.  
![image](https://user-images.githubusercontent.com/88817336/150493542-1ca033e9-bf57-40c3-90b5-9fb1e46dcf87.png)  
This image is multi-task loss fomula. (But, I'm bad at Math. XD)

---

* ### Faster R-CNN  
_Ren, Shaoqing, et al. "Faster r-cnn: Towards real-time object detection with region proposal networks." Advances in neural information processing systems 28 (2015): 91-99._  
</br>
In this paper, There would be small but powerful technique. For me, It is hard...  
In my think, Faster R-CNN looks at the problems with different perspective from Fast R-CNN.  
</br>
#### Contribution 1  
One of the problem of R-CNN is when it uses Selective search as Region proposal, The Selective search is used as an external modlue. Since it is an external, it is loaded on CPU. Therefore the speed was slow. For example, during the inference time 2.3 seconds, the 2 seconds was used for Region proposal. And the rest 0.3 seconds used for CNN.  
As it leads Bottle neck, it is highly inefficient.  
For this problems, Faster R-CNN uses Region proposal not from external module but its own network that is made as a new region proposal(RPN).  
With RPN, It is available to use GPU computing and makes high speed openration.  
![image](https://user-images.githubusercontent.com/88817336/150667961-425c1000-2ca9-46a0-9040-a21ffd68d74e.png)  
As you can see the structure of Faster R-CNN from the image, The structure is very similar to Fast R-CNN only differs from whether RPN is in or not.  
The difference is just the image for projection to feature map (change from result of Selective search to result of RPN)  
</br>
#### Contribution 2  
There is another big technique in RPN.  
The method is different from Selective search. RPN makes various size and shape of anchor boxes rely on the features from CNN (Because features is small image of original image as the basic onf CNN). The anchor boxes let RPN find small objects.  
Let's assume that there is 8 X 8 feature. by inserting 9 anchors to each single feature, It finds various size and shape objcets and it can find big objects by choosing multiple features.  
![image](https://user-images.githubusercontent.com/88817336/150667909-75660e76-74f5-444d-9dec-b81eb4f5fcbe.png)  
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

---

* ### Cascade R-CNN  
_ Cai, Zhaowei, and Nuno Vasconcelos. "Cascade r-cnn: Delving into high quality object detection." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018. _  

Understanding...   

---

* ### SE-Net (Squeeze and Excitation)  
_ Hu, Jie, Li Shen, and Gang Sun. "Squeeze-and-excitation networks." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018. _  
</br>
Note : From this, It may different flow from above. Because, I'm currently studying USV project. and previous research in USV, there was SE-Net. So I decieded to study SE-Net and now review here.  
</br>
This SE-Net is from attention mechanism. Briefly to say, The attention mechanism is from NLP like when NLP needs to focus some special words. (I would skip describing the attetntion mechanism.)  
Anyway, The attention mechasim came to computer vision from NLP. And the mechanism is quite simple but powerful.  
The motivation is (I guess) when we see an image, We tend to focus on somthing importance or interesting. That means there could be importance part in image.  
The author who write this paper focused on this.  
</br>
![image](https://user-images.githubusercontent.com/88817336/152290151-52ac6e60-0e83-415e-9b5e-86b75ef06f72.png)   
Like the image, There are importance priorities among featrues. and SE-Net sets the weight of each features.  
The procedure is like below.  
1. Do CNN.  
2. Add featrues that come out from CNN to SE-Net.  
3. The feature forms is probably H X W X C. and need to convert 1 X 1 X C. Due to ranking those feature, The feature forms should be vector.  
4. Convert H X W X C feature to 1 X 1 X C by Global average pooling.  
5. 
