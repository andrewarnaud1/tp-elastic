# TP: Maîtriser Elasticsearch avec Python

## Partie 2: Elasticsearch librairie Python
Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

### Importation de la librairie
```python
from elasticsearch import Elasticsearch
```
Initialement, cette commande peut échouer avec l'erreur suivante :
![Import Error](https://github.com/andrewarnaud1/tp-elastic/blob/main/1_erreur_module.png?raw=true)

Pour résoudre cette erreur, il faut installer la librairie `elasticsearch` en utilisant `pip` :
```python
pip install elasticsearch
```
Voici l'installation réussie :
![Pip Install](https://github.com/andrewarnaud1/tp-elastic/blob/main/2_pip_install.png?raw=true)

Ensuite, la commande d'importation fonctionne correctement :
![Import Success](https://github.com/andrewarnaud1/tp-elastic/blob/main/3_import_elastic.png?raw=true)
