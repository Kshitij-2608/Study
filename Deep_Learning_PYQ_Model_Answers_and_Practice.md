# Deep Learning PYQs, Extra Questions, Probable Questions, and Model Answers

Sources used:
- `Deep Learning_ESE Question paper format  AY 23-24.pdf`
- `Deep Learning_ESE Question paperAY 24-25 (2).pdf`
- `Syllabus.pdf`
- `Unit 01.pptx` to `Unit 06.pptx`

Use this file after reading `Deep_Learning_ESE_Study_Material_Enhanced.md`. This file is question-answer focused.

---

# 1. PYQs from 2023-24 with Model Answers

## Q1(a). Mention considerations and strategies for choosing the most appropriate activation function.

**Model answer:**  
Choose an activation function based on task type, output range, gradient behavior, and training stability. For binary classification, sigmoid is suitable at the output because it gives a probability between `0` and `1`. Hidden layers should use non-linear activations so the network can learn complex patterns. Avoid activation choices that make gradients too small, because this slows learning during backpropagation.

## Q1(b). Explain with example the hyperparameter tuning process using GridSearchCV to evaluate a neural network.

**Model answer:**  
Hyperparameters are values selected before training, such as learning rate, dropout rate, optimizer, batch size, epochs, and number of neurons. Grid search tests multiple combinations of these values and selects the best one based on validation performance.

Example:

```text
learning_rate = [0.01, 0.001]
dropout = [0.2, 0.3]
optimizer = [SGD, Adam]
```

The model is trained using each combination. The combination that gives the best validation accuracy or lowest validation loss is selected. This improves generalization and helps avoid overfitting.

## Q2(a). Explain covariate shift and internal covariate shift in batch normalization. How does batch normalization stabilize and speed training?

**Model answer:**  
Covariate shift means the distribution of input data changes. Internal covariate shift means the distribution of activations inside hidden layers changes during training because weights and biases are continuously updated.

Batch normalization reduces this problem by normalizing activations in each mini-batch. It computes batch mean and variance, normalizes activations, then rescales and shifts them using learnable parameters `gamma` and `beta`. This keeps layer inputs stable, improves gradient flow, reduces sensitivity to initialization, and speeds up convergence.

## Q2(b). Perform one epoch of forward and backward pass for a sigmoid neural network.

**Model answer format:**  
Use this structure when a diagram/weights are given.

Forward pass:

```text
z = w1x1 + w2x2 + ... + b
a = sigmoid(z)
```

Loss:

```text
L(a,y) = -[y log(a) + (1-y) log(1-a)]
```

Backward pass for sigmoid output:

```text
dz = a - y
dw = dz*x
db = dz
```

Update:

```text
w_new = w - alpha*dw
b_new = b - alpha*db
```

If there is a hidden layer, first calculate output-layer gradients, then pass the error backward to hidden neurons using the chain rule.

## Q3(a). Calculate CNN for 28x28 grayscale image.

Given:

