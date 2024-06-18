# TP Elasticsearch avec Python

## Partie 2: Maîtriser Elasticsearch avec Python

### Elasticsearch librairie Python

Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

1. **Installation et importation de la librairie Elasticsearch**

    - **Installation de la librairie**

      Pour installer la librairie Elasticsearch, exécutez la commande suivante :

      ```python
      pip install elasticsearch
      ```
      ![Installation Elasticsearch](https://github.com/andrewarnaud1/tp-elastic/blob/main/2_pip_install.png?raw=true)

      Cette commande installe la librairie `elasticsearch` ainsi que ses dépendances. 

    - **Importation de la librairie**

      Après l'installation, importez la classe `Elasticsearch` depuis la librairie :

      ```python
      from elasticsearch import Elasticsearch
      ```
      ![Erreur d'importation](https://github.com/andrewarnaud1/tp-elastic/blob/main/1_erreur_module.png?raw=true)

      La capture d'écran montre une erreur `ModuleNotFoundError`, indiquant que le module `elasticsearch` n'a pas été trouvé. Cela peut être dû à un problème d'installation ou à un environnement Python incorrect.

      Une fois l'erreur résolue, l'importation devrait réussir :

      ![Import réussi](https://github.com/andrewarnaud1/tp-elastic/blob/main/3_import_elastic.png?raw=true)

      La capture d'écran ci-dessus montre que l'importation de la classe `Elasticsearch` a été effectuée avec succès.
