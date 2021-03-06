|논문명 | On-Board Object Detection: Multicue, Multimodal, and Multiview Random Forest of Local Experts|
| --- | --- |
| 저자\(소속\) | \(\) |
| 학회/년도 | IEEE Transactions on Cybernetics 2016, [논문](http://ieeexplore.ieee.org/document/7533479/) |
| 키워드 |EarlyFusion,  SVM & 랜덤포레스트 이용(딥러닝 기반X) |
| 데이터셋(센서)/모델 |KITTI |
| 참고 | |
| 코드 | |

# EarlyFusion 

여러 정보`(multicue, multimodality, and strong MV classifier)`들이 합쳐 졌을때 어떤 영향을 미치는지 살펴 보겠다 `we provide an extensive evaluation that gives insight into how each of these aspects (multicue, multimodality, and strong MV classifier) affect accuracy both individually and when integrated together`

사용 센서 :  fusion of RGB and depth maps(Lidar)

## 1. Introducion 

현실세계에서 Detector의 성능 향상을 위해서는 다음이 중요하다. `In order to obtain a detector that successfully operates under realistic conditions, it becomes critical to exploit sources of information along three orthogonal axis`
- 1) the integration of multiple feature cues (contours, texture, etc.); 
- 2) the fusion of multiple image modalities (color, depth, etc.); 
- 3) the use of multiple views (frontal, lateral, etc.) of the object 
by learning a strong classifier that accommodates for both different 3-D points of view and multiple flexible articulations.

