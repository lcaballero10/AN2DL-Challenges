# Challenge 1

## Description
A Multivariate time series data collection were give, each of them consisting on 180 time steps and measurements from various input channels. The goal of the project is to assign a single pain category (no_pain, low_pain, high_pain) to each user based on the recorded signals.

We focus the work concentrating on the development, training and evaluation of the neural network architecture with the objective of modeling pain-related patterns over time.

### Exploratory Data Analysis
The Pirate Pain Dataset contains data from 661 individuals, each of them providing a time series of 160 time steps for 31 joints, 3 attributes including the number of legs, hands and eyes, and 4 self-reported pain surveys. After performing some processing, no missing values in either sequential or static features were found.

The dataset presents class imbalance, having the "no_paint" category as the most prevalent, followed by "low_pain" and "high_pain". This class imbalance must be considered during the model development, to avoid biasing the learning toward majority classes.

Many analysis over the joint variables time series were performed in order to find any possible correlations among themselves. Besides, data in "joint_30" variable is constant in every time step for all the individuals, so it was discarded from further analysis.

A completed and detailed analysis over the dataset can be found in the following [Python Notebook]{https://github.com/lcaballero10/AN2DL-Challenges/blob/main/Challenge_1/challenge1_VaAlianzaTocaLaU_Exploration.ipynb}.
