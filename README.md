# tutoScraping :alien:
Comment scraper un site web avec python 3

### c'est quoi le scrapping ?

Scraper consiste à récuperer les données d'un site internet sans l'autorisation du propriétaire.

### A qu'elle moment il faut scraper un site?

Lorsque nous avons besoin des données d'un site et nous n'avons pas accès à l4API du site alors on fait recoure au scrapping :sunglasses:




** En cas de soucis Merci de créer des issues et je vais complèter le tutorial **

# I- Utilitie

###Vérifier que python est installé 
:smile_cat:

```bash
$ python
```
##Setup 2: créer un environnement virtuel
```bash
$ python -m venv venv
```
##Setup 3: activer l'environnement virtuel
```bash
$ source venv/bin/activate
```

Pour le scraping vous avez plusieur modules. 
Ce tutorial visant la convergence en Django, utilisera des librairie simple pour faciliter le deploiement et le scraping instatané.

### Installation

```bash
	
$ pip install bs4

# bs4 : joue le role de parseur HTML

$ pip install requests

# requests : comme le nom l'indique va nous aider à faire de appels HTTP ( GET, POST)

```

### Premier pas

créer un fichier *scraper.py*

```bash

# importation

import requests
from bs4 import BeautifulSoup

# Utilisation

url =  #URL à scraper

# recuperer la page à scraper

response = requests.get(url)


# Parser le resultat en HTML pour l'exploitation

html_soup = BeautifulSoup(response.text, 'html.parser')


# Verifier que le scrapping à marché

print(response.status_code)
200 # tout va bien


```

## Les bonnes habitudes avec python3

Toujour utiliser *try* et *except* pour être rasuré que le visiteur ne verra pas d'erreur sur sa page et l'admin sera informé de l'erreur en cours afin de le resoudre.

```bash

try:

  #Code si valide ici
  
except:

  # code sinon ici, signaler l'admin

```

#### Application

```bash

import requests
from bs4 import BeautifulSoup
try:

  url = 'https://zestedesavoir.com/bibliotheque/'
  response = requests.get(url)
  
  html_soup = BeautifulSoup(response.text, 'html.parser')

  print(response.status_code)
  
except:

  # code sinon ici, signaler l'admin
  print("Une erreur s'est produit lors de l'execution de code, si le problème persite signaler l'administrateur")

```

# Ok. bs4 on jette un coup d'oeil

beautifulsoup4 va nous permete d'exploiter la page html scraper

[documentation](https://python-django.dev/page-beautifulsoup-html-parser-python-library-xml)

# Passons au chose serieuse
On va scraper le site de sport **match en direct** :scream_cat:


```bash

from requests import get
from bs4 import BeautifulSoup

url = 'https://www.matchendirect.fr/'
response = get(url)
html_soup = BeautifulSoup(response.text, 'html.parser')

table = html_soup.find('div', attrs = {'id':'livescore'}) 
compt = 1
for row in table.findAll('div', attrs = {'class':'panel panel-info'}): 
 

    a_desc = row.find('h3', attrs = {'class':'panel-title'}).get_text() 

    for el in row.findAll('tr'):
        heure = el.find('td', attrs = {'class':'lm1'}).get_text() 
        eqA = el.find('span', attrs = {'class':'lm3_eq1'}).get_text()
        eqB =  el.find('span', attrs = {'class':'lm3_eq2'}).get_text()

        scors =  el.find('span', attrs = {'class':'lm3_score'}).get_text()
        print(heure, eqA, "-" ,scors ,"-", eqB)


    compt += 1

print(response.status_code)


```


# ET SI ON PASSAIT A DJANGO 

dans mon fichier views.py j'ajoute le code suisvant:



```bash

from django.http import JsonResponse
import json

from requests import get
from bs4 import BeautifulSoup



def scrap(request):

	url = 'https://www.matchendirect.fr/'
	response = get(url)
	html_soup = BeautifulSoup(response.text, 'html.parser')

	table = html_soup.find('div', attrs = {'id':'livescore'}) 
	compt = 1

	mydata = []


	for row in table.findAll('div', attrs = {'class':'panel panel-info'}): 


	    a_desc = row.find('h3', attrs = {'class':'panel-title'}).get_text() 

	    for el in row.findAll('tr'):

		resultat = {}
		heure = el.find('td', attrs = {'class':'lm1'}).get_text() 
		eqA = el.find('span', attrs = {'class':'lm3_eq1'}).get_text()
		eqB =  el.find('span', attrs = {'class':'lm3_eq2'}).get_text()

		scors =  el.find('span', attrs = {'class':'lm3_score'}).get_text()

		resultat['heure'] = heure
		resultat['eqa'] = eqA
		resultat['eqb'] = eqB
		resultat['scors'] = scors

		mydata.append(resultat)

	data = mydata
	
	return JsonResponse(data, safe=False) # retourn du json
	

```



