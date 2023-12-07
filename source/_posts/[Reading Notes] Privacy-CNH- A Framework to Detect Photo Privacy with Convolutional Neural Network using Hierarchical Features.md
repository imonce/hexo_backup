---
title: '[Reading Notes]Privacy-CNH: A Framework to Detect Photo Privacy with Convolutional Neural Network using Hierarchical Features'
date: 2016-12-10 16:44:19
tags: [PCNH, CNN, Privacy Detect, Reading Notes]
comments: true
---

## Motivation

Mobile devices have revolutionized how people share photos with each other on social networks with a single click of a button.

The content of the photos people shared to the internet are rarely analyzed by the websites before the photos are made available to view. After the photos are posted on the social network to the public to view, it is close to impossible to permanently delete the uploaded photos.

Photo leakage, regretion after posting and malicious posting happens from time to time.
<!-- more-->
## Related Works

Existing works on photo privacy detection, which rely on low-level vision features, are non-informative to the users regarding what privacy information is leaked from their photos.

## Detailed Design

![framework](http://ohykn376o.bkt.clouddn.com/20161210PCNH1.jpg)

PCNH is a combination of PCNN and PONN. Given the image features in the input layer, the object features learning pipeline processes the features using hi(x) as the activation functions and the param eters of network structure is encoded as Vi, finally obtaining the photo privacy detection result in the output layer. The convolutional features learning pipeline processes the features using j(x) as the activation function and the parameters of network structure is encoded as Ws, finally obtaining the photo privacy detection result in the output layer. hi(x), j(x) are activation functions, which map from a vector to a scalar.

## Evaluation

![Evaluation Chart](http://ohykn376o.bkt.clouddn.com/20161210PCNH2.jpg)

