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
production of waste due to modern world demands.
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

## Description Of the Project:

The focus of this project is to build a system which takes a live video feed (or multiple mages) and extracts data from the images as to identify places contaminated with waste/litter. We could use gyroscope, accelerometers and proximity sensors from old mobile phones to construct an Arduino based drone. The drone would capture a broad area such as a college campus or a street and provide an aerial view of the location. The drone will be able to transfer the videos/images recorded by it to the local server which is a computer system applying the Machine Learning Algorithms. Arduino will be used for handling the transfer operations which will allow for processing of captured images. GSM and GPS module placed over the drone, which could be extracted from old cellular devices and GPS systems, would provide location of the place to be shared. This data can then be processed using Machine Learning image processing algorithms coded in Python. The dataset would contain images of waste such as waste metal cans, bottles, crumpled paper, plastic bags, cigarettes, etc. We could use old mobile phones directly to monitor public places. We could use solar power generation as it is a renewable source of energy.

## Work flow/ Sequence of Working
### How we classified Images Step by Step 
We have classified images of Waste and Non-waste, First we have created an image classifier using used `tf.keras.Sequential` then we have loaded data using `tf.keras.utils.image_dataset_from_directory`
#### 1. First and foremost, we will import libraries so that we can use functionalities 

```python
import matplotlib.pyplot as plt
import numpy as np
import os
import PIL
import tensorflow as tf

from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.models import Sequential
```
### 2. Download and explore the dataset
Our dataset contains around ______ of photos. This dataset contains two directories, one per class: <br />
`1. Waste\` <br />
`2. Non-Waste\` <br />


