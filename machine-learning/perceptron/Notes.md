## Perceptron

Perceptron is a very simple linear model for classification where we have an assumption that our data is linearly separable.
What that means is that it is possible to draw a hyper-plane through the space such that all points of the first class
lie on one side of it while all points of the second class lie on the other side of it.

This assumption is a very stringent constraint in low dimensions but in high dimensions it becomes very likely that the data
will be linearly separable. Infact, for infinite dimensional space, the data will always be linearly separable. So, contrary
to kNN, we should use perceptron in high dimensional spaces. But just because the data can be mapped to a higher dimensional
space to make it linearly separable does not mean that a model that does that is going to generalize well. In some cases the
mapping might make sense while in other cases it might not.

    While a non-linear mapping may allow you to separate an existing data set, you should worry if such a mapping will "generalize to unseen data. 
    For example, you can design a transformation that maps all positive examples onto the point 1 (on a 1-d line) and all negative examples onto the point 0 (on a 1-d line). 
    All other points will be mapped arbitrarily. Will such a mapping be useful?

[1] https://stats.stackexchange.com/questions/33437/is-it-true-that-in-high-dimensions-data-is-easier-to-separate-linearly

[2] https://www.quora.com/Are-problems-of-high-dimensionality-linearly-separable


### Convergence

IF the data is linearly separable, it can be proved that the Perceptron Learning Algorithm will converge. When all the points
in the space are rescaled such that all their norms are <= 1, then given a perfectly separating hyperplane w*, such that the 
nearest data point to it is at distance d, it can be shown that the learnt weight vector w will converge towards w* in
(1 / (d ^ 2)) steps.