---
layout: post
title: "Backpropagation"
categories: misc
---

Essentially, neural networks perform an optimisation task. Let the objective function function be

```math
C=\frac{1}{2}\left\|y-a^{L}\right\|^{2}=\frac{1}{2} \sum_{j}\left(y_{j}-a_{j}^{L}\right)^{2}
```
 
where aLj is the activation of the jth neuron in the output layer (denoted with capital L) given an input sample x, and y is the correct output layer activation of the sample x.
For any single data point (x, y) of input vector x and correct output vector y, one the calculates the derivatives of the objective function C wrt all weights and biases in the neural network*. Note however, that C(aL) has a simple shape (a parabola) and we can easily calculate dC(aL)/daL analytically. However, a is a function of the weights, w and the biases b in the neural network and calculating C(w, b) is much more demanding. I think it is not possible analytically, but the backpropagation algorithm efficiently calculates the gradient of C(w, b) at the current location (w, b) numerically.
For a simpler notation, lets define the derivative of the cost function wrt zjl (zjl = ail-1*wjl + bjl, the weighted sum of inputs from the previous layer plus the bias; i.e. the input in the sigmoid function) of neuron j in layer l as deltajl
 
Note that for the output layer L these derivatives are easily calculated using the chain rule dC(a(z))/dz = dC/da * da/dz:
  or in vector notation  
Where the encircled dot means element wise multiplication (Hadamart product) rather than dot product.
Now observe that delta in layer l is amplified through layer l+1 proportionally to the weights in layer l+1:
 
Now use the chain rule again to get from delta = dC/dz(w, b) to dC/db
dC/dbl = dC/dzL * dzl/dbl = deltal * 1 = deltal.
 

And similarly for dC/dz(w, b) to dC/dw:
dC/dwl = dC/dzL * dzl/dwl = deltal * al-1
	 

Now that we have the gradient of C for o single data point (x, y), we technically could adapt the weights (called online learning). In praxis it is faster to repeat the process for a few more randomly sampled (without replacement) data points (called mini-batch) add all derivatives up, multiply them with a learning rate, and then update the weights. This is done for several epochs (where one epoch means going through all the data points once).
A quite easy to read code can be found in my bookshelf.

Why does gradient descent not end up in a local minimum? It probably does, I guess. But the objective function value in this local minimum is usually low enough for the neural network to be a good classifier.

* a sigmoidal neuron follows the function
		Σ(z)=1/(1+e^(-z) )
where z is the weighted sum of the outputs of neurons of the previous layer minus a bias
		z=∑_j▒〖w_j x_j*-b〗