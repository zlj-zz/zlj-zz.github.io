# Tips for Training DNN

Recipe of Deep Learning

![](/images/deep-learn-1/recipe-of-deep-learning.png)

Do not always blame Over fitting

dropout for good results on testing data

## Vanishing Gradient Problem

## ReLU

- Rectified Liner Unit(ReLU)

  **Reason** :

  1. Fast to compute
  2. Biological reason
  3. Infinite sigmoid with different biases
  4. Vanishing gradient problem

  ![](/images/deep-learn-1/relu.png)

### ReLU - variant

![](/images/deep-learn-1/relu-variant.png)

## Maxout

**ReLU is a special cases of Maxout**.

- Learnable activation function

  ![](/images/deep-learn-1/maxout.png)

  You can have more than 2 elements in a group.

### Maxout - Training

- Given a training data $x$, we know which $z$ would be the max.
- Train this thin and linear network.
  Different thin and linear network for different example.

### RMSProp

$w^{1} \leftarrow w^{0} - \frac{\eta}{\sigma^{0}}g^{0} \qquad \sigma^{0}=g^{0}$

$w^{2} \leftarrow w^{1} - \frac{\eta}{\sigma^{1}}g^{1} \qquad \sigma^{1}=\sqrt{\alpha(\sigma^{0})^{2} + (1-\alpha)(g^{1})^{2}}$

$w^{3} \leftarrow w^{2} - \frac{\eta}{\sigma^{2}}g^{2} \qquad \sigma^{2}=\sqrt{\alpha(\sigma^{1})^{2} + (1-\alpha)(g^{2})^{2}}$

.......

$w^{t+1} \leftarrow w^{t} - \frac{\eta}{\sigma^{t}}g^{t} \qquad \sigma^{t}=\sqrt{\alpha(\sigma^{t})^{2} + (1-\alpha)(g^{t})^{2}}$

Root Mean Square of the gradients with previous gradients being decayed.

## Regularization

- New loss function to be minimized

  - Find a set of weight not only minimizing original cost but also close to zero.

  ![](/images/deep-learn-1/regularization.png)

  ![](/images/deep-learn-1/regularization-2.png)

## Dropout

**Training**

- Each time before updating the parameters.

  - Each neuron has p% to dropout.

    The structure of the network is changed.

  - Using the new network for training

_For each mini-batch, we resample the dropout neurons._

**Testing**

No dropout

- If the dropout rate at training is p%, all the weights times (1-p)%
- Assume that the dropout rate is 50%.

  If a weight $w=1$ by training, set $w=0.5$ for testing

### Dropout - Intuitive Reason

- When teams up, if everyone expect the partner will do the work, nothing will be done finally.
- However, if you know your partner will dropout, you will do better.
- When testing, no one dropout actually, so obtaining good results eventually.

![](/images/deep-learn-1/dnn.png)