![](https://i.imgur.com/Kqp3Cl6.png)
```
Fig. 1. General scheme: from RGB images and LIDAR data to object detection. 
- RGB images and LIDAR data synchronized for multimodal representation.
- Multimodal representation based on RGB images and dense depth maps (obtained from LIDAR sparse data). 
- Multicue feature extraction over the multimodal representation. 
- MV detection of different objects.
```

- 제안 방식은 그림1에서 보는 바와 같이 성능 평가 할것이다. `The proposed method (general scheme in Fig. 1) will be evaluated in key objects for autonomous and semi-autonomous vehicles such as pedestrians, cyclists, and cars.`

### 1.1 integrate different cues

- In order to integrate different cues we use 
	- **Histogram of oriented gradients (HOG)** [9], that provides a good description of the object contours 
	- **Local binary pattern (LBP)** [10] as texture-based feature. 

- [11-2009]-[13-2011]에 따르면 위 두 특징들은 **상호 보안적**인 요소로 **Fusion시** 좋은 효과가 난다. `These two types of features provide complementary information and the fusion of both types of features has been seen to boost the performance of either feature separately [11]–[13]. `

- [9-2005]에 따르면 다른 종류의 **gradient-based features**와 그 **특징의 spatial distribution**을 쓰는것은 좋은 표현력을 가지게 된다. `From the seminal work of Dalal and Triggs [9], it has been seen that using different types of gradient-based features and their spatial distribution,such as in the HOG descriptor [9] provides a distinctive representation of both humans and other objects classes. `

- [14-2009]의 연구에 따러면 제안한 **integral channel features**여러 종류의  low-level features들을 합치게 되면 spatial distribution에 대한 유연성 있는 representation 이 된다. `However, there exist in the literature other approaches such the integral channel features proposed by Dollár et al. [14] that allows integrating multiple kinds of low-level features (such as the gradient orientation over the intensity and LUV images, extracted from a large number of local windows of different sizes and at multiple positions), allowing for a flexible representation of the spatial distribution. `
	- low-level features  = gradient orientation over the intensity and LUV images, extracted from a large number of local windows of different sizes and at multiple positions

-  In [15-2013] and [16-2011], it has been seen that including color boosts the performance significantly, being this type of feature complementary to the ones we used in this paper. 

- **Context features**를 사용하는 방법도 제안 되었다. `Context features have also been seen to aid [17], [18]and could be easily integrated as well. `

- 지역특징`(local features)`에 대한 spatial pooling에 대한 대안들도 연구 되었다. `Exploring alternative types of spatial pooling of the local features is also beneficial as shown in [6] and is also complementary to the approach used in this paper.`

### 1.2 integrate multiple image modalities,

여러 이미지 모딜라티의 결합을 위해 본 논문에서는 **depth maps**과 **visible spectrum images**를 퓨젼 하였다. ` In order to integrate multiple image modalities, we considered the fusion of dense depth maps with visible spectrum images. `

- 저가의 키넥트등의 보급으로 Depth 정보 사용이 주목 받고 있다. `The use of depth information has gained attention, thanks to the appearance of cheap sensors such as the one inKinect, which provides a dense depth map registered with an RGB image (RGB-D). `
	- 하지만 키넥트 류의 센서는 반경 4M이고 실외에서는 정확도가 안 좋다. `However, the sensor of Kinect has a maximum range of approximately 4 m and is not very reliable in outdoor scenes, thus having limited applicability for objects detection in on-board sequences. `
	- 반면에 LiDAR 센서는 반경 50M이고 실외에서의 정확도도 좋다. `On the other hand, lightdetection and ranging (LIDAR) sensors such as the VelodyneHDL-64E have a maximum range of up to 50 m and are appropriate for outdoor scenarios. `

- In this paper, we explore the fusion of **dense depth maps** `(obtained based on the sparse cloud of points)` with **RGB images.** 

[19]에 따르면 각 모달리티는 아래 두 방법으로 퓨전 가능 하다. `Following [19], the information provided by each modality can be fused using `
- either an **early-fusion scheme**, i.e., at the **feature level**, 
- or a **late fusion scheme**, i.e., at the **decision level**. 

```
[19] D. L. Hall and J. Llinas, “An introduction to multisensor data fusion,” Proc. IEEE, vol. 85, no. 1, pp. 6–23, Jan. 1997.
```

- 본 논문에서는  early fusion`(where descriptors from each modality are concatenated)`을 사용 하여 성능 향상을 보였다. `In this paper, using an early fusion scheme provided the best results.`


### 1.3 관련 연구 논문들 

- 멀티 모달을 이용한 Object detection은 [1]에서 연구가 많이 되었으며, 특히 2D 레이져 스캐너를 이용를 이용한 연구는 [20][21]에서 많이 연구 되었다. `Object detection based on data coming from multiple modalities has been a relatively active topic of study [1], and in particular the use of 2-D laser scanners and visible spectrum images has been studied in several works, for instance [20] and [21]. `

- 최근에 들어서야 LiDAR를 이용한 연구가 진행 되었따. `Only recently authors are starting to study the impact of high-definition 3-D LIDAR [20]–[26].`


- 대부분의 연구는 3D 포인트 클라우드에서 정보 추출을 위한 **descriptors** 제안에 초점을 두었다. ` Most of these works propose specific descriptors for extracting information directly from the 3-D cloud of points [20], [22]–[26]. `

```
[20] L. Spinello, K. O. Arras, R. Triebel, and R. Siegwart, “A layered approach to people detection in 3D range data,” in Proc. AAAI, Atlanta, GA, USA, 2010, pp. 1625–1630.
```

- 일반적인 접근법은 물체 탐지를 **3D 포인트 클라우드**와 **visible spectrum images**에서 독립적으로 시행한후 이를 적절한 알고리즘으로 합치는 방법 이다.  `A common approach is to detect objects independently in the 3-D cloud of points and in the visible spectrum images, and then combining the detections using an appropriate strategy [22], [23], [26]. `

```
[22] K. Kidono, T. Miyasaka, A. Watanabe, T. Naito, and J. Miura, “Pedestrian recognition using high-definition LIDAR,” in Proc. IV, Baden-Baden, Germany, 2011, pp. 405–410
[23] K. Kidono, T. Naito, and J. Miura, “Reliable pedestrian recognition combining high-definition LIDAR and vision data,” in Proc. ITSC, Anchorage, AK, USA, 2012, pp. 1783–1788.
[26] J. Xu, K. Kim, Z. Zhang, H.-W. Chen, and Y. Owechko, “2D/3D sensor exploitation and fusion for enhanced object detection,” in Proc. CVPR, Columbus, OH, USA, 2014, pp. 778–784.
```


- [21]에서 제안한 방법은 Following the steps of [21], 
	- **dense depth maps** are obtained by first registering the 3-D cloud of points captured by a Velodyne sensor with the RGB image captured with the camera, 
	- and then **interpolating** the resulting sparse set of pixels to obtain a dense map where each pixel has an associated depth value. 
- Given this map, 2-D descriptors in the literature can be extracted in order to obtain a highly distinctive object representation. 

- 본 논문과 [21]의 다른점 
	- This paper differs from [21] in that we use **multiple descriptors** and adapt them to have a good performance in dense depth images. 
	- [21]은 late fusion을 쓰지만, 본 논문은 두개 모두 테스트 하였다. `While [21]employs a late fusion scheme, in our experimental analysis we evaluate both early and late fusion approaches in the given multicue and multimodality framework.`

```
[21] C. Premebida, J. Carreira, J. Batista, and U. Nunes, “Pedestrian detection combining RGB and dense LIDAR data,” in Proc. IROS, Chicago, IL, USA, 2014, pp. 4112–4117
```

- flexible한 모델을 만드는건 어려운 일이다. `Learning a model flexible enough for dealing with multiple views and multiple positions of an articulated object is a hard task for a holistic classifier. `
	- 이를 위해 **random forests (RFs) of local experts **를 이용하였다. `In order to fulfill this aspect we make use of random forests (RFs) of local experts [27], which has a similar expressive power than the popular DPMs [28] and less computational complexity.`
	- 이 방법에서는 각 Tree는 local experts에 대한 서로다른 설정을 제공하며 각 local expert이 part model.의 역할을 수행 한다. `In this method, each tree of the forest provides a different configuration of local experts, where each local expert takes the role of a part model. `
	- At learning time, each tree learns one of the characteristic configurations of local patches, thus accommodating for different flexible articulations occurring in the training set. 
	- In [27], the RF approach consistently outperformed DPM. 
	- RF의 장점은 : `An advantage of the RF method is that only a single descriptor needs to be extracted for the whole window,and each local expert reuses the part of the descriptor that corresponds to the spatial region assigned to it. `
	- 연삭부하도 적다 .`Its computational cost is further significantly reduced by applying a soft cascade, operating in close to real time. `
	- DPM과 반대로 오리지널 RF는 Contrary to the DPM,the original RF method learns a single model, thus not considering different viewpoints separately. 

- 본 논문에서는 위 방식을 확장 하였다. In this paper, we extend this method to learn multiple models, one for each 3-D pose,and evaluate both the original single model approach and the multi-model approach. 

- 많은 논문들이  **local detectors**나 **multiple local patches**를 합치는 방법을 제안 하였다. `Several authors have proposed methods for combining local detectors [28], [29] and multiple local patches [30]–[32]. `

- [33]역시 RF를 이용하였다. `The method in [33] also makes use of RF with local classifiers at the node level, although it requires to extract many complex region-based descriptors, making it computationally more demanding than [27].`

```
[33] B. Yao, A. Khosla, and L. Fei-Fei, “Combining randomization and discrimination for fine-grained image categorization,” in Proc. CVPR, Providence, RI, USA, 2011, pp. 1577–1584
```

#### 1.4 본 논문과 유사한 연구 

- [13]논문은 아래 내용을 합쳤으며 본 논문 방식과 유사 하다. `Most relevant to this paper is the approach where Enzweiler and Gavrila [13] combined `
	- multiple views (front,left, back, and right), 
	- modalities (luminance, depth based onstereo, and optical flow), 
	- features (HOG and LBP). 

- 본 논문과 [13]의 차이는 아래와 같다. `The main differences between [13] and this paper are as follows.`
	- 1) In order to complement RGB information, we make use of a **sensor modality**, **high-definition 3-D LIDAR**, which has received relatively little attention in pedestrian detection until now, but it is being used for autonomous driving.
	- 2) While [13] makes use of an **holistic classifier**, we make use of a more expressive **patch-based model**.
	- 3) In [13], multiples cues are combined following **late fusion** style, while we consider also **early-fusion**, which,in fact, gives better results in our framework.

