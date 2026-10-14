# Challenge 2

This document contains the description, data analysis, methodology used and experiments performed for Challenge 2 of the Artificial Neural Networks and Deep Learning course.

## Description
In this challenge a dataset containing a collection of images is provided. Images come from a low-magnification (WSI) of human tissue. A correct image classification is requested, in which each image must be correctly assigned to one out of four possible classes corresponding to molecular subtypes identifying a potential tissue disease.

## Exploratory Data Analysis

### Dataset

The dataset is composed of 691 histopathological tissue images paired with corresponding segmentation masks, so in total we have 1382 images. The four molecular subtypes are: `Luminal A`, `Luminal B`, `HER2(+)`, and `Triple Negative`. Similar as we have seen in Challenge 1 dataset, this dataset presents a class imbalance, as `Luminal A` and `Luminal B` are the most prevalent, and `Triple Negative` is the least represented.

### Content Analysis

As part of the dataset analysis, a pre-trained ConvNeXt model was used for the extraction of deep feature embeddings and UMAP for visualization. The data was divided into clusters, in which the outliers correspond to corrupted images with artifacts, such as green staining or random backgrounds (e.g. Shrek-themed imagery). These corrupted images may significantly affect the performance of the neural network, so the dataset may be preprocess before training.

## Methods

### Preprocessing, data augmentation and encoding

### Model architecture and transfer learning

## Results
