# 🆆🅰🆆 - WasteAgainstWaste
>"There is no such thing as "Away". When we throw anything away, it must go somewhere." <br />                                                  
                                                                ~_Annie Leonard_    <br />
## Our Team  🎳<br />
Name - Nityanand Mathur  <br />
Email - nityanand.mathur@iiitg.ac.in. Contact number: `7247412358`     
Name - Ayush Pratap Singh  <br />
Email - ayush.pratap@iiitg.ac.in.  Contact number: `98391662530` <br />
Name - Nisheet Karan <br />
Email - nisheet.karan@iiitg.ac.in.  Contact number: `8529468896` <br />

## Project Details  💻<br />
**Domain**: Artificial Intelligence and Machine Learning <br />
**Project Name**: Waste Against Waste<br />
**Theme**: Health and Safety<br />

The libraries used in project are as follows:<br /> 
• **NumPy** <br />
• **Pandas**<br />
• **TensorFlow** <br />

 ### Serious predicament  ➖➕✖️➗<br />
India is getting buried in its own garbage as a huge quantity of waste produced daily is <br />
never picked up and pollutes land, air and water. Also, it is evident that we could not stop <br />
production of waste due to modern world demands. <br />
![E-waste](https://image.freepik.com/free-vector/ewaste-banner_106317-3673.jpg)            
### Simulation How we'll be encountering this. 🦾 ⚙️      
Our project's ultimate goal is to get acknowledged about any waste that is accumulated in <br />
nearby areas by the means of cameras and sensors of old phones/ E-devices integrated with <br />
Arduino and GSM technology implemented in a drone or the redundant cell phones themselves,<br />
using a Machine Learning model that will identify waste materials and will inform nearest <br />
local authorities about the location of waste. <br />
[![Watch the video](https://github.com/nisheetkaran/Simulation/blob/master/Thumbnaill%20(1).jpg)](https://www.youtube.com/watch?v=MCLqEWsy0A0)<br />
The basic idea is to use the products which became redundant with time. Since the production <br />
of waste is inevitable, we can still try to use redundant items which if not used will be considered<br />
as electronic-waste. <br />

Following is a list of some items that can be used from old devices to reduce the cost of<br />
implementation:<br />
• Infrared sensor from old Laptops.<br />
• Proximity Sensor and Accelerometer from old phones.<br />
• Temperature sensors from old devices like Microwaves.<br />
• Cameras from Xbox, PlayStation, etc.<br />
• Micro-controllers from PCs'.<br />

## Description Of the Project: 📝

The focus of this project is to build a system which takes a live video feed (or multiple mages) and extracts data from the images as to identify places contaminated with waste/litter. We could use gyroscope, accelerometers and proximity sensors from old mobile phones to construct an Arduino based drone. The drone would capture a broad area such as a college campus or a street and provide an aerial view of the location. The drone will be able to transfer the videos/images recorded by it to the local server which is a computer system applying the Machine Learning Algorithms. Arduino will be used for handling the transfer operations which will allow for processing of captured images. GSM and GPS module placed over the drone, which could be extracted from old cellular devices and GPS systems, would provide location of the place to be shared. This data can then be processed using Machine Learning image processing algorithms coded in Python. The dataset would contain images of waste such as waste metal cans, bottles, crumpled paper, plastic bags, cigarettes, etc. We could use old mobile phones directly to monitor public places. We could use solar power generation as it is a renewable source of energy.

## Sequence of Implementations
### How we classified Images Step by Step 
We have classified images of Waste and Non-waste, First we have created an image classifier using used `tf.keras.Sequential` then we have loaded data using `tf.keras.utils.image_dataset_from_directory`
### 1. First and foremost, we will import libraries so that we can use functionalities and it will make our work easier.

```python
import numpy as np
import pandas as pd
import PIL
import os
import tensorflow as tf
import matplotlib.pyplot as plt
import pathlib
import glob2
```
Now, using sub-libraries of tensorflow.
```python
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.models import Sequential
```
### 2. Downloading and ~~exploring~~ the dataset
Our dataset contains around ______ of photos. This dataset contains two directories, one per class: <br />
`1. Garbage\` <br />
`2. Clean\` <br />

We are using the dataset which is already available in our device.
```python
data_dir = 'F:\WAW-WasteAgainstWaste\data'
data_dir = pathlib.Path(data_dir)
```
We are using the function glob2.glob() directly from glob module to retrieve paths recursively from inside the directories and subdirectoriess.
```python
files = glob2.glob('F:\WAW-WasteAgainstWaste\data\*.jpg')
```

### 3. Loading data using a Keras utility

`tf.keras.utils.image_dataset_from_directory` utility will help us to load images off-disk, Now, We will move from directory of images on disk to `tf.data.Dataset`

#### Creating a dataset
Defining parametres for the loader.
```python
batch_size = 32
image_height = 180
image_width = 180
```
To ensure our model is trained well (that is, its neither overfitted nor underfitted). We used validation splitting during fine-tuning of our dataset (that too with 80 % for model training and 20 % for validation, that is considered good ratio) 

```python
train_ds = tf.keras.utils.image_dataset_from_directory(
  data_dir,
  validation_split=0.2,
  subset="training",
  seed=123,
  image_size=(image_height, image_width),
  batch_size=batch_size)
```

```python
val_ds = tf.keras.utils.image_dataset_from_directory(
    data_dir,
    validation_split=0.2,
    subset="validation",
    seed=123,
    image_size=(image_height, image_width),
    batch_size=batch_size)
```
To find the class names in the class_names attribute on these datasets, we can use.
```python
class_names = train_ds.class_names
print(class_names)
```
It'll show the names of the directories in alphabhabetical order.
In our case it will look like as following :
`['Garbage','Clean']`


### 4. Visualizing the data 
To see first nice images from the training dataset
```python
plt.figure(figsize=(10,10))
for images, labels in train_ds.take(1):
    for i in range(6):
        ax = plt.subplot(3,2,i+1)
        plt.imshow(images[i].numpy().astype("uint8"))
        plt.title(class_names[labels[i]])
        plt.axis("off")
```

#### Configuring the dataset for performance
Using buffered prefetching is important as we will be able to yield data from disk without having I/O being blocked. Two important methods we are using to load data are:
1)**Dataset.cache** Will help in keeping the images in memory after they're loaded off disk during the first epoch. This will ensure the dataset does not become a gridlock while training our model. If your dataset is too large to fit into memory, you can also use this method to create a performant on-disk cache.
2)**Dataset.prefetch** Will help in overlaping data preprocessing and model execution while training.

```python
AUTOTUNE = tf.data.AUTOTUNE

train_ds = train_ds.cache().shuffle(1000).prefetch(buffer_size=AUTOTUNE)
val_ds = val_ds.cache().prefetch(buffer_size=AUTOTUNE)
```

#### Standardizing the data
The RGB channel values are in the [0, 255] range. This is not ideal for a neural network; in general you should seek to make your input values small.
Here, you will standardize values to be in the [0, 1] range by using `tf.keras.layers.Rescaling` :
```python
normalization_layer = layers.Rescaling(1./255)
```
We applied it to the dataset by calling `Dataset.map` :
```python
normalized_ds = train_ds.map(lambda x, y: (normalization_layer(x),y))
image_batch, labels_batch = next(iter(normalized_ds))
first_image = image_batch[0]
print(np.min(first_image), np.max(first_image))
```
### 4. Creating the model
The Sequential model consists of three convolution blocks `tf.keras.layers.Conv2D` with a max pooling layer `tf.keras.layers.MaxPooling2D` in each of them. There's a fully-connected layer `tf.keras.layers.Dense` with 128 units on top of it that is activated by a ReLU activation function `relu`. 
```python
num_classes = 5

model = Sequential([
    layers.Rescaling(1./255, input_shape=(image_height, image_width, 3)),
    layers.Conv2D(16, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Conv2D(32, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Conv2D(64, 3, padding='same', activation='relu'),
    layers.MaxPooling2D(),
    layers.Flatten(),
    layers.Dense(128, activation='relu'),
    layers.Dense(num_classes)
])
```
### 5. Compiling the model
Using `tf.keras.optimizers.Adam` optimizer to implement adam's algorithm and `tf.keras.losses.SparseCategoricalCrossentropy` to compute the crossentropy loss between the labels and predictions. To view training and validation accuracy for each training epoch,we are passing the metrics argument to `Model.compile`.

```python
model.compile(optimizer='adam',
             loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
             metrics=['accuracy'])
```
### 6. Model summary
It's good to view all the layers of the network using the model's `Model.summary` method, it gives us a clarity and status of models. How many of our total params are Trainable and how many are not.

```python
model.summary()
```
### 7. Training the model

We already reached adequate amount of accuracy on implementing our algorithm three to four times but, our confidence level/percentage for checking on external test images was low, so increasing number of epocs helped us in reaching the confidence level close to that we wanted our model to have.

```python
epochs=10
history = model.fit(
    train_ds,
    validation_data=val_ds,
    epochs=epochs
)
```
### 8. Visualizing training results
Creating plots of **loss** and **accuracy** on the train and validation sets:

```python
acc = history.history['accuracy']
val_acc = history.history['val_accuracy']

loss = history.history['loss']
val_loss = history.history['val_loss']

epochs_range = range(epochs)

plt.figure(figsize=(8,8))
plt.subplot(1,2,1)
plt.plot(epochs_range, acc, label='Training Accuracy')
plt.plot(epochs_range, val_acc, label='Validation Accuracy')
plt.legend(loc='lower right')
plt.title('Training and Validation Accuracy')

plt.subplot(1,2,2)
plt.plot(epochs_range, loss, label = 'Training Loss')
plt.plot(epochs_range, val_loss, label = "Validation Loss")
plt.legend(loc = 'upper right')
plt.title("Training and Validation Loss")
plt.show()
```





        
    





