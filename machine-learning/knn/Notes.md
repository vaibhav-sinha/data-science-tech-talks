## k - Nearest Neighbour

Basic idea of this very simple algorithm is to find the distance between features of the test datapoint and the training
datapoints and select the k points which are nearest to the test datapoint. Then select the label that the majority of
the k datapoints have. Since we take majority, k needs to be odd and generally has values between 1 and 7. For multi-class
classification, if there is a tie, then a good approach is to predict for that datapoint with a lower value of k.

The underlying assumption of this algorithm is that datapoints which are close in feature space also have the same label.
If intuitively the dataset would violate this assumption, then this would be a bad algorithm for that specific problem.

While we do not have a training phase in this algorithm, evaluating the algorithm on the training set would have 0 error
for k=1 and the error would keep growing for higher values of k. The real measure of error should obviously be done on
the test set.

kNN does not make any assumptions about the underlying data and hence can adapt to any situation. Hence it has a low bias.
But it is very sensitive to the exact points used for training and hence it is not very stable and has high variance.

Kernel methods are actually just a modification of kNN where the weights of points decay smoothly rather than in a binary
manner. In high dimensional spaces distance kernels are modified to emphasize some features more than others.

There are two things that need to be decided to implement this algorithm:

1. The value of k
2. The distance measure

#### k

k is a hyperparameter of the algorithm and we can try out different values of it to figure out which one works best for
our dataset.

#### Distance measure

We can choose any distance measure that we like and one might make more sense for a given problem than another. The most
common distance measure that is used is called Minkowski distance. 

                                            dist(x, z) = (sum(|xr - zr| ^ p))^(1/p)

Here `p` is a parameter that we need to choose. For p = 1, this becomes the Manhattan distance. For p = 2, it becomes
the Euclidean distance and for p = infinity, it becomes the `max` function.

If the feature vector has a high dimension, then lower values of p (even < 1) should be preferred as with high dimension
the difference in distance becomes almost 0 between the nearest and the farthest neighbour. Hence, the algorithm isn't
meaningful anymore.

[1] https://stats.stackexchange.com/questions/99171/why-is-euclidean-distance-not-a-good-metric-in-high-dimensions/99191#99191



### Usage

While it is a basic algorithm, it is actually useful in certain scenarios. As mentioned in the reference, 

    If you have a system that has to learn a sophisticated (i.e. nonlinear) pattern with a small number of samples (and usually dimensions), 
    K-NN models are usually one of the best options out there.

Also, it can be used smartly for certain problems. Example for the reference is as follows

    I'll often use it for image classification. That's right, the thing that everyone brings out the big guns (deep convolutional networks) for. 
    I use it when different colors mean clearly different objects. Classifying terrain from satellite images, for example. 
    Blue is almost always water. White is either snow or clouds (a flood fill and perimeter and area calculation will help differentiate).

Above is the case where the space is very high dimensional but the actual data has a low intrinsic dimensionality. So the
data actually takes a small sub-space or sub-manifold and hence kNN can still work. Algorithms like SVD and PCA try and
find the sub-space in which the data actually resides and draw a new coordinate system there so that the dimensionality
of the space then becomes the same as the low intrinsic dimensionality of the data.

Another example of data in high dimensional space but with low intrinsic dimensionality is images of faces. While there
may be 10s of thousands pixels in the image of a face, the face can actually be described by 10s of attributes. Hence if we can
convert the pixel information to some 10s of features then kNN can be used on those features.

[2] https://www.quora.com/Do-people-in-the-industry-actually-use-the-K-Nearest-Neighbor-algorithm-in-practice



### Data 

For kNN to work, data normalization is needed. If data is not normalized, then due to different ranges of different
features, one feature might dominate all the other.

    With kNN you need to think carefully about the distance measure. For instance, if one feature is measured in 1000s of kilometers, 
    another feature in 0.001 grams, the first feature will dominate the distance measure. You can normalize the features, 
    or give certain importance weights, based on the domain knowledge.

In high dimensional space, the amount of data needed for coverage of the space is going to be very high. Will less data
and high dimensional space, overfitting would happen. 

[3] https://qr.ae/pNvnPq



### Parameters

While it may look like there is only 1 parameter k for the algorithm, actually effective number of parameters are N/k.
This is because there are N/k neighbourhoods created by the training set and we a one parameter (the mean) in each one of them.

[4] https://stats.stackexchange.com/questions/357524/why-is-n-k-the-effective-number-of-parameters-in-k-nn



### Performance

1. The training time performance is really good since there is no real training that is done. The model just memorizes
the entire training data by storing it as is.

2. The test time performance is bad especially when the number of training data points is large, which they need to be
if the dimensionality is large. This is because at test time, the distance of test data point needs to be computed with 
respect to every point in the training data set across each of the dimensions. Hence the test time complexity is O(nd). 