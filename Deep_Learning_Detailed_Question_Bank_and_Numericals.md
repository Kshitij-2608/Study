# Deep Learning Detailed Question Bank and Numerical Workbook

Sources used:
- `Syllabus.pdf`
- `Unit 01.pptx` to `Unit 06.pptx`
- `Deep Learning_ESE Question paper format  AY 23-24.pdf`
- `Deep Learning_ESE Question paperAY 24-25 (2).pdf`

Purpose of this file:
- Give detailed, point-wise model exam answers.
- Cover all PYQs from the supplied papers.
- Add topic-wise probable theory questions.
- Add many solved numericals for ANN, CNN, RNN/LSTM/GRU, attention, batch normalization, dropout, optimizers, and object detection.

---

# 1. How to Write Answers in the Exam

For a 2-mark answer:
- Write definition first.
- Add one example or formula.
- Keep it direct.

For a 4-mark answer:
- Write definition.
- Add 3 to 5 key points.
- Add formula/diagram/example if relevant.

For a 6 or 8-mark answer:
- Start with definition.
- Add labelled diagram if possible.
- Explain working step by step.
- Add advantages/applications.
- For numericals, always write formula before substitution.

---

# 2. Detailed PYQ Answers: 2023-24

## 2.1 Activation Function Selection

**Question:** Mention considerations and strategies for choosing the most appropriate activation function.

**Model answer:**
- Activation functions decide whether a neuron should be activated and introduce non-linearity.
- Selection depends on:
  - output range required;
  - type of task;
  - whether the layer is hidden or output;
  - gradient behavior;
  - training stability.
- For binary classification, sigmoid is suitable at the output layer because it gives probability between `0` and `1`.
- For hidden layers, non-linear activation is needed so the network can learn complex patterns.
- Avoid activation choices that make gradients extremely small, because this can slow learning.
- Example: Logistic regression uses sigmoid because output must represent probability of class `1`.

## 2.2 GridSearchCV / Hyperparameter Tuning

**Question:** Explain with example the hyperparameter tuning process using GridSearchCV.

**Model answer:**
- Hyperparameters are chosen before training.
- Examples:
  - learning rate;
  - batch size;
  - number of layers;
  - number of neurons;
  - dropout rate;
  - optimizer;
  - number of epochs.
- Grid search tries all combinations from a predefined set.
- Each combination is trained and evaluated using validation performance.
- The best combination is selected based on validation accuracy/loss.

Example:

```text
learning_rate = [0.01, 0.001]
dropout = [0.2, 0.3]
optimizer = [SGD, Adam]
```

- Grid search tests all combinations and selects the one giving best validation score.
- This helps improve generalization and reduce overfitting.

## 2.3 Batch Normalization and Internal Covariate Shift

**Question:** Explain covariate shift and internal covariate shift in batch normalization. Discuss how BN stabilizes training.

**Model answer:**
- Covariate shift means change in input data distribution.
- Internal covariate shift means change in hidden-layer activation distribution during training.
- It happens because weights and biases keep changing after every update.
- Batch normalization normalizes activations within a mini-batch.
- Steps:
  - compute mini-batch mean;
  - compute mini-batch variance;
  - normalize activations;
  - apply learnable scale `gamma`;
  - apply learnable shift `beta`.
- Benefits:
  - stabilizes layer input distributions;
  - speeds training;
  - improves gradient flow;
  - reduces sensitivity to initialization;
  - can slightly reduce overfitting.

## 2.4 ANN Forward and Backward Pass

**Question:** Perform forward and backward pass for a neural network with sigmoid activation.

**Model answer method:**
- Forward pass:

```text
z = w1x1 + w2x2 + ... + b
a = sigmoid(z)
```

- Loss for binary output:

```text
L = -[y log(a) + (1-y) log(1-a)]
```

- Backward pass:

```text
dz = a - y
dw = dz*x
db = dz
```

- Update:

```text
w_new = w - alpha*dw
b_new = b - alpha*db
```

- If hidden layers are present:
  - calculate output-layer error first;
  - propagate error backward using chain rule;
  - update all weights and biases.

## 2.5 CNN Numerical: 28x28 Grayscale

