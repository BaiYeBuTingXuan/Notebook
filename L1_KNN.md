# K -Nearest Neighbos

## LIST

1. Distance Metirc
2. Hyperparameters
3. Image


## Distance Metirc：

- L1 （Manhattan）distance
  - depend on the coordinate axis
- L2 （Euclidean）distance
  - not depend on the coordinate axis

## Hyperparameters

- Best value of K to use

- Best distance Metric to use

'Hyperparameters':Choices about the algorithm that we set rather than learn which is very problem-dependent.

## Setting Hyperparameters

Divide the data to 3 part:
- train data
- validation
- test data

No validation?The Hyperparameters we chose might only do well in solving the uniqun test data.

Attach the test data when the experiment end and take guarantee to getting away from it.

## Cross Validation Method:
Split data into folds,try each fold as validation and average the results.

Useful for small datasets,but not used too frequently in deep learning.

# Disadvantage:Image

- Very slow at test time
- Distance metrics on pixels are not 'informative'
- Curse of dimensionality