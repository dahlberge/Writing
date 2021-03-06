# Machine Learning Basics - Part 2 - Concept of neural networks and how to debug a learning algorithm

[<img src="https://images.unsplash.com/photo-1507090960745-b32f65d3113a?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=92a1714119e42bc9d947f75ec4733ed6&auto=format&fit=crop&w=2250&q=80">](
https://unsplash.com/photos/PI8Hk-3ZcCU)
Photo by Matteo Catanese on Unsplash - https://unsplash.com/photos/PI8Hk-3ZcCU

In this article I revisit the learned material from the amazing [machine learning course by Andre Ng on coursera](https://www.coursera.org/learn/machine-learning) and create an overview about the concepts. All quotes refer to the material from the course if not explicitly stated otherwise.

## Table of Contents

- [Neural Networks Model Representation](#neural-networks-model-representation)
- [Cost function in neural networks](#cost-function-in-neural-networks)
  - [Backpropagation](#backpropagation)
  - [Unrolling parameters](#unrolling-parameters)
  - [Gradient checking](#gradient-checking)
  - [Random initialization](#random-initialization)
- [Checklist on training a neural network](#checklist-on-training-a-neural-network)
- [Debugging a learning algorithm](#debugging-a-learning-algorithm)
  - [Evaluate the hypothesis](#evaluate-the-hypothesis)
  - [Model selection](#model-selection)
  - [Bias and Variance](#bias-and-variance)
  - [Learning curves and the size of a set](#learning-curves-and-the-size-of-a-set)
  - [Summary](#summary)
- [Designing a machine learning system](#designing-a-machine-learning-system)
  - [Skewed classes and classification](#skewed-classes-and-classification)
  - [High accuracy](#high-accuracy)



## Neural Networks Model Representation

For neural networks we take the findings from exploring the statistical regression and try to put it in brain-like architecture.

The used terms change a little as the logistic function is often referred to as the sigmoid activation function and the theta parameters as weights. The underlying concept stays the same. Instead of the [bias term theta 0, now a bias unit with the value of 1 is used.](https://stackoverflow.com/questions/2480650/role-of-bias-in-neural-networks)

The neural network architecture is made of at least 3 layers. That is
- input,
- hidden, 
- output 

layer. (Many neural networks have more than 1 hidden layer though)

In an activation unit, the weighted input of each unit in the previous layer is re-calculated and re-measured. You can say that neural networks can basically implement the concept of a statistical regression multiple times with more and more advanced input.

![activationNodes](../assets/mlIntro/activationNodes.png)

Of course this concept can also be applied using vectorization. Therefore we use a new variable, that encompasses the weight parameters inside our g function as an activation unit. Here it is really important to track and visualize the dimensions of your matrices, since it can get quickly very complex (depending on your neural network architecture).

Check out [this incredible article](http://www.ebc.cat/2017/01/08/understanding-neural-networks-part-2-vectorized-forward-propagation/), that explains the concept very good with nice graphics.

A great introduction example is the XOR Problem. [This article](https://medium.com/@jayeshbahire/the-xor-problem-in-neural-networks-50006411840b) explains it well.

## Cost function in neural networks

For a logistic regression to be used in a neural network, the cost function has to be extend to hold the output units K and the regularization part needs the number of layers, the number of nodes in the current layer (plus the bias term) and the number of nodes in the next layers to localized the theta value correctly.

Cost function for logistic regression:
![logRegReg](../assets/mlIntro/logRegReg.png)

Cost function for logistic regression in a neural network:
![nnCostFReg](../assets/mlIntro/nncostFReg.png)

### Backpropagation

>"Backpropagation" is neural-network terminology for minimizing our cost function, just like what we were doing with gradient descent in logistic and linear regression.

Whereas forward propagation (activation of nodes) takes in the theta parameters of each node in the previous layer, backpropagation does basically the opposite. An error for each node is calculated by comparing the activation node's output with the calculated output of the node. Afterwards this error is minimized gradually by adapting the used parameter theta. 

The formula for calculating the error is:

![errorCostF](../assets/mlIntro/errorCostF.png)

### Unrolling parameters

Since some, more advanced, algorithms need vectorized versions for computation. Unrolling matrices into vectors is a great way for calculating the cost function and getting the vector of the calculated parameters and reshaping the result back into matrices.

### Gradient checking

To ensure that your backpropagation works as intended you should check your gradient. This is done calculating an approximation in respect to theta with the following formula:

![approxDer](../assets/mlIntro/approxDer.png)

If the result is similar to the gradient vector the implementation works correct.

### Random initialization

To use gradient descent in a neural network the initial values for theta cannot be symmetrical and must be initialized randomly. Using symmetrical initialization always lead to the same learning result, since the is no variety provided.

## Checklist on training a neural network

1. Randomly initialize weights
1. Implement forward propagation to get the hypothesis
1. Compute the cost function to get the errors
1. Implement backpropagation to compute partial derivatives (optimizing the parameters through errors)
1. Apply gradient checking (comparing backpropagation with numerical estimate)
1. Disable gradient checking
1. Use an optimization method to minimize the cost function with it's corresponding parameters

## Debugging a learning algorithm

Sometimes the learned algorithm produces large errors. The following strategies help you debugging.

### Evaluate the hypothesis

The first steps you can always take is to get more test data, increase or decrease features or your regularizing lambda.

After that split the data into a training set (~70%) and a test set (~30%). This technique give you immediate feedback on how well your hypothesis is performing.

### Model selection 

>Just because a learning algorithm fits a training set well, that does not mean it is a good hypothesis. It could over fit and as a result your predictions on the test set would be poor. The error of your hypothesis as measured on the data set with which you trained the parameters will be lower than the error on any other data set.

>Given many models with different polynomial degrees, we can use a systematic approach to identify the 'best' function. In order to choose the model of your hypothesis, you can test each degree of polynomial and look at the error result.

Therefore the data can be split into 3 sets:
1. Training Set
1. Cross Validation Set
1. Test Set

This allows us to 1. calculate the optimal parameters,  2. apply it to different polynomial models and find the one with the smallest error and 3. estimate the general error of the best model.

### Bias and Variance

The bias vs. variance problem describes the issue of the hypothesis under-, or overfitting a data set. Whereas a high bias underfits the data, a high variance overfits it.

For diagnostics, the errors of the sets can be compared. If the errors of the cross-validation and the test set are high the hypothesis is suffering from high bias. If the cross validation sets shows a much higher error than the training set the problem is most likely a variance problem.

These problems can be addressed using different regularizing lambda parameter.

![costFReg](../assets/mlIntro/costFReg.png)

Remember that, a lambda of the value 1 equals a completely biased hypothesis (underfitting), whereas a lambda of 0 is essentially high variance one (overfitting).

To apply this in practice it is useful to create a list of lambdas (eg. 0,0.01,0.02,0.04,0.08,0.16,0.32,0.64,1.28,2.56,5.12,10.24) and supply them when working on the different polynomial models in the trainings set and pick the one with the smallest error. It's important to note, that when computing the errors of the cross-validation set to not use regularization again, since it would distort the result.

### Learning curves and the size of a set

With increasing size of a set the errors will increase until a certain point where it plateaus.

If the algorithm is suffering from high bias, getting more data won't help as it is already underfitting. However if the problem is a overfitting one with high variance, getting more data is likely to improve the algorithm. 

### Summary

High bias can be addressed by
- adding features
- adding polynomial features
- decreasing the regularization parameter lambda

High variance can be addressed by
- getting more training data
- reducing features
- increasing  the regularization parameter lambda

>In reality, we would want to choose a model somewhere in between, that can generalize well but also fits the data reasonably well.


## Designing a machine learning system

Important questions one have to ask themselves:

- how mach data should be collected?
- how can sophisticated features be developed. What feature actually works for the goal?
- how can algorithms be developed that help reduce misinterpretation?

A recommended approach to design a machine learning system is to
1. start with a simple algorithm and test it on cross-validation data
2. plot learning curves to make the right decisions on what to improve next
3. examine errors manually and see what type of error it is and what could be improved to avoid those errors

### Skewed classes and classification

Skewed classes appear when one class is over-represented in the data set. 

To test if your data is suffering from this problem implement precision and recall tests.
You are essentially testing the true positives of all predicted positives (precision) and compare it top all true positives of all actual positives.

![precRec](../assets/mlIntro/precRec.png)

Depending on the goal of your classification problem, the way of weighting precision and recall varies.
As the hypothesis returns the probability between 0 or 1, the set boundary threshold decides whether to classify an outcome as positive or negativ.  

Often the starting point is 0.5, ie. everything under 0.5 is classified as negative. Depending whether you want to predict very confidently or rather avoid missing many cases, it makes sense to test different values for 0 and 1 (eg 0.3, 0.5, 0.7, 0.9) and compare the resulting algorithms. As you will have 2 values (one for Precision, one for Recall), the desired threshold can afterwards be calculated with the F-Score formula:

![fScore](../assets/mlIntro/fScore.png)

### High accuracy

To achieve the highest possible accuracy it is best to have as much useful(!) data as possible (low variance), but also to have an algorithm with many features or parameters (low bias). 


---
 
This wraps up the second part. In the next one, support vector machines and unsupervised learning will be described. Stay tuned!

---

Thanks for reading my article! Feel free to leave any feedback! 

---

Daniel is a LL.M. student in business law, working as a software engineer and organizer of tech-related events in Vienna. 
His current personal learning efforts focus on machine learning. 

Connect on:
- [LinkedIn](https://www.linkedin.com/in/createdd) 
- [Github](https://github.com/DDCreationStudios)
- [Medium](https://medium.com/@ddcreationstudi)
- [Twitter](https://twitter.com/DDCreationStudi)
- [Steemit](https://steemit.com/@createdd)
- [Hashnode](https://hashnode.com/@DDCreationStudio)