**Question:** Calculate CNN for `28 x 28 x 1` image with Conv1, Pool1, Conv2, Pool2.

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*number_of_filters
FC parameters = (input_units + 1)*output_units
```

Given:

```text
Input = 28 x 28 x 1
Conv1: k=16, f=5, s=1, p=2
Pool1: f=2, s=2
Conv2: k=32, f=5, s=1, p=2
Pool2: f=2, s=2
Classes = 10
```

Solution:

| Layer | Output shape | Parameters |
|---|---:|---:|
| Conv1 | `28 x 28 x 16` | `(5*5*1+1)*16 = 416` |
| Pool1 | `14 x 14 x 16` | 0 |
| Conv2 | `14 x 14 x 32` | `(5*5*16+1)*32 = 12,832` |
| Pool2 | `7 x 7 x 32` | 0 |
| Flatten | `1568` | 0 |
| FC | `10` | `(1568+1)*10 = 15,690` |

Total parameters:

```text
416 + 12,832 + 15,690 = 28,938
```

## 2.6 Data Augmentation

**Question:** What is the need of data augmentation? Explain four techniques.

**Model answer:**
- Data augmentation creates modified versions of training images.
- It increases training variety without collecting new data.
- It reduces overfitting because the model does not memorize fixed images.
- Techniques:
  - Rotation: rotate image by small angle.
  - Flipping: mirror image horizontally/vertically.
  - Scaling: zoom in or out.
  - Shifting: move image left/right/up/down.
- Example: A digit image can be shifted slightly so the CNN learns the digit regardless of exact position.

## 2.7 Depth in CNN

**Question:** Define depth in convolution layer with example.

**Model answer:**
- Depth means number of channels or feature maps.
- Grayscale image has depth `1`.
- RGB image has depth `3`.
- If a CNN layer uses 16 filters, output depth becomes `16`.
- Example:

```text
Input = 32 x 32 x 3
Conv layer filters = 6
Output depth = 6
```

## 2.8 LSTM Architecture

**Question:** Explain LSTM with gates and loss function.

**Model answer:**
- LSTM is a recurrent neural network designed to remember long-term dependencies.
- It solves the limitation of basic RNN, which struggles with vanishing gradients.
- Main components:
  - cell state: long-term memory;
  - hidden state: short-term output memory;
  - forget gate;
  - input gate;
  - output gate.
- Forget gate decides what old information to remove.
- Input gate decides what new information to store.
- Output gate decides what memory should be exposed as output.
- Formula:

```text
C_t = f_t*C_(t-1) + i_t*C~_t
h_t = o_t*tanh(C_t)
```

- For classification tasks, cross-entropy loss can be used.
- For sequence prediction, loss is calculated between predicted sequence and actual sequence.

## 2.9 L2 Regularization

**Question:** How does L2 regularization reduce overfitting?

**Model answer:**
- L2 regularization adds squared-weight penalty to cost.
- It discourages large weights.
- Smaller weights make the model simpler.
- Simpler models generalize better and reduce overfitting.
- L2 is also called weight decay.

## 2.10 Transfer Learning Scenarios

**Question:** Write scenarios where transfer learning is most effective.

**Model answer:**
- Useful when target dataset is small.
- Useful when source and target domains are related.
- Useful when training from scratch is expensive.
- Useful when faster training is needed.
- Example: use pre-trained CNN features and replace final classifier for new image classes.

## 2.11 GAN Generator and Discriminator

**Question:** Explain generator and discriminator in GAN.

**Model answer:**
- GAN has two networks:
  - generator;
  - discriminator.
- Generator:
  - takes random noise;
  - generates fake samples;
  - tries to fool discriminator.
- Discriminator:
  - receives real and generated samples;
  - predicts real or fake;
  - improves as a binary classifier.
- Training is adversarial because both networks improve against each other.

## 2.12 SGD vs Batch Gradient Descent

**Question:** Compare SGD with batch gradient descent.

**Model answer:**
- Batch GD:
  - uses entire dataset for one update;
  - stable gradient;
  - slow for large datasets.
- SGD:
  - uses one example for one update;
  - faster updates;
  - noisy path.
- Mini-batch GD is a balance between both.

## 2.13 RNN Architecture and Gradient Problems

**Question:** Explain RNN with vanishing and exploding gradients.

**Model answer:**
- RNN processes sequence data using hidden state.
- Formula:

```text
h_t = activation(Wxh*x_t + Whh*h_(t-1) + b)
```

- Current hidden state depends on current input and previous hidden state.
- Applications:
  - text;
  - speech;
  - time series.
- Vanishing gradient:
  - gradients become very small;
  - model fails to learn long-term dependencies.
- Exploding gradient:
  - gradients become very large;
  - loss becomes unstable or `NaN`.

## 2.14 Bounding Boxes and Anchor Boxes

**Question:** Write a short note on bounding boxes and anchor boxes.

**Model answer:**
- Bounding box locates an object in an image.
- It is represented using:

```text
bx, by, bh, bw
```

- `bx`, `by` represent center of box.
- `bh`, `bw` represent height and width.
- Anchor boxes are predefined shapes.
- They help detect multiple objects or different object shapes in a grid cell.
- Example: tall anchor for pedestrian, wide anchor for car.

---

# 3. Detailed PYQ Answers: 2024-25

## 3.1 SGD vs Adam Convergence

**Question:** Model A uses SGD with learning rate `0.01`; Model B uses Adam with learning rate `0.001`. Why does Adam converge faster?

**Model answer:**
- SGD updates parameters using current gradient directly.
- It can be slow when gradients oscillate.
- It uses same learning-rate behavior for all parameters.
- Adam combines:
  - momentum-like past-gradient information;
  - adaptive update behavior.
- Adam adjusts updates more effectively for each parameter.
- Therefore, Adam can converge faster even with smaller learning rate.

## 3.2 Logistic Regression as Simplified NN

**Question:** In what way is logistic regression a simplified neural network?

**Model answer:**
- Logistic regression has:
  - input features;
  - weights;
  - bias;
  - weighted sum;
  - sigmoid activation;
  - probability output.
- Formula:

```text
z = wTx + b
a = sigmoid(z)
```

- These are basic components of a neural network.
- It is simplified because it has no hidden layers.

## 3.3 Dropout Numerical

**Question:** Dropout rate `0.3`, original layer has 100 neurons. How many active?

Formula:

```text
Active neurons = total neurons * (1 - dropout rate)
```

Solution:

```text
Active neurons = 100 * (1 - 0.3)
Active neurons = 100 * 0.7 = 70
```

Answer: `70 neurons`.

## 3.4 Hyperparameter Tuning for Overfitting

**Question:** Training accuracy is 99%, test accuracy is 75%. Which tuning techniques help?

**Model answer:**
- This is overfitting.
- The model memorizes training data but does not generalize.
- Tune:
  - dropout rate;
  - L1/L2 regularization;
  - learning rate;
  - number of hidden layers;
  - number of neurons;
  - batch size;
  - epochs;
  - optimizer.
- Use early stopping when validation loss increases.
- Use data augmentation for image data.

## 3.5 Sigmoid Numerical

**Question:** `x=0.9`, `w=-0.6`, `b=0.2`, sigmoid. Then change `w=-0.3`.

Formula:

```text
z = wx + b
sigmoid(z) = 1 / (1 + e^-z)
```

Case 1:

```text
z = (-0.6)(0.9) + 0.2 = -0.34
a = sigmoid(-0.34) = 0.4158
```

Case 2:

```text
z = (-0.3)(0.9) + 0.2 = -0.07
a = sigmoid(-0.07) = 0.4825
```

Conclusion:
- Output increases from `0.4158` to `0.4825`.
- Reason: weight became less negative, so weighted sum increased.

## 3.6 Learning Rate Numerical

**Question:** `w=0.4`, gradient `-0.1`. Compare learning rates `0.01` and `0.1`.

Formula:

```text
w_new = w - eta*gradient
```

For `eta=0.01`:

```text
w_new = 0.4 - 0.01(-0.1) = 0.401
```

For `eta=0.1`:

```text
w_new = 0.4 - 0.1(-0.1) = 0.41
```

Answer:
- Learning rate `0.1` gives faster update.
- Larger learning rate produces larger step size.

## 3.7 CNN Numerical: 32x32 Color Image

Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
Conv parameters = (f*f*input_depth + 1)*filters
FC parameters = (input_units + 1)*output_units
```

