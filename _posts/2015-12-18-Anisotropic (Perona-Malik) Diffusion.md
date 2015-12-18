---
layout: post
title: "Anisitropic (Perona-Malik) Diffusion"
comments: True
date:  2015-12-18
tags: Image-Processing
---

## Introduction

In Image processing, anisotropic diffusion, also called Perona–Malik diffusion, is a technique aiming at reducing image noise without removing significant parts of the image content, typically edges, lines or other details that are important for the interpretation of the image. In this project we explore the process of anisotropic diffusion through the work of Perona & Malik. This work introduces a slight modification to the conventional diffusion process by modeling the flux as a function of edge-strength in the image, thereby giving us "anisotropy". I have converted [this](http://www.mathworks.com/matlabcentral/fileexchange/14995-anisotropic-diffusion--perona---malik-/content/anisodiff_Perona-Malik/anisodiff2D.m) Matlab script to C++ code using OpenCV.

## Tools

[OpenCV 3](http://sourceforge.net/projects/opencvlibrary/files/opencv-win/), an Open-source Computer Vision library is used with Python 2.7. For instructions regarding the installation of OpenCV refer [documentation](http://docs.opencv.org/3.0-beta/doc/tutorials/introduction/windows_install/windows_install.html).

## Implementation

<script src="https://gist.github.com/IshankGulati/53a515022c959b14aeee.js"></script>

## Results

I have compared my result with the results of [Insight Toolkit](http://itk.org/ITKExamples/src/Filtering/AnisotropicSmoothing/ComputePeronaMalikAnisotropicDiffusion/Documentation.html).

<center><img src="/assets/perona-malik.PNG" alt="Lena Genetic" style="width:660px;height:670px;"/></center>

Thanks!