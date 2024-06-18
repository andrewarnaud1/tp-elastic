# TP Elasticsearch avec Python

## Partie 2: Maîtriser Elasticsearch avec Python

### Elasticsearch librairie Python

Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

1. **Import de la librairie**

    Tout d'abord, nous devons installer la librairie `elasticsearch` en utilisant pip. L'erreur initiale était due au fait que le module `elasticsearch` n'était pas installé.

    ```python
    pip install elasticsearch
    ```
    ![Installation Elasticsearch](https://github.com/andrewarnaud1/tp-elastic/blob/main/2_pip_install.png?raw=true)

    Après l'installation, nous pouvons importer le module. La capture d'écran suivante montre l'erreur rencontrée avant l'installation du module.

    ```python
    from elasticsearch import Elasticsearch
    ```
    ![Erreur d'importation](https://github.com/andrewarnaud1/tp-elastic/blob/main/1_erreur_module.png?raw=true)

    Une fois le module installé avec succès, l'importation fonctionne correctement.

    ![Import réussi](https://github.com/andrewarnaud1/tp-elastic/blob/main/3_import_elastic.png?raw=true)
