# KITCHEN-UTENSILS-CLASSIFIER
# CODE:
~~~
PROGRAM DEVELOPED BY:R.SOMEASVAR
REGISTER NUMBER: 212221230103
~~~
~~~
import splitfolders  # or import split_folders
splitfolders.ratio("Raw", output="output", seed=1337, ratio=(.9, .1), group_prefix=None) # default values
import matplotlib.pyplot as plt
import matplotlib.image as mping
img = mping.imread("output/val/BREAD_KNIFE/breadkniferaw2.jpg")
plt.imshow(img)
~~~
![1](https://user-images.githubusercontent.com/93434149/172606034-f21aa36a-097d-4679-9262-b2874951ddb1.jpg)
~~~
import tensorflow as tf
from tensorflow.keras.preprocessing.image import ImageDataGenerator
train_datagen = ImageDataGenerator(rescale=1./255,
    rotation_range=0.2,
    width_shift_range=0.2,
    height_shift_range=0.2,
    horizontal_flip=True,
    zoom_range=0.2)
test_datagen = ImageDataGenerator(rescale=1./255)
train = train_datagen.flow_from_directory("output/train/",target_size=(224,224),seed=42,batch_size=32,class_mode="categorical")
test = train_datagen.flow_from_directory("output/val/",target_size=(224,224),seed=42,batch_size=32,class_mode="categorical")
from tensorflow.keras.preprocessing import image
test_image = image.load_img('output/val/BREAD_KNIFE/breadkniferaw2.jpg', target_size=(224,224))
test_image = image.img_to_array(test_image)
test_image = tf.expand_dims(test_image,axis=0)
test_image = test_image/255.
test_image.shape
import tensorflow_hub as hub
m = tf.keras.Sequential([
hub.KerasLayer("https://tfhub.dev/tensorflow/efficientnet/b0/feature-vector/1"),
tf.keras.layers.Dense(20, activation='softmax')
])
m.compile(loss=tf.keras.losses.CategoricalCrossentropy(),optimizer=tf.keras.optimizers.Adam(),metrics=["accuracy"])
history = m.fit(train,epochs=5,steps_per_epoch=len(train),validation_data=test,validation_steps=len(test))
classes=train.class_indices
classes=list(classes.keys())
m.predict(test_image)
classes[tf.argmax(m.predict(test_image),axis=1).numpy()[0]]
import pandas as pd
pd.DataFrame(history.history).plot()
~~~
![2](https://user-images.githubusercontent.com/93434149/172606508-0b5a1457-3ada-47a6-aad0-68a29777440b.jpg)

