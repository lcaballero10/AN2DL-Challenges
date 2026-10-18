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

After analyzing the data, it was obviously noticed that some preprocessing must be applied, as corrupted images (noise) was present in our dataset. Besides, many deep learning concepts and techniques seen in the course, such as grid search, data augmentation, k-fold cross-validation, transfer learning, and others were applied.

### Preprocessing, data augmentation and encoding

As initial step, images were cropped (considering padding), resized and normalized. Besides, a data augmentation pipelane was employed to enhance generalization including transformations, horizontal/vertical flips, Gaussian blurs, among others. A test-time augmentation step was applied at inference, aggregating this way predictions from multiple views. Finally, the molecular subtypes were decoding according to the following table:

| Molecular Subtype | Encoded value |
|:-----------:|:-------:|
| Luminal A | 0 |
| Lumimal B | 1 |
| HER2(+) | 2 |
| Triple Negative | 3 |


### Model architecture and transfer learning

A pre-trained model was used (specifically the ```ConvNeXt``` neural network). The original head was replaced with a head composed of fully connected layers considering dropout regularization. As a classification problem was being solved, a cross-entropy loss was employed. An ideal learning rate was selected to optimize the training and generalization enhancements techniques, such as weight decay, dropout, and early stopping were used.

Besides, to improve the robustness of the solution, a test-time augmentation (TTA) step was applied at inference time.

## Experiments

As mentioned above, the ```ConvNeXt Small``` architecture was used as baseline for the transfer learning approach. An initial grid search was performed to find the most suitable hyperparameters for the network, which are shown in Table X. Besides, in the following figure is depicted the network architecture used for the challenge.

<p align="center">
  <img src="../media/Challenge 2/model_final_chall2.png" alt="Model architecture Challenge 1" width="500">
</p>

Then, another grid search was performed to finetune the network, specially the backbone unfreezing depth and the learning rates. A small batch size of 16 was used to enhance generalization, the values obtained can also be seen in Table X.

<table>
  <thead>
    <tr>
      <th>Parameter</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2" align="center">
        <strong>Initial grid search (baseline)</strong>
      </td>
    </tr>
    <tr>
      <td>Dropout rate</td>
      <td align="center">0.3</td>
    </tr>
    <tr>
      <td>Learning rate</td>
      <td align="center">0.001</td>
    </tr>
    <tr>
      <td>Batch size</td>
      <td align="center">32</td>
    </tr>
    <tr>
      <td>Warm-up epochs</td>
      <td align="center">5</td>
    </tr>
    <tr>
      <td>Minimum learning rate</td>
      <td align="center">1 × 10<sup>−6</sup></td>
    </tr>
    <tr>
      <td colspan="2" align="center">
        <strong>Fine-tuning grid search</strong>
      </td>
    </tr>
    <tr>
      <td>Unfrozen backbone stages</td>
      <td align="center">2</td>
    </tr>
    <tr>
      <td>Backbone learning rate</td>
      <td align="center">1 × 10<sup>−5</sup></td>
    </tr>
    <tr>
      <td>Head learning rate</td>
      <td align="center">0.0001</td>
    </tr>
  </tbody>
</table>

Finally, as done with Challenge 1, a 5-fold cross-validation was performed to enhance the rosbustness and generalization capability of the network, considering that the dataset provided is quite small.

It is worth mentioning that these same experiments were applied with other networks, such as ```ResNet18```, ```ConvNeXt Tiny``` and ```RetCCL```, obtaining the best results with the ```ConvNeXt``` network.

Several events and trends were observed during the training. Firstly, the network tended to overfit pretty soon; for this reason, an early stopping was applied. Data augmentation, as well as test-time augmentation helped by improving the generalization capability.

The following Figure shows the averaged loss and F1-score time histories across the 5-fold cross-validation.

<p align="center">
  <img src="../media/Challenge 2/f1_loss.jpeg" alt="Model architecture Challenge 1" width="500">
</p>

## Results

We reached the followin f1-score in the Kaggle Competition:

### Public:
F1-score = 0.3994