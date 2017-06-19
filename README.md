# Analyse des opinions sous twitter
## 1 Objectif  

L’objectif de ce TP est d’analyser un corpus de tweets en fonction des opinions exprimées (positif/-
négatif). Le langage utilisé est Python.

## 2 Documentation : Python et NLTK - Natural Language processing Toolkit  

Si vous débutez en python et que vous souhaitez utiliser votre propre machine, vous pouvez installer
la distribution anaconda de python. Plus d’information sur l’installation ou pour bien démarrer avec
Python sur les pages suivantes :
- http://perso.telecom-paristech.fr/~gramfort/liesse_python/1-Intro-Python.html  
- http://perso.telecom-paristech.fr/~gramfort/liesse_python/2-Numpy.html  
- http://perso.telecom-paristech.fr/~gramfort/liesse_python/3-Scipy.html  
- http://scikit-learn.org/stable/index.html  
- http://www.loria.fr/~rougier/teaching/matplotlib/matplotlib.html  
- http://jrjohansson.github.io/  
Vous aurez sûrement besoin d’une documentation sur NLTK :
– Le site de NLTK : http://www.nltk.org/  
– La notice d’utilisation de l’interface de WordNet pour NLTK : http://www.nltk.org/howto/wordnet.html  
– La notice d’utilisation du PoS tagger : http://www.nltk.org/book/ch05.html
... et sur WordNet et SentiWordNet :  
– Le site web de WordNet : http://wordnet.princeton.edu/  
– Le site web de SentiWordNet : http://sentiwordnet.isti.cnr.it/  
– Le site de Christopher Potts, qui explique comment utiliser WordNet avec NLTK et fournit un script pour pouvoir utiliser

## 3 Implémentation d’un système de détection d’opinions dans les tweets  

### 3.1 Matériel : Présentation du corpus de tweets  

Les tweets à analyser sont disponibles à l’adresse suivante :
http://perso.telecom-paristech.fr/~clavel/DonneesTweets/testdata.manual.2009.06.14.csv  
Cette base (Sentiment140) a été obtenue sur le site de l’université de Stanford http://help.sentiment140.com/for-students  
La base contient 498 tweets annotés manuellement. La base propose 6 champs correspondant aux informations suivantes :

1. la polarité du tweet : Chaque tweet est accompagné d’un score pouvant être égal à 0 (négatif), 2
(neutre) ou 4 (positif).
2. l’identifiant du tweet (2087)
3. la date du tweet (Sat May 16 23 :58 :44 UTC 2009)
4. la requête associée (lyx). Si pas de requête la valeur est NO_ QUERY.
5. l’utilisateur qui a tweeté (robotickilldozr)
6. le texte du tweet(Lyx is cool)

### 3.2 Prétraitements
Les tweets contiennent des caractères spéciaux susceptibles de nuire à la mise en place des méthodes
d’analyse d’opinions. Ecrire un programme permettant pour chaque tweet de :
– récupérer le texte associé
– segmenter en tokens
– supprimer les urls
– nettoyer les caractères inhérents à la structure d’un tweet
– corriger les abréviations et les spécificités langagières des tweets à l’aide du dictionnaire
DicoSlang disponible ici :
http://perso.telecom-paristech.fr/~clavel/DonneesTweets/SlangLookupTable.txt  

### 3.3 Etiquetage grammatical
Développer une fonction capable de déterminer la catégorie grammaticale (POS : Part Of Speech) de
chaque mot du tweet.

### 3.4 Algorithme de détection v1 : appel au dictionnaire Sentiwordnet
NLTK dispose entre autre d’une interface pour manipuler la base de données WordNet. Ainsi, après
installation de NLTK et du package WordNet, un utilisateur peut accéder à l’ensemble des synsets qui
sont liés à un mot donné à l’aide d’une commande simple sous Python.

SentiWordNet est une extension de WordNet disponible à  
http://perso.telecom-paristech.fr/~clavel/DonneesTweets/SentiWordNet_3.0.0_20130122.txt  
Vous aurez également besoin de télécharger dans votre répertoire de travail le fichier suivant
http://compprag.christopherpotts.net/code-data/sentiwordnet.py
Pour cette étape, vous devez développer un programme permettant :
– de récupérer uniquement les mots correspondant à des adjectifs, noms, adverbes et verbes
– d’accéder aux scores (positifs et négatifs) des synsets dans la librairie NLTK. Ce script définira dans
une classe Python l’objet SentiSynset sur le même modèle que le Synset développé dans NLTK pour
WordNet, et permettra de lire le tableau de SentiWordNet.
– de calculer pour chaque mot les scores associés à leur premier synset,
– de calculer pour chaque tweet la somme des scores positifs et négatifs des SentiSynsets du tweet,
– de comparez la somme des scores positifs et des scores négatifs de chaque tweet pour décider de la
classe à associer au tweet.

### 3.5 Algorithme de détection v2 : gestion de la négation et des modifieurs
La liste des mots en anglais correspondant à des négations est disponible à http://perso.telecom-paristech.fr/~clavel/DonneesTweets/NegatingWordList.txt  
et celle correspondant aux modifieurs à  
http://perso.telecom-paristech.fr/~clavel/DonneesTweets/BoosterWordList.txt  
Pour chaque mot, l’algorithme doit effectuer les opérations suivantes :
– multiplie par 2 le score négatif et le score positif associés au mot si le mot précédent est un modifieur ;
– utilise uniquement le score négatif du mot pour le score positif global du tweet et le score positif
du mot pour le score négatif global du tweet si le mot précédent est une négation.

### 3.6 Algorithme de détection v3 : gestion des emoticons
Le dictionnaire d’émoticons est disponible dans  
http://perso.telecom-paristech.fr/~clavel/DonneesTweets/EmoticonLookupTable.txt  
Cet algorithme demande en entrée deux listes supplémentaires
: une liste d’emoticons positifs et une liste d’émoticons négatifs. Les émoticons positifs rencontrés
augmentent de 1 la valeur du score positif du tweet, tandis que les émoticons négatifs augmentent de 1
la valeur du score négatif du tweet.

### 3.7 Votre version : v4
En analysant les sorties des algorithmes proposés précédemment, proposez votre propre algorithme
d’analyse des opinions dans les tweets et les performances que vous obtenez.