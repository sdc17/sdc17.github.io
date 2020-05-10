# A Note for Medical Image Segmentation

### An Overview

##### Time Span

from 2015 U-Net to 2020

83 papers with available codes

##### Subfields

* Lesion(病灶) Segmentation
* Brain Segmentation
* 3D Medical Image Segmentation
* Retinal Vessel(视网膜血管) Segmentation
* Cell Segmentation
* Lung Nodule(肺结节) Segmentation
* Electron Microscopy Image Segmentation
* ...

##### Trunk

Mainly a series of **U-Net derived models**

* Reconstruct some parts of U-Net architecture
* Cooperate with other network structures
* More precise loss function
* Data preprocess or postprocess
* ...

#####  from other image segmentation task

* demands higher accuracy
  * Segmenting lesions(病灶) or abnormalities in medical images demands a higher level
    of accuracy than what is desired in natural images.

* imperfect dataset, expensive to acquire
  * **scarce**(稀缺) annotations. limited annotated data is available for training
  * **weak** annotations.  noisy annotations, or image-level annotations

##### Solutions

* Road 1, create new models with higher accuracy and robustness, mainly derived from U-Net.

* Road 2, tackle imperfect dataset

![image-20200422164627618](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422164627618.png)

* Scarce annotations

  * Enlarge training set(4.1~4.3)

    * Data augmentation
    * Leveraging external but related labeled datasets
    * Cost effective annotation
    * Leveraging unlabeled data

  * Strengthen regularization during model training(4.5)

    * Regularization can be applied to the **input space** by **image representation**
    * To the **output space** by constraining the segmentation results with **shape priors**
    * To the **gradients** by leveraging additional supervision signals through **multi-task learning**.

    **NOTE** that except for **multi-task learning**, regularization-based methods do **NOT** require any further data or annotations.

  * Refine the segmentation mask(4.6)

    * Use different variants of CRFs either as a post-processing or during model training.

    **NOTE** that these methods require **NO** further data other than the currently available segmentation dataset.

* Weak annotations

  * tackle image-level annotations (5.1)
    * use different variants of class activation maps to leverage weak image-level
      labels
  * leverage sparse annotations(5.2)
    * typically based on some variants of **selective loss** where only sparsely
      labeled pixels contribute to the segmentation loss
  * handle noisy annotations(5.3)
    * typically use **noise-resilient(耐噪音) loss functions** to learn from noisy annotations


##### Abbreviation

| Abbreviation | Full Name                  | Chinese    |
| ------------ | -------------------------- | ---------- |
| MRI          | Magnetic Resonance Imaging | 磁共振成像 |
| CRF          | Conditional Random Field   | 条件随机场 |
| ROI          | Region of interest         |            |

##### Evaluation Metric

* IoU/mIoU

  <img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422180550894.png" alt="image-20200422180550894" style="zoom:50%;" />
  $$
  IoU = \frac{TP}{TP+FP+FN}
  $$

* Dice(F1)

  <img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422180751883.png" alt="image-20200422180751883" style="zoom:50%;" />

$$
Dice = \frac{2\times TP}{(TP+FP)+(TP+FN)}
$$

$$
IoU = \frac{Dice}{2-Dice}
$$



* Hausdorff distance

  Dice which pay main attention to the **inner of mask**, does not sensitive to the **shape pf image boundary**.

  As a measure of shape similarity, Hausdorff distance can make a good supplement for Dice

  ![image-20200422202721575](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422202721575.png)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422202757873.png" alt="image-20200422202757873" style="zoom: 67%;" />

##### Datasets

* PROMISE2012
* Data Science Bowl 2018
* ASU-Mayo
* MICCAI 2018 LiTS Challenge
* LIDC-IDRI
* Multi-Atlas Labelling Challenge (MALC) dataset
* OASIS
* Visceral dataset
* ...

##### Ongoing Challenges

