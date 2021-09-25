# Convolution Neural Network
## Shift Invariance && Scanning
MLP assumes all recordings are exactly the same length. Conventional MLPs are sensitive to the location of the pattern, and moving of one component can lead to distinct results. The required network must be shift invariant. The solution is to scan over the input data. The entire operation can be viewed as a single giant network composed of many identical subnets (one per window), and the network is shift invariant. In a scanning MLP each neuron is connected to a subset of neurons in the previous layer. The weights matrix is sparse, and block structured with identical blocks. The network is a shared-parameter model. Equivalently, the network is composed with repeating subnets.

The operations in scanning the input with a full network can be equivalently reordered as scanning the input with individual neurons in the first layer to produce scanned maps of the input, and then jointly scanning the maps of outputs by all neurons in the previous layers by neurons in subsequent layers. We can distribute the pattern matching over two layers and still achieve the same block analysis at the second layer. The first layer evaluates smaller blocks of pixels, and the next layer evaluates windows of outputs from the first layer which effectively evaluates the larger window of the original image. The higher layer implicitly learns the arrangement of sub patterns that represents the larger pattern (the flower in this case). The CNN is typically for 2D or higher dimension, and the 1D version is named as a Tim-delay neural network.

The distributing reduces the number of parameters, because of the existence of identical neural units.

Normally, the scanning maintains or reduces the time resolution of the signal at each layer. If we want to increase the time resolution through layers, there is a problem of losing intermediate information from the previous layer. The common technique is synthetically fill in the missing intermediate values of the previous layer, e.g. with zeros or interpolation values. The expanding operations are sometimes called transpose convolution.

CNN is shift invariant but not retain rotation/scale/reflection invariance. Hence, we should generate different filter to produce transformed maps so that the network becomes invariant to all the transforms considered.

Finding bounding box, two output, one for label and the other for bounding box location. The divergence minimized is the sum of the cross entropy of the classify layer and the L2 loss of the bounding box predictor.

The depth-wise convolution, performs the convolution only once. Then conduct weighted sum to get the output. It's efficient to reduce the model size.

**Receptive Field**: The pattern in the input image that each neuron responds to. The actual receptive field for a first layer neuron is simply its arrangement of weights, while for the higher layer neurons, the actual receptive field is not immediately obvious and must be calculated. The deeper tge layer, the larger the receptive field of each neuron.

**Flattening**: The  restructuring of the maps into a
vector before passing them to the final softmax or MLP.


### NeoConitron
The layer is composed of S-type and C-type neurons. The S-type responds to the signal in the previous layer, and the C-type confirm the S-types’ response. In each subsequent module, the planes of the S layers detect plane-specific patterns in the previous layer (C layer or retina). The planes of the C layers “refine” the response of the corresponding planes of the S layers. The S-type is RELU like activation, and the C-type is also RELU like but with an inhibitory bias.

**Unsupervised Learning**: Randomly initialize S layers, perform Hebbian learning updates in response to input. Within any layer, at any position, only the maximum S from all the layers is selected for update. Updates are distributed across all cells within the plane. The Winner-take-all strategy makes it robust to distortion. The unsupervised learning do learn semantic visual concepts. The supervision can be added into this, resulting in convolution neural network, CNN. The S-type corresponds to the scanning with convolution, while the C-type is the affine map with max-pooling. Each filter learns different patterns.

The convolution neural network is a supervised version of a computational model of mammalian vision. It includes: convolution layers comprising learned filters that scan the outputs of the previous layer; and down sampling by pooling layers that vote over groups of outputs from the convolution layer. Convolution can change the size of the output. This may be controlled via zero padding. Regular convolution layers with stride > 1 also perform down sampling

## Parameters in the CNN

### Convolution Size
Input $ N \times N$, filter $ M \times M$, and stride $S$, resulting in the output of $ \lfloor (N-M)/S \rfloor + 1 $ (assuming not allowed to go beyond the edge of the input).

Situation of padding to ensure the input and output have the same size. The $P_L$ and $P_R$ are chosen such that: $|P_L -P_R| < S$ and $P_L + P_R = M-1$ for stride of $S =1$. 

**Max pooling and down sampling**: Max pooling scans with a stride of 1 confer jitter-robustness, but do not constitute down sampling. Down sampling requires a stride greater than 1.Typically, max pooling is conducted without zero pad.

### Parameters in each convolution layer and down sampling/pooling layer
For each convolution layer:
- Filter parameter $K_i \times L_i \times L_i$, and stride $S_i$.

For  each down sampling/pooling layer:
- Filter $P_i \times P_i$, and the stride $D_i$.

Input layers, $H \times W \times D$.

After convolution, $K_1 \times \lfloor (H-L_i)/S_i \rfloor + 1 \times \lfloor (W-L_i)/S_i \rfloor + 1  $.

After down sampling, $K_2 \times \lfloor\rfloor \times \lfloor \rfloor $.


## Computing the divergence of shared parameters
Each of the individual terms can be computed via back propagation, and the divergence pf shared parameters acts as the sum of each individual terms.
$$ \frac{d Div}{d w^{S}} = \sum_{e \in S} \frac{\partial Div}{\partial w^e} $$
where $S$ represents the set of edges have a common value.

## Back propagation in CNN
Convolution layer + down sampling/pooling layer
### For convolution layers
Activation, same as previous. The convolution layer, the accumulation of different components. The derivation is also a convolution operation with a flipped filter. For convolution with stride, it's the same except conducting down sampling to obtain corresponding derivative.

### For pooling layers (down sampling/up sampling map)
E.g. derivative of max pooling, the derivation also only considers the largest from a pool of elements. The mean pooling is similar except computing the mean of a pool of elements. The derivative of mean pooling is distributed uniformly across the pool. The up sampling follow the same procedure, except we drop out the derivative for filled zero.