```text
Input = 28 x 28 x 1
Conv1: k=16, f=5, s=1, p=2
Pool1: f=2, s=2
Conv2: k=32, f=5, s=1, p=2
Pool2: f=2, s=2
Classes = 10
```

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv params = (f*f*input_depth + 1)*filters
```

| Layer | Output shape | Parameters |
|---|---:|---:|
| Conv1 | `28 x 28 x 16` | `(5*5*1+1)*16 = 416` |
| Pool1 | `14 x 14 x 16` | 0 |
| Conv2 | `14 x 14 x 32` | `(5*5*16+1)*32 = 12,832` |
| Pool2 | `7 x 7 x 32` | 0 |
| Flatten | `1568` | 0 |
| FC to 10 classes | `10` | `(1568+1)*10 = 15,690` |

Total parameters including FC:

```text
416 + 12,832 + 15,690 = 28,938
```

## Q3(b). What is the need of data augmentation? Explain four image augmentation techniques.

**Model answer:**  
Data augmentation increases the effective size and variety of training data. It helps reduce overfitting because the model sees different versions of the same image instead of memorizing fixed images.

Techniques:
- Rotation: rotates image by a small angle.
- Flipping: mirrors image horizontally or vertically.
- Scaling: zooms image in or out.
- Shifting: moves image slightly left, right, up, or down.

Example: A cat image can be rotated, flipped, or shifted so the CNN learns the cat shape under different conditions.

## Q3(c). Define depth in convolution layer with example.

**Model answer:**  
Depth means the number of channels or feature maps. A grayscale image has depth `1`, while an RGB image has depth `3`. If an input is `32 x 32 x 3` and the convolution layer uses 6 filters, the output depth becomes `6`, so output shape may become `28 x 28 x 6`.

## Q4(a). Explain LSTM networks with gates and state the loss function.

**Model answer:**  
LSTM is a recurrent network designed to remember long-term dependencies. It has a cell state for long-term memory and hidden state for short-term output. Three gates control information flow:

- Forget gate: decides what old information to remove.
- Input gate: decides what new information to store.
- Output gate: decides what information to send as hidden state.

Important formulas:

```text
C_t = f_t*C_(t-1) + i_t*C~_t
h_t = o_t*tanh(C_t)
```

For classification, cross-entropy loss is commonly used. For sequence prediction, loss is calculated between predicted output sequence and actual output sequence.

## Q4(b). How does L2 regularization reduce overfitting?

**Model answer:**  
L2 regularization adds a penalty for large squared weights to the cost function. This pushes weights toward smaller values and makes the model simpler. A simpler model is less likely to memorize training data, so it generalizes better to test data. L2 is also called weight decay.

## Q4(c). Write scenarios where transfer learning is most effective.

**Model answer:**  
Transfer learning is most effective when:
- the new dataset is small;
- the source and target tasks are related;
- training from scratch is expensive;
- faster training is required;
- a pre-trained model already learns useful features.

Example: Use a pre-trained CNN for image features and replace the final classifier for a new image classification task.

## Q5(a). Explain generator and discriminator in GAN architecture.

**Model answer:**  
A GAN has two parts: generator and discriminator. The generator takes random noise and creates fake/generated samples. The discriminator receives real and generated samples and predicts whether each sample is real or fake.

The generator tries to fool the discriminator. The discriminator tries to correctly identify fake samples. This adversarial training improves both models. Example: in image generation, the generator creates fake images and the discriminator judges if they look real.

## Q5(b). Compare Stochastic Gradient Descent with Batch Gradient Descent.

**Model answer:**  
Batch gradient descent computes gradients using the entire dataset before one update. It is stable but slow for large datasets. SGD computes gradients using one training example at a time. It updates frequently and may converge faster, but its path is noisy. Mini-batch GD is often preferred because it balances speed and stability.

## Q5(c). Explain the process of image generation using Hugging Face.

**Model answer:**  
The process uses a pre-trained generative model. A user gives a text prompt, the model converts it into a representation, and the generator produces an image matching the prompt. The output can be refined by changing the prompt or model settings. In exam, write this as a high-level pipeline: prompt input -> pre-trained generative model -> generated image output.

## Q6(a). Illustrate RNN architecture with example. Explain vanishing and exploding gradient.

**Model answer:**  
RNN processes sequential data by passing hidden state from one time step to the next.

```text
h_t = activation(Wxh*x_t + Whh*h_(t-1) + b)
```

Example: In sentiment analysis, each word is processed in order, and the hidden state stores context from earlier words.

Vanishing gradient happens when gradients become very small during backpropagation through time, so long-term dependencies are not learned. Exploding gradient happens when gradients become very large, causing unstable training and sudden loss spikes.

## Q6(b). List applications of memory networks.

**Model answer:**  
Applications include question answering, dialogue systems, reading comprehension, information retrieval, and tasks where the model must remember facts or context before producing an answer.

## Q6(c). Write a short note on bounding boxes and anchor boxes.

**Model answer:**  
Bounding boxes locate objects using coordinates and size. In object detection, a box may be represented by `bx`, `by`, `bh`, and `bw`. Anchor boxes are predefined shapes assigned to grid cells. They help detect multiple objects or objects with different shapes in the same grid cell.

---

# 2. PYQs from 2024-25 with Model Answers

## Q1(a). Analyze why Adam converges faster than SGD.

**Model answer:**  
SGD updates parameters using gradients directly and may converge slowly or noisily. Adam uses adaptive update behavior and momentum-like past-gradient information, so each parameter can receive a more suitable update. Therefore, even with learning rate `0.001`, Adam can converge faster than SGD with `0.01`.

## Q1(b). In what way is logistic regression a simplified neural network?

**Model answer:**  
Logistic regression has input features, weights, bias, weighted sum, sigmoid activation, and output probability. These are the same basic components of a neural network. It is called simplified because it has no hidden layers and is mainly used for binary classification.

## Q1(c). Dropout rate is 0.3 and layer has 100 neurons. How many are active?

**Answer:**

Formula:

```text
Active neurons = total neurons * (1 - dropout rate)
```

```text
Active fraction = 1 - 0.3 = 0.7
Active neurons = 100 * 0.7 = 70
```

So, `70 neurons` are active.

## Q2(a). Model has 99% training accuracy and 75% testing accuracy. Which hyperparameter tuning techniques help?

**Model answer:**  
This indicates overfitting. Improve generalization by tuning dropout rate, L1/L2 regularization strength, learning rate, number of layers, number of neurons, batch size, and epochs. Use early stopping to stop training when validation loss increases. Grid search can test combinations and select the best validation performance.

## Q2(b). Compute sigmoid output for `x=0.9`, `w=-0.6`, `b=0.2`; then for `w=-0.3`.

Formula:

```text
z = wx + b
sigmoid(z) = 1 / (1 + e^-z)
```

Case 1:

```text
z = wx + b = (-0.6)(0.9) + 0.2 = -0.34
a = sigmoid(-0.34) = 0.4158
```

Case 2:

```text
z = (-0.3)(0.9) + 0.2 = -0.07
a = sigmoid(-0.07) = 0.4825
```

Output increases because the weight becomes less negative.

## Q2(c). Initial weight `w=0.4`, gradient `-0.1`. Compare learning rates `0.01` and `0.1`.

Formula:

```text
w_new = w - eta*gradient
```

For `eta = 0.01`:

```text
w_new = 0.4 - 0.01(-0.1) = 0.401
```

For `eta = 0.1`:

```text
w_new = 0.4 - 0.1(-0.1) = 0.41
```

Learning rate `0.1` gives a faster update.

## Q3(a). Calculate CNN for 32x32 color image and mention FC parameters.

Given:

```text
Input = 32 x 32 x 3
Conv1: k=6, f=5, s=1, p=0
Pool1: f=2, s=2
Conv2: k=10, f=5, s=1, p=0
Pool2: f=2, s=2
Classes = 10
```

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*number_of_filters
FC parameters = (input_units + 1)*output_units
```

| Layer | Output shape | Parameters |
|---|---:|---:|
| Conv1 | `28 x 28 x 6` | `(5*5*3+1)*6 = 456` |
| Pool1 | `14 x 14 x 6` | 0 |
| Conv2 | `10 x 10 x 10` | `(5*5*6+1)*10 = 1,510` |
| Pool2 | `5 x 5 x 10` | 0 |
| Flatten | `250` | 0 |
| FC to 10 classes | `10` | `(250+1)*10 = 2,510` |

FC parameters = `2,510`. Total parameters including FC = `4,476`.

## Q3(b). Explain batch normalization and performance improvement.

**Model answer:**  
Batch normalization normalizes activations within each mini-batch. It calculates mean and variance, normalizes activations, then applies learnable scale `gamma` and shift `beta`. It improves performance by reducing internal covariate shift, stabilizing activations, improving gradient flow, speeding convergence, and reducing sensitivity to initialization.

## Q3(c). Explain transfer learning with example and benefits.

**Model answer:**  
Transfer learning reuses a pre-trained model for a new task. For image classification, earlier CNN layers can be used as feature extractors because they already learn edges, textures, and shapes. We replace the final classifier with new output classes and train it.

Benefits: less data required, faster training, lower computation, and better performance when new data is limited.

## Q4(a). Compare GD, SGD, and mini-batch GD.

**Model answer:**  
Batch GD uses the full dataset for each update. It is stable but slow. SGD uses one example for each update. It is faster but noisy. Mini-batch GD uses a small group of examples per update, giving a balance between speed and stability. Mini-batch is commonly used in deep learning.

