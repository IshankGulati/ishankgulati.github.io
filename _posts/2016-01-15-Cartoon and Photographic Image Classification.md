---
layout: post
title: "Cartoon and Photographic image classification"
comments: True
date:  2015-11-01
tags: Image-Processing Machine-Learning
---

This was my second project as a part of my internship @Interra Systems, Noida from 1st June – 15th July 2015 under the guidance of Mr. Shekhar and Mr. Rishi Gupta.

## Introduction:

Accessing multimedia information or ”content” is now part of our daily routine. The actual challenge is how to make useful this information and retrieve relevant content. We reached the point where a device should find multimedia content for us just as another person would do. To this end, significant efforts are currently made to develop innovative automatic content- based indexing techniques. Of particular interest is the automatic cataloging of video footage into some predefined semantic categories. This can be performed globally, by classifying videos into one of several main genres, e.g. cartoons, music, news, sports. Also, sub-genres can be involved, e.g. identifying specific types of sports (football, hockey, etc.), movies (drama, thriller, etc.), and so on. Another solution aims to classify movie content locally, thus considering video segments and specific concepts, e.g. outdoor, action, violence, etc.

## Abstract

We address a particular case of genre classification among animated and photographic images. We use spatial content descriptors primarily color based, which are adapted to this particular task. Color descriptors are determined using color perception which is quantified in terms of statistics of color distribution, elementary hues, color properties and color relationship. Experimental tests conducted on more than 2000+ images and various classification schemes show the efficiency of this approach. Despite the high diversity of the data, the proposed descriptors are able to provide an average global correct classification up to 85.25%.

## Feature Engineering

* Average Saturation

The average saturation of cartoons is much higher when compared against photographic images. We compute our first descriptor as the average color saturation using the S channel of the image in the HSV color space.

<center><img src="/assets/saturation.PNG" alt="floor" style="width:420px;height:600px;"/></center>

* Threshold Brightness

The percentage of pixels having brightness (the V channel of the image in the HSV color space) above a certain threshold. The average brightness for cartoons is much higher than photos and we obtained best results using a threshold of 0.3.

<center><img src="/assets/brightness.PNG" alt="floor" style="width:420px;height:600px;"/></center>

* Black Outlines

The edges in frame are computed using canny edge detector (both canny and sobel were tested). The edge map thus produced is dilated. The strong edges in the resultant image (intensity above 200) were matched with the input image in grayscale color space. If at the spatial location of strong edges the pixels in input image are black (intensity 0-20) than they are concluded as black outlines. Finally a metric of black outlines/strong edges was used as a feature.

* DCT Coefficients

The DCT coefficients consists of a single DC coefficient and rest AC coefficients. The DC coefficient represents the average color in the region whereas AC coefficients represent color change across the block. The average value of AC DCT coefficients is computed and used as a feature.


* Gray Level Co-occurrence Matrix features

A statistical method of examining texture that considers the spatial relationship of pixels is the gray-level co-occurrence matrix (GLCM), also known as the gray-level spatial dependence matrix. The GLCM functions characterize the texture of an image by calculating how often pairs of pixel with specific values and in a specified spatial relationship occur in an image, creating a GLCM, and then extracting statistical measures from this matrix.

The statistical measures evaluated from the matrix and used as features are:

+ Energy

The Energy feature is calculated as:

<center><img src="/assets/energy.PNG" alt="floor" style="width:470px;height:120px;"/></center>

+ Entropy

The Entropy feature is calculated as:

<center><img src="/assets/entropy.PNG" alt="floor" style="width:260px;height:85px;"/></center>

Pi,j = Element i,j of the normalized symmetrical GLCM
N = Number of gray levels in the image

* Homogeneity Parameter 

This (RGB) feature depends on the difference of current pixel and the neighbors pixels, because of the cartoon images have little change in colors the zero difference for cartoon would riddance more than zero for photographs, according to equations:

<center><img src="/assets/homogeneity.PNG" alt="floor" style="width:520px;height:250px;"/></center>

* Dominant Peaks

A dominant peak is defined as the one for which full width at half maximum value is greater than 10, but less than 60. The color histogram in complete 256 bins are evaluated and then the number of dominant peaks and distance between them is computed. A photographic image generally have a fewer number of dominant peaks as compared to cartoon image. Also, the distance between dominant peaks in a cartoon image is quite large.

<center><img src="/assets/peaks.PNG" alt="floor" style="width:250px;height:250px;"/></center>

## Classifier Training and Testing

A dataset of 2100 training images (1100 cartoons and 1000 natural) and 800 test images (400 cartoons and 400 natural) is obtained from different video streams. The features for training set is computed and scaled between 0-1. The classifiers used for classification is a Support Vector Machine. Training set was divided in 7 sets and was used for cross validation among different models. The following parameters gave the best test set accuracy.

## Results

Kernel: RBF
C: 1.18921
Gamma: 16
Max Iterations: 150

Training Set accuracy: 97.381%
Test Set accuracy: 85.25%
