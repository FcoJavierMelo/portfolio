---
layout: article
title: "Obtención de datos de ofertas de trabajo"
categories: portfolio
modified: 2021-06-09T09:43:11-04:00
tags: [Web Scrapping, AWS]
comments: true
ads: false
share: false
image:
  feature: WSA1024x256.jpg
  teaser: WSA400x250.PNG
---

Mediante Web Scrapping obtenemos datos de relevancia de Linkedin y Glassdoor 
que luego consumimos a través de una API alojada en AWS.

Este proyecto fue desarrollado junto a [Arturo Guzmán](https://www.linkedin.com/in/arturo-guzm%C3%A1n-solera/) y [Cristina Martínez](https://www.linkedin.com/in/cristina-mart%C3%ADnez-garc%C3%ADa-438209170/)

[Enlace al código en github](https://github.com/FcoJavierMelo/my_projects/tree/main/Linkedin%26Glassdoor%20Data)

La idea surge en un proyecto multidisciplinar, donde trabajamos codo con codo con desarrolladores fullstack, 
diseñadores UI/UX y equipo de ciberseguridad.

Se plantea una app de cursos formativos donde el punto diferenciador sea que, para cada curso,
se den referencias de que ofertas laborales existen para las tecnologías que se cursaran 
y los sueldos medios de los perfiles profesionales que encajan con dichos cursos.

Para el MPV (Mínimo Producto Viable) realizamos el Scrapping de dos webs, Linkedin para 
la obtención de los datos de las ofertas de trabajo y Glassdoor para los datos de sueldos  
medios. El scrapping se realiza mediante la librería [Selenium](https://selenium-python.readthedocs.io/)

Los datos obtenidos se consolidan en una BBDD MySQL alojada en RDS de AWS.

Para el consumo de datos por parte de la app se crea un API REST con la librería [Flask](https://flask.palletsprojects.com/en/2.0.x/) 
y se aloja en AWS con el servicio [Elastic Beanstalk](https://docs.aws.amazon.com/es_es/elasticbeanstalk/latest/dg/Welcome.html)

<figure>
	<img src="{{ site.url }}/images/WSA1.PNG">
	<figcaption>Diagrama de arquitectura</figcaption>
</figure>  

Para una siguiente fase se pretendía incluir el scrapeo en un [AWS Lambda](https://aws.amazon.com/es/lambda/?nc2=type_a) 
para automatizar la obtención de datos y su actualización.


 
