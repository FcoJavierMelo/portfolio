---
layout: article
title: "¿Qué dices colega? | Kaggle"
categories: portfolio
modified: 2021-06-08T09:43:11-04:00
tags: [Kaggle]
comments: true
ads: false
share: false
image:
  feature: QDC1024x345.jpg
  teaser: kaggle2.png
---

Predicción mediante imágenes del lenguaje de signos.

Mediante un dataset de 9000 imágenes para entrenamiento y 1150 para test se busca
un modelo que prediga las letras del lenguaje de signos.  

En este [enlace a kaggle](https://www.kaggle.com/c/que-dices-colega/overview) se definen las normas de la competición.

La competición se realiza junto a [Cristina Martiñan](https://www.linkedin.com/in/cristinamantinan/), [Mario Massaro](https://www.linkedin.com/in/mariomassaro/) y [Marta Miñana](https://www.linkedin.com/in/martaminana1203/) 

[Enlace al código en github](https://github.com/FcoJavierMelo/my_projects/tree/main/Kaggle-que%20dices%20colega)

Utilizamos el modelo VGG16 sin incluir weights ni la capa densa

```python 
## VGG16

from tensorflow.keras.applications.vgg16 import VGG16

base_model = VGG16(input_shape = (IMAGE_SIZE, IMAGE_SIZE, 3),
                  include_top=False)

for layer in base_model.layers:
    layer.trainable = False

    
##### FULLY CONNECTED LAYER #####
# Flatten the output layer to 1 dimension
x = keras.layers.Flatten()(base_model.output)

# Add a fully connected layer with 512 hidden units and ReLU activation
x = keras.layers.Dense(1000, activation='relu')(x)

# Add a fully connected layer with 512 hidden units and sigmoid activation
x = keras.layers.Dense(1000, activation='sigmoid')(x)

# Add a fully connected layer with 512 hidden units and ReLU activation
x = keras.layers.Dense(1000, activation='relu')(x)

# Add a dropout rate of 0.5
x = keras.layers.Dropout(0.5)(x)

# Add a final sigmoid layer for classification
x = keras.layers.Dense(29, activation='softmax')(x)

model = tf.keras.models.Model(base_model.input, x)

model.compile(optimizer = 'adam', loss = 'sparse_categorical_crossentropy',metrics = ['acc'])
```

Tras el entrenamiento del modelo obtuvimos un accuracy en kaggle de 0.90 

<figure>
	<img src="{{ site.url }}/images/QDC1.PNG">
	<figcaption>Resultados tanto en privado como en publico</figcaption>
</figure>

Tras la finalización de la competición nor pico el gusanillo de probar con otros modelos
y nos decidimos por EfficientNetB7, en este caso incluimos tambien la capa densa del propio modelo.

```python 
## EfficientNetB7

import tensorflow.keras.applications as apli
from tensorflow.keras.optimizers import RMSprop

base_model = apli.EfficientNetB7(input_shape = (IMAGE_SIZE, IMAGE_SIZE, 3),
                                 include_top=True,
                                 weights=None)

base_model.compile(RMSprop(lr=0.0001, decay=1e-6), loss = 'sparse_categorical_crossentropy',metrics = ['acc'])
```

Tras muchas (de verdad, muchas) horas de entrenamiento y muchos epochs...

<figure>
	<img src="{{ site.url }}/images/QDC2.PNG">
	<figcaption>Resultados tanto en privado como en publico</figcaption>
</figure>

Wow!!, un 0.968 en la parte privada, nada mal.

Nos quedo la duda de si el modelo mejoraría aumentando la resolución hasta el 
máximo de las imágenes (200x200) en vez del 125x125 que usamos.
