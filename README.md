# mnist-bnn
Bayesian Neural Network for image classification on MNIST dataset, for ETH Zurich Probabilistic AI course. Credit to Prof. Andreas Krause and the Probabilistic AI teaching team for project design as well as significant amounts of skeleton code. 

## Key Files
* **solution.py**: Includes definition of Model class with training and evaluation functions, BayesNet class, Bayesian Linear Layer class, and several ParameterDistribution classes. Also includes main function loop to evaluate and train model.
* **utils.py**: Includes parent class ParameterDistribution with abstract methods and helper functions for Expected Calibration Error (ECE) calculation.
