# TP: Maîtriser Elasticsearch avec Python

## Partie 2: Elasticsearch librairie Python
Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

### Importation de la librairie
```python
from elasticsearch import Elasticsearch
```
Initialement, cette commande peut échouer avec l'erreur suivante :
![Import Error](https://github.com/votrecompte/votreprojet/images/import_error.png)

Pour résoudre cette erreur, il faut installer la librairie `elasticsearch` en utilisant `pip` :
```python
pip install elasticsearch
```
Voici l'installation réussie :
![Pip Install](https://github.com/votrecompte/votreprojet/images/pip_install.png)

Ensuite, la commande d'importation fonctionne correctement :
![Import Success](https://github.com/votrecompte/votreprojet/images/import_success.png)
