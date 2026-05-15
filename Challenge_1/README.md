# Challenge 1

## Description
A Multivariate time series data collection were give, each of them consisting on 180 time steps and measurements from various input channels. The goal of the project is to assign a single pain category (no_pain, low_pain, high_pain) to each user based on the recorded signals.

We focus the work concentrating on the development, training and evaluation of the neural network architecture with the objective of modeling pain-related patterns over time.

## Exploratory Data Analysis
The Pirate Pain Dataset contains data from 661 individuals, each of them providing a time series of 160 time steps for 31 joints, 3 attributes including the number of legs, hands and eyes, and 4 self-reported pain surveys. After performing some processing, no missing values in either sequential or static features were found.

The dataset presents class imbalance, having the "no_paint" category as the most prevalent, followed by "low_pain" and "high_pain". This class imbalance must be considered during the model development, to avoid biasing the learning toward majority classes.

Many analysis over the joint variables time series were performed in order to find any possible correlations among themselves. Besides, data in "joint_30" variable is constant in every time step for all the individuals, so it was discarded from further analysis.

A completed and detailed analysis over the dataset can be found in the following [Python Notebook](https://github.com/lcaballero10/AN2DL-Challenges/blob/main/Challenge_1/challenge1_VaAlianzaTocaLaU_Exploration.ipynb).

## Methods

### Preprocessing

Prior to perform the model design, the categorical attributes, such as the number of legs, number of hands, number of eyes, were encoded into ordinal numerical values; while the continues variables were normalized within a range of 0 to 1.

<table>
<tr>

<td width="33%" valign="top">

## Number of legs

| n_legs | Ordinal value |
|-----------|-------|
| one+peg_leg | 1 |
| two | 2 |

</td>

<td width="33%" valign="top">

## Number of hands

| n_hands | Ordinal value |
|-----------|-------|
| one+hook_hand | 1 |
| two | 2 |

</td>

<td width="33%" valign="top">

## Number of eyes

| n_eyes | Ordinal value |
|------|----|
| one+eye_patch | 1 |
| two | 2 |

</td>

</tr>
</table>

### Sliding-Window Sequence Construction

A sliding-window functions was developed to segment each of the time series into fixed-length overlapping sequences. A proper window-size and stride must be defined to produce the uniform input segments and assign to each of them a pain label.

After performing an iterative search for the proper values, the results show that a window-size of 40 and stride of 20 achieved the best performance.

### Model Architecture
A standard deep learning architecture for time-series classification was implemented. A multi-layer bidirectional LSTM networks was considered, including dropout regularization between recurrent layers. Besides, each direction of the LSTM uses a hidden state size of 64 units. Moreover, regularization was implemented prior testing with different dropout rates, K2 weight decays and and optional L1 decays.


### Experiments

Many tests were performed to determined the most suitable architecture for the current problem. An extensive grid search to find the hyperparameters considering the GRU and LSTM, in both unidirectional and bidirectional forms was performed. A bi-directional LSTM classifiera was found as the most suitable model architecture. The results obtained for the hyperparameters are shown in the following table:

| Hyperparameters | Value |
|-----------|-------|
| Learning rate | 5e-4 |
| CNN units | 64 |
| Dropout rate | 2e-1 |
| L1 lambda | 0 |
| L2 lambda | 1e-4 |

Then, a second grid search was performed to determine the propers window-size and stride, as well as 5-fold cross validation was conducted. As result, we obtained that a window-size of 40 and a stride of 20 are the best parameters for the sliding-window sequence construction.

Finally, to evaluate the model comprising the CNN, Bi-LSTM and attention layers, a combination of LSTM units and number of hidden layers was performed, obtaining the architecture with 64 units in both the CNN and LSTM layers and 2 LSTM hidden layers as the best configuration with the highest average F1-score.
