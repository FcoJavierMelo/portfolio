---
layout: article
title: "Electric Vehicles"
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
*	Datos de generación de electricidad  
https://ourworldindata.org/energy
https://github.com/owid/energy-data

*	Ventas de enchufables en Europa  
[https://en.wikipedia.org/wiki/Electric_car_use_by_country]()  
[https://carsalesbase.com/]()  

*	Ventas de vehículos en Europa  
[https://www.km77.com/]()

*	Emisiones en la fabricación de vehículos  
[https://theicct.org/]()

Además, se utilizan otras fuentes para el conocimiento de campo  

[http://www.energimyndigheten.se/globalassets/forskning--innovation/transporter/c243-the-life-cycle-energy-consumption-and-co2-emissions-from-lithium-ion-batteries-.pdf]()  
  
[https://theicct.org/sites/default/files/publications/EV-life-cycle-GHG_ICCT-Briefing_09022018_vF.pdf]()


Para la realización del estudio se ha usado:

* Streamlit
* Pandas
* Plotly
* BeautifulSoup

Puedes acceder ala dashboard de Streamlit en este enlace:

[https://share.streamlit.io/fcojaviermelo/eda_ev_europe/src/EDA_streamlit.py](Dashboard)

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
