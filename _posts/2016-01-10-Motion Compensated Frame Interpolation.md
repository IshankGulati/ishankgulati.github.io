---
layout: post
title: "Motion Compensated Frame Interpolation"
comments: True
date:  2016-01-10
tags: Video-Processing
---

This was my first project as a part of my internship @Interra Systems, Noida from 1st June – 15th July 2015 under the guidance of Mr. Shekhar and Mr. Rishi Gupta.

## Introduction

As the technology of display devices is developed, various high-performance display devices have become available to users in the market. While several current video coding and transmission specifications have been defined for relatively old devices having HD spatial resolution and a temporal rate of 30 fps, spatial and temporal resolutions of many commercial display devices have become higher. Furthermore, they have the functionality to enhance spatial and temporal resolution during post-processing. Frame rate up-conversion (FRU) methods are suited for the post-processing of display devices for high perceptual quality, as FRU algorithms can enhance temporal quality without increasing the bitrates. Up-conversion methods in a temporal axis can be used to produce an inter-frame in between two consecutive frames at a decoder side without any direct information for generation of the inter frame from an encoder.

## Abstract

In this project, I implemented a motion-compensated (MC) interpolation algorithm to enhance the temporal resolution of video sequences. First, bilateral motion estimation scheme to obtain the motion field of an interpolated frame without less holes and overlapping problems is developed. In order to estimate accurate MVs, a search range and the order of blocks to be searched is adaptively alternated. Then the variable-size block MC (VS-BMC) algorithm is applied to object boundaries in order to reconstruct edge information with a higher quality. Finally, we perform spatial linear Interpolation to fill the occlusion regions. The adaptive OBMC (AOBMC) can overcome the limitations of the conventional OBMC, such as over-smoothing and poor de-blocking. Experimental results show that the proposed algorithm provides a better image quality than conventional methods both objectively and subjectively.


## Results

<center><img src="/assets/t-1Frame.jpg" alt="t-1frame" style="width:640px;height:400px;"/></center>
<center><sub>t-1 frame</sub></center><br>

<center><img src="/assets/tFrame.jpg" alt="tframe" style="width:640px;height:400px;"/></center>
<center><sub>t Frame</sub></center><br>

<center><img src="/assets/forward.jpg" alt="forward" style="width:640px;height:400px;"/></center>
<center><sub>Only forward estimation</sub></center><br>

<center><img src="/assets/backward.jpg" alt="backward" style="width:640px;height:400px;"/></center>
<center><sub>Only backward estimation</sub></center><br>

<center><img src="/assets/bidirectional.jpg" alt="bidirectional" style="width:640px;height:400px;"/></center>
<center><sub>Bidirectional estimation</sub></center><br>

<center><img src="/assets/obmc.jpg" alt="obmc" style="width:640px;height:400px;"/></center>
<center><sub>OBMC + Spatialy interpolated frame</sub></center><br>

## References

* Yuanzhouhan CAO, Xiaohai HE, Qizhi TENG, Wei WU. Motion Compensated Frame Rate Up-conversion Using Soft-decision Motion Estimation and Adaptive-weighted Motion Compensated Interpolation (2013)
* Kyung-Yeon Min and Dong-Gyu Sim. Confidence-based adaptive frame rate up-conversion (2013)
* Jun-Geon Kim and Daeho Lee. Motion Compensated Frame Interpolation Using Motion Vector Refinement with Low Complexity (2013)
* Natan Jacobson, Yen-Lin Lee, Vijay Mahadevan, Nuno Vasconcelos, Truong Q. Nguyen. Motion vector refinement for FRUC using saliency and segmentation (2010)
* Tsung-Han Tsai , Hong-Guang Chen and Hsueh-Yi Lin. Frame Rate Up-Conversion Using Adaptive Bilateral Motion Estimation (2008)
* Byeong-Doo Choi, Jong-Woo Han, Chang-Su Kim and Sung-Jea Ko. Motion-Compensated Frame Interpolation UsingBilateral Motion Estimation and Adaptive Overlapped Block Motion Compensation (2007)
* Manzur Murshed, Laurence S. Dooley. Filtering of block motion vectors for use in motion based video indexing and retrieval (2005)
* Yao Nie, Kai-Kuang Ma. Adaptive Rood Pattern Search for fast Block-Matching Motion Estimation(2002)
* Ralph Braspenning, Gerard de Haan. True – Motion Estimation using Feature Correspondences (2001)
