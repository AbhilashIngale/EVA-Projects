#                                                    Architectural Basics 
The concepts discussed below are arranged in the order of their occurance while writing a Neural Network Architecture.

## Q.1 3x3 Convolution

In CNNs, the most common kernel used is a 3x3 kernel. In earlier years, 5x5 and 11x11 type kernels were also used but the recent practice is of using 3x3 kernel. This is due to following reason reasons -

 the total number of tunable weights in a 3x3 kernels are 9 and two 3x3 convolutions one after the other can give you the same effect as that of a single 5x5 but with lesser weights (18) as compared to 5x5 (25). Now-a-days, all GPUs are optimized for 3x3 operations.

## Q.2 Receptive Field 

Receptive field of a particular layer is defined as the dimension (in pixels) of the original image visible as seen by the network. 

So a 3x3 kernel when convolved on top of input image will have a receptive field of 3x3, this is also called local receptive field. In other words, local receptive field is the effective area as seen till a particular layer. Global receptive field is the receptive field at the last layer of the convolution. In an ideal network, for an ideal dataset (where size of object is almost equal size of image) , the global receptive field should be close to or approximately equal to the image size.

## Q.3 1x1 Convolution 

1x1 Convolution is a point wise convolution. It is used for both - increasing the number of channels as well as decreasing the number of channels. In simple use cases, we use it to combine the channels before/ after max-pooling. 1x1 is used for increasing the number of channels in Pixel Shuffle, used for Super-resolution

## Q.4 How many Layers? 

If we have an image of 100x100 then typically (without MaxPool) , we need 50 layers (49 convolution operations) as there is a dimension reduction of 2 pixels every time we convolve on top an input image with a 3x3 kernel.

If we are using Max Pooling then the numbers of layers required reduces.

## Q.5 Max Pooling
In Max-Pooling we sort the features from a particular region that speak the loudest, meaning only those features that contain the most information are passed onto the next layers 

## Q.6 Position of Max-Pooling

Max Pooling is done once constituents of an image (edges and gradients in former layers, textures and patterns in middle layers and parts of objects in) start forming and we want to extract the loudest or most prominent features from the image. We do no use Max-Pooling very near to the prediction layer to avoid the loss of information.

## Q.7 The distance of MaxPooling from Prediction
It is a good practice to avoid the use of Max Pooling near the prediction layer, as we loose 75% of the information from previous layer. Hence, we generally keep a distance of 2-3 code blocks from prediction layer.

## Q.8 Concept of Transition Layers,
Max pooling and 1x1 together consist of the the transition layer. Transition layer is called so because we re-initiate our number of kernels after this layer according to our choice and this layer clearly marks the end of previous layer and start of next layer.  

## Q.9 Position of Transition Layer
Transition layer should be placed right after the end of a layer i.e. the architecture should be something like this- 
Layer 1--Transition Layer --Layer 2-- Transition Layer-- Layer 3 ... etc 

## Q.10 SoftMax, 

Softmax is an activation function use to convert predictions in "probability - like " form, meaning it gives a number between 0-1. Softmax is used when we have to distinguish a class from another having very close values. In such cases, softmax separates the prediction values by a larger margin so that it is clear to distinguish the class.

## Q.11 Learning Rate

Learning Rate (LR) is simply the rate at which an improvement / update in the parameters is made. Learning rate is a hyper-parameter meaning, the network architect has to figure out best values for a specific network. 

## Q.12 Kernels and how do we decide the number of kernels

Kernels are feature detectors/ filters that output a particular Feature Map when we convolve them on top of an input image. This feature map contains condensed information about all the pixels in the image. 3x3 and 1x1 are both examples of kernels.

Number of kernels used in a network depends upon the end application. Usually, we need more kernels to extract more information from the image. However, due to computation and cost requirements, we follow a geometrically increasing ratio in the number of kernels like - 32,64,128,256.... and so on. If the hardware permits then we could start from a higher number of kernels too. 

