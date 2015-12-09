---
layout: post
title: "Image Segmentation using K-Means Clustering"
comments: True
date:  2015-12-09
tags: Machine-Learning Image-Processing
---

## Introduction

Images are considered as one of the most important medium of conveying information. Understanding images and extracting the information from them such that information can be used for other tasks is an important aspect of Machine Learning. There are enormous applications of the same such as use of images in navigation of robots, in self driving cars, medical diagnosis etc. Image Segmentation is used to seperate different objects in the image such that the segmented objects can be used in further computation and analysis. There are several algorithms available for this purpose and in this post we will use K-Means Clustering which is a very popular Unsupervised Learning Algorithm. For this project I have referred [Image Segementation using k-means clustering, EM and Normalized Cuts](http://ares.utcluj.ro/tsg/projetTSG/Theme04/image_segmentation_using_k-means_clustering.pdf) research paper by Suman Tatiraju and Avi Mehta.

## Frequently used Notations

| Notation | Description |
| ------ | ----------- |
| n_samples  | Number of samples in dataset |
| n_features | Dimension of a sample |
| n_clusters | Number of clusters into which image is to be segmented |

## K-Means Clustering

The objective of the K-means clustering algorithm is to divide an image into K segments minimizing the total within-segment variance. The variable K must be set before running the algorithm. The grouping is done by minimizing the sum of squares of distances between data and the corresponding cluster centroid. Thus, the purpose of K-mean clustering is to cluster the data.


1. Initialize Cluster centers
The first step of algorithm is to initialize k random cluster centers z1, z2,...., zk from the input data x1, x2,....,xn.

Input data dimension - n_samples x n_features
Cluster Centers dimension - n_clusters x n_features

2. Assign labels to input data
In this step distance of all the cluster centers z1, z2,...., zk initialized in last step from each input data point x1, x2,....,xn is computed and stored in a dist matrix. After computing this matrix, each data point is assigned a cluster center whose distance from this data point is least and is stored in a lablels vector. Mathematically, this step can be formulated as 

<center><img src="/assets/label.png" alt="label" style="width:260px;height:50px;"/></center>

dist dimension - n_samples x n_clusters
labels dimension - n_samples x 1

3. Update the cluster centers 
Compute the new cluster centers z1', z2',...., zk' by taking mean of the data points that belong to that particular cluster center. Mathematically, this step can be formulated as

<center><img src="/assets/centroid.png" alt="centroid" style="width:275px;height:80px;"/></center>
 
4. Termination
Repeat step 2 and step 3 until the algorithm converges i.e. the difference between newly assigned labels and older labels is less than a particular tolerance value or until the maximum number of iterations have been reached.

5. Reshaping input data
Normally whe use a dataset for clustering, it is already in the form of n_samples x n_features but when we use an image sa input data we will have to reshape it before applying K-Means clustering.

Grayscale image - n_rows x n_columns
Rehaped image - (n_rows * n_columns) x 1

RGB image - n_rows x n_columns x 3
Rehaped image - (n_rows * n_columns) x 3

K-Means Clustering has a drawback that it provides sub-optimal clusterings depending n the choice of initial clusters. To avoid convergence to local minima we can execute the above algorithm for multiple times using different initializations and choosing the best result at the end. Another solution would be to use some other algorithm like Genetic Algorithm which will be discussed in later posts.


## Implementation

<script src="https://gist.github.com/IshankGulati/5f0dc71a5695c8b2d104.js"></script>

## Results

<center><img src="/assets/kmeans.png" alt="Lena Kmeans" style="width:660px;height:670px;"/></center>


Thanks!