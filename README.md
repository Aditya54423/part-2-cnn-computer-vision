
# **Part 2: Computer Vision Problem Formulation and CNN Prototype**

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python) 
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange?logo=tensorflow) 
![Keras](https://img.shields.io/badge/Keras-API-red?logo=keras)

# ** Project Overview**
This project formulates a computer vision solution to automate the detection of vehicle surface defects. A custom Convolutional Neural Network (CNN) is built from scratch using TensorFlow/Keras to learn visual patterns associated with different types of damage.

# **Task 1: Problem Identification**
Selected Problem Type: Multi-Class Image Classification

Reasoning: The dataset consists of images where each image features a single, dominant vehicle surface condition. There are four mutually exclusive categories (normal, scratch, dent, stain). Since the objective is to assign exactly one label to the entire image rather than locating the defects with bounding boxes or pixel masks, image classification is the correct and most efficient approach.

# ** Task 2: Dataset Exploration**
Number of Classes: 4 (normal, scratch, dent, stain)

Total Images: 480 images

Images per Class: 120 images per class.

Imbalance: The dataset is perfectly balanced (1.00 ratio). No class-weight correction is required.

Image Dimensions: The original images are standardized. During exploration, they are validated as 3-channel RGB images.

# **Task 3: Image Preprocessing**
To ensure the CNN trains efficiently and effectively, the following preprocessing pipeline was implemented:

Resizing: All images were down-sampled to 64x64 pixels to balance spatial detail with computational efficiency.

Normalization: Pixel values were scaled by dividing by 255.0, squeezing the range to [0, 1] to help gradient descent converge reliably.

Splitting: A stratified split of 70% Train, 15% Validation, and 15% Test was used to ensure an equal distribution of classes across all sets.

Augmentation: Applied to the training set only (rotation ±20°, zoom 15%, shear 10%, horizontal/vertical flips, and brightness shifts) to prevent overfitting and simulate real-world camera variations.

# **Task 4: CNN Model Creation**
A custom 3-block CNN was designed to progressively extract features.

Convolution Layers: Two sequential Conv2D layers per block (32 → 64 → 128 filters) to detect increasingly complex patterns.

Activation Function: ReLU applied to all hidden layers to introduce non-linearity.

Pooling Layer: MaxPooling2D (2x2) applied after each convolutional block to reduce spatial dimensions and provide translation invariance.

Flatten Layer: Converts the final 3D feature maps (8x8x128) into a 1D vector (8,192 elements).

Dense Layer: A fully connected layer of 256 units with L2 regularization and Dropout (0.40) to synthesize features.

Output Layer: A final Dense layer with 4 units and a Softmax activation function to output class probabilities.

# **Task 5: Model Training and Evaluation**
The model was compiled using the Adam optimizer (lr=1e-3) and sparse_categorical_crossentropy loss. EarlyStopping and ReduceLROnPlateau callbacks were utilized to manage the learning rate and prevent over-training.

Performance Challenge: Due to the extremely limited dataset size (only 84 training images per class), the model struggled to generalize, collapsing to a local minimum early in training.

Test Accuracy: ~25.0%

Test Loss: ~1.40

Observation: The model currently outputs a majority-class prediction strategy (which equates to 25% accuracy on a perfectly balanced 4-class dataset). In a future iteration, utilizing Transfer Learning (e.g., MobileNetV2 or ResNet) would drastically improve performance on a dataset of this size.

# ** Task 6: CNN Concept Explanation**
## What is convolution?
Convolution is a mathematical operation where a small, learnable filter (like a magnifying glass) slides across an image. As it slides, it multiplies its weights against the image's pixels. This process creates a "feature map" that detects specific visual patterns like straight edges, curves, or color shifts.

##  Why is pooling used?
Pooling (usually Max Pooling) shrinks the dimensions of the feature maps by taking only the most prominent signals (the maximum values) from a small grid. This reduces the computational load and makes the model "translation invariant," meaning it can recognize a scratch whether it's on the left or the right side of the image.

## Why is ReLU commonly used in CNNs?
ReLU (Rectified Linear Unit) is an activation function that simply turns all negative numbers to zero and keeps positive numbers as they are. It is computationally incredibly fast and prevents the "vanishing gradient problem," allowing deep networks to learn effectively without stalling.

## Why are CNNs better than regular feed-forward networks for image data?
A standard neural network flattens an image into a single 1D line of pixels, instantly destroying all spatial relationships (how pixels relate to their neighbors). CNNs preserve the 2D grid structure. Furthermore, CNNs use "parameter sharing" (the same filter slides across the whole image), which drastically reduces the number of weights the model needs to learn compared to a standard network.

# Task 7: Business Use Case Mapping
Domain: Manufacturing & Quality Assurance
Automated Vehicle Assembly Line Inspection
In an automotive manufacturing plant, hundreds of vehicles leave the paint and body shop every day. Traditionally, human inspectors must walk around every car to check for superficial damage, a process that is slow, expensive, and subject to human fatigue.

By mapping this Computer Vision solution to the factory floor:

Camera Integration: High-resolution cameras are mounted in a brightly lit inspection tunnel.

Real-time Routing: As the vehicle passes, the CNN instantly classifies the surface panels.

Actionable Output: If the model outputs normal, the car goes to dispatch. If it detects a stain, it triggers an alert for a detailing wipe-down. If it flags a scratch or dent, the vehicle is automatically routed to a specialized repair bay.

This automation drastically speeds up the production line, reduces human labor costs, and ensures a standardized, fatigue-free quality benchmark for every vehicle shipped.