## Q.13 Batch Normalization
Batch Normalization is the process of normalizing all the feature values so that they fall into the same range and hence, work efficiently with backpropagation. As we know that we normalize the input image so that all the values no longer lie between 0-255 but 0-1, we could do the same for the inputs to the next layers. So batch normalization has to be applied before every convolution layer (except the input).

## Q.14 The distance of Batch Normalization from Prediction 
Batch Normalization is genrally used right before the final convolution layer. So distance is usually 0 r 1. 

## Q.15 Image Normalization  
Image Normalization changes the pixel range values of an image so that it is better suited for extracting features and for segmentation. It is also called as histogram stretching or contrast stretching.  

## Q.16 Number of Epochs and when to increase them ? 
One epoch is when all the training data has been passed through the backpropagation once. So we increase the number of epochs when we observe under-fitting or the model has nt been trained completely. One quick way to identify this is to plot a graph of training accuracy Vs. number of epochs. If the graph does not settle for a training accuracy value then, we know that we need more epochs for training.  

## Q.17 DropOut
Dropout is defined as blocking some randomly selected feature extractors of the network so that the other kernels learn unique features of the data. Generally, dropout reduces training accuracy and increases test accuracy, as the network is not learning training-data-specific features. 

## Q.18 When do we introduce DropOut, or when do we know we have some overfitting ?
Dropout is generally introduced when we observe that the model is overfitting the training data. We say that a model is overfitting the training data if it shows high difference in training accuracy and validation / test accuracy. This generally implies that 

## Q.19 Batch Size, and effects of batch size
When working with datasets, we often find that we are unable to fit all the training data into one particular object due to GPU memory limitations. So we make random batches of data with a particuar sample size called batch_size (hyper-parameter) and send these batches for training one after the other. 
The main advantage of using batch-wise approach is that we can avoid local minimas more efficiently. 
Batch size tuning is important since the training accuracy relies heavily on it. If we choose a large batch size, the model does not have enough information to generalize and hence we see a comparatively lower train accuracy in such cases. However, too much small of a batch size could potentially cause overfitting.
Selecting a proper batch_size is a matter of experimentation and intuition.

## Q.20 Adam vs SGD
Adam is an adaptive learning optimzer while SGD (Stochastic Gradient Descent) is a very simple approach for optimizing. However, the choice of SGD or Adam depends on the dataset. If input data is sparse then it is better to use an adaptive optimzer like Adam. Adam also provides faster convergence so it is better suited for most applications.   

## Q.21 How do we know our network is not going well, comparatively, very early ?
If we do not observe that there is a substantital increase in the training accuracy in the first 2-3 epochs then we can safely conclude that our network is not correctly defined.

## Q.22 LR schedule and concept behind it
The learning rate is the most important factor when a model starts converging. In such cases, it might happen that the model did not proceed to its global minima due to a higher learning rate. This skipping of the global minima happens because the leaerning rate or improvement step was higher as compared to its distance from the global minima. Hence, we use LR scheduling in which we tune the learning rate with accuracy or with number of epochs so that the LR is revised at every epoch and its value keeps on reducing as the model approaches convergence.

## Q.23 When to add validation checks ?
In a given dataset with train and test data, we could split the train data even more into 9:1 or 8:2 ratio such that the former major part is used for training the weights in the network and the latter part (validation data) is used to evaluate the performance, without touching the test data. This gives the network architect a good idea about the performance of model on unknown data. In some cases, this validation data is not kept fixed but shuffled so that the model is genralized (cross validation)

## Q.24 When do we stop convolutions and go ahead with a larger kernel or some other alternative ? 
We stop convolution operation when we reach very close to the required receptive field i.e. if we have a dataset where size of object is approximately equal to the size of image, then we stop once we reach the receptive field equal to image size. We could use a larger kernel at any layer for image dimensionality reduction.
Some other alternatives to this are Global Average Pooling (GAP) and Global Max Pooling.