* 2020 MICCAI-MyoPS: Myocardial pathology segmentation combining multi-sequence CMR [(MyoPS 2020)](http://www.sdspeople.fudan.edu.cn/zhuangxiahai/0/myops20/)

* 2020 ICIAR: Automatic Lung Cancer Patient Management (LNDb) [(LNDb)](https://lndb.grand-challenge.org/Home/)

* 2019 MICCAI: Structure Segmentation for Radiotherapy Planning [(StructSeg)](https://structseg2019.grand-challenge.org/)

* 2019 MICCAI: Kidney Tumor Segmentation Challenge [(KiTS19)](https://kits19.grand-challenge.org/)

* [2018 MICCAI Medical Segmentation Decathlon](http://medicaldecathlon.com/)

* 2017 ISBI & MICCAI: Liver tumor segmentation challenge [(LiTS)](https://competitions.codalab.org/competitions/17094)

* 2012 MICCAI: Prostate MR Image Segmentation [(PROMISE12)](https://promise12.grand-challenge.org/)


### ROAD 1: U-Net derived models

##### 2015

***

*U-Net: Convolutional Networks for Biomedical Image Segmentation*

***

> SOTA for [Medical Image Segmentation on ISBI 2012 EM Segmentation](https://paperswithcode.com/sota/medical-image-segmentation-on-isbi-2012-em)
>
> Citation [13338](https://scholar.google.com/scholar?cites=10845403114495995712&as_sdt=2005&sciodt=0,5&hl=en)
>
> Pytorch implementation available

![image-20200422174229489](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422174229489.png)

##### 2016

***

*V-Net: Fully Convolutional Neural Networks for Volumetric Medical Image Segmentation*

***

> SOTA for [Volumetric Medical Image Segmentation on PROMISE 2012](https://paperswithcode.com/sota/volumetric-medical-image-segmentation-on)
>
> Citation [1798](https://scholar.google.com/scholar?cites=10041077447911870942&as_sdt=2005&sciodt=0,5&hl=en)
>
> Pytorch implementation available

**Motivation** most approaches are only able to process 2D images while most medical data used in clinical practice consists of 3D volumes

**Features** 

* CNN is trained end-to-end on MRI volumes
* predict segmentation for the whole volume at once
* a novel objective function
* deal with situations where there is a strong **imbalance** between the number of foreground and background voxels
* augment the data applying random non-linear transformations and histogram matching
* requiring only a fraction of the processing time needed by other previous methods

**Architecture**

![image-20200422190758692](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422190758692.png)

* each stage learns a **residual function**

* convolutions in each stage use **volumetric kernels** having size ```5*5*5``` voxels

* convolutions **between** each stage use```2*2*2``` voxels, **replace pooling**

  ![image-20200422192708197](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422192708197.png)

* Replacing pooling operations with convolutional ones

  * have a **smaller memory** footprint during training, due to the fact that no switches mapping the output of pooling layers back to their inputs are needed for back-propagation

* the same for replacing de-convolutions with un-pooling

* Right portion of the network output a **two** channel volumetric segmentation, converted to probabilistic segmentations of the foreground and background regions by applying **soft-max voxelwise**.

* Skip connection

  * gather fine grained details
  * improve the convergence time

  ![image-20200422194000742](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422194000742.png)

* Dice loss layer

  Network predictions consist of **two volumes** having the **same resolution** as the original input data, are processed through a **soft-max layer** which outputs the probability of each voxel to belong to **foreground** and to **background**

  It is not uncommon that the anatomy of interest occupies only **a very small region of the scan**. This often causes the learning process to get trapped in **local minima** of
  the loss function yielding a network whose predictions are **strongly biased towards background**(classes label imbalance). As a result the foreground region is often missing or only partially detected.

  Propose a novel **objective function** based on **dice** coefficient between two binary
  volumes, ranging between **0 and 1** which we aim to **maximise**

  ![image-20200422194934300](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422194934300.png)

  predicted binary segmentation volume $p_i \in P$ and the ground truth binary volume $g_i \in G$.

  differentiated yielding the gradient

  ![image-20200422195654208](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422195654208.png)

  Using this formulation we do **NOT** need to **assign weights** to samples of different classes to establish the right balance between foreground and background voxels.

  results show it better than the ones computed through the same network trained optimising a multinomial logistic loss with sample re-weighting.

* Data Augmentation

  * dense deformation field obtained through a ```2*2*2``` grid of control-points and B-spline interpolation, on-the-fly.

  * vary the intensity distribution of the data by adapting, using **histogram matching**, the intensity distributions of the training volumes used in each iteration, to the ones of other randomly chosen scans belonging to the dataset

    * **histogram matching**

      <img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422200929088.png" alt="image-20200422200929088" style="zoom:50%;" />

      ![image-20200422200737620](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422200737620.png)

**Results**

![image-20200422201257123](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422201257123.png)

![image-20200422201539256](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422201539256.png)

![image-20200422201617033](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422201617033.png)

![image-20200422201733018](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422201733018.png)

**Conclusion**

* pixel to voxel

* residual function on every stage
* A novel **objective function** does not need sample re-weighting when the amount of background and foreground pixels is strongly **unbalanced** and is indicated for binary segmentation tasks.

##### 2017



##### 2018

***

*UNet++: A Nested U-Net Architecture for Medical Image Segmentation*

***

> Citation [190](https://scholar.google.com/scholar?cites=3438577305591755991&as_sdt=2005&sciodt=0,5&hl=en)
>
> Pytorch implementation available

**Motivation** 

Re-designed **skip pathways** aim at reducing the **semantic gap** between the feature maps of the encoder and decoder sub-networks.

They argue that the optimizer would deal with an **easier** learning task when
the feature maps from the decoder and encoder networks are **semantically
similar**.

**Features** 

* Re-designed skip pathways

  * UNet++ consists of an encoder and decoder that are connected through a series of **nested dense convolutional blocks**.

  * The main idea behind UNet++ is to bridge the **semantic gap** between the feature maps of the encoder and decoder prior to fusion

* Deep supervision

**Architecture**

![image-20200422211357431](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422211357431.png)

(a)**black** indicates the original **U-Net**, **green and blue** show **dense convolution blocks on the skip pathways**, and **red** indicates **deep supervision**. Red, green, and blue components distinguish UNet++ from U-Net.

(b)Detailed analysis of the first skip pathway of UNet++.

(c) UNet++ can be pruned at inference time, if trained with deep supervision.

* Re-designed skip pathways

  ![image-20200422212805078](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422212805078.png)

  * j = 0 receive only one input from the previous layer of the encoder
  * j = 1 receive two inputs, both from the encoder sub-network but at two consecutive levels
  * j > 1 receive j + 1 inputs
  * (b)  shows how the feature maps travel through the top skip pathway of UNet++.

* Deep supervision

  two modes

  * **accurate** mode wherein the outputs from all segmentation branches are **averaged**;
  * **fast** mode wherein the final segmentation map is selected from only **one** of the segmentation branches, the choice of which determines the extent of model pruning and speed gain.

* Loss function, combination of binary cross-entropy and dice coefficient, used  to each of the above four semantic level

  ![image-20200422213815877](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422213815877.png)



**Results**

![image-20200422214000893](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422214000893.png)

* U-Net as baseline
* A wide U-Net with similar number of parameters as U-Net++ to ensure that
  the performance gain yielded by our architecture is not simply due to increased
  number of parameters.

![image-20200422214534476](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422214534476.png)

![image-20200422220651009](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422220651009.png)

* Model pruning. UNet++ L3 achieves on average 32.2% reduction in inference time while degrading IoU by only 0.6 points.
  More aggressive pruning further reduces the inference time but at the cost of significant accuracy degradation.
* Specially, the use of **deep supervision** leads to marked improvement for **liver and lung nodule segmentation**, but such improvement **vanishes** for **cell nuclei and colon polyp(结肠息肉) **segmentation. This is because **polyps and liver(cell nuclei?)** appear at **varying scales** in video frames and CT slices; and thus, a **multi-scale approach** using all segmentation branches (deep
  supervision) is essential for accurate segmentation.

**Conclusion**

* takes advantage of re-designed **skip pathways** and **deep supervision**
  * The re-designed skip pathways aim at **reducing the semantic gap** between the feature maps of the encoder and decoder sub-networks, resulting in a possibly simpler optimization problem for the optimizer to solve.
  * Deep supervision also enables more accurate segmentation particularly
    for lesions that appear at **multiple scales** such as polyps in colonoscopy videos.



***

*Concurrent Spatial and Channel `Squeeze & Excitation' in Fully Convolutional Networks*

***



**Motivation** 

draw inspiration from the recently proposed squeeze & excitation (**SE**) module for channel recalibration(重新校准) of feature maps for image classication

explore an alternate direction of recalibrating the feature maps adaptively, to **boost meaningful features**, while suppressing weak ones.

**Features** 

* The **SE blocks** can be **seamlessly** integrated within any F-CNN model by placing them after every encoder and decoder block
* The SE block factors out the **spatial dependency** by global average pooling to learn a channel specific descriptor, which is used to recalibrate the feature map to emphasize on useful channels
* refer to the previously introduced SE block as channel SE (cSE), because it **only excites channel-wise**, which proved to be effective for **classication**.
  * spatial SE (**sSE**), another SE block, which **squeezes along the channels and excites spatially**
  * concurrent spatial and channel SE blocks (**scSE**), which recalibrate the feature maps **separately along channel and space**, and then **combines the output**.

**Architecture**

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422231750226.png" alt="image-20200422231750226" style="zoom:50%;" />

* Spatial Squeeze and Channel Excitation Block (cSE)(Origin)

  <img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422231819844.png" alt="image-20200422231819844" style="zoom: 67%;" />

  ![image-20200422231538172](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422231538172.png)

* Channel Squeeze and Spatial Excitation Block (sSE)

  <img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422231919332.png" alt="image-20200422231919332" style="zoom:67%;" />

  ![image-20200422232438000](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422232438000.png)

* Spatial and Channel Squeeze & Excitation Block (scSE)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422231952486.png" alt="image-20200422231952486" style="zoom:67%;" />

​	![image-20200422232714484](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422232714484.png)

* Model Complexity

  * cSE block introduces $C^2$ new weights

  * sSE block introduces $C$ weights.

  * increase in model complexity of a F-CNN with h encoder/decoder blocks is
    $$
    \sum_{i=1}^{h}(C_i^2+C_i)
    $$
    e.g., U-Net has about $2.1 \times 10^6$ parameters, scSE block adds $3.3 \times 10^4$ parameters.

    SE blocks only increase overall network complexity by a **very small fraction**.

**Results**

![image-20200422234235978](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422234235978.png)

* inclusion of **any SE block** consistently provides a statistically signicant **increase** in Dice score
* the **spatia**l excitation yields a **higher** increase than the **channel-wise** excitation,
  which conrms our hypothesis that spatial excitation is more important for seg-
  mentation.
* for whole brain segmentation
  * **sSE and scSE** outperform the normal model consistently for **all** the structures
  * **cSE** model outperforms the normal model in **most** structures except some **chal-
    lenging** structures like 3rd/4th ventricles(心室), amygdala(杏仁核) and ventral(腹侧) DC where its performance degrades.
  * One possible explanation could be the **small size** of these structures, which might have got **overlooked** by only exciting the channels.

![image-20200422234248213](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422234248213.png)

* whole body segmentation
  * a similar pattern

![image-20200422234304269](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422234304269.png)

![image-20200422234845422](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200422234845422.png)

**Conclusion**

* integration of squeeze & excitation blocks within F-CNNs for **image segmentation**

* introduced the **spatial squeeze & excitation**, which outperforms the previously proposed channel-wise squeeze & excitation.
* recalibration with SE blocks seems to be a fairly **generic** concept to boost performance in CNNs
* the substantial increase in segmentation accuracy comes with a **negligible** increase in model complexity.
* **seamless** integration



***

*MDU-Net: Multi-scale Densely Connected U-Net for biomedical image segmentation*\*

***



**Motivation**

directly **fuses** the neighboring **different scale feature maps** from both **higher layers and lower layers** to strengthen feature propagation in current layer.

![image-20200502111030463](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502111030463.png)

two categories of similar work

* intra-block dense connections which embeds the dense block to the traditional convolutional block such as FDU-Net. In addition, cascaded of stacked U-Nets also gain enough attention.
* Inter-block dense connections. Which means current layer can fuses from previous layer with different scale

**Features**

* three different multi-scale dense connections for U shaped architectures **encoder**, **decoder** and **across** them
* combining three dense connected architecture with **quantization**

**Architecture**

* Dense Encoder and Decoder Block

![image-20200502153659529](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153659529.png)

traditional:

![image-20200502153720697](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153720697.png)

new:

![image-20200502153736450](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153736450.png)

![image-20200502153829072](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153829072.png)

* Dense Cross connections Block

  ![image-20200502153847652](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153847652.png)

  traditional:

  ![image-20200502153900772](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153900772.png)

  ![image-20200502153907294](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153907294.png)

  new: 

  ![image-20200502153916399](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153916399.png)

  ![image-20200502153927707](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502153927707.png)

* Fully Multiscale Dense connected Ushape architecture

* ![image-20200502154004424](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502154004424.png)

  ![image-20200502154019807](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502154019807.png)

* Network Quantization(2017)

  Incremental Quantization (INQ) to compress the parameters as a regularization function
  against potential overfitting.

  INQ quantizes the parameters to the **power of two or zero** which makes shift operation possible.

  ![image-20200502154106848](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502154106848.png)

**Conclusion**

the accuracy generally gets higher as the number of dense connections increases.



***

*Attention U-Net: Learning Where to Look for the Pancreas(胰腺)*

***

> SOTA for [Pancreas Segmentation on CT-150](https://paperswithcode.com/sota/pancreas-segmentation-on-ct-150)
>
> Citation [197](https://scholar.google.com/scholar?cites=8060673631923393987&as_sdt=2005&sciodt=0,5&hl=en)
>
> Pytorch implementation available

**Motivation** 

Models trained with **attention gate (AGs)** implicitly learn to **suppress irrelevant regions** in an input image while **highlighting salient features** useful for a specific task

these(FCNs and U-Nets) architectures rely on **multi-stage cascaded** CNNs when the target organs show large inter-patient variation in terms of shape and size.

However, this approach(FCNs and U-Nets) leads to **excessive** and redundant use of computational resources and model parameters; for instance, **similar low-level features** are **repeatedly **extracted by all models within the cascade

**Features** 

AGs **automatically** learn to **focus on target structures** without additional supervision.

these gates generate **soft region proposals** implicitly on-the-fly and highlight salient features useful for a specific task

they do **NOT** introduce significant computational overhead and do **NOT** require a large number of model parameters as in the case of multi-model frameworks

the necessity of using an **external organ localization model** can be **eliminated** while maintaining the high prediction accuracy

Similar attention mechanisms have been proposed for natural image classification and captioning to perform adaptive feature pooling, where model predictions are **conditioned only on a subset of selected image regions**.

**Architecture**

![image-20200424171052553](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424171052553.png)

* New proposal
  * **grid-based gating** that allows attention coefficients to be more specific to local regions instead of **gating based on a global feature vector**.
  * one of the first use cases of **soft-attention technique** in a feed-forward CNN
    model applied to a medical imaging task, which can **replace hard-attention**
    approaches used in image classification and **external organ localization models**
    in image segmentation frameworks.

* In order to improve the accuracy, current segmentation frameworks rely on **additional preceding object localization models** to simplify the task into separate **localization and subsequent segmentation** steps. Here, we demonstrate that the same
  objective can be achieved by **integrating attention gates (AGs)** in a standard CNN model. This does **NOT** require the training of multiple models and a large number of extra model parameters.

![image-20200424200555441](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424200555441.png)

a gating vector $g_i \in R^{F_g}$ is used for each pixel $i$ to determine focus regions.

![image-20200424201435812](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424201435812.png)

![image-20200424201750356](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424201750356.png)

**Results**

dataset:

* 150 abdominal(腹部的) 3D CT scans acquired from patients diagnosed with gastric(胃的) cancer (CT-150)
* (CT-82) consists of 82 contrast enhanced 3D CT scans with pancreas(胰腺) manual annotations performed slice-by-slice.

![image-20200424202940344](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424202940344.png)

![image-20200424202949298](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424202949298.png)

![image-20200424203001027](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200424203001027.png)

**Conclusion**

eliminates the necessity of applying an external object localization model

generic and modular, can be easily applied to image classification and regression
problems

AGs are highly beneficial for tissue/organ identification and localization



***

*Recurrent Residual Convolutional Neural Network based on U-Net (R2U-Net) for Medical Image Segmentation*\*

***

>SOTA for [Lung Nodule Segmentation on LUNA](https://paperswithcode.com/sota/lung-nodule-segmentation-on-luna)
>
>Citation [106](https://scholar.google.com/scholar?cites=502026793750745707&as_sdt=2005&sciodt=0,5&hl=en)
>
>Pytorch implementation available

**Motivation** 

The proposed models utilize the power of U-Net, Residual Network, as well as RCNN

* a **residual unit** helps when training deep architecture.
* **feature accumulation** with recurrent residual convolutional layers ensures better feature representation for segmentation tasks
* it allows us to design better U-Net architecture with same number of network parameters with better performance for medical image segmentation

**Features** 

* recurrent convolution networks(RU-Net)(2015)
* recurrent residual convolutional networks(R2U-Net)

**Architecture**

![image-20200425101829652](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200425101829652.png)

Recurrent Convolutional Layers (RCL) are performed with respect to the discrete time steps that are expressed according to the RCNN

![image-20200501214630110](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501214630110.png)

![image-20200425103424319](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200425103424319.png)

##### 2019

***

*HyperDense-Net: A hyper-densely connected CNN for multi-modal image segmentation*\*

***

> SOTA for [iSEG 2017 Challenge](https://paperswithcode.com/sota/medical-image-segmentation-on-iseg-2017)
>
> Citation [64](https://scholar.google.com/scholar?cites=18025051143919752338&as_sdt=2005&sciodt=0,5&hl=en)
>
> Pytorch implementation available

a hyper-dense architecture for multi-modal image segmentation that extends the concept of dense connectivity to the **multi-modal** setting: each imaging modality has a path, and dense connections occur not only between layers **within the same path**, but also **between layers across different paths**

![image-20200504084900631](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504084900631.png)

![image-20200504085119661](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504085119661.png)

each stream performs a **different shuffling** of inputs, which can enhance robustness to the model and
mitigate the risk of overfitting

![image-20200504085722395](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504085722395.png)

Baseline

![image-20200504090414301](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504090414301.png)

loss function

![image-20200504090358923](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504090358923.png)

Comparison

![image-20200504090858202](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504090858202.png)



***

*Bi-Directional ConvLSTM U-Net with Densley Connected Convolutions*\*

***

> SOTA for [DRIVE](https://paperswithcode.com/sota/medical-image-segmentation-on-drive)

**Motivation**

Instead of a simple concatenation, combining these two kinds of feature maps between encoder and decoder with **nonlinear functions** may result in more precise segmentation output.

**Architecture**

![image-20200506213326174](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506213326174.png)

![image-20200506222922555](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506222922555.png)



##### 2020

***

*UNet++: Redesigning Skip Connections to Exploit Multiscale Features in Image Segmentation*\*

***

Basically a supplement to the previous paper, *UNet++: A Nested U-Net Architecture for Medical Image Segmentation*, without new features or architecture.

Pass.



***

*Multi-scale self-guided attention for medical image segmentation*\*

***

> SOTA for [CHAOS MRI Dataset](https://paperswithcode.com/sota/medical-image-segmentation-on-chaos-mri)
>
> Pytorch implementation available

**Motivation**

integrated self-attention to model the relation of local features with their corresponding global dependencies.

the application of **stacked attention modules** remains unexplored in semantic segmentation.

**Architecture**

Overview

![Spatial and Channel self-attention modules](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504205626018.png)

Spatial and Channel self-attention modules

* Position attention module (PAM)

  ![image-20200505225624427](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200505225624427.png)

  ![image-20200505225632028](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200505225632028.png)

* Channel attention module (CAM)

  ![image-20200505225640313](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200505225640313.png)

  ![image-20200505225656627](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200505225656627.png)

![Guiding attention](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504213018970.png)

* Guiding attention

  ![image-20200505225916939](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200505225916939.png)

  The objective is that the class information can be embedded into the subsequent guided attention modules by forcing the latent representation of encoder-decoders to be close

  ![image-20200506170717975](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506170717975.png)

  ![image-20200506170843081](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506170843081.png)

  Totally

  ![image-20200506170857925](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506170857925.png)

  ![image-20200506170907022](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506170907022.png)

* Deep supervision

  ![image-20200506171127313](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506171127313.png)

* Objective function

  ![image-20200506171133837](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506171133837.png)

  

### ROAD 2: Tackle Imperfect Dataset

##### Data augmentation

***

*Data augmentation using learned transformations for one-shot medical image segmentation*\*

***

> CVPR 2019

**Motivation**

many supervised biomedical segmentation methods focus on **hand-engineered preprocessing** steps and architectures.

It is also common to use **hand-tuned data augmentation** to increase the number of training examples

Data augmentation functions such as **random image rotations** or **random nonlinear deformations** are easy to implement, and are effective at improving segmentation accuracy in some settings

However, these functions have limited ability to **emulate real variations**, and can be highly **sensitive to the choice of parameters**.

address the challenges of limited labeled data by **learning to synthesize diverse and realistic labeled examples**

Propose a novel, automated approach to data augmentation leverages **unlabeled images**.

Using learning-based registration methods, we **model the set of spatial and appearance
transformations** between images in the dataset.

These models **capture the anatomical and imaging diversity** in the unlabeled images.

We **synthesize new examples** by sampling transformations and applying them to a single labeled example.

**Architecture**

Many existing segmentation methods rely on scan preprocessing to **mitigate these intensity-related** challenges.
Our augmentation method tackles these intensity-related challenges from another angle: rather than **removing intensity variations**, it **enables a segmentation method to be robust to the natural variations** in MRI scans.

In atlas-based approaches, anatomical variations between subjects are captured by a **deformation model**, and the challenges of intensity variations are mitigated using pre-processed scans, or intensity-robust metrics such as normalized cross-correlation.

However, **ambiguities in tissue** appearances can still lead to inaccurate registration and segmentations.

We address this limitation by **training a segmentation model on diverse realistic examples**, making the segmenter more robust to such ambiguities.

In practice, collections of images are more readily available than segmentations. Rather than rely on segmentations, our method leverages a set of unlabeled images.

![image-20200508082616326](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508082616326.png)

* Spatial and appearance transform models

  ![image-20200508105723612](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508105723612.png)

* Learning

  ![image-20200508112705693](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508112705693.png)

  * Spatial transformation module loss:

    normalized cross-correlation

  * appearance transformation module loss:

    ![image-20200508114724101](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508114724101.png)

    ![image-20200508115223235](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508115223235.png)

    ![image-20200508115104355](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508115104355.png)

    ![image-20200508114734764](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508114734764.png)

* Synthesizing new examples

  ![image-20200508115859471](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508115859471.png)

* Variants

  ![image-20200508120644150](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508120644150.png)

##### Loss Function



### Other Networks

##### 2015 and before

***

*Recurrent Convolutional Neural Network for Object Recognition*\*

***

> Citation [608](https://scholar.google.com/scholar?cites=11116024186348758537&as_sdt=2005&sciodt=0,5&hl=en)

**Motivation**

a recurrent CNN (RCNN) for object recognition by incorporating recurrent connections into each convolutional layer

**Architecture**

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501211643136.png" alt="image-20200501211643136" style="zoom: 150%;" />

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501211712326.png" alt="image-20200501211712326" style="zoom:150%;" />

![image-20200501211725662](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501211725662.png)

***

*Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting*\*

***

> NIPS
>
> Citation [1854](https://scholar.google.com/scholar?cites=5469943753756181884&as_sdt=2005&sciodt=0,5&hl=en)

**Motivation**

formulate precipitation nowcasting as a spatiotemporal sequence forecasting problem in which both the input and the prediction target are spatiotemporal sequences.

**Architecture**

![image-20200507120554528](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507120554528.png)

![image-20200507154524224](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507154524224.png)

The major drawback of FC-LSTM in handling spatiotemporal data is its usage of full connections in input-to-state and state-to-state transitions in which no spatial information is encoded.

![image-20200507154533638](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507154533638.png)

![image-20200507182614645](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507182614645.png)

FC-LSTM

![image-20200507185343382](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507185343382.png)

convLSTM

![image-20200507182728301](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507182728301.png)

encoder-decoder

![image-20200507175727390](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507175727390.png)

final prediction given by

![image-20200507175749683](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507175749683.png)

##### 2016



##### 2017

***

*Incremental network quantization: Towards lossless cnns with low-precision weights*\*

***

> Citation 418

**Architecture**

* Weight Partition

  * random strategy

    weights are divided into two different groups with the same probability

  * pruning-inspired strategy

    weights are divided into two different groups given by  thresholds

* Group-wise Quantization

  ![image-20200502203020657](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502203020657.png)

* Re-training

![image-20200502202000430](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502202000430.png)

![image-20200502202050989](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502202050989.png)

**Result**

![image-20200502202430904](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502202430904.png)

![image-20200502202456964](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200502202456964.png)

***

*SegNet: A Deep Convolutional Encoder-Decoder Architecture for Image Segmentation*

***

> Citation [4586](https://scholar.google.com/scholar?cites=8599254436676320906&as_sdt=2005&sciodt=0,5&hl=en)

**Motivation**

FCN : too much features maps produced in encoder are passed to decoder

**Architecture**

SegNet: use max pooling indices to upsample the feature maps, which needs less memory during training and delineate boundaries better

![image-20200501204820485](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501204820485.png)

***

*Attention Is All You Need*\*

***

> Citation 7407

**Architecture**

![image-20200504201754834](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504201754834.png)

![image-20200504201808969](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504201808969.png)

![image-20200504201824060](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504201824060.png)

![image-20200504201838298](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504201838298.png)



***

Focal Loss for Dense Object Detection\*

***

> ICCV
>
> Citation [3102](https://scholar.google.com/scholar?cites=1010598353453664537&as_sdt=2005&sciodt=0,5&hl=en)

**Motivation**

address this class imbalance by reshaping the standard cross entropy loss such that it **down-weights** the loss assigned to **well-classified** examples.

**Architecture**

![image-20200508123212035](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508123212035.png)

![image-20200508130331707](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508130331707.png)

![image-20200508130341963](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508130341963.png)

* Class Imbalance and Twostage Detectors

![image-20200508130738301](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508130738301.png)

* RetinaNet

  ![image-20200508130910840](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200508130910840.png)



##### 2018

***

*Pyramid Dilated Deeper ConvLSTM for Video Salient Object Detection*\*

***

> ECCV

**Architecture**

* overall

![image-20200507195540205](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507195540205.png)

* Pyramid Dilated Convolution (PDC) module

![image-20200507200142601](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507200142601.png)

* Bidirectional ConvLSTM (B-ConvLSTM)

![image-20200507201817864](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507201817864.png)

![image-20200507202256022](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507202256022.png)

* Deeper Bidirectional ConvLSTM

![image-20200507202307063](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507202307063.png)

![image-20200507202654111](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507202654111.png)

![image-20200507201817864](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507201817864.png)

​	compared with Vanilla ConvLSTM

![image-20200507202811084](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507202811084.png)

In this way, information are encouraged to flow between the forward and backward ConvLSTM units, and deeper spatiotemporal features can be extracted by the backward units.

* Deeper Bidirectional ConvLSTM with Pyramid Dilated Convolution

  * extend DB-ConvLSTM with a PDC-like structure.

  * convolution operation ‘∗’ is further replaced by dilated convolution ‘⊛’ and different dilation factors are adopted.

* Loss Function

  Let $G \in \{0, 1\}^{473 \times 473}$ and $S \in \{0, 1\}^{473 \times 473}$

![image-20200507205105795](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507205105795.png)

![image-20200507205121908](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507205121908.png)

![image-20200507205128464](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507205128464.png)

##### 2019

***

*Long short-term memory - Fully connected (LSTM-FC) neural network for PM2.5 concentration prediction*\*

***

**Architecture**

![](D:\CV\Cut\record\week12\media\1-s2.0-S0045653518324639-gr2_lrg.jpg)

LSTM

![image-20200507153318895](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507153318895.png)

NN

![image-20200507153342749](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507153342749.png)

##### 2020



### Supplements

#### GRU & LSTM

* overall

![image-20200506230927394](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506230927394.png)

* The problem

  If a sequence is long enough, they’ll have a hard time carrying information from earlier time steps to later ones. So if you are trying to process a paragraph of text to do predictions, RNN’s may leave out important information from the beginning

  These gates in GRU and LSTM can learn which data in a sequence is important to keep or throw away. By doing that, it can pass relevant information down the long chain of sequences to make predictions. 

  ![](D:\CV\Cut\record\week12\media\1_ygAgowqTZjR6ABzZHd8Bqg.gif)

* RNN workflow

  First words get transformed into machine-readable vectors. Then the RNN processes the sequence of vectors one by one.

  ![](D:\CV\Cut\record\week12\media\1_AQ52bwW55GsJt6HTxPDuMA.gif)

  While processing, it passes the previous hidden state to the next step of the sequence. The hidden state acts as the neural networks memory. It holds information on previous data the network has seen before.

  ![](D:\CV\Cut\record\week12\media\1_o-Cq5U8-tfa1_ve2Pf3nfg.gif)

  the input and previous hidden state are combined to form a vector. That vector now has information on the current input and previous inputs. The vector goes through the tanh activation, and the output is the new hidden state, or the memory of the network.

  ![](D:\CV\Cut\record\week12\media\1_WMnFSJHzOloFlJHU6fVN-g.gif)

* LSTM

  LSTM cell

  ![image-20200506232238752](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506232238752.png)

  The cell state act as a transport highway that transfers relative information all the way down the sequence chain. 

  The cell state, in theory, can carry relevant information throughout the processing of the sequence. So even information from the earlier time steps can make it’s way to later time steps, reducing the effects of short-term memory.

  conveyor belt

  ![image-20200507092334661](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507092334661.png)

  Gates are a way to optionally let information through. They are composed out of a sigmoid neural net layer and a pointwise multiplication operation.

  ![image-20200507092403321](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507092403321.png)

  The sigmoid layer outputs numbers between zero and one, describing how much of each component should be let through. A value of zero means “let nothing through,” while a value of one means “let everything through!”

  * Forget Gate

    This gate decides what information should be thrown away or kept

    ![](D:\CV\Cut\record\week12\media\1_GjehOa513_BgpDDP6Vkw2Q.gif)

    ![image-20200507092647880](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507092647880.png)

  * Input Gate

    The next step is to decide what new information we’re going to store in the cell state.

    ![](D:\CV\Cut\record\week12\media\1_TTmYy7Sy8uUXxUXfzmoKbA.gif)

    ![image-20200507092848651](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507092848651.png)

  * Cell State

    It’s now time to update the old cell state, $C_{t-1}$, into the new cell state $C_{t}$.

    ![](D:\CV\Cut\record\week12\media\1_S0rXIeO_VoUVOyrYHckUWg.gif)

    ![image-20200507093201732](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507093201732.png)

  * Output Gate

    The output gate decides what the next hidden state should be.

    ![](D:\CV\Cut\record\week12\media\1_VOXRGhOShoWWks6ouoDN3Q.gif)

    ![image-20200507093500331](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507093500331.png)

  To review, the Forget gate decides what is relevant to keep from prior steps. The input gate decides what information is relevant to add from the current step. The output gate determines what the next hidden state should be.

  * Variants

    * adding “peephole connections.

      ![image-20200507094914653](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507094914653.png)

    * use coupled forget and input gates

      ![image-20200507095130420](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507095130420.png)

* GRU(Gated Recurrent Unit)

  ![image-20200506233403742](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200506233403742.png)

  ![image-20200507095304346](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200507095304346.png)

  * Update Gate

    acts similar to the forget and input gate of an LSTM. It decides what information to throw away and what new information to add.

  * Reset Gate

    used to decide how much past information to forget.

#### Attention Mechanisms

***

2014 *Recurrent Models of Visual Attention*

***

> Image classification

***

2015 *Show, attend and tell: Neural image caption generation with visual attention*

***

> Image Caption

***

2015 *Effective Approaches to Attention-based Neural Machine Translation*

***

> Machine Translation
>
> Global Attention and Local Attention

2017 *Attention Is All You Need*

***

>Machine Translation
>
>Transformer
>
>self-attention

##### Introduction

Attention motivated by how we pay **visual attention** to **different regions** of an image or correlate words in one sentence

![image-20200428101213984](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200428101213984.png)

Given a small patch of an image, pixels in the **rest provide clues** what should be displayed there. We expect to see a **pointy ear** in the **yellow box** because we have seen a **dog’s nose**, **another pointy** ear on the right, and Shiba’s **mystery eyes** (stuff in the red boxes). However, the **sweater and blanket** at the bottom would **NOT** be as helpful as those doggy features.

Similarly for words in one sentence or close context

![image-20200428142830292](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200428142830292.png)

in order to predict or infer one element, such as a pixel in an image or a word in a sentence, we estimate using the **attention vector how strongly it is correlated with** (or “*attends to*” as you may have read in many papers) **other elements** and take the sum of their values weighted by the attention vector as the approximation of the target.

##### Attention

![image-20200501195734683](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501195734683.png)

![image-20200501195754203](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501195754203.png)

![image-20200501195806657](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501195806657.png)

##### Soft vs Hard Attention

![image-20200428170136977](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200428170136977.png)

whether the attention has access to the entire image or only a patch

- Soft Attention: the alignment weights are learned and placed “softly” over all patches in the source image; essentially the same type of attention

  - *Pro*: the model is smooth and differentiable.
  - *Con*: expensive when the source input is large.

  ![image-20200501195830945](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501195830945.png)

- Hard Attention: only selects one patch of the image to attend to at a time.

  - *Pro*: less calculation at the inference time.
  - *Con*: the model is non-differentiable and requires more complicated techniques such as variance reduction or reinforcement learning to train

![image-20200501195850881](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200501195850881.png)

Here is also a [brief introduction](https://blog.csdn.net/songbinxu/article/details/80739447) of attention Mechanisms in Chinese.

##### Self-Attention

* overall

![image-20200504190055203](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504190055203.png)

![image-20200504190143243](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504190143243.png)

![image-20200504190151940](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504190151940.png)

![image-20200504190214628](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504190214628.png)

* encoder

  * embedding

    ![image-20200504192038077](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192038077.png)

  * flows through each of the two layers of the encoder.

    ![image-20200504192507500](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192507500.png)

  * Self-Attention

    * Query/Key/Value

    ![image-20200504192547394](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192547394.png)

    ![image-20200504200314496](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504200314496.png)

    * Details

    ![image-20200504192702468](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192702468.png)

    * In matrix form

    ![image-20200504192837058](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192837058.png)

    * Multi-Head

      expands the model’s ability to focus on different positions.

      gives the attention layer multiple “representation subspaces”.

    ![image-20200504192902550](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192902550.png)

    ![image-20200504192924233](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192924233.png)

    condense these heads down into a single matrix

    ![image-20200504192952472](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504192952472.png)

    * Overall![image-20200504193114974](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193114974.png)

    * different attention heads

    ![image-20200504193210020](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193210020.png)

    * add all the attention heads

    ![image-20200504193322679](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193322679.png)

  * Representing the order of the sequence using positional encoding

    One thing that’s missing from the model as we have described it so far is a way to account for the **order of the words** in the input sequence.

    To address this, the transformer adds a vector to each input embedding. These vectors **follow a specific pattern that the model learns**, which helps it **determine the position of each word**, or the **distance between different words** in the sequence.

  ![image-20200504193428495](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193428495.png)

  ![image-20200504193440490](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193440490.png)

  * Residuals

  ![image-20200504193516682](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193516682.png)

  ​	![image-20200504193559901](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193559901.png)

  * Transformer of 2 stacked encoders and decoders

  ![image-20200504193644064](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504193644064.png)

  * Decoder

  ![](D:\CV\Cut\record\week12\media\transformer_decoding_2.gif)

  * Transformer

  ![image-20200504194324930](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200504194324930.png)