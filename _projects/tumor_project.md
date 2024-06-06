---
layout: page
title: TensorFlow Brain Tumor Image Classification
description: Personal
img: assets/img/early-detection-of-brain-tumors-and-beyond-354432-960x540.jpg
importance: 1
category: my work
---

## Introduction
Brain tumors pose significant challenges in medical diagnostics, necessitating accurate and efficient identification methods. This project aims to develop a robust model capable of classifying three brain tumor types from MRI images: pituitary, glioma, and meningioma tumors. By utilizing advanced neural networks and comprehensive data augmentation techniques, this project strives to enhance diagnostic accuracy and provide valuable support to medical professionals in early detection and treatment planning of brain tumors.

## Dataset
The images that make up this dataset comes from [Brain Tumor Classification (MRI)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri). The dataset contains 4 separate classification materials for brain tumors, totaling in 3160 brain scans. The 4 classifications include pituitary, glioma, meningioma and no tumors. Below are examples of each type of tumor, and the cross sections of scans exist in all three planes of the brain.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tum1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

I chose to use 80% of my data for my training set, and the remaining 20% for my testing set. Since image size varies within the dataset, I resized all the images to 180x180 pixels and batch units of 32 for the model.

## Data Augmentation and Preventing Overfitting

Before creating the model, I had to augment my data and also prevent future overfitting. Some of the techniques I used to augment the data was flip the image horizontally, change the rotation of the image, zooming in on the image, and finally changing the brightness of the image. Augmenting the image gives our dataset more variety without adding more data, and can help prevent overfitting. 

```python
data_augmentation = keras.Sequential(
  [
    layers.RandomFlip("horizontal",
                      input_shape=(img_height,
                                  img_width,
                                  3)),
    layers.RandomBrightness(0.25),
    layers.RandomRotation(0.2),
    layers.RandomZoom(0.2)
  ]
)
```

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tum2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This single brain scan is augmented to have different rotatins, size, and even brightness levels.
</div>

## Creating the Model

I used the Keras Sequential method to create the model. After the model was created, it showed that the optimal number of epochs for this seed was 40. The accuracy of this model on this specific testing and validation set is 86%, meaning 86 out of 100 images were identified with the right tumor type. 

```python
model = tf.keras.Sequential([
    data_augmentation,
    layers.Rescaling(1./255),
    layers.Conv2D(16, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Conv2D(32, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Conv2D(64, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Dropout(0.5),  # Increased dropout rate
    layers.Flatten(),
    layers.Dense(128, activation='relu'),
    layers.Dense(len(class_names), activation='softmax')
])


model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])
```

Below is an image of what the validation vs testing accuracy and loss appear to be. For both graphs, the validation and testing curve both closely follow each other, which means that overfitting does not seem to be a problem, as we have prevented earlier. The graphs also show that as epochs approaches 40, the validation loss hits the minimum.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tum3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Our goal is to have the Training Loss and Validation Loss curves to follow close to each other, while they increase in accuracy and decrease in loss as epoch size elevates.
</div>

A confusion matrix is important to understand which tumors are harder to diagnose, but even more importantly, whether we had false positives or false negatives. Given the confusion matrix below, it shows that two hardest tumors to diagnose are meningioma and glioma. Out of the 632 observations in our validation set 12 of them are false positives, and 7 of them are false negatives. This means our model has a 1.9% to classify a brain scan of a healthy person to have a tumor, and a 1.1% to classify a brain scan of a tumor having a person as completely healthy.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tum4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Results

In this dataset, our model is able to correctly identify 544 brain tumors out of the 632 validation elements, meaning that the model has an 86% accuracy rate. However, its also important to test random data outside the current dataset I used. Using an meningioma tumor image provided by [UCSF](https://www.ucsf.edu/news/2023/11/426541/can-gene-expression-predict-if-brain-tumor-likely-grow-back), I tested the model I created to see if it accurately identified the tumor.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/menin2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Meningioma tumor found in brain scan.
</div>

```python
test_path = "menin2.jpg"

img = tf.keras.utils.load_img(
    test_path, target_size=(img_height, img_width)
)
img_array = tf.keras.utils.img_to_array(img)
img_array = tf.expand_dims(img_array, 0) # Create a batch

predictions = model.predict(img_array)
score = tf.nn.softmax(predictions[0])
```

After running the model on this image, it correctly identified the tumor as a meningioma tumor with 78.89% confidence.