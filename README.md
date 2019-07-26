# tutoScraping
Comment scraper un site web avec python 3

# I- Utilitie

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


