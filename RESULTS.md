# Model Training Results

## Setup

We use a scale mixture multivariate Gaussian distribution as our prior over the weights. This introduces the following hyper parameters:

- $\rho_1$
- $\rho_2$
- $\alpha$

Where $\rho$ are initialized to matrices of size (out\_features, in\_features) and used to parameterize the standard deviation via the softplus function:

$\sigma = log(1 + exp(\rho)$

and \alpha is the mixing coefficient between the two Gaussians.

Initially, these parameters have been set to $\rho_1 = 1.7$, $\rho_2 = 0.0001$, and $\alpha = 0.5$. 

Note that these weight parameters are not modified during training.

In Bayes by backprop, they considered $-\log\sigma_1 = {0, 1, 2}$ and $-\log\sigma_2 = {6, 7, 8}$. Note that these need to be transformed to get the equivalent $\rho$ values because of the softmax function:

$\rho = \log(\exp(\sigma) - 1)$

## Experiment 0

- Using values from above

| ECE   | Public Score |
|-------|--------------|
| 0.208 | 0.678        |

**Goal is 0.05, 0.65**

## Experiment 1

| $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|------------|------------|----------|----------|----------|
| 1e-1       | 1e-6       | -2.25    | -13.8    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.173 | 0.691        |

## Experiment 2

| $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|------------|------------|----------|----------|----------|
| 1e-1       | 1e-8       | -2.25    | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.171 | 0.694        |

All the test samples that the model is sure about, it gets correct. All of the test samples that the model is unsure about, usually has the correct answer nearby.

Not sure how to make it more robust to stuff like noise and rotation.

Note that the accuracy on the training set is 0.999 and the ECE is 0.003. Takes around 14min to train.

## Experiment 3

We will try increasing the standard deviation of the "block" gaussian and leave alpha the same. Next round, we will increase the alpha to give less priority to the zero weights.

This should be correct. Since inherently we believe (prior) that the weights should be somewhat uncertain.

**Really we want to decrease the ECE. So, the model needs to know when it's wrong**

| $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|------------|------------|----------|----------|----------|
| 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.170 | 0.692        |

## Experiment 4

Increase alpha

| $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|------------|------------|----------|----------|----------|
| 1e-0       | 1e-6       | 0.54     | -18.4    | 0.75      |

| ECE   | Public Score |
|-------|--------------|
| 0.175 | 0.692        |

Ok, essentially no difference. We need a paradigm shift.


## Experiment 5

We seem to converge pretty quickly to a good accuracy on the training set. Try setting the number of epochs from 100 to 50.

| Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------|------------|------------|----------|----------|----------|
| 50          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.154 | 0.700        |

Much better, train even less now lmao. Would be nice to implement early stopping using a validation set.


## Experiment 6

Even fewer epochs

| Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------|------------|------------|----------|----------|----------|
| 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.098 | 0.668        |

Much better, train even less now lmao. Would be nice to implement early stopping using a validation set.

## Experiment 7

Fewer?

| Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------|------------|------------|----------|----------|----------|
| 5          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.063 | 0.660        |


## Experiment 8

nani??

| Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------|------------|------------|----------|----------|----------|
| 1          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.066 | 0.585        |

## Experiment 9

Try changing the model size? Maybe (200, 200) hidden layers?
(All previous runs were done at (100, 100) hidden layer size)

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (200, 200)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.121 | 0.664        |

## Experiment 10

Changing layers again

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (50, 50)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.096 | 0.645        |

Very minimal difference, if any.


## Experiment 11

Changing the learning rate from 1e-3 to 1e-5. Model should learn slower?

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.103 | 0.498        |

Learned slower but not very good.

## Experiment 12

Changing the learning rate from 1e-3 to 1e-4. Should learn slightly slower.

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.053 | 0.598        |

Not bad, getting close.
Takeaways: 
- Reduce number of epochs
- Reduce learning rate

We're initializing our weights with pretty small variances (-5 to -4). Maybe up this to include more uncertainty?

## Experiment 13

Keep learning rate at 1e-4. Increase epochs again.

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 20          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.053 | 0.630        |

Better accuracy, still short.

## Experiment 14

Increase initial weight uncertainty from (-5, -4) to (-1, 1)

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.099 | 0.103        |

Ok that's pretty bad

## Experiment 15

Increase initial weight uncertainty from (-5, -4) to (-4, -3).
While training, accuracy seems to be still getting close to 1 again on the training set. We'll see if it affects performance on the test set.

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.041 | 0.564        |

Much better for ECE, but we need to bring that accuracy up.


## Experiment 16

Try to increase learning rate from 1e-4 to 1e-3 again, see if it will learn the correct weights faster while staying well-calibrated.

From previous testing, I think the learning rate will ruin the ECE. It might require raising the number of epochs with the 1e-4 learning rate to bump up accuracy.

| Hidden Layer Size | Num. Epochs | $\sigma_1$ | $\sigma_2$ | $\rho_1$ | $\rho_2$ | $\alpha$ |
|-------------------|-------------|------------|------------|----------|----------|----------|
| (100, 100)        | 10          | 1e-0       | 1e-6       | 0.54     | -18.4    | 0.5      |

| ECE   | Public Score |
|-------|--------------|
| 0.073 | 0.636        |


## Experiment 17

Reduce learning rate again, bump up number of epochs to hopefully increase convergence on the training set. Remove hidden layer and prior parameters from table since these aren't changing anymore.

| Num. Epochs | Learning Rate |
|-------------|---------------|
| 25          | 1e-4          |

| ECE   | Public Score |
|-------|--------------|
| 0.061 | 0.597        |

Better than before with the 1e-4 learning rate

## Experiment 18

Bump up epochs some more. ALso bump up the weights uncertainty.

| Num. Epochs | Learning Rate | Weights rho |
|-------------|---------------|-------------|
| 50          | 1e-4          | (-3, -2)    |

| ECE   | Public Score |
|-------|--------------|
| 0.042 | 0.579        |

Good, but the accuracy is lacking. We really need those weights to converge onto the right value.

## Experiment 19

Try increasing the spread of the initial weights from (-0.2, 0.2) to (-0.5, 0.5). Hopefully this will lead it to an optimal weight configuration faster?

| Num. Epochs | Learning Rate | Weights rho | Weights Mu |
|-------------|---------------|-------------|------------|
| 50          | 1e-4          | (-3, -2)    | (-0.5, 0.5)|

| ECE   | Public Score |
|-------|--------------|
| 0.025 | 0.582        |

No, accuracy is still shit.

## Experiment 20

Increase learning rate. Early impressions: the stochastic samples during training seem to be pretty accurate. Makes me worried that the model might overfit, but hopefully it retains some uncertainty in it's predictions. Model seemed very well calibrated before. 

Recall from the definition of calibration, it just means that the model accurately predicts what it's actual accuracy is going to be. So, just because the accuracy goes up doesn't tell us much about the calibration. That said, there seems to be some factors (spread of initial weight variance, spread of initial weight means, number of epochs training) that affects the ECE.

| Num. Epochs | Learning Rate | Weights rho | Weights Mu |
|-------------|---------------|-------------|------------|
| 50          | 1e-3          | (-3, -2)    | (-0.5, 0.5)|

| ECE   | Public Score |
|-------|--------------|
| 0.076 | 0.632        |

## Experiment 21

Accuracy getting worse. Need that to get better.

| Num. Epochs | Learning Rate | Weights rho | Weights Mu |
|-------------|---------------|-------------|------------|
| 100          | 1e-4          | (-3, -2)    | (-1, 1)|

| ECE   | Public Score |
|-------|--------------|
| 0.052 | 0.594        |



## Experiment 22

Accuracy improved. Reduce epochs to reduce overfitting.

| Num. Epochs | Learning Rate | Weights rho | Weights Mu |
|-------------|---------------|-------------|------------|
| 100          | 1e-3          | (-3, -2)    | (-1, 1)|

| ECE   | Public Score |
|-------|--------------|
| 0.058 | 0.676        |


Pretty good. Just need to bring down ECE.

## Experiment 23

Winner winner chicken dinner.

| Num. Epochs | Learning Rate | Weights rho | Weights Mu |
|-------------|---------------|-------------|------------|
| 80          | 1e-3          | (-3, -2)    | (-1, 1)|

| ECE   | Public Score |
|-------|--------------|
| 0.049 | 0.683        |

## Experiment 24

Now plotting val and ece over time

Results:
Congrats, you have passed the checks!
Your PUBLIC test ECE is 0.040 and PUBLIC test accuracy is 0.667
Your PUBLIC compound score is 2.047

Saved as results_check_2021-11-11_02.byte (the previous one is 01)

## Experiment 25

80 epochs with 1D batch norm:

Your algorithm has not passed the checks
Your PUBLIC test ECE is 0.079 and PUBLIC test accuracy is 0.592
Try to improve them!
Dumped check file to /results/results_check.byte


## Experiment 26

100 epochs with 1D batch norm:

Your algorithm has not passed the checks
Your PUBLIC test ECE is 0.053 and PUBLIC test accuracy is 0.670
Try to improve them!
Dumped check file to /results/results_check.byte

## Experiment 27

200 epochs with 1D batch norm:

Your algorithm has not passed the checks
Your PUBLIC test ECE is 0.090 and PUBLIC test accuracy is 0.500
Try to improve them!


Seems like the 80 case isn't so good. Probably around 100 epochs might be the new optimal point. Maybe slightly higher.

## Experiment 28

Adding randomly blurred and rotated images. 100 Epochs. Still 1D batch norm

Your algorithm has not passed the checks
Your PUBLIC test ECE is 0.056 and PUBLIC test accuracy is 0.686
Try to improve them!
Dumped check file to /results/results_check.byte

## Experiment 29

Added randomness to the dataset. 100 Epochs. Removed 1D batch norm.

Congrats, you have passed the checks!
Your PUBLIC test ECE is 0.038 and PUBLIC test accuracy is 0.674
Your PUBLIC compound score is 2.060
Dumped check file to /results/results_check.byte

## Experiment 30

Added randomness to the dataset. 80 Epochs. With 1D batch norm.

Congrats, you have passed the checks!
Your PUBLIC test ECE is 0.046 and PUBLIC test accuracy is 0.685
Your PUBLIC compound score is 2.046
Dumped check file to /results/results_check.byte


## Experiment 31

60 epochs. W/ 1D batch norm.


Your algorithm has not passed the checks
Your PUBLIC test ECE is 0.153 and PUBLIC test accuracy is 0.702
Try to improve them!
Dumped check file to /results/results_check.byte


## Experiment 32

120 epochs, no 1D batch norm

Congrats, you have passed the checks!
Your PUBLIC test ECE is 0.041 and PUBLIC test accuracy is 0.672
Your PUBLIC compound score is 2.049
Dumped check file to /results/results_check.byte