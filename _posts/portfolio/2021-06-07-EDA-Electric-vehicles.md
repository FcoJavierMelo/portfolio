---
layout: article
title: "Electric Vehicles (EDA & MachineLearning)"
categories: portfolio
modified: 2021-06-07T16:28:11-04:00
tags: [EDA]
comments: true
ads: false
share: false
image:
  feature: EVS1024x682.jpg
  teaser: EVS400x250.jpg
---

[Enlace al código en github](https://github.com/FcoJavierMelo/my_projects/tree/main/EDA_electric_vehicles)

# EDA: Coches electricos en Europa

En este EDA se va a analizar la irrupción del coche electrico desde dos puntos de vista:

* Impacto medioambiental
* Evolución de las ventas

Los datos se centraran en el mercado europeo por varios motivos:

 * Mercado mas heterogeneo que el de USA o China
 * Mayor variedad en la oferta de vehiculos electricos
 * Regulación de emisiones a nivel de la EU
 
Se manejan las siguientes hipotesis:
 
* El coche electrico ayuda a rebajar las emisisones, sobre todo con energia renovable
* Las ventas estan empezando un punto de inflexión


Se utilizan varias fuentes de datos para el EDA:

*	Datos de generación de electricidad: 
[https://ourworldindata.org/energy]() 
[https://github.com/owid/energy-data]()

*	Ventas de enchufables en Europa:  
[https://en.wikipedia.org/wiki/Electric_car_use_by_country]()  
[https://carsalesbase.com/]()  

*	Ventas de vehículos en Europa:  
[https://www.km77.com/]()

*	Emisiones en la fabricación de vehículos:  
[https://theicct.org/]()

Además, se utilizan otras fuentes para el conocimiento de campo: 

[http://www.energimyndigheten.se/globalassets/forskning--innovation/transporter/c243-the-life-cycle-energy-consumption-and-co2-emissions-from-lithium-ion-batteries-.pdf]()  
  
[https://theicct.org/sites/default/files/publications/EV-life-cycle-GHG_ICCT-Briefing_09022018_vF.pdf]()


Para la realización del estudio se ha usado:

* Streamlit
* Pandas
* Plotly
* BeautifulSoup

Puedes acceder ala dashboard de Streamlit en este enlace:

[Dashboard](https://share.streamlit.io/fcojaviermelo/eda_ev_europe/src/EDA_streamlit.py)


# Machine Learning: Reconocimiento de modelo mediante imagenes

Se crea un dataset de imágenes para 11 modelos de vehículos eléctricos.

Mediante transfer learning y utilizando dos modelos distintos (ResNet50 y EfficentNetB7)
se comprueba si una imagen corresponde a uno de los modelos.

Para la realización del modelo se ha usado:

* tensorflow
* keras

## Contaminación 

<figure>
	<img src="{{ site.url }}/images/EVS1.PNG">
	<figcaption>Producción de electricidad en Europa (Tw/h)</figcaption>
</figure>

<figure>
	<img src="{{ site.url }}/images/EVS2.PNG">
	<figcaption>Producción de electricidad en Europa (porcentaje)</figcaption>
</figure>

<figure>
	<img src="{{ site.url }}/images/EVS3.PNG">
	<figcaption>Producción de electricidad por pais (porcentaje)</figcaption>
</figure>

<figure>
	<img src="{{ site.url }}/images/EVS4.PNG">
	<figcaption>Mapa produccion por fuente de generación</figcaption>
</figure>

<figure>
	<img src="{{ site.url }}/images/EVS5.PNG">
	<figcaption>Emisiones vida util vehículo</figcaption>
</figure>

## Ventas 

<figure class="half">
	<img src="{{ site.url }}/images/EVS6.PNG">
	<img src="{{ site.url }}/images/EVS7.PNG">
</figure>

<figure class="half">
	<img src="{{ site.url }}/images/EVS8.PNG">
	<img src="{{ site.url }}/images/EVS9.PNG">
</figure>

## MACHINE LEARNING

<figure>
	<img src="{{ site.url }}/images/portfolio6.PNG">
	<figcaption>Predicciones</figcaption>
</figure>


Al ser un dataset poco numeroso utilizamos Data Augmentation: 

[Articulo explicativo](https://enmilocalfunciona.io/tratamiento-de-imagenes-usando-imagedatagenerator-en-keras/)

```python 
train_datagen = ImageDataGenerator(
    rescale=1. / 255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True)

test_datagen = ImageDataGenerator(rescale=1. / 255)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(img_height, img_width),
    batch_size=batch_size,
    class_mode='categorical')

validation_generator = test_datagen.flow_from_directory(
    TEST_PATH,
    target_size=(img_height, img_width),
    batch_size=batch_size,
    class_mode='categorical')
```

En el caso del modelo ResNet50 no utilizamos weights por defecto: 

```python 
model = applications.ResNet50(input_shape=(img_width, img_height, 3),
                              include_top=False)
```

Si lo hacemos para EfficientNetB7 usando weights='imagenet':
```python                        
model = efn.EfficientNetB7(input_shape=(img_width, img_height, 3),
                           weights='imagenet',
                           include_top = False, 
                           classes=11)                          
```

Definimos las capas densas:
```python                        
for layer in model.layers:
    layer.trainable = False
    
x = model.output
x = Flatten()(x)
x = Dense(1024, activation="relu")(x)
x = Dropout(0.5)(x)
x = Dense(1024, activation="sigmoid")(x)
predictions = Dense(11, activation="softmax")(x)
model_final = Model(model.input, predictions)                        
```

Y hacemos el compile y el entrenamiento del modelo:
```python                        
fmodel_final.compile(loss='categorical_crossentropy',
              optimizer=optimizers.SGD(lr=1e-4, momentum=0.9),
              metrics=['accuracy'])

history_nasa = model_final.fit(
    train_generator,
    epochs=50, 
    shuffle = True, 
    verbose = 1,
    validation_data=validation_generator)                       
```
<figure class="half">
	<img src="{{ site.url }}/images/EVS10.PNG">
	<img src="{{ site.url }}/images/EVS11.PNG">
	<figcaption>Ejemplo de predicción sobre una imagen de un Ford Mustang Mach-E</figcaption>
</figure>
