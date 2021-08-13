## Backward Propagation

### Gradient descend algorithms
Total training error:
$$ Loss = \frac{1}{T}  \sum_{t}{Div(Y_t, d_t; W_1 , W_2, ..., W_K)} $$
Gradient descent algorithm:
$$ \nabla_{W_k} Loss =  \frac{1}{T}  \sum_{t}{ \nabla_{W_k}Div(Y_t, d_t)} $$
$$ W_k = W_k -\eta \nabla_{W_k}{{Loss}^{T}}$$

**Vector Derivative** for $ z_k = W_k y_{k-1} + b_k$, $ y_k = f(z_k)$
$$ \nabla_{y_{k-1}}z_{k} = J_{y_{k-1}}(z_k) = W_{k}$$
$$ \nabla_{b_{k}}z_{k} = J_{b_{k}}(z_k) = \text{Diag}(len(z_k)) $$
$$ \nabla_{z_{k}}y_{k} = J_{z_{k}}(y_k) = \text{Diag}(f'(z_k)) $$
If $ z = Wy +b$ and $ D = f(z) $, then $\nabla _y D = \nabla_z (D) W$, $\nabla _b D = \nabla_z (D) W$ and $\nabla _W D = y \nabla_z (D) $

### Issues with GD algorithms
In classification problems, the classification error is a non-differentiable function of weights. The divergence function minimized is only a proxy for classification error. Minimizing divergence may not minimize classification error.

Backpropagation will often not find a separating solution even though the solution is within the class of functions learnable by the network. This is because the separating solution is not a feasible optimum for the loss function. One resulting benefit is that a backprop-trained neural network classifier has lower variance than an optimal classifier for the training data.

Backprop is not guaranteed to find a “true” solution, even if it exists, and lies within the capacity of the network to model. The optimum for the loss function may not be the “true” solution. For large networks, the loss function may have a large number of unpleasant saddle points or local minima.

The learning rate must be lower than twice the smallest optimal learning rate for any component to ensure convergence. Convergence behaviors become increasingly unpredictable as dimensions increase. For the fastest convergence, ideally, the learning rate must be close to both, the largest and the smallest. The scaling helps to reduce the differnce of optimal learning rates in different directions (lowering the conditional number of the Hessian matrix).

Decaying learning rate is considered to ensure fast convergence, e.g. linear/exponentic decay.


**Resilient propagation**: If the derivative at the current location recommends continuing in the same direction as before (i.e. has not changed sign from earlier), increase the step, and continue in the same direction; If the derivative has changed sign (i.e. we’ve overshot a minimum), reduce the step and reverse direction.

**Quick propagation**: Quickprop employs the Newton updates with two modifications but with two modifications. It treats each dimension independently, and approximates the second derivative through finite differences. Maintain a running average of all past steps. 

**Momentum methods**: In directions in which the convergence is smooth, the average will have a large value. In directions in which the estimate swings, the positive and negative swings will cancel out in the average. Update with the running average, rather than the current gradient. The training get longer in directions where gradient retains the same sign, and become shorter in directions where the sign keeps flipping.

**Nestorov’s Accelerated Gradient**: At any iteration, first extend the previous step, then compute the gradient step at the resultant position, and add the two to obtain the final step.

### Batch vs SGD vs Mini-batch
The evaluation requires to update for all training points. The training using batch is considered, which adjust the training each batch to accelerate convergence. Batch update, and shuffle is required to avoid cyclic oscillation. The individual training instances contribute different directions to the overall gradient, and the final gradient points is the average of individual gradients pointing towards the net direction. The provided training instances are provided in random order, named as **Stochastic Gradient Descent**. Batch gradient descent operates over T training instances to get a single update, while SGD gets T updates for the same computation. Note SGD converges faster, but to a poorer minimum and large variation between runs. Each iteration, SGD focuses on the divergence of a single sample $div$, and the expected value of the sample error is still the expected divergence $E[div]$. Hence, SGD leads to much greater variance. **Mini-batch update**  adjust the function at a small, randomly chosen subset of points, by keeping adjustments small and adjusted the function with good choice of subsets. We also can vary the subsets randomly in different passes through the
training data.

> About Empirical and Variance:
> The empirical risk $E[div]$ is an unbiased estimate of the expected divergence. Though there is no guarantee that minimizing it will minimize the expected divergence.
> The variance of the empirical risk: $Var(Loss) = 1/N Var(div)$. The variance of the estimator is proportional to $1/N$. The larger this variance, the greater the likelihood that the W that minimizes the empirical risk will differ significantly from the W that minimizes the expected divergence.

**RMS propagation**: Maintain a unning estimate of the mean suqared value of derivatives for each parameter, and scale update of the parameter by the inverse of the root mean squared derivative.
$$ E[\partial_w^2D]_k = \gamma E[\partial_w^2D]_{k-1}
 + (1-\gamma)(\partial_w^2 D)_k $$
$$ w_{k+1} = w_k - \frac{\eta}{\sqrt{E[\partial_w^2D]_k + \epsilon}} \partial_w D $$

**ADAM: RMS prop with momentum**: RMS prop only considers a second-moment normalized version of the current gradient.  ADAM utilizes a smoothed version of the momentum-augmented gradient by considering both first and second momentss. It maintains a running estimate of the mean derivative and the the mean suqared value of derivative for each parameter, and conduct scale update of the parameter by the inverse of the root mean squared derivative.
$$ m_k = \delta m_{k-1} + (1-\delta) (\partial_w D)_k $$
$$ v_k = \gamma v_{k-1} + (1-\gamma) (\partial_w^2 D)_k $$
$$ \hat{m}_k = \frac{m_k}{1-\delta^k}, \hat{v}_k = \frac{v_k}{1-\gamma^k} $$
$$ w_{k+1} = w_k - \frac{\eta}{\hat{v}_k +\epsilon} \hat{m}_k $$
The normalization of $m_k$ and $v_k$ is used to ensure that $\delta$ and $\gamma$ not dominate in early iterations.
Typical params for RMSProp: $\eta = 0.001, \gamma = 0.9$.
Typical params for ADAM: $\eta = 0.001, \delta = 0.9, \gamma = 0.999$. 

## Batch Normalization
### Issues of variance shift
Training assumes the training data are all similarly  distributed, and minibatches have similar distribution. In practice, each minibatch may have a different distribution, named as a *covariate shift*, which may occur in each layer of the network. The batch normilization is proposed to first  move  all batches to have a mean of 0 and unit standard deviation to eliminates covariate shift between batches, and then move the entire collection to appropriate position to ensure correct resolution of training data.

Batch normalization is a covariate adjustment unit that happens after the weighted addition of inputs but before the application of activation, and is done independently for each unit, to simplify computation. The adjustment occurs over individual minibatches.
$$ \text{input}: z_i, \text{output}: u_i = \frac{z_i-\mu_B}{\sigma_B}, \text{final}: \hat{z}_i = \gamma u_i + \beta $$
In this way, the training instances in the mini batch is no longer independent. Batch normalization may only be applied to some layers or even only selected neurons in the layer. It do improve both convergence rate and neural network performance. Anecdotal evidence that BN eliminates the need for dropout. To get maximum benefit from BN, learning rates must be increased and learning rate decay can be faster, since the data generally remain in the high-gradient regions of the activations. BN also needs better randomization of training data order.

## Smooth addressing overfiting
Regularized training: minimize the loss while also minimizing the weights. The regularization parameter whose value depends on how important it is for us to want to minimize the weights. Increasing the coefficient assigns greater importance to shrinking the weights, which makes greater error on training data, to obtain a more acceptable network.

Smoothness constraints can also be imposed through the network
structure. For a given number of parameters deeper networks impose more smoothness than shallow ones due to the fact that each layer works on the already smooth surface output by the previous layer.

## Drop out
During training: For each input, at each iteration, turn off 
each neuron (including inputs) with a probability. In practice, set them to 0 according to the failure of a Bernoulli random number generator with success probability $\alpha$. Backpropagation is effectively performed only over the remaining network, which is different for different inputs. Gradients are obtained only for the weights and biases from On nodes to On nodes. For the remaining, the gradient is just 0.


During test time, we will use the expected output of the neuron, which consists of simply scaling the output of each neuron by $\alpha$. Alternately, during training, replace the activation of all neurons in the network by $\alpha^{-1} \sigma$. This does not affect the dropout procedure itself, but we can use $\sigma$ as the activation during testing, and not
modify the weights.

For a network with a total of $N$ neurons, there are $2^N$
possible sub-networks. Hence, the dropout samples over all $2^N$ possible networks, and the effective network is the averages over all possible networks. Dropout forces the neurons to learn denser patterns with redundancy.

### Variation of dropout
**Zoneout**: For RNNs, randomly chosen units remain unchanged across a time transition.
**Dropconnect**: Drop individual connections, instead of nodes
**Shakeout**: Scale up the weights of randomly selected weights $w \rightarrow \alpha w + (1- \alpha) c $, and fix remaining weights to a negative constant $ w \rightarrow -c $.
**Whiteout**: Add or multiply weight-dependent Gaussian noise to the signal on each connection.

## Additional Heuristic
**Early Stoping**: Continued training can result in over fitting to training data. Hence, track performance on a held-out validation set, and apply one of several early-stopping criterion to terminate training when performance on validation set degrades significantly.

**Gradient Clipping**: High derivative can result in instability. Gradient clipping set a ceiling on derivative value
( typical value is 5).

**Data Augumentation**: Available training data will often be small, and we can extend it by distorting examples in a variety of ways to generate synthetic labelled examples, e.g. rotation, stretching, adding noise, other distortion.

**Normalize Input**: Normalize entire training data to make it 0 mean, unit variance, which is equivalent of batch norm on input.
**Initialization techniques**: Xavier, Kaiming, SVD, etc. The key point is that neurons with identical connections that are identically initialized will never diverge.