Given:

```text
Input = 32 x 32 x 3
Conv1: k=6, f=5, s=1, p=0
Pool1: f=2, s=2
Conv2: k=10, f=5, s=1, p=0
Pool2: f=2, s=2
Classes = 10
```

Solution:

| Layer | Output | Parameters |
|---|---:|---:|
| Conv1 | `28 x 28 x 6` | `(5*5*3+1)*6 = 456` |
| Pool1 | `14 x 14 x 6` | 0 |
| Conv2 | `10 x 10 x 10` | `(5*5*6+1)*10 = 1510` |
| Pool2 | `5 x 5 x 10` | 0 |
| Flatten | `250` | 0 |
| FC | `10` | `(250+1)*10 = 2510` |

Answer:
- FC parameters = `2510`.
- Total parameters = `456 + 1510 + 2510 = 4476`.

## 3.8 GD vs SGD vs Mini-Batch GD

**Model answer:**
- Gradient descent updates weights to reduce loss.
- Batch GD:
  - uses complete dataset;
  - stable but slow.
- SGD:
  - uses one example;
  - fast but noisy.
- Mini-batch GD:
  - uses small group of examples;
  - balances speed and stability.
- Deep learning usually uses mini-batches.

## 3.9 L1 vs L2 vs Dropout

**Model answer:**
- L1:
  - penalizes absolute weights;
  - can make weights zero;
  - useful for model compression.
- L2:
  - penalizes squared weights;
  - shrinks weights smoothly;
  - common for overfitting reduction.
- Dropout:
  - randomly disables neurons;
  - changes network structure temporarily;
  - does not add a penalty term.

## 3.10 Encoder-Decoder Architecture

**Model answer:**
- Encoder reads input sequence.
- It compresses input into hidden/context vector.
- Decoder uses context vector to generate output sequence.
- Used when input and output lengths differ.
- Applications:
  - translation;
  - summarization;
  - image captioning;
  - speech recognition.

## 3.11 Attention Long-Range Dependency

**Model answer:**
- Attention lets each token compare with every other token.
- It computes attention scores using query-key similarity.
- Important tokens receive higher weights.
- This helps capture long-range dependency directly.
- Unlike RNN, information does not need to pass step by step through many time steps.

## 3.12 ResNet Skip Connection

**Model answer:**
- ResNet uses identity shortcut connections.
- Input is added to output of later layers.
- Formula idea:

```text
Output = F(x) + x
```

- Helps gradient flow through deep networks.
- Reduces vanishing-gradient problem.
- Addition is element-wise:

```text
[1,2] + [3,4] = [4,6]
```

## 3.13 Object Detection and Sliding Window

**Model answer:**
- Image classification gives one label for whole image.
- Object detection gives:
  - class label;
  - object location;
  - bounding box.
- Sliding window:
  - choose window size;
  - slide across image;
  - classify each crop using ConvNet;
  - repeat with different window sizes.
- Drawback:
  - computationally expensive;
  - window may not align exactly with object.

## 3.14 Deep Learning in NLP

**Model answer:**
- NLP deals with text/language data.
- Text is sequential, so word order matters.
- Deep learning models learn semantic/contextual features.
- Models:
  - RNN: sequence processing;
  - LSTM: long-term dependency;
  - GRU: efficient gated sequence learning;
  - BERT/Transformers: attention-based contextual understanding.
- Application: sentiment/text classification.

---

# 4. Complete Topic-Wise Theory Question Bank

## 4.1 ANN and Neural Network Basics

### Q1. What is deep learning?

**Answer:**
- Deep learning is a machine learning approach using multi-layer neural networks.
- It learns features automatically from data.
- It is useful for images, text, audio, and large unstructured datasets.
- Example: CNN for image classification.

### Q2. Explain artificial neuron.

**Answer:**
- Artificial neuron receives inputs.
- Each input has a weight.
- Weighted inputs and bias are added.
- Activation function produces output.
- Formula:

```text
z = w1x1 + w2x2 + ... + b
a = activation(z)
```

### Q3. Explain loss and cost function.

**Answer:**
- Loss is error for one example.
- Cost is average loss over all examples.
- Training minimizes cost.
- Binary cross-entropy:

```text
L = -[y log(a) + (1-y) log(1-a)]
```

### Q4. Why is backpropagation needed?

**Answer:**
- Backpropagation calculates gradients.
- Gradients show how weights affect loss.
- It uses chain rule.
- Parameters are updated using gradient descent.
- It helps neural networks learn from error.

### Q5. Explain activation functions.

**Answer:**
- Activation functions introduce non-linearity.
- They decide neuron output.
- Sigmoid is useful for binary probability.
- Binary activation gives hard 0/1 decision.
- Bipolar binary gives -1 or 1.
- Hidden layers need non-linearity to learn complex patterns.

## 4.2 Training Improvements

### Q1. Explain overfitting.

**Answer:**
- Overfitting means high training accuracy but low test accuracy.
- Model memorizes training data.
- It fails to generalize.
- Example: 99% train accuracy and 75% test accuracy.
- Remedies: dropout, regularization, early stopping, augmentation.

### Q2. Explain dropout.

**Answer:**
- Dropout randomly disables neurons during training.
- It prevents dependence on specific neurons.
- It reduces overfitting.
- Example: dropout `0.3` keeps 70% neurons active.

### Q3. Explain batch normalization.

**Answer:**
- Batch normalization normalizes mini-batch activations.
- It computes mean and variance.
- It normalizes values.
- It applies scale `gamma` and shift `beta`.
- It stabilizes and speeds training.

### Q4. Explain optimizers.

**Answer:**
- Optimizers update weights to reduce loss.
- GD uses full dataset.
- SGD uses one example.
- Mini-batch uses small batch.
- Momentum smooths updates.
- RMSProp adapts learning rate.
- Adam combines momentum and adaptive behavior.

## 4.3 CNN and Computer Vision

### Q1. Why use CNN for images?

**Answer:**
- Fully connected networks create too many parameters for images.
- CNN uses filters to learn local features.
- It shares weights across image regions.
- It reduces computation.
- It learns edges, textures, shapes, and objects.

### Q2. Explain convolution layer.

**Answer:**
- A filter slides across the image.
- At each location, element-wise multiplication and sum are calculated.
- Output is a feature map.
- Filters learn patterns such as edges and textures.

### Q3. Explain padding and stride.

**Answer:**
- Padding adds border pixels, usually zeros.
- It prevents output from shrinking too much.
- Stride controls movement step of filter.
- Larger stride reduces output size.

### Q4. Explain pooling.

**Answer:**
- Pooling reduces spatial dimensions.
- Max pooling keeps maximum value from a region.
- It reduces computation.
- It keeps strongest feature response.
- It does not change depth.

### Q5. Explain VGG, ResNet, Inception, MobileNet.

**Answer:**
- VGG uses repeated small `3 x 3` filters.
- ResNet uses skip connections to train deeper networks.
- Inception uses multiple filter sizes in parallel.
- MobileNet uses depthwise separable convolution for efficiency.

## 4.4 Sequence Models

### Q1. What is sequential data?

**Answer:**
- Sequential data has order.
- Current element depends on previous/future elements.
- Examples:
  - text;
  - speech;
  - stock prices;
  - temperature readings.

### Q2. Explain RNN.

**Answer:**
- RNN processes sequences step by step.
- It maintains hidden state.
- Hidden state stores previous information.
- Formula:

```text
h_t = activation(Wxh*x_t + Whh*h_(t-1) + b)
```

### Q3. Explain LSTM.

**Answer:**
- LSTM is an improved RNN.
- It uses cell state for long-term memory.
- It has forget, input, and output gates.
- It handles long-term dependencies better.

### Q4. Explain GRU.

**Answer:**
- GRU is a gated recurrent unit.
- It has update gate and reset gate.
- It is simpler than LSTM.
- It has fewer parameters and trains faster.

## 4.5 Transformers and Attention

### Q1. What are embeddings?

**Answer:**
- Embeddings convert words into vectors.
- Similar words have similar vectors.
- They are dense and low-dimensional.
- Example: word embeddings represent meaning better than one-hot encoding.

### Q2. Explain self-attention.

**Answer:**
- Self-attention lets each token look at all other tokens.
- It decides which tokens are important.
- It creates contextual embeddings.
- It helps capture long-range dependency.

### Q3. Explain Q, K, V.

**Answer:**
- Query: what current token is looking for.
- Key: what each token offers for matching.
- Value: information passed forward.
- Attention score is calculated using query-key similarity.

### Q4. Explain BERT.

**Answer:**
- BERT stands for Bidirectional Encoder Representations from Transformers.
- It uses transformer encoder.
- It understands context from both directions.
- Used for question answering, sentiment analysis, and named entity recognition.

## 4.6 Object Detection

### Q1. Classification vs localization vs detection.

**Answer:**
- Classification predicts class for whole image.
- Localization predicts class and one bounding box.
- Detection predicts multiple objects with boxes and classes.

### Q2. Explain IoU.

**Answer:**
- IoU measures overlap between predicted and actual boxes.
- Formula:

```text
IoU = intersection area / union area
```

- Higher IoU means better prediction.

### Q3. Explain non-max suppression.

**Answer:**
- NMS removes duplicate boxes.
- Keeps box with highest confidence.
- Removes highly overlapping lower-confidence boxes.

### Q4. Explain YOLO.

**Answer:**
- YOLO means You Only Look Once.
- It is a single-stage detector.
- It divides image into grid.
- Each grid predicts boxes and class probabilities.
- It is faster than two-stage detectors.

---

# 5. Numerical Workbook: ANN

## ANN Formula Sheet

```text
z = w1x1 + w2x2 + ... + b
sigmoid(z) = 1 / (1 + e^-z)
unipolar binary = 1 if z >= threshold else 0
bipolar binary = 1 if z >= threshold else -1
dz = a - y
dw = dz*x
w_new = w - alpha*dw
b_new = b - alpha*db
```

## ANN-1. Single Neuron with Sigmoid

Question: `x1=2`, `x2=1`, `w1=0.4`, `w2=-0.2`, `b=0.1`. Find output using sigmoid.

Solution:

```text
z = w1x1 + w2x2 + b
z = 0.4(2) + (-0.2)(1) + 0.1 = 0.7
a = sigmoid(0.7) = 0.668
```

Answer: `0.668`.

## ANN-2. Single Neuron with Unipolar Binary Activation

Question: `x1=1`, `x2=3`, `w1=0.5`, `w2=-0.1`, `b=0`, threshold `0`. Find output.

Formula:

```text
z = w1x1 + w2x2 + b
output = 1 if z >= 0 else 0
```

Solution:

```text
z = 0.5(1) + (-0.1)(3) + 0
z = 0.5 - 0.3 = 0.2
```

Since `z >= 0`, output = `1`.

## ANN-3. Single Neuron with Bipolar Binary Activation

Question: `x1=2`, `x2=4`, `w1=0.2`, `w2=-0.3`, `b=0.1`, threshold `0`.

Formula:

```text
output = 1 if z >= 0 else -1
```

Solution:

```text
z = 0.2(2) + (-0.3)(4) + 0.1
z = 0.4 - 1.2 + 0.1 = -0.7
```

Since `z < 0`, output = `-1`.

## ANN-4. Logistic Regression One Pass and Update

Question: `x=[1,2]`, `w=[0.2,0.3]`, `b=-0.1`, `y=1`, `alpha=0.05`.

Formula:

```text
z = wTx + b
a = sigmoid(z)
dz = a - y
dW = dz*x
db = dz
w_new = w - alpha*dW
b_new = b - alpha*db
```

Solution:

```text
z = 0.2(1) + 0.3(2) - 0.1 = 0.7
a = sigmoid(0.7) = 0.668
dz = 0.668 - 1 = -0.332
dW = [-0.332, -0.664]
db = -0.332
w1_new = 0.2 - 0.05(-0.332) = 0.2166
w2_new = 0.3 - 0.05(-0.664) = 0.3332
b_new = -0.1 - 0.05(-0.332) = -0.0834
```

## ANN-5. Two Passes of Logistic Regression

Use result from ANN-4 for second pass with same `x` and `y`.

Formula:

```text
Repeat forward pass and update using new weights.
```

Second pass:

```text
z = 0.2166(1) + 0.3332(2) - 0.0834
z = 0.7996
a = sigmoid(0.7996) = 0.690
dz = 0.690 - 1 = -0.310
dW = [-0.310, -0.620]
db = -0.310
w1_new = 0.2166 - 0.05(-0.310) = 0.2321
w2_new = 0.3332 - 0.05(-0.620) = 0.3642
b_new = -0.0834 - 0.05(-0.310) = -0.0679
```

Observation:
- After two passes, output moved from `0.668` to `0.690`.
- Since target is `1`, prediction is improving.

## ANN-6. Same Inputs with Different Activation Functions

Question: `z = 0.7`. Find output using sigmoid, unipolar binary, and bipolar binary.

Formula:

```text
sigmoid(z) = 1 / (1 + e^-z)
unipolar = 1 if z >= 0 else 0
bipolar = 1 if z >= 0 else -1
```

Solution:

```text
sigmoid(0.7) = 0.668
unipolar output = 1
bipolar output = 1
```

Exam point:
- Same net input can produce different outputs depending on activation function.

## ANN-7. 2-2-1 Network Forward Pass

Question:

```text
x = [1,0]
h1: w=[0.2,-0.3], b=0.1
h2: w=[-0.4,0.1], b=-0.2
output: w=[0.3,-0.2], b=0.05
activation = sigmoid
```

Formula:

```text
z = wTx + b
a = sigmoid(z)
```

Solution:

```text
z_h1 = 0.2(1) + (-0.3)(0) + 0.1 = 0.3
a_h1 = sigmoid(0.3) = 0.574

z_h2 = -0.4(1) + 0.1(0) - 0.2 = -0.6
a_h2 = sigmoid(-0.6) = 0.354

z_out = 0.3(0.574) + (-0.2)(0.354) + 0.05
z_out = 0.151
a_out = sigmoid(0.151) = 0.538
```

Answer: Final output = `0.538`.

## ANN-8. 2-2-1 Backprop One Update

Use ANN-7 with target `y=1`, learning rate `alpha=0.5`.

Formula:

```text
dz_out = a_out - y
dw_out = dz_out * hidden_activation
db_out = dz_out
```

Solution:

```text
dz_out = 0.538 - 1 = -0.462
dw_out1 = -0.462(0.574) = -0.265
dw_out2 = -0.462(0.354) = -0.164
db_out = -0.462

w_out1_new = 0.3 - 0.5(-0.265) = 0.4325
w_out2_new = -0.2 - 0.5(-0.164) = -0.118
b_out_new = 0.05 - 0.5(-0.462) = 0.281
```

## ANN-9. Dropout Numerical

Question: 200 neurons, dropout rate `0.4`. Find active neurons.

Formula:

```text
Active neurons = total neurons * (1 - dropout rate)
```

Solution:

```text
Active neurons = 200 * 0.6 = 120
```

## ANN-10. Learning Rate Update

Question: `w=0.8`, gradient `0.3`. Compute updated weight for `eta=0.01` and `eta=0.1`.

Formula:

```text
w_new = w - eta*gradient
```

Solution:

```text
eta=0.01: w_new = 0.8 - 0.01(0.3) = 0.797
eta=0.1:  w_new = 0.8 - 0.1(0.3) = 0.770
```

Conclusion:
- Larger learning rate gives larger update.

---

# 6. Numerical Workbook: CNN

## CNN Formula Sheet

```text
Output size = floor((n + 2p - f)/s) + 1
Output depth = number of filters
Conv parameters = (f*f*input_depth + 1)*filters
Pooling parameters = 0
Flatten size = height*width*depth
FC parameters = (input_units + 1)*output_units
```

## CNN-1. Same Padding

Question: Input `32 x 32 x 3`, Conv `f=3`, `p=1`, `s=1`, `filters=16`.

Solution:

```text
Output size = floor((32 + 2(1) - 3)/1) + 1 = 32
Output = 32 x 32 x 16
Params = (3*3*3 + 1)*16 = 448
```

## CNN-2. Valid Convolution

Question: Input `28 x 28 x 1`, Conv `f=5`, `p=0`, `s=1`, `filters=8`.

Solution:

```text
Output size = (28 - 5)/1 + 1 = 24
Output = 24 x 24 x 8
Params = (5*5*1 + 1)*8 = 208
```

## CNN-3. Strided Convolution

Question: Input `64 x 64 x 3`, Conv `f=5`, `p=0`, `s=2`, `filters=10`.

Solution:

```text
Output size = floor((64 - 5)/2) + 1
Output size = floor(29.5) + 1 = 30
Output = 30 x 30 x 10
Params = (5*5*3 + 1)*10 = 760
```

## CNN-4. Pooling

Question: Input `28 x 28 x 16`, max pool `f=2`, `s=2`.

Solution:

```text
Output size = ((28 - 2)/2) + 1 = 14
Output = 14 x 14 x 16
Params = 0
```

## CNN-5. Full CNN Architecture