```
[13] M. Enzweiler and D. M. Gavrila, “A multilevel mixture-of-experts framework for pedestrian classification,” IEEE Trans. Image Process., vol. 20, no. 10, pp. 2967–2979, Oct. 2011.
```

- Our analysis reveals that, although all the aforementioned components `[the use of multiple feature cues, multiple modalities and a strong multiview (MV) classifier] `are important, 
	- the fusion of **visible spectrum** and **depth information** allows to boost the accuracy significantly by a large margin. 

## 2. MULTIVIEW RGBD-RF FOR OBJECT DETECTION


### 2.1  Multicue Feature Representation

### 2.2 Multimodal Image Fusion

### 2.3 Multiview Classifier

### 2.4 Object Model

- In this paper, we focus on two different models: 
	- 1) **holistic**, where the object descriptor takes into account the candidate detection window as a whole and 
	- 2) a **patch-based** one where random subsets of patches are used for generating differentobject configurations which are further assembled to form the overall object model. 

#### A. holistic model

-  As holistic model we use the **linear SVM** (linSVM) classifier 
	- - which has a good compromisebetween computation time and accuracy. 

This model learns the max-margin hyper plane that better splits the positive and negative samples in the descriptor space (either HOG, LBP, or HOGLBP in our case). 

#### B.  patch-based
- As patch-based model we use our **RF of local experts**.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzMjUzNjk2Nl19
-->