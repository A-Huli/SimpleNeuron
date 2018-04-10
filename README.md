<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [HOW IT WORKS - THEORY](#how-it-works---theory)
  - [Neurons](#neurons)
    - [Example](#example)
  - [Activation Function](#activation-function)
  - [Layers](#layers)
  - [Weights](#weights)
  - [HOW DOES NEURAL NETWORK LEARN?](#how-does-neural-network-learn)
    - [Errors](#errors)
    - [Errors on layers](#errors-on-layers)
    - [Learning](#learning)
- [HOW IT WORKS  - PYTHON](#how-it-works----python)
- [INSTALATION](#instalation)
  - [Requirements](#requirements)
  - [Installing SimpleNueron](#installing-simplenueron)
- [USAGE](#usage)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->






# HOW IT WORKS - THEORY


## Neurons

------------


Neural network consists, as the name indicates, of neurons. Each neuron takes an input from neurons connected to it. All inputs have their **weights** . Neuron sums inputs multiplied by their weights, and applies activation function to this sum.
[![Neuron](https://i.imgur.com/PqtErEy.png "Neuron")](https://i.imgur.com/PqtErEy.png "Neuron")

All **inputs** typically have a range from 0.00 to 1.00 . Zero means that there is little to no chance of something and one means we are almost sure that there is some likelihood.

**Weights** have a range from -1.00 to 1.00. -1 means that the inputs decrease the probability of the the thing we are trying to predict and 1 means it increases the probability.

The goal of a neural network is to adjust its weights so that it, with certain probability, can predict solution from the inputs. If some input is more important and it acts in favour of some action, it should have positive weight close to 1. If it is not that important, then weight should be around 0. And if the input makes an action less probable, it should have negative weight close to -1.

### Example

Let's consider the example below. We are trying to predict if there is a cat in a given picture. Previous layer of neurons has stated there is 67% probability that there is a tail in the image, 83% probability that there are four paws in the picture, 91% probability that there is a horn and 48 % chance that there are two eyes in the picture.

[![cat](https://i.imgur.com/AkMXLMg.png "cat")](https://i.imgur.com/AkMXLMg.png "cat")

Of course we know that cats have tails, four paws and two eyes, but they don't have horns. That's why inputs  **1**, **2** and **4** should have **positive** weights. If we know there is a tail, this should increase the chance that there is a cat. On the other hand input 3 should have negative weight, because if we know there is a horn, we can be sceptical about image presenting a cat. If we have shown our neural network a thousand of images of cats, we can hope that it will learn that cats don't have horns and put negative weight to the input, which role was to deduce if there is a horn. However we can't know for sure what neural network picked as important and what didn't, all inputs and outputs are just numbers that somehow lead to the solution.

## Activation Function

------------
The sum of inputs multiplied by their weights can often be larger than 1 or less than 0, that's why we need something called **activation function**. Activation fuction applied to the output limits it to the range (0.00,1.00). The example of such a funtion is **sigmoid **.

[![sigmoid function](https://i.imgur.com/FHlAJ2r.png "sigmoid function")](https://i.imgur.com/FHlAJ2r.png "sigmoid function")

As the arguments approch infinity the function aproches 1, as they approach minus infinity it approches 0 with the value of 0.5 when the argument is equal to 0.

## Layers
------------

Full neural network consists of not one but many neurons arranged in **layers**, each layer takes the  output of the previous one and treats it as an input. 

[![layers](https://i.imgur.com/njeYOhI.png "layers")](https://i.imgur.com/njeYOhI.png "layers")


## Weights
------------
Let's consider a neural network with 2 layers, first one has 3 neurons and the second one has 2 neurons.
![weights](https://i.imgur.com/YHD5ROU.png)

In this layer inputs to layer B should look like this:

![b1](https://i.imgur.com/v9xTCPN.png)

- - -
![b2](https://i.imgur.com/ESwR61r.png)

We can use **Matrix multiplication** to simplify this proccess
![matrix](https://i.imgur.com/j272ldD.png)


## HOW DOES A NEURAL NETWORK LEARN?



_ _ _

### Errors
- - -
In order to make a neural network learn something, first it has to know how big mistakes it makes. Let's consider the following example:
![erroer](https://i.imgur.com/0897Cjp.png)

Let's say we guessed the value of Y1 but it's wrong and we know the value of an error. Let's call this error EY1. What should be the error of X1 or X2? One might say we should split the error of Y1 evenly because there are two nodes connected to Y1, so the error of X1 should be 1/2 of error of Y1 and the error of X2 should be 1/2 of the error of Y1. Well it's generally not a good way of doing this, because we can see that weight coming from X2 is 3 times larger than the one of X1. Because of that X2 contributes to the mistake of Y1 3 times as much as X1. Therefore it would be better to describe the error of X1 as 1/4 of the error of Y1 and the error of X2 as 3/4 of the error of Y1. This leads to a general principle:
<br/>



![](https://i.imgur.com/NV1ZNFR.png)  
<br/>

![](https://i.imgur.com/G88y0W7.png)  

### Errors on layers





- - -



![](https://i.imgur.com/NfLZ1Bi.png)  

If we consider the following example, then we can see that X1 and X2 contribute not only to the error of Y1 but also Y2's. Therefore their error should come from both Y1 and Y2. If we write it in an equation, it should look like this:
<br/>
![](https://i.imgur.com/meU10hK.png)  
<br/>
![](https://i.imgur.com/ktDUcdk.png)  
If we write it once again in matrices, we would get something like this:

![](https://i.imgur.com/T8alM7z.png)  

Then we can ommit the values in the denominator. If we do this, we only lose the information about the normalization of the errors. Not the information about how much each weight contributs to it.

![](https://i.imgur.com/xpZOn4h.png)  

Now we can see that the weight of a matrix is the same as the one we used to feed forward values between the layers, except now it's transposed, which means it is flipped along the diagonal line. And if you think about it, it makes a perfect sence, now we are going backwards, instead of forwards, which means we need to invert the matrix:

![](https://i.imgur.com/CutKCIe.png)  

Now we can write the general priciple:  

![](https://i.imgur.com/HX7j6p7.png)  

With this alghoritm we can desribe errors from the end to the beggining of the neural network.

### Learning
- - -
Ok, now we know the errors but how can we adjust the weight to correct them?



# HOW IT WORKS  - PYTHON

works in progress

# INSTALLATION
## Requirements
Simple neuron requires libraries :
Numpy
Scipy

to install them you can simply do it with pip:
```
pip install numpy

```
```
pip install scipy

```
## Installing SimpleNueron
To install SimpleNeuron all you have to do is to copy SimpleNeuron.py file to your directory



# USAGE

First you need to import NeuralNetwork object
```
from SimpleNeuron import NeuralNetwork
```
then you need to initialize neural network object, it takes 2 parameters as an input table consisting of numbers of neurons in each layer and learning rate, for example:

```
nn = NeuralNetwork([3,6,8],0.3)
```
will create a neural network with 3 layers that consist of, as follows 3,6,8 neurons with learning rate of 0.3
Then you have to train your network on some data to do this you use:
```
nn.learn(input,output)
```
finally when your network is trained, you can use predict function to predict the output based on an input:

```
nn.predict(input)
```
