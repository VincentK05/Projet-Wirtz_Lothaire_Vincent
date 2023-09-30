# LuxResto
## Sommaire
- Description
- Utilisation
- Explication du code
- Principales difficultées
- Développement
## Description
Vous vous apprêtez à explorer le plus beau (et unique...) Grand Duché au monde, le pays de toutes les opportunités! Cependant, bien que le Luxembourg ne soit pas célèbre pour sa gastronomie, à part peut-être son incontournable 'Bouneschlupp', vous ne savez pas où satisfaire vos papilles! C'est là que LuxResto intervient, pour vous aider à dénicher de délicieuses pépites culinaires.
\
\
LuxResto est votre meilleur allié pour trouver le restaurant parfait en fonction de vos envies gastronomiques, de vos exigences en matière de qualité et de votre emplacement. Que vous ayez envie de déguster une spécialité locale ou de vous aventurer vers d'autres horizons culinaires, nous avons le plan idéal pour vous.

## Utilisation
LuxResto a été élaboré en mettant l'accent sur la facilité d'utilisation.
Pour lancer l'application, l'utilisateur doit tout d'abord télécharger les fichiers *allRestos.json* et ... dans un même dossier. Ensuite, il suffit d'ouvrir le fichier ... sur Jupyter notebook et de lancer le code.
Afin d'assurer le fonctionnement du programme, les packages suivantes doivent être téléchargés au préalable:
```
pip install pandas
pip install geopy
pip install selenium
pip install ttkthemes
```

## Explication du code
Nous importons d’abord les différentes bibliothèques.
Les plus importantes étant la bibliothèque tkinter, qui va nous permettre de créer l’interface
graphique, pandas qui va nous permettre de manipuler les données, geopy.distance qui va nous
permettre de calculer la distance géographique entre l’adresse indiquée dans l’application et les
différents restaurants.
```
import tkinter as tk
from tkinter import filedialog
from tkinter import ttk
from ttkthemes import ThemedStyle
import pandas as pd
import geopy
from tkinter import ttk
import json
import time
import selenium
from selenium import webdriver
import random
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, WebDriverException
import geopy.distance
from geopy.geocoders import Nominatim
```
Nous créons ensuite une fonction appelée load_data, qui va nous permettre de charger les données
depuis le fichier allRestos.json où nous avons enregistré les données de tous les restaurants
luxembourgeois, d’effectuer un filtrage sur les données en fonction du type de cuisine sélectionné et
de la notation minimale sélectionnée, et d’ensuite afficher les résultats dans une interface graphique.
```
def load_data():
    # Open a file dialog to select the JSON file
    print(address_entry.get())
    address = address_entry.get()
    coordinates = getCoordinates(address)
    df = pd.read_json("./allRestos.json")
```
Pour le filtrage des données, nous enlevons tous les caractères inutiles de la variable « Tags » qui
donne le type de cuisine, comme les guillemets, pour n’avoir que le texte et que cela corresponde
aux choix de type de cuisine proposés dans l’application. On convertit la variable Rating en
numérique, pour pouvoir proposer une note minimale dans l’application.
```
df["Tags"] = df["Tags"].astype(str)
    df["Tags"] = df.apply(lambda row: row.Tags.replace("[",""), axis=1)
    df["Tags"] = df.apply(lambda row: row.Tags.replace("]",""), axis=1)
    df["Tags"] = df.apply(lambda row: row.Tags.replace("'"," "), axis=1)
    df["Tags"] = df.apply(lambda row: row.Tags.replace(","," "), axis=1)
    df.Rating = pd.to_numeric(df.Rating, errors='coerce')
    dataset = df
    selected_cuisine = cuisine_var.get()
    selected_rating = rating_var.get()
```
La ligne « filtered_df » va nous permettre de ne sortir dans l’application que les résultats
correspondant au type de cuisine sélectionné et supérieur ou égal à la note minimale sélectionnée.
```
filtered_df = dataset[(dataset["Tags"].str.contains(selected_cuisine)) & (dataset["Rating"]>int(selected_rating))]
```
On crée ensuite l’interface graphique avec tk.Tk() de la bibliothèque tkInter.
```
root = tk.Tk()
root.title("LuxResto")
```

## Principales difficultées
- Trouver le site à scraper pour récupérer les données:
  \
  Tous les sites ne permettent pas le scrapage sans difficultées. Certaines pages web ont des systèmes de sécurité en place qui 'bloquent' les utilisateur qui tentent de les scraper. Le processus pour trouver le site convenable a ainsi été fait d'essais et d'erreurs. Cependant, pour éviter des ennuis inutiles, nous avons pris des précautions pour minimiser le risque de se faire 'bloquer du site choisi.
1. Lors du lancement de l'instance du navigateur chrome avec webDriver, nous avons ajouté une option qui randomise l'user-agent utilisé pour accéder à la page 'resto.lu'. Ainsi, a chaque fois que le code est lancé, c'est un autre user-agent qui est utilisé, ce qui permet de ne pas paraitre suspect en se connectant plusieurs fois d'affilé.
2. Lors du passage automatique à la page suivante du site, nous avons ajouté une condition time.sleep afin que le programme attende quelques secondes (durée randomisée entre 7s et 16s) entre deux pages. Ceci évite de dévoiler le traitement automatisé.
3. Pour s'assurer de ne pas perdre les données récupérées avant que le programme ne récupère les données de toutes les pages, nous avons rajouté une ligne de code indiquant que les données soient stockées dans un dossier json toutes les 10 pages (json.dump).
\

- Passer à la page suivante:
  Dans le site scrapé, resto.lu, un paramètre de randomisation, 'randomseed', est présent dans l'url de chaque page, ce qui introduit un élément aléatoire dans chaque requête. Ainsi, il n'était pas possible d'indiquer au programme de tout simplement changer l'url en modifiant le numéro de la page actuelle pour accéder à la page suivante et recupérer les données souhaitées. Nous avons ainsi du ajouter une ligne de code permettant d'intéragir avec du code JavaScript, pour indiquer au programme de défiler le long de la page sur laquelle il se trouve, jusqu'à ce qu'il trouve le bouton 'next page' et d'appuyer sur ce bouton, imitant ainsi le comportemnt d'un utilisateur humain sur le site.
  ```
  driver.execute_script("return arguments[0].scrollIntoView(true);", WebDriverWait(driver, 30).until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".cwb-page-"+str(pagenumber)))))
  driver.find_element(By.CSS_SELECTOR,".cwb-page-"+str(pagenumber)).click()
  ```
## Développement

