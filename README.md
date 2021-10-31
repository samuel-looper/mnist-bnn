# mnist-bnn
Bayesian Neural Network for image classification on MNIST dataset, for ETH Zurich Probabilistic AI course. Credit to Prof. Andreas Krause and the Probabilistic AI teaching team for project design as well as significant amounts of skeleton code. 

## Key Files
* **solution.py**: Includes definition of Model class with training and evaluation functions, BayesNet class, Bayesian Linear Layer class, and several ParameterDistribution classes. Also includes main function loop to evaluate and train model.
* **utils.py**: Includes parent class ParameterDistribution with abstract methods and helper functions for Expected Calibration Error (ECE) calculation.

## Project To-Do:
* ~~Implement ParameterDistribution functions (Univariate, Multivariate, ScaleMixture)~~ [Done by Sam, tested for ~100 iterations]
* ~~Implement Bayesian linear layer forward pass and weight initializations~~ [Done by Sam, tested for ~100 iterations]
* ~~Implement BayesNet forward pass and layer initialization~~ [Done by Sam, tested for ~100 iterations]
* ~~Implement Bayes By Backprop training loop~~ [Done by Sam, tested for ~100 iterations]
* Look into Euler computer cluster setup for training and hyperparam optimization
* Model training and evaluation

## Notes
* Implemented a Scale mixture Gaussian as per the paper
* The log prior and posterior terms may appear as though they overshadow the dataset likelihood terms, but after dividing by the batch size and cancelling each other out (since one is an addition and the other one is a subtraction) it's not as bad as it looks
* Had to do some hacks to make the scale mixture log likelihood function work: basically if there is a large difference in the log likelihoods between the two components it just takes the max (since one will be comparitively ~0)
* Was getting an error unless I set retain_graph=True in loss.backwards... dunno if that's supposed to happen or a sign of a bug I haven't seen in our model???
