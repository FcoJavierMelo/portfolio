---
layout: article
title: "Give me some credit | Kaggle"
categories: portfolio
modified: 2021-06-05T11:21:11-04:00
tags: [Kaggle]
comments: true
ads: false
share: false
image:
  feature: GSC1024x256.png
  teaser: kaggle2.png
---

A partir del dataset que se facilita se debe generar un modelo que prediga si un cliente tiene una situación financiera adecuada para otorgarle un crédito.

En este [enlace a kaggle](https://www.kaggle.com/c/give-me-some-credit-20210326/overview) se definen las normas de la competición.

Esta competición ha sido realizada junto a [Arturo Guzmán Solera](https://www.linkedin.com/in/arturo-guzm%C3%A1n-solera/)

[Enlace al código en github](https://github.com/FcoJavierMelo/my_projects/tree/main/Kaggle-Give%20Me%20Some%20Credit)

Tras analizar el dataset decidimos aplicar un KNN para imputar los NaN 

[Artículo sobre el tema](https://medium.com/@kyawsawhtoon/a-guide-to-knn-imputation-95e2dc496e)

```
RangeIndex: 104805 entries, 0 to 104804
Data columns (total 12 columns):
 #   Column                                Non-Null Count   Dtype  
---  ------                                --------------   -----  
 0   Id                                    104805 non-null  int64  
 1   SeriousDlqin2yrs                      104805 non-null  int64  
 2   RevolvingUtilizationOfUnsecuredLines  104805 non-null  float64
 3   age                                   104805 non-null  int64  
 4   NumberOfTime30-59DaysPastDueNotWorse  104805 non-null  int64  
 5   DebtRatio                             104805 non-null  float64
 6   MonthlyIncome                         84024 non-null   float64
 7   NumberOfOpenCreditLinesAndLoans       104805 non-null  int64  
 8   NumberOfTimes90DaysLate               104805 non-null  int64  
 9   NumberRealEstateLoansOrLines          104805 non-null  int64  
 10  NumberOfTime60-89DaysPastDueNotWorse  104805 non-null  int64  
 11  NumberOfDependents                    102056 non-null  float64
dtypes: float64(4), int64(8)
```

```python 
#Imputación de NaN mediante KNN
imputer = KNNImputer(n_neighbors=3)
imputer.fit(df_train[['Id', 'SeriousDlqin2yrs', 'RevolvingUtilizationOfUnsecuredLines', 'age',
       'NumberOfTime30-59DaysPastDueNotWorse', 'DebtRatio', 'MonthlyIncome',
       'NumberOfOpenCreditLinesAndLoans', 'NumberOfTimes90DaysLate',
       'NumberRealEstateLoansOrLines', 'NumberOfTime60-89DaysPastDueNotWorse',
       'NumberOfDependents']])

df_train[['Id', 'SeriousDlqin2yrs', 'RevolvingUtilizationOfUnsecuredLines', 'age',
       'NumberOfTime30-59DaysPastDueNotWorse', 'DebtRatio', 'MonthlyIncome',
       'NumberOfOpenCreditLinesAndLoans', 'NumberOfTimes90DaysLate',
       'NumberRealEstateLoansOrLines', 'NumberOfTime60-89DaysPastDueNotWorse',
       'NumberOfDependents']] = imputer.transform(df_train[['Id', 'SeriousDlqin2yrs', 'RevolvingUtilizationOfUnsecuredLines', 'age',
       'NumberOfTime30-59DaysPastDueNotWorse', 'DebtRatio', 'MonthlyIncome',
       'NumberOfOpenCreditLinesAndLoans', 'NumberOfTimes90DaysLate',
       'NumberRealEstateLoansOrLines', 'NumberOfTime60-89DaysPastDueNotWorse',
       'NumberOfDependents']])
```

Tras probar varios modelos nos decidimos por xgboost, usamos GridSearchCV para 
obtener los mejores parametros

```python 
# XGBoost
grid_xgboost = {
                          "learning_rate": [0.05, 0.1, 0.15, 0.2, 0.25, 0.4, 0.5],  # Cuanto más alto, mas aporta cada nuevo arbol
                          
                          "n_estimators": [20,50,75,100, 150,200], # Cuidado con poner muchos estiamdores ya que vamos a
                                                           # sobreajustar el modelo
                          
                          "max_depth": [1,2,3,4,5] # No es necesario poner una profundiad muy alta. Cada nuevo
                                                    # arbol va corrigiendo el error de los anteriores.
                          
                          }

xgb_clas = xgboost.XGBClassifier()

model = [('grid_xgboost', xgb_clas, grid_xgboost1)]
models_gridsearch = {}

for i in model:
    models_gridsearch[i[0]] = GridSearchCV(i[1],
                                          i[2],
                                          cv = 10,
                                          verbose =1,
                                          scoring = 'roc_auc',
                                          n_jobs = -1)
    
    models_gridsearch[i[0]].fit(X_train, y_train)
```

Tras el entrenamiento obtenemos un [roc_auc](https://es.wikipedia.org/wiki/Curva_ROC) de 0.86

Los resultados en kaggle se mantienen 

<figure>
	<img src="{{ site.url }}/images/GSC1.PNG">
	<figcaption>Publico</figcaption>
</figure>

<figure>
	<img src="{{ site.url }}/images/GSC2.PNG">
	<figcaption>Privado</figcaption>
</figure>  