## Q4(b). Compare L1 and L2 regularization. How is dropout different?

**Model answer:**  
L1 regularization penalizes absolute weights and can make some weights exactly zero, useful for feature selection/compression. L2 penalizes squared weights and shrinks weights smoothly toward zero, reducing overfitting.

Dropout is different because it does not add a penalty term to the cost. It randomly disables neurons during training, forcing the network to learn robust features.

## Q4(c). Explain role of encoders and decoders in neural network architectures.

**Model answer:**  
The encoder reads the input sequence and compresses it into a hidden/context vector. The decoder uses this vector to generate output sequence step by step. Encoder-decoder models are used for sequence-to-sequence tasks such as translation, summarization, image captioning, and speech recognition, where input and output lengths may differ.

## Q5(a). How is training different for generator and discriminator?

**Model answer:**  
The discriminator is trained to classify real samples as real and generated samples as fake. The generator is trained to create samples that fool the discriminator into classifying them as real. Thus, the discriminator improves as a classifier, while the generator improves as a creator of realistic samples.

## Q5(b). How does attention help transformers capture long-range dependencies?

**Model answer:**  
Attention allows each token to directly compare itself with all other tokens in the sequence. This direct connection helps capture relationships even between far-apart words. Unlike RNNs, transformers do not need to pass information step by step through many time steps, so long-range context is preserved better.

## Q5(c). Explain identity shortcut/skip connection in ResNet.

**Model answer:**  
An identity shortcut passes input directly to a later layer and adds it to the main path output. This helps gradients flow through deep networks and reduces vanishing-gradient difficulty. It allows deeper networks to train effectively.

## Q5(d). Is residual addition element-wise addition or concatenation?

**Answer:**  
It is element-wise addition, not concatenation.

```text
addition([1,2], [3,4]) = [4,6]
not [1,2,3,4]
```

## Q6(a). What is object detection and how does it differ from image classification? Explain sliding window.

**Model answer:**  
Image classification predicts one class label for the whole image. Object detection predicts object classes and their locations using bounding boxes, possibly for multiple objects.

Sliding window detection moves a fixed-size window across the image. Each crop is passed to a ConvNet to decide whether it contains the object. The process is repeated for different window sizes. Its drawback is high computation and less accurate localization.

## Q6(b). How to know whether model is suffering from exploding gradients?

**Model answer:**  
Signs include sudden very large loss, unstable training, extremely large weight updates, `NaN` values, fluctuating accuracy, and very large gradient magnitudes. In RNNs, exploding gradients occur because gradients are repeatedly multiplied through many time steps.

## Q6(c). Explain types of RNN with respect to application.

**Model answer:**  
One-to-one: single input to single output, like basic classification.  
One-to-many: one input to sequence output, like image captioning.  
Many-to-one: sequence input to one output, like sentiment classification.  
Many-to-many: sequence input to sequence output, like translation or speech recognition.

## Q6(d). Discuss deep learning in NLP for text classification and common models.

**Model answer:**  
Deep learning helps text classification by learning semantic and contextual features automatically. RNN processes text sequentially. LSTM remembers long-term context using gates. GRU is a simpler gated sequence model. Transformers/BERT use attention for contextual understanding and are effective for tasks such as sentiment classification and named entity recognition.

---

# 3. Extra Numerical Questions by Type with Model Answers

## 3.1 ANN / Logistic Regression / Backpropagation

### ANN-1. Compute sigmoid output.

Question: `x = 1.2`, `w = 0.5`, `b = -0.1`. Compute `z` and sigmoid output.

**Answer:**

Formula:

```text
z = wx + b
sigmoid(z) = 1 / (1 + e^-z)
```

```text
z = wx + b = 0.5(1.2) - 0.1 = 0.5
a = sigmoid(0.5) = 0.622
```

### ANN-2. Perform weight update.

Question: `w = 0.6`, gradient `dE/dw = 0.25`, learning rate `0.1`. Find updated weight.

**Answer:**

Formula:

```text
w_new = w - eta*gradient
```

```text
w_new = w - eta*gradient
w_new = 0.6 - 0.1(0.25) = 0.575
```

### ANN-3. One logistic-regression update.

Question: `x=[1,2]`, `w=[0.2,0.3]`, `b=-0.1`, `y=1`, `alpha=0.05`.

**Answer:**

Formula:

```text
z = w1x1 + w2x2 + b
a = sigmoid(z)
dz = a - y
dW = dz*x
db = dz
w_new = w - alpha*dW
b_new = b - alpha*db
```

```text
z = 0.2(1) + 0.3(2) - 0.1 = 0.7
a = sigmoid(0.7) = 0.668
dz = a - y = -0.332
dW = [-0.332, -0.664]
db = -0.332
w1_new = 0.2 - 0.05(-0.332) = 0.2166
w2_new = 0.3 - 0.05(-0.664) = 0.3332
b_new = -0.1 - 0.05(-0.332) = -0.0834
```

### ANN-4. Dropout active neurons.

Question: A layer has 120 neurons and dropout rate is 0.25. How many active neurons?

**Answer:**

Formula:

```text
Active neurons = total neurons * (1 - dropout rate)
```

```text
Active fraction = 1 - 0.25 = 0.75
Active neurons = 120*0.75 = 90
```

## 3.2 CNN

### CNN-1. Conv + pool calculation.

Question: Input `64 x 64 x 3`, Conv `k=16`, `f=3`, `p=1`, `s=1`, then max pool `f=2`, `s=2`.

**Answer:**

Formula:

```text
Conv/Pool output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*number_of_filters
Pooling parameters = 0
```

```text
Conv output size = ((64 + 2 - 3)/1) + 1 = 64
Conv output = 64 x 64 x 16
Conv params = (3*3*3 + 1)*16 = 448
Pool output = 32 x 32 x 16
```

### CNN-2. No-padding convolution.

Question: Input `28 x 28 x 1`, Conv `k=8`, `f=3`, `p=0`, `s=1`.

**Answer:**

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*number_of_filters
```

```text
Output size = ((28 - 3)/1) + 1 = 26
Output = 26 x 26 x 8
Params = (3*3*1 + 1)*8 = 80
```

### CNN-3. 1x1 convolution.

Question: Input `14 x 14 x 16`, apply `1 x 1` convolution with 32 filters.

**Answer:**

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Output depth = number of filters
Conv parameters = (f*f*input_depth + 1)*number_of_filters
```

```text
Output = 14 x 14 x 32
Params = (1*1*16 + 1)*32 = 544
```

### CNN-4. Strided convolution.

Question: Input `100 x 100 x 3`, Conv `k=10`, `f=7`, `p=0`, `s=2`.

**Answer:**

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*number_of_filters
```

```text
Output size = floor((100 - 7)/2) + 1
Output size = floor(46.5) + 1 = 47
Output = 47 x 47 x 10
Params = (7*7*3 + 1)*10 = 1480
```

## 3.3 RNN / LSTM

### RNN-1. One-step RNN hidden state.

Question: `Wxh=0.4`, `Whh=0.3`, `h0=0`, `x1=1`. Calculate `h1`.

**Answer:**

Formula:

```text
h_t = tanh(Wxh*x_t + Whh*h_(t-1) + b)
```

```text
h1 = tanh(0.4(1) + 0.3(0))
h1 = tanh(0.4) = 0.380
```

### RNN-2. Two-step RNN.

Question: `x1=1`, `x2=0`, `Wxh=0.6`, `Whh=0.5`, `h0=0`.

**Answer:**

Formula:

```text
h_t = tanh(Wxh*x_t + Whh*h_(t-1) + b)
```

```text
h1 = tanh(0.6(1) + 0.5(0)) = tanh(0.6) = 0.537
h2 = tanh(0.6(0) + 0.5(0.537)) = tanh(0.2685) = 0.262
```

### RNN-3. LSTM cell state.

Question: `C_(t-1)=0.7`, `f_t=0.5`, `i_t=0.4`, `C~_t=0.8`. Calculate `C_t`.

**Answer:**

Formula:

```text
C_t = f_t*C_(t-1) + i_t*C~_t
```

```text
C_t = f_t*C_(t-1) + i_t*C~_t
C_t = 0.5(0.7) + 0.4(0.8)
C_t = 0.35 + 0.32 = 0.67
```

### RNN-4. Exploding gradient diagnosis.

Question: Training loss suddenly becomes `NaN` and weight updates are extremely large. Which problem is likely?

**Answer:**  
The model is likely suffering from exploding gradients. Gradients become very large during backpropagation through time, causing unstable updates, abnormal loss, and sometimes `NaN` values.

## 3.4 Attention Score

### ATT-1. Single key-value attention.

Question: `Q=[1,2]`, `K=[3,1]`, `V=[2,5]`, `d_k=2`.

**Answer:**

Formula:

```text
score = (Q.K) / sqrt(d_k)
attention output = softmax(score)*V
```

```text
Q.K = 1(3) + 2(1) = 5
score = 5/sqrt(2) = 3.536
Only one value, so output = [2,5]
```

### ATT-2. Single key-value attention.

Question: `Q=[2,0]`, `K=[1,2]`, `V=[4,1]`, `d_k=2`.

**Answer:**

Formula:

```text
score = (Q.K) / sqrt(d_k)
attention output = softmax(score)*V
```

```text
Q.K = 2(1) + 0(2) = 2
score = 2/sqrt(2) = 1.414
Output = [4,1]
```

### ATT-3. Two key-value pairs.

Question: `Q=[1,1]`, `K1=[1,0]`, `K2=[0,1]`, `V1=[2,3]`, `V2=[4,1]`, `d_k=2`.

**Answer:**

Formula:

```text
score_i = (Q.K_i) / sqrt(d_k)
weights = softmax(scores)
attention output = sum(weight_i * V_i)
```

```text
s1 = 1/sqrt(2) = 0.707
s2 = 1/sqrt(2) = 0.707
softmax weights = [0.5, 0.5]
output = 0.5[2,3] + 0.5[4,1] = [3,2]
```

### ATT-4. Three key-value pairs.

Question: `Q=[3,1]`; `K1=[1,1]`, `K2=[2,0]`, `K3=[0,2]`; `V1=[1,0]`, `V2=[0,2]`, `V3=[3,1]`; `d_k=2`.

**Answer:**

Formula:

```text
score_i = (Q.K_i) / sqrt(d_k)
weights = softmax(scores)
attention output = sum(weight_i * V_i)
```

```text
s1 = (3+1)/sqrt(2) = 2.828
s2 = (6+0)/sqrt(2) = 4.243
s3 = (0+2)/sqrt(2) = 1.414
softmax weights approx [0.187, 0.768, 0.045]
output = 0.187[1,0] + 0.768[0,2] + 0.045[3,1]
output approx [0.322, 1.581]
```

---

# 4. Extra Theory Questions by Type with Model Answers

## 4.1 ANN and Training

### T-ANN-1. Why is non-linearity important in neural networks?

**Model answer:**  
Non-linearity allows neural networks to learn complex patterns. Without activation functions, multiple layers would behave like one linear transformation. For example, image recognition requires combining edges into shapes and objects, which needs non-linear transformations.

### T-ANN-2. Explain loss function and cost function.

**Model answer:**  
Loss is the error for one training example. Cost is the average loss over all training examples. During training, gradient descent updates weights and biases to minimize the cost function.

### T-ANN-3. Explain effect of learning rate.

**Model answer:**  
Learning rate controls step size during weight updates. A small learning rate makes training slow. A very large learning rate may overshoot the minimum and cause unstable training. A suitable learning rate helps the model converge efficiently.

### T-ANN-4. Why is vectorization useful in logistic regression?

**Model answer:**  
Vectorization allows calculations for many training examples using matrix operations instead of slow loops. It makes training faster and is useful for implementing logistic regression and neural networks efficiently.

## 4.2 CNN and Computer Vision

### T-CNN-1. Explain convolution layer in CNN.

**Model answer:**  
The convolution layer applies filters over an image. Each filter slides across the image and performs element-wise multiplication and summation to produce a feature map. Early filters detect simple features such as edges, while deeper filters detect complex patterns.

### T-CNN-2. Why is pooling used?

**Model answer:**  
Pooling reduces spatial size and computation. Max pooling keeps the strongest feature response from each region. It also makes the model less sensitive to small shifts in the input image.

### T-CNN-3. Explain `1 x 1` convolution.

**Model answer:**  
`1 x 1` convolution changes the number of channels while keeping height and width unchanged. It can reduce depth before expensive convolution, lowering computation. For example, `64 x 64 x 192` can become `64 x 64 x 32`.

### T-CNN-4. Explain ResNet skip connection.

**Model answer:**  
ResNet uses skip connections that add input directly to the output of later layers. This helps gradients flow through deep networks and reduces vanishing-gradient issues. The operation is element-wise addition, not concatenation.

## 4.3 RNN / LSTM / GRU

### T-RNN-1. Why are RNNs suitable for sequential data?

**Model answer:**  
RNNs maintain hidden state from previous time steps, allowing them to remember earlier inputs. This makes them suitable for text, speech, and time-series data where order matters.

### T-RNN-2. Compare RNN and LSTM.

**Model answer:**  
Basic RNN has recurrent hidden state but struggles with long-term dependencies due to vanishing gradients. LSTM uses cell state and gates to remember useful information for longer time. LSTM is better for long sequences.

### T-RNN-3. Explain GRU gates.

**Model answer:**  
GRU uses update gate and reset gate. Update gate controls how much previous hidden state is carried forward. Reset gate controls how much past information is ignored while forming new memory. GRU is simpler than LSTM.

### T-RNN-4. Explain encoder-decoder model.

**Model answer:**  
The encoder reads input sequence and compresses it into a context vector. The decoder uses this vector to generate output sequence. It is used in translation, summarization, image captioning, and speech recognition.

## 4.4 Attention and Transformers

### T-ATT-1. Why is attention useful?

**Model answer:**  
Attention helps a model focus on important parts of the input sequence. It assigns higher weight to relevant tokens. This is useful for long-range dependencies because far-apart words can directly influence each other.

### T-ATT-2. Explain Q, K, and V.

**Model answer:**  
Query represents what the current token is looking for. Key represents what each token offers for matching. Value contains the information that is passed forward. Attention scores are computed from query-key similarity, and values are combined using attention weights.

### T-ATT-3. Compare transformer with RNN.

**Model answer:**  
RNN processes tokens sequentially, so it is slower and may struggle with long dependencies. Transformer processes tokens in parallel using self-attention. It captures long-range relationships more directly.

### T-ATT-4. Explain BERT.

**Model answer:**  
BERT is Bidirectional Encoder Representations from Transformers. It uses an encoder-only transformer architecture for language understanding. It is pre-trained and fine-tuned for tasks such as question answering, sentiment analysis, and named entity recognition.

## 4.5 Object Detection

### T-OBJ-1. Explain bounding box terms.

**Model answer:**  
Bounding box terms include `bx`, `by`, `bh`, and `bw`. `bx` and `by` represent center coordinates, while `bh` and `bw` represent height and width. These locate the object in an image.

### T-OBJ-2. Explain anchor boxes.

**Model answer:**  
Anchor boxes are predefined box shapes used in object detection. They allow one grid cell to detect objects of different shapes or multiple objects. For example, one anchor can be tall for pedestrians and another wide for cars.

### T-OBJ-3. Explain non-max suppression.

**Model answer:**  
Non-max suppression removes duplicate bounding boxes for the same object. It keeps the box with highest confidence and suppresses other boxes that overlap too much.

### T-OBJ-4. Compare R-CNN and YOLO.

**Model answer:**  
R-CNN is a two-stage detector: it first proposes regions, then classifies them. YOLO is a single-stage detector that predicts boxes and classes directly using grids. R-CNN can be accurate but slower; YOLO is faster.

---

# 5. Probable Questions Based on PYQs and Syllabus

## P1. Explain overfitting and methods to reduce it.

**Model answer:**  
Overfitting occurs when training accuracy is high but test accuracy is low. It means the model memorizes training data and fails to generalize. Methods to reduce it include dropout, data augmentation, L1/L2 regularization, early stopping, batch normalization, and hyperparameter tuning.

## P2. Explain CNN architecture for image classification.

**Model answer:**  
A CNN takes an image as input, applies convolution layers to extract features, activation functions for non-linearity, pooling layers to reduce size, flattening to convert feature maps into a vector, and fully connected layers to predict class labels. Early layers detect edges, while deeper layers detect complex objects.

## P3. Calculate CNN output shape and parameters for a given architecture.

**Model answer approach:**  
Use:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv params = (f*f*input_depth + 1)*filters
FC params = (input_units + 1)*output_units
```

