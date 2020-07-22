---
layout: post
title:  "Training a feed-forward Neural Network with Back-propagation and Gradient Descent"
date:   2020-07-19 13:19:41 -0400
categories: Machine_Learning
---

The training of an Artificial Neural Network is dependent on an algorithm called Back-propagation, through which we calculate the effect each weight has on an error function.
Each of these 'effects' is called a partial derivative, more specifically the <i>partial derivative</i> of the error function with respect to a given weight.
A collection of partial derivatives (one for each weight), is called the <i>gradient</i>, and enables training using <i>gradient descent</i>.

Here's the example network we'll be working with. It has just two input neurons, two hidden neurons, and one output.
We'll also be using the Mean-Squared Error function to evaluate the correctness of each output.

![simple feed-forward neural network 1](/images/ff_neural_net_1.jpg)


## Forward-propagation in a feed-forward network

To show what a forward-pass would look like through this network, let's give it a simple set of weights and the inputs 1 and 2.

![simple feed-forward neural network 2](/images/ff_neural_net_2.jpg)

As you can see, the final output of the network is $1.9$. We got this because each neuron after the input layer computes its value
as the weighted sum of the values in the previous layer.
People generally have no problem understanding the forward-pass when learning about neural networks,
it's Back-propagation (or backward-pass) that tends to be more difficult to comprehend.

## Computing error

The error (or cost) function gives a measure of how close the network's output was to the desired output for any given input.
Suppose that, given the inputs of $1$ and $2$ that we used in the above example, we would like the network to output the value $5$.
As mentioned earlier, we will be using the Mean-Squared Error function, defined as:
$$E = \frac{1}{2} \sum_{i = 1}^{n} (o_i - y_i)^2$$
Where $o$ is a vector of $n$ outputs and $y$ is a vector of $n$ desired values for each output. <br> <br>
In our network, the output was $1.9$ for the input $(0.1,0.2)$. Let's suppose the desired value for this input was $1.0$. Then, the error would be:
$$E = \frac{1}{2}(o_1 - y_1)^2 = \frac{1}{2}(n_{3,1} - y_1)^2 = \frac{1}{2}(1.9 - 1.0)^2 = 0.405$$
That's all there is to it, the error for this training example is $0.405$.

## Back-Propagation

Now that we've discussed Forward-propagation and the procedure for computing error, let's walk through back-propagation for this network.
Keep in mind that we computed our error using a desired output value of $1$, so the gradient that will result from back-propagation will point the weights
in a direction that will adjust the network to produce this output. In fact, we will test the network again later after we perform gradient descent to see
if the output is closer to our desired value of $1$.

Let's begin by computing the partial derivative of the error function with respect to the value in the output neuron. In other words, we want to figure out
how the value in the output neuron affects the error (which is the value we want to minimize).
$$\frac{\partial E}{\partial n_{3,1}} = \frac{\partial}{\partial n_{3,1}}(\frac{1}{2}(n_{3,1} - y_1)^2) = n_{3,1} - y_1$$
The partial derivative here just requires a simple application of the power rule and chain rule. As you can see from the result,
the instantaneous rate of change of the error with respect to the output of the network is just $n_{3,1} - y_1$.
