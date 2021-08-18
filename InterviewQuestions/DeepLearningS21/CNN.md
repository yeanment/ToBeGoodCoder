# Convolutional Neural Network
## Shift Invariance && Scanning
MLP assumes all recordings are exactly the same length. Conventional MLPs are sensitive to the location of the pattern, and moving of one component can lead to distinct results. The required network must be shift invariant. The solution is to scan over the input data. The entire operation can be viewed as a single giant network composed ofmany identical subnets (one per window), and the network is shift invariant. In a scanning MLP each neuron is connected to a subset of neurons in the previous layer. The weights matrix is sparse, and block structured with identical blocks. The network is a shared-parameter model. Equivalently, the ntework is composed with repeating subnets.

The operations in scanning the input with a full network can be equivalently reordered as scanning the input with individual neurons in the first layer to produce scanned maps of the input, and then jointly scanning the maps of outputs by all neurons in the previous layers by neurons in subsequent layers. We can distribute the pattern matching over two layers and still achieve the same block analysis at the second layer. The first layer evaluates smaller blocks of pixels, and the next layer evaluates windows of outputs from the first layer which effectively evaluates the larger window of the original image. The higher layer implicitly learns the arrangement of sub patterns that represents the larger pattern (the flower in this case). The CNN is typically for 2D or higher dimension, and the 1D version is named as a Tim-delay neural network.

The distributing reduces the number of parameters, becaouse of the existance of identical neural units.

Normally, the scanning maintains or reduces the time resolution of the signal at each layer. If we want to increase the time resolution through layers, there is a problem of losing intermediate information from the previous layer. The common technique is synthetically fill in the missing intermediate values of the previous layer, e.g. with zeros or interpolation values. The expanding operations are sometimes called transpose convolution.

CNN is shift invariant but not retain rotation/scale/reflection invariance. Hence, we should generate different filter to produce transformed maps so that the network becomes invariant to all the transfroms considered.

Finding bounding box, two output, one for label and the other for bounding box location. The divergence minimized is the sum of the cross entropy of the calssify layer and the L2 loss of the bounding box predictor.

The depth-wise convolution, performs the convolution only once. Then conduct weighted sum to get the output. It's efficient to reduce the model size.

**Receptive Field**: The pattern in the input image that each neuron responds to. The actual receptive field for a first layer neuron is simply its arrangement of weights, while for the higher layer neurons, the actual receptive field is not immediately obvious and must be calculated.

**Flattening**: The  restructuring of the maps into a
vector before passing them to the final softmax or MLP.


## Computing the divergence of shared parameters
Each of the individual terms can be computed via backpropagation, and the divergence pf shared parameters acts as the sum of each individual terms.
$$ \frac{d Div}{d w^{S}} = \sum_{e \in S} \frac{\partial Div}{\partial w^e} $$
where $S$ represents the set of edges have a common value.

## Backpropagation in CNN
Convolution layer + downsampling/pooling layer
### For convolutional layers
Activation, same as previous. The convolution layer, the accumulation of different components. The derivation is also a convolution operation with a flipped fliter. For convolution with stride, it's the same except conducting downsampling to obtain corresponding derivative.

### For pooling layers (downsampling/upsampling map)
E.g. derivative of max pooling, the derivation also only considers the largest from a pool of elements. The mean pooling is similar except computing the mean of a pool of elements. The derivative of mean poooling is distributed uniformly across the pool. The upsampling follow the same procedure, except we drop out the derivative for filled zero.