Question:

```text
Input = 32 x 32 x 3
Conv1: f=3, p=1, s=1, filters=8
Pool1: f=2, s=2
Conv2: f=3, p=1, s=1, filters=16
Pool2: f=2, s=2
FC: 10 classes
```

Solution:

```text
Conv1 output = 32 x 32 x 8
Conv1 params = (3*3*3 + 1)*8 = 224

Pool1 output = 16 x 16 x 8

Conv2 output = 16 x 16 x 16
Conv2 params = (3*3*8 + 1)*16 = 1168

Pool2 output = 8 x 8 x 16
Flatten = 8*8*16 = 1024
FC params = (1024 + 1)*10 = 10250

Total params = 224 + 1168 + 10250 = 11642
```

## CNN-6. 1x1 Convolution

Question: Input `14 x 14 x 64`, apply `1 x 1` convolution with 32 filters.

Solution:

```text
Output = 14 x 14 x 32
Params = (1*1*64 + 1)*32 = 2080
```

## CNN-7. Find Padding for Same Convolution

Question: For `f=5`, what padding keeps output size same when `s=1`?

Formula:

```text
p = (f - 1)/2
```

Solution:

```text
p = (5 - 1)/2 = 2
```

## CNN-8. FC Parameters Only

Question: Feature map is `5 x 5 x 10`, output classes = 10. Find FC parameters.

Formula:

```text
FC params = (input_units + 1)*output_units
```

Solution:

```text
input_units = 5*5*10 = 250
FC params = (250 + 1)*10 = 2510
```

---

# 7. Numerical Workbook: RNN, LSTM, GRU

## RNN Formula Sheet

```text
h_t = tanh(Wxh*x_t + Whh*h_(t-1) + b)
y_t = sigmoid(Why*h_t + b_y)
C_t = f_t*C_(t-1) + i_t*C~_t
h_t = o_t*tanh(C_t)
```

## RNN-1. One-Step Hidden State

Question: `Wxh=0.4`, `Whh=0.3`, `x1=1`, `h0=0`.

Solution:

```text
h1 = tanh(0.4(1) + 0.3(0))
h1 = tanh(0.4) = 0.380
```

## RNN-2. Two Time Steps

Question: `Wxh=0.6`, `Whh=0.5`, `h0=0`, `x1=1`, `x2=0`.

Solution:

```text
h1 = tanh(0.6(1) + 0.5(0)) = 0.537
h2 = tanh(0.6(0) + 0.5(0.537))
h2 = tanh(0.2685) = 0.262
```

## RNN-3. RNN Output

Question: `h_t=0.5`, `Why=1.2`, `b_y=-0.1`. Use sigmoid output.

Solution:

```text
y_t = sigmoid(Why*h_t + b_y)
y_t = sigmoid(1.2(0.5) - 0.1)
y_t = sigmoid(0.5) = 0.622
```

## RNN-4. LSTM Cell State

Question: `C_(t-1)=0.7`, `f_t=0.5`, `i_t=0.4`, `C~_t=0.8`.

Solution:

```text
C_t = f_t*C_(t-1) + i_t*C~_t
C_t = 0.5(0.7) + 0.4(0.8)
C_t = 0.67
```

## RNN-5. LSTM Hidden State

Question: `C_t=0.67`, `o_t=0.6`.

Formula:

```text
h_t = o_t*tanh(C_t)
```

Solution:

```text
h_t = 0.6*tanh(0.67)
h_t = 0.6(0.585) = 0.351
```

## RNN-6. Vanishing Gradient Numeric Idea

Question: If gradient factor is `0.2` for 5 time steps, what happens?

Solution:

```text
gradient multiplier = 0.2^5 = 0.00032
```

Conclusion:
- Gradient becomes almost zero.
- This is vanishing gradient.

## RNN-7. Exploding Gradient Numeric Idea

Question: If gradient factor is `2` for 5 time steps, what happens?

Solution:

```text
gradient multiplier = 2^5 = 32
```

Conclusion:
- Gradient becomes very large.
- This is exploding gradient.

---

# 8. Numerical Workbook: Attention Scores

## Attention Formula Sheet

```text
score_i = (Q.K_i) / sqrt(d_k)
weights = softmax(scores)
output = sum(weight_i * V_i)
```

## ATT-1. Single Key-Value

Question: `Q=[2,1]`, `K=[1,3]`, `V=[4,2]`, `d_k=2`.

Solution:

```text
Q.K = 2(1) + 1(3) = 5
score = 5/sqrt(2) = 3.536
Only one value vector, so output = [4,2]
```

## ATT-2. Two Keys Equal Scores

Question: `Q=[1,1]`, `K1=[1,0]`, `K2=[0,1]`, `V1=[2,3]`, `V2=[4,1]`, `d_k=2`.

Solution:

```text
s1 = 1/sqrt(2) = 0.707
s2 = 1/sqrt(2) = 0.707
weights = [0.5, 0.5]
output = 0.5[2,3] + 0.5[4,1] = [3,2]
```