Calculate layer by layer and remember pooling has zero parameters.

Mini solved revision:

```text
Input = 28 x 28 x 1, Conv: f=3, p=1, s=1, filters=8
Output size = floor((28 + 2(1) - 3)/1) + 1 = 28
Output shape = 28 x 28 x 8
Conv params = (3*3*1 + 1)*8 = 80
```

## P4. Explain batch normalization with diagram.

**Model answer:**  
Batch normalization normalizes mini-batch activations using mean and variance. It then applies scale `gamma` and shift `beta`. It reduces internal covariate shift, stabilizes training, improves gradient flow, and speeds up convergence.

## P5. Explain LSTM gates with example.

**Model answer:**  
LSTM has forget, input, and output gates. Forget gate removes unnecessary old memory. Input gate stores useful new information. Output gate controls what part of memory becomes hidden state. Example: in text, LSTM may remember subject information across several words.

## P6. Compare SGD, mini-batch GD, RMSProp, and Adam.

**Model answer:**  
SGD updates using one example and is noisy. Mini-batch GD updates using small batches and balances speed/stability. RMSProp adapts learning rates using squared gradient averages. Adam combines adaptive learning with momentum-like behavior and often converges faster.

## P7. Explain attention mechanism and compute attention output.

**Model answer:**  
Attention computes similarity between query and key, scales it by `sqrt(d_k)`, applies softmax to get weights, and combines value vectors. It helps the model focus on important tokens and capture long-range dependencies.

Formula:

```text
score = (Q.K) / sqrt(d_k)
weights = softmax(scores)
output = sum(weights * V)
```

Mini solved revision:

```text
Q=[2,1], K=[1,3], V=[4,2], d_k=2
score = (2*1 + 1*3)/sqrt(2) = 5/sqrt(2) = 3.536
Only one V, so output = [4,2]
```

## P8. Explain object detection pipeline with sliding window, IoU, NMS, and anchor boxes.

**Model answer:**  
Sliding window scans image regions with a ConvNet. IoU evaluates overlap between predicted and actual boxes. Non-max suppression removes duplicate overlapping boxes. Anchor boxes help detect objects of different shapes in the same grid cell.

## P9. Explain transfer learning and fine-tuning.

**Model answer:**  
Transfer learning uses a pre-trained model for a new task. Earlier layers are used as feature extractors, final classifier is replaced, and the new classifier is trained. Fine-tuning unfreezes some layers and trains them with a smaller learning rate.

## P10. Explain GAN training.

**Model answer:**  
GAN has a generator and discriminator. The generator creates fake samples from noise. The discriminator identifies real and fake samples. The generator learns to fool the discriminator, while the discriminator learns to detect generated samples. Their competition improves generation quality.

## P11. Explain ResNet and why skip connections help.

**Model answer:**  
ResNet uses skip connections that add input to the output of later layers. This helps gradients flow through deep networks and reduces vanishing-gradient difficulty. Residual addition is element-wise addition.

## P12. Explain deep learning models for NLP text classification.

**Model answer:**  
RNN processes text sequentially. LSTM remembers long-term dependencies using gates. GRU is a simpler gated RNN. BERT/transformers use self-attention to understand context and are effective for text classification, sentiment analysis, and named entity recognition.

---

# 6. Last-Minute PYQ Priority List

Prepare these first:

1. CNN numerical from both papers.
2. Sigmoid and learning-rate numericals.
3. Dropout active-neuron numerical.
4. ANN forward/backward propagation format.
5. Batch normalization.
6. GD vs SGD vs mini-batch and SGD vs Adam.
7. L1, L2, dropout comparison.
8. LSTM gates and RNN gradient problems.
9. Attention score and long-range dependency.
10. Object detection, sliding window, anchor boxes, IoU, NMS.
11. Transfer learning.
12. GAN generator/discriminator training.
13. ResNet skip connection and element-wise addition.
