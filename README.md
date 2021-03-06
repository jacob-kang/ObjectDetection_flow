# The Obeject detection papers review

This Read Me is for catching object detection flow (+ English study).  
The flow procedure is from top to bottom and it consists of my paper understanding + some informations from blogs.  
There __may__ be misunderstanding or typo or etc.  
_Made by Jacob Jisu Kang._  

Note. In this Read Me, I skip some basic knowledges such as Perceptron, FFNN, Backpropagation etc.  

---  
Let's begin!  
</br>
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
For instance, let's assume that input image is a dog.  
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
</br>
One of the biggest problems in object detection is to detect small objects. Of course, It is hard for human. Nevertheless it is big in our thing, models often don't find.  
There are various researches for this,  
![image](https://user-images.githubusercontent.com/88817336/151366610-83d001f7-aac4-426a-8e58-e2a48eb711ab.png)   
This image shows some representative reseraches.  
In case of __(a)__ , This is to find objects by resizing the input images into various size. After resizng, It is easy to find small objects with anchor box by use of sliding window which finds from start to end. This is quite eidetic and simple.  
But the problem is making various size images costs much memory and like the way sliding window, it operates too many times. Therefore it costs numerous comuting power. Simply to say, inefficient.  
The relate work is  _Zhang, Kaipeng, et al. "Joint face detection and alignment using multitask cascaded convolutional networks." IEEE Signal Processing Letters 23.10 (2016): 1499-1503._  
</br>  

In case of __(b)__ , This is the way the traditional use of CNN as we know. Do object detection to the last level features from CNN.  
Usually traditional CNN usage is repetition of Covolution -> Pooling. and this makes the feature have less size of width,height and deep channel.
The problem is the repetition make the image be abstracted. that means small object could be disappeared like a noise. 
And since it is affected from backbone model a lot, It is more likely that the model is much important.  
The relate work is ,the very famous, YOLO.  
_Redmon, Joseph, et al. "You only look once: Unified, real-time object detection." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016._  
</br>  
In case of __(c)__ , This is smarter(?)way which is to predict every single feature after Convolutional layers.  
That is, It saves all the changing process then predict to those. (Am I describing correctly?? Oh my English...)  
Single Shot multibox Detector (SSD) uses this structure.  
SSD is 1 stage detection as well. it is quite fast and good at looking for small object than YOLO.  
![image](https://user-images.githubusercontent.com/88817336/151375176-5afe5452-6b03-41d2-9235-e0449e8eae61.png)   
(The image of structure of SSD, YOLO)  
_Liu, Wei, et al. "Ssd: Single shot multibox detector." European conference on computer vision. Springer, Cham, 2016._  
</br>  
And Last. __(d)__ is the method of Feature Pyramid Network.  
The idea is quite simple.  
Every feature extracting, the size of feature decreases in constant portion. Then multiply the protion to the decreased feature (like up-sampling). then you can get the same size of feature.  
And overlap those images. Then you can get images which have all the features and is same size.  
![image](https://user-images.githubusercontent.com/88817336/151378180-5738b027-d984-4237-afb4-6caeb32740d5.png)  
This is the picture what I wrote.  
</br>  
The upsamling mehtod is like below. Neares Neighbor. There are many method to upsample image. but The author said he just choosed the way as it is simple.  
![image](https://user-images.githubusercontent.com/88817336/151378220-bac2bdee-afbd-4bae-b161-ef957c36388d.png)  
(Upsampling method)  

---

* ### Cascade R-CNN  
_ Cai, Zhaowei, and Nuno Vasconcelos. "Cascade r-cnn: Delving into high quality object detection." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018. _  
</br>  
In object detection, The criterion og intersection over union(IoU) is required to define postive and negative.  
Usually, 0.5 IoU is used as it works empirically well. But, sometimes, 0.5 IoU make results some 'close' false positive which means the results is close to the right answer but not correct.  
![image](https://user-images.githubusercontent.com/88817336/152478647-647b13e8-be24-4550-9c49-ef81355f2379.png)  
Like this, (a) has quite many false postives. but (b) has less than (a) and seems like more accurate.  
</br>
The author mentioned 2 problems. One is in case of high IoU, some positive sample is disappeared therefore it becomes overfitting. The other is when it trains and inferences, The IoU has been changed due to the metrics. For example, In COCO Metrics, the IoU changes from 0.5 to 0.95 step 0.05.  
So There is kind of trade-off between accuracy and IoU. and the Author tried to catch both of them with brilliant idea.  
![image](https://user-images.githubusercontent.com/88817336/152478910-a544f1ab-1eb8-4b29-87cf-cb7c9ffe37d0.png)  
This is a grapth about results of IoU and AP. (For me, It was terribly hard to understand.)  
(c) is about bbox regressor performance. the X axis is the input IoU value A.K.A hyper parameter. the Y axis is the ouput IoU of the trained model with specifically fixed value.  
![image](https://user-images.githubusercontent.com/88817336/152640868-f3236cb7-250b-435c-85e1-1948bdc1951a.png)  
This is the values which a model trained with.  
</br>
Here is the example. this may be helpful.  
There is a model which is trained IoU threshold 0.6 (The green one). and the author tested with this trained model to the testset. the author set the input IoU prameter as 0.6. and the result performance was 0.775.  
![image](https://user-images.githubusercontent.com/88817336/152641130-d566f529-e101-470e-9290-90cf94e6dc7c.png)  
Hopefully, You may catch the sense.  
As I was saying, In (c), The 0.5 trained model perfroms well near 0.5 IoU parameter. and the 0.7 trained model performs well near 0.95 IoU parameter.  
That could mean, A model which is trained specific value tends to be robust to specific section of IoU. and also the proposed bbox is not concrete, The IoU 0.5 model works well and the proposed bbox is quite concrete, The IoU 0.7 model works better than 0.5 model.  
So we can make a conclusion that If the propsoed region is concrete and near GT, The detector performs better. And with the almost exactly performing region proposal, We can get high AP with trained with high value IoU. So Hight performing region propsal and high IoU value trained model, We can achive high AP without 'close' false positive. Of course, it is natural consequence.  
So In the graph, We can get high quality proposals in 0.5 without overfitting (deu to proposing lots of positive samples) and We can get high AP with 0.7.  
</br>
(d) is the detector performance (= classification). and this image shows that the higher input of IoU parameter, the lower AP. So the model trained by high IoU value is not the perfect job.  

Anyway, The author focused on the problem then suggested multi-stage R-CNN form (= Cascade R-CNN)  
![image](https://user-images.githubusercontent.com/88817336/152669008-29bce7db-f45c-43d6-b63c-82bd484ae062.png)  
(a) is the faster R-CNN. You can detail in the above.  
</br>
(b) is quite similar with Cascade R-CNN but it uses same network head. And use it iteratively at inference.  
First of all, The rigion proposal B0 is not concrete but abstract and bigger than Ground Truth. And after projection and pooling, the H1 model predicts more concrete than B0.  
That is the principle of (b). It works and It achives higher accuracy than (a) but the increase is not outstanding.  
(c) is Integral Loss. It uses multi models. And as you can see, It is kind of ensemble.  
After CNN and ROI pooling, It uses 3 models and calcurates the losses integrally. But the problem is this method doesn't focus on the region proposal. It's not about what the author mentioned above.  
</br>
(d) is the Cascade R-CNN.
The procedure is like below.  
1. Do CNN  
2. Do ROI pooling with B0  
3. Do classificaiotn and bbox regression. (The B1 is more concrete and specifie area than B0)  
4. Do ROI pooling with B1 (So The ROI pooling area is more accurate)  
5. Do classification and bbox regression. (repeatition like iamge)  

The method is quite simple but powerful. and the author mentioned that more than 3 stage decreases the accuracy. and it uses not only train phase but also inference phase unlike (b).  

---

Note : From this, It may different flow from above. Because, I'm currently studying USV project. and previous research in USV, there was SE-Net. So I decieded to study SE-Net and now review here.  

---

* ### Residual Attention Network for Image Classification  
_Wang, Fei, et al. "Residual attention network for image classification." Proceedings of the IEEE conference on computer vision and pattern recognition. 2017._  
</br>
This Residual Attention Network(RAN) is from attention mechanism. Briefly to say, The attention mechanism is from NLP like when NLP needs to focus some special words. (I would skip describing the attetntion mechanism.)  
Anyway, The attention mechasim came to computer vision from NLP. And the mechanism is quite simple but powerful.  
The motivation is (I guess) when we see an image, We tend to focus on somthing importance or interesting. That means there could be importance part in image.  
The author focused on this.  
This is Residual "Attention" network. Very similar to ResNet.  
In this paper, The author focused on pixel-level relationships. The idea is very simple.  
With mask information, Give depedencies and priority to the image.  
![image](https://user-images.githubusercontent.com/88817336/152731412-f67d674f-8e77-416a-802b-4b8e9c473280.png)  
Let's look at this image.  
In low-level featrue, The soft attention mask has more attetions to wide area.  
BUt In high-level feature, The soft attention mask has more attentions to narrow area.  
So after extract soft attention mask, The author projected the spatial area to the feature then she/he got the feature after mask.  
In low-level, The Feature after mask has less attention to the background but wide area in hot-air balloon.  
In high-level, The feature after mask has less attention to the background but narrow are in hot-air balloon as well.  
</br>
![image](https://user-images.githubusercontent.com/88817336/152734167-21b88b5f-555f-4afb-9f5e-42862a071bfe.png)  
The author suggested "attention module". The structure is in this image.  
It is configure as two branches, Soft mask branch and Trunk branch.  
The Trunk branch is to extract feature map with backbone model (The author mentioned that he recommended using SOTA model)  
The soft Mask branch is to extract mask map with down sample, up sample. The down sample is used with max pooling and the up sample is used with linear interpolation. With these two strategies, The only powerful features remains and little noise or meaningless featrue disappear. So it can pull out important feature and be robust from noise.  
</br>
![image](https://user-images.githubusercontent.com/88817336/152736943-e9822012-99b8-4fe1-a3c0-769324376e56.png)  
And like the author mentioned above, Each level of feature has different mask and informations.  
So the author said the "Residual Attention Network" structure consists of more than 1 attention parts. because, each attetion module tends to catch one feature only.  
</br>
But as you may feel, It consists of many residual block as the author mentiond "each attetion module tends to catch one feature only". Therefore, It costs many memory and computational power.  

---

* ### SE-Net (Squeeze and Excitation)  
_ Hu, Jie, Li Shen, and Gang Sun. "Squeeze-and-excitation networks." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018. _  
</br>
Although Residual Attention Network (RAN) is powerful and more likely be same with "Attention", it costs a lot.  
In this paper, The author focused on feature level attention.  
RAN uses mask then do pixel-wise operation. But SE-Net uses feature dependencies.  
The struchture is like below.  
![image](https://user-images.githubusercontent.com/88817336/152290151-52ac6e60-0e83-415e-9b5e-86b75ef06f72.png)   
Like the image, There are importance priorities among featrues. and SE-Net sets the weight of each features.  
The procedure is like below.  
1. Do CNN.  
2. Add featrues that come out from CNN to SE-Net.  
3. The feature forms is probably H X W X C. and need to convert 1 X 1 X C. Due to ranking those feature, The feature forms should be vector.  
4. Convert H X W X C feature to 1 X 1 X C by Global average pooling. (Squeeze)  
5. Calcurate the dependencies by fc layer and sigmoid (Because only with repetition of fc layer and sigmoid, It can express non-linearity). (Excitation)  
![image](https://user-images.githubusercontent.com/88817336/152670992-6c313327-d069-4d72-bba0-35f974474e4f.png)  
6. Multyply to the feature map  

With this SE Net, You can recalibrate your feature map. and this is easy to attach any model simply and doesn't have many parameter so the time-complexity rises a bit.  
![image](https://user-images.githubusercontent.com/88817336/152671038-b5ac7bb4-e722-40ed-8c12-8c428bb50651.png)  
</br>
Unlike Residual Attention Network for Image Classification

---

* ### BAM: Bottleneck Attention Module  
_Park, Jongchan, et al. "Bam: Bottleneck attention module." arXiv preprint arXiv:1807.06514 (2018)._  
</br>
The problem of Residua Attention Network(RAN) is costing too much computational power. As it cosists of more than 2 residual block, Time complexity rises.  
And The SE-Net focuses on only channel depedencies. And The residual network focuses on pixel-level dependencies.  
So the author mentioned that not only channel attention but also spatial attention is important. and The attention module should be light-weight and easy to attach to model. But, RAN is not.  

BAM can be explained with just one image and one senteces. SE-Net + Spatial attention.  
![image](https://user-images.githubusercontent.com/88817336/152753613-62c78791-96d1-4c7d-967c-64d1bb472657.png)  
The Channel attention procedure is almost same with SE-Net.
1. Do Global average pooling. (Flatten)
2. Fully connect each 1X1 features and reduce the channel with parameter r (The author said the optimal value r is **16**. it is experimental.)
3. And fully connect the features and increase the channel to the before feature which not reduce yet.  
4. Channel attetion completed.  
The procedure is very similar with SE-Net. just where ther is sigmoid or not. (The sigmoid is after concatenateing of spatial and channel attention)  
</br>
The Spatial attention consists of the dilated convolution. (Reference:_Fisher Yu and Vladlen Koltun. Multi-scale context aggregation by dilated convolutions.
2015._)  
The author chooses the dillated convolution as it enlarges the recpetive fields with high effciency. And the author said he/she ovserved the dilated convolution distructs a more effective spatial map than the standard convolution.  
To get the Spatial attention procedure, first of all, Do 1X1 convolution to control feature channel.  
</br>
Tips. When you see some paper, There might be lots of 1x1 convolution operation.  
Becuase 1x1 convolution pros are less parameter, non-linearity, controlling channels.  

![image](https://user-images.githubusercontent.com/88817336/153346940-accd2895-076b-490e-bc3a-79dcaa8ccd89.png)  
</br>
As I was saying about procedure of spatial attention, after 1X1 convolution, do dilated convolution with value **d = 4**. it is experimental as well.  
Then reduce the channel to 1 channel by 1x1 convolution. So you can get H x W x 1 shape Spatial attention and 1 x 1 x C shape.  
We need to combine those two. So the author choosed element-wise as combining mehtod to expand the shape to H x W x C. (This is empirically selected)  
after that, take sigmoid to get the range from 0 to 1. then you can get the bam attention and multipy it to origin featre F.  
These are the whole procedure. This is quite simple but powerful method.  

![image](https://user-images.githubusercontent.com/88817336/153352602-95af0118-64df-4c64-b545-6eb72b2134d2.png)  

With this, The author mentioned that BAM works better than SE-Net.  
And here is the comparing table between BAM and SE-Net.  
![image](https://user-images.githubusercontent.com/88817336/153548173-ef6ea4c8-a4cd-4ee5-a5e3-0dab4f76b655.png)


---

* ### CBAM: Convolutional Block Attention Module  
_Woo, Sanghyun, et al. "Cbam: Convolutional block attention module." Proceedings of the European conference on computer vision (ECCV). 2018._  
</br>
This CBAM is after work of BAM. Of course, The corresponding author is same person.  
Anyway, in this paper, the author said very very similary to BAM. The CBAM can be attached any strucure and just a few parameter is added but the performance significantly rised.  
In BAM, The structures are spatial attention and channel attention. and operate those at the same time and combine them together to determine attention priority.  
But CBAM is a little bit different.  
![image](https://user-images.githubusercontent.com/88817336/153548887-dc4659ff-03d3-4e3f-8aaf-ff3249accc0b.png)  
The procedure is like above. CBAM gets channel attention first then gets spatial attention with the channel attention. (For me, I think the channel attention and spatial attention is hierarchical. So channel attention is first and next is spatial attention)  
The authrod suggested that spatial attention focuses on 'where' is an informative part and channel attention focuses on 'what' is an informative. and they are complementary.  
And by not only just using average pooling but also using max pooling, the author said he/she doesn't miss the informative information.  
![image](https://user-images.githubusercontent.com/88817336/153549252-713b265e-3a3e-4cd8-9be0-3817543c0b90.png)  
The channel attention procedure is like below.  
1. Do max pooling and average pooling.
2. Insert to the Multi-layer Perceptron (1 hidden layer) and share it. (To reduce the parameter overhead, the hidden activation size is C/r (same like BAM))  
3. After shared network applied to the discriptor (The max pooled feature and average pooled feature), Combine them by element-wise summation.  
4. Go throw sigmoid then The channel attention completed.  
</br>
Last the spatial attention.  

![image](https://user-images.githubusercontent.com/88817336/153550362-888cd655-c07d-4ca4-a5d5-aace8a6b6c17.png)  
The basic idea is very similar to channel attention.  
The spatial procedure is like below.
1. Do max pooling and average pooling.
2. Concatenate them toegether and do cnn.
3. Go throw sigmoid then The spatial attention completed.
</br>
With this, CBAM also works better than SE-Net almost all task (In the paper, Classification)  
And it also easy to attach to the model.  

![image](https://user-images.githubusercontent.com/88817336/153551868-4e85c5f6-5918-43d4-96ca-b9ece8ec49ff.png)  

With grad-CAM, We can see where the model looks.  
![image](https://user-images.githubusercontent.com/88817336/153552005-11504bf2-c0fa-4c6e-b633-b281cc6cb29e.png)  

The model which includes CBAM tends to look objects specifically.