## ATT-3. Three Keys

Question:

```text
Q=[2,1]
K1=[1,0], K2=[0,2], K3=[1,1]
V1=[3,1], V2=[2,4], V3=[5,2]
d_k=2
```

Solution:

```text
s1 = 2/sqrt(2) = 1.414
s2 = 2/sqrt(2) = 1.414
s3 = 3/sqrt(2) = 2.121
softmax weights = [0.248, 0.248, 0.503]
output = 0.248[3,1] + 0.248[2,4] + 0.503[5,2]
output = [3.76, 2.248]
```

## ATT-4. Highest Attention Weight

Question: `Q=[3,1]`, `K1=[1,1]`, `K2=[2,0]`, `K3=[0,2]`, `d_k=2`. Which key gets highest attention?

Solution:

```text
s1 = (3+1)/sqrt(2) = 2.828
s2 = (6+0)/sqrt(2) = 4.243
s3 = (0+2)/sqrt(2) = 1.414
```

Answer:
- `K2` gets highest attention because score `4.243` is largest.

## ATT-5. Attention Output with Two Values

Question: Scores after softmax are `[0.7, 0.3]`, `V1=[2,4]`, `V2=[6,1]`. Find output.

Solution:

```text
output = 0.7[2,4] + 0.3[6,1]
output = [1.4,2.8] + [1.8,0.3]
output = [3.2,3.1]
```

---

# 9. Other Important Numericals

## BN-1. Batch Normalization

Question: Batch activations are `[2,4,6]`. Compute mean and normalized values using variance `8/3` and epsilon `0`.

Formula:

```text
mean = sum(x)/m
x_norm = (x - mean)/sqrt(variance + epsilon)
```

Solution:

```text
mean = (2+4+6)/3 = 4
std = sqrt(8/3) = 1.633

For 2: (2-4)/1.633 = -1.225
For 4: (4-4)/1.633 = 0
For 6: (6-4)/1.633 = 1.225
```

## OBJ-1. IoU

Question: Intersection area = 40, Union area = 100. Find IoU.

Formula:

```text
IoU = intersection / union
```

Solution:

```text
IoU = 40/100 = 0.4
```

## OBJ-2. Anchor Output Vector Length

Question: If one anchor output has `[Pc,bx,by,bh,bw,c1,c2,c3]`, what is vector length for 3 anchors?

Formula:

```text
Vector length = anchors * values_per_anchor
```

Solution:

```text
values_per_anchor = 8
anchors = 3
length = 3*8 = 24
```

---

# 10. Most Probable Long Questions

## P1. Explain CNN with all layers and numerical formula.

**Answer points:**
- CNN is used for image data.
- Convolution extracts features.
- Activation introduces non-linearity.
- Pooling reduces size.
- Flattening converts 3D features to 1D.
- Fully connected layer predicts class.
- Formula:

```text
Output size = floor((n + 2p - f)/s) + 1
```

## P2. Explain LSTM and compare with RNN.

**Answer points:**
- RNN stores previous information using hidden state.
- RNN struggles with long-term dependencies.
- LSTM has cell state and gates.
- Forget gate removes old information.
- Input gate stores new information.
- Output gate controls hidden output.
- LSTM handles vanishing gradient better.

## P3. Explain attention mechanism with numerical.

**Answer points:**
- Attention focuses on important tokens.
- Uses query, key, value.
- Score:

```text
score = (Q.K)/sqrt(d_k)
```

- Softmax converts scores into weights.
- Output is weighted sum of values.
- Helps capture long-range dependency.

## P4. Explain object detection pipeline.

**Answer points:**
- Detection predicts class and location.
- Bounding boxes locate objects.
- Sliding window scans image regions.
- IoU evaluates box overlap.
- NMS removes duplicate boxes.
- Anchor boxes handle different object shapes.
- YOLO is single-stage; R-CNN is two-stage.

## P5. Explain techniques to improve neural network performance.

**Answer points:**
- Regularization reduces overfitting.
- Dropout disables random neurons.
- Data augmentation increases training variety.
- Batch normalization stabilizes activations.
- Optimizers improve convergence.
- Hyperparameter tuning selects best settings.

---

# 11. Final Revision Checklist

Must practice numericals:
- Sigmoid output.
- Binary/bipolar activation output.
- One-pass and two-pass ANN update.
- 2-2-1 forward pass.
- CNN output shape and parameters.
- Pooling output.
- FC parameter count.
- 1x1 convolution.
- RNN hidden state.
- LSTM cell and hidden state.
- Attention score and output.
- Batch normalization.
- IoU.
- Anchor vector length.

Must practice theory:
- Logistic regression as neural network.
- Activation selection.
- Overfitting remedies.
- Batch normalization.
- Optimizer comparison.
- CNN layers.
- VGG, ResNet, Inception, MobileNet.
- Transfer learning.
- RNN, LSTM, GRU.
- Encoder-decoder.
- Attention and transformer.
- BERT.
- Object detection, YOLO, R-CNN.
- GAN.

