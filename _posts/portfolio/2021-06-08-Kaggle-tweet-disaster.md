---
layout: article
title: "Tweet disaster | Kaggle"
categories: portfolio
modified: 2021-06-08T13:33:11-04:00
tags: [Kaggle]
comments: true
ads: false
share: false
image:
  feature: TD1002x408.png
  teaser: kaggle2.png
---

¿Se habla en un tweet sobre un desastre?.

Con un dataset de 5346 tweet para el entrenamiento y 2247 para test se busca
un modelo que prediga si un en un tweet se está hablando de algún tipo de desastre.  

En este [enlace a kaggle](https://www.kaggle.com/c/the-bridge-nlp/overview) se definen las normas de la competición.

La competición se realiza junto a [José Serrat](https://www.linkedin.com/in/joseserrat/) y [Ramón Ascanio](https://www.linkedin.com/in/ram%C3%B3n-ascanio-armada-78196a176/) 

[Enlace al código en github](https://github.com/FcoJavierMelo/my_projects/tree/main/Kaggle-Tweet%20Disaster)

En primer lugar eliminamos los duplicados, eliminamos signos de puntuación, caracteres como '@' o '#'
, dejamos todo el texto en minúsculas y eliminamos los espacios iniciales y finales del texto.

```python 
# Eliminamos los duplicados
X_train = X_train.drop_duplicates(subset='text')

import re

signos = re.compile("(\.)|(\;)|(\:)|(\!)|(\?)|(\,)|(\")|(\()|(\))|(\[)|(\])|(\d+)|(\>)|(\=)|(\<)")
signos_arroba = re.compile("(\.)|(\;)|(\:)|(\!)|(\?)|(\,)|(\")|(\()|(\))|(\[)|(\])|(\d+)|(\>)|(\=)|(\<)|(\@)|(\#)")

def signs_tweets(tweet):
    return signos_arroba.sub('', tweet.lower())

X_train['text'] = X_train['text'].apply(signs_tweets)
X_train['text'].head()
X_test['text'] = X_test['text'].apply(signs_tweets)
X_test['text'].head()

X_train['text'] = X_train['text'].str.strip()
X_test['text'] = X_test['text'].str.strip()
```

También eliminamos los links del texto

```python 
def remove_links(df):
    return " ".join(['{link}' if ('http') in word else word for word in df.split()])

X_train['text'] = X_train['text'].apply(remove_links)
X_test['text'] = X_test['text'].apply(remove_links)
```

Tras esto eliminamos las 'stop words' (artículos, preposiciones, etc..)

```python 
from nltk.corpus import stopwords

english_stopwords = stopwords.words('english')

def remove_stopwords(df):
    return " ".join([word for word in df.split() if word not in english_stopwords])

X_train['text'] = X_train['text'].apply(remove_stopwords)
X_test['text'] = X_test['text'].apply(remove_stopwords)
```

Ahora buscamos la raíz de las palabras aplicando [stemming](https://es.wikipedia.org/wiki/Stemming), en este caso usamos
[SnowballStemmer](https://www.geeksforgeeks.org/snowball-stemmer-nlp/)

```python 
from nltk.stem.snowball import SnowballStemmer

def english_stemmer(x):
    stemmer = SnowballStemmer('english')
    return ' '.join([stemmer.stem(word) for word in x.split()])

X_train['text'] = X_train['text'].apply(english_stemmer)
X_test['text'] = X_test['text'].apply(english_stemmer)
```

Una vez preparados los datos probamos varios modelos (SVC, RandomForestClassifier, LogisticRegression) pero al final
usamos un [ensemble](https://www.iartificial.net/ensembles-voting-bagging-boosting-stacking/), el Voting Classifier.
 
```python 
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import VotingClassifier

vectorizer2 = CountVectorizer(binary=True)

vectorizer2.fit(X_train['text'])
               
X_train_baseline = vectorizer2.transform(X_train['text'])
X_test_baseline = vectorizer2.transform(X_test['text'])
                
log_clf = LogisticRegression(C=1.0, penalty='l2')
rnd_clf = RandomForestClassifier(n_estimators=90, max_depth=2, max_features=2)
svm_clf = SVC(gamma='scale', probability=True, C=9.8, degree=0.0009)

estimators = [('lr', log_clf), ('rf', rnd_clf), ('svc', svm_clf)]


voting_clf = VotingClassifier(estimators = estimators,
                             voting='soft')
```

Obtenemos un buen resultado:

<figure>
	<img src="{{ site.url }}/images/TD1.png">
	<figcaption>Resultado en Kaggle</figcaption>
</figure>
 


