# TP: Maîtriser Elasticsearch avec Python

## Importation de la librairie
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

### Vérification de la connexion
Pour vérifier la connexion, vous pouvez utiliser les fonctions `ping()` ou `info()` avec la variable `client`.

Test de la commande ping :
![Test ping](https://github.com/andrewarnaud1/tp-elastic/blob/main/5_test_api.png?raw=true)

La commande renvoie True ce qui signifie que la connexion à l'API fonctionne.

Test de la commande infos :
![Test infos](https://github.com/andrewarnaud1/tp-elastic/blob/main/7_resultat_info.png?raw=true)


