---
title: "[Reading Notes] Object detection via a multi-region and sematic segmentation-aware CNN model"
date: 2016-12-13 22:58:00
tags: [Object Detection, CNN, multi-region, Sematic segmentation-aware, Reading Notes]
comments: true
---

## Abstract

This article propose an object detection system that relies on a multi-region deep convolutional neural network that also encodes sematic segmentation-aware features. The module aims at capturing a diverse set of discriminative appearance factors and exhibits localization sensitivity that is essential for accurate object localization. They exploit the above properties of their recognition module by intergrating it on an iterative localization mechanism that alternates between socring a box proposal and refgining its location with a deep CNN regression model. And consiquently, they detect objects with very high localization accuracy.

## I. Introduction

The definition of object detection:

> Given an image return all the instances of one or more type of objects in form of bounding boxes that tightly enclose them.

<!-- more-->

Overfeat:

> Using two CNN models that apply in a sliding window fashion on multiple scales of an image. The first is used to classify if a window contains an object. The second is to predict the true bounding box location of the object. And use **greedy algorithm** in the end to merge them.

R-CNN:
> Using **Alex Krizhevsky's Net**  to extract features from box proposals provided by selective search and then it classifies them with class specific linear SVMs.

How to further advance the state-of-the-art on object detection?

> Focusing on object representation and object localization. 

Object representation:

> Indeed features matter a lot on object detection. Instead of proposing only a network architecture that is deeper, here they also opt for an architecture of greater width. And that was accomplished at two levels:

> 1. They want their object representation to capture several different aspects of an object. To achieve this, they propose a multi-component CNN model (multi-region CNN). Each component of it is steered to focus on a different region.
> 2. They wish to enrich the above representation so that it also captures semantic segmentation information

Object localization:

> They attempt to built a more powerful localization system that combines their multi-region CNN model with a CNN-model for bounding box regression, which are used within an iterative scheme that alternates between scoring candidate boxes and reﬁning their coordinates.

Their Contributions:

> 1. They develop a multi-region CNN recognition model that yields an enriched object representation capable of capturing a diversity of discriminative appearance factors and of exhibiting localization sensitivity that is desired for the task of accurate object localization.
> 2. They furthermore extend the above model by proposing a uniﬁed neural network architecture that also learns semantic segmentation-aware CNN features for the task of object detection.
> 3. They show how to significantly improve the localization capability by coupling the aforementioned CNN recognition model with a CNN model for bounding box regression.
> 4. Their detection system achieves **mAP** of 78.2% and 73.9% on **VOC2007 and VOC2012** detection challenges respectively.

## II. Multi-Region CNN Model

1. Activation maps module
 - This part of the network gets as input the entire image and outputs activation maps (feature maps) by forwarding it through a sequence of convolutional layers.
2. Region adaptation module
 - Given a region R on the image and the activation maps of the image, this module projects R on the activation maps, crops the activations that lay inside it, pools them with a spatially adaptive (max-)pooling layer, and then forwards them through a multi-layer network.

This is the architecture of the Multi-Region CNN model:

![Figure2](http://ohykn376o.bkt.clouddn.com/OBDE2.png)

There are two aims of that:

> 1. to force the network to capture various complementary aspects of the objects appearance, thus leading to a much richer and more robust object representation
> 2. to also make the resulting representation more sensitive to inaccurate localization, which is also crucial for object detection

***The regions they deploy:***

> 1. Original candidate box
> 2. Half boxes
> 3. Central Regions
> 4. Border Regions
> 5. Contextual Region

***Why these regions helps?***

> 1. Discriminative feature diversification
> 2. Localization-aware representation

## III. Semantic Segmentation-Aware CNN Model

![figure4](http://ohykn376o.bkt.clouddn.com/OBDE1.png)

 - Activation maps module for semantic segmentation aware features
     - ***Weakly supervised training***(see Figure 4)
     - Activation maps
 - Region adaptation module for semantic segmentation aware features
 
They combine the Multi-Region CNN features and the semantic segmentation aware CNN features by concatenating them. The resulting network thus jointly learns deep features of both types during training.

## IV. Object Localization

There are three main components in this section:

1. CNN region adaptation module for bounding box regression

 - It is applied on top of the activation maps produced from the Multi-Region CNN model and, instead of a typical one-layer ridge regression model, consists of two hidden fully connected layers and one prediction layer that outputs 4 values per category. In order to allow it to predict the location of object instances that are not in the close proximity of any of the initial candidate boxes, we Use as region a box obtained by enlarging the candidate box by a factor of 1.3. This combination offers a significant boost on the detection performance of out system by allowing it to make more accurate predictions and for more distant objects.

2. Iterative Localization

 - Their localization scheme starts from the selective search proposals and works by iteratively scoring them and refining their coordinates.

3. Bounding box voting

 - Because of the multiple regression steps, the generated boxes will be highly concentrated around the actual objects of interest. They exploit this "by-product" of the iterative localization scheme by adding a step of bounding box voting.

## V. Implementation Details

> For all the CNN models involved in their proposed system, we used the publicly available **16-layers VGG model** pre-trained on ImageNet for the task of image classification.

Multi-Region CNN model

> Its activation maps module consistes of the convolustional part of the 16-layers VGG-Net that outputs 512 feature channels. The max-pooling layer right after the last convolutional layer is omitted on this module. Each region adaptation module inherits the fully connected layers of the 16-layers VGG-Net and is fine-tuned separately from the others.

Semantic segmentation-aware CNN model

> The activation maps module architecture consists of the 16-layers VGG-Net without the last classification layer and transformed to a Fully Convolutional Network.

Classification SVMs

> The ground truth bounding boxes are used as positive samples and the selective search proposals that overlap with the ground truth boxes by less than 0.3, are used as negative samples.

CNN region adaptation module for bounding box regression

> The region adaptation module for bounding box regression inherits the fully connected hidden layers of the 16-layers VGG-Net. As a loss function they use the euclidean distance between the target values and the network predictions.

## VI. Experimental Evaluation

1. Results on PASCAL VOC2007
2. Detection error analysis
3. Results on PASCAL VOC2012

## VII. Conclusion

Two key factors:

1. the diversification of the discriminative appearance factors that it captures by steering its focus on different regions of the object
2. the encoding of semantic segmentation-aware features. By using it in the context of a CNN-based localization refinement scheme, they showed that it achieves excellent results that surpass the state-of-the-are by a significant margin