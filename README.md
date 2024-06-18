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

4. **Vérification de la connexion**

   Pour vérifier la connexion, nous pouvons utiliser les fonctions `ping()` et `info()` fournies par le client Elasticsearch.

    - **Commande et Résultat avec `ping()`**

      La fonction `ping()` permet de vérifier si le cluster Elasticsearch est accessible. Si la connexion est réussie, elle retourne `True`.

      ```python
      is_connected = client.ping()
      print(f"Connected to Elasticsearch: {is_connected}")
      ```
      ![Résultat ping](https://github.com/andrewarnaud1/tp-elastic/blob/main/6_resultat_ping.png?raw=true)

    - **Commande et Résultat avec `info()`**

      La fonction `info()` fournit des informations détaillées sur le cluster Elasticsearch, telles que le nom du cluster, la version, etc.

      ```python
      info = client.info()
      print(info)
      ```
      ![Résultat info](https://github.com/andrewarnaud1/tp-elastic/blob/main/7_resultat_info.png?raw=true)

   Avant d'exécuter ces commandes, assurez-vous que le client Elasticsearch est correctement configuré avec votre endpoint et clé d'API.

    ```python
    client = Elasticsearch(
        "https://iut-deployment.es.us-central1.gcp.cloud.es.io",
        api_key="enLHN...bQQ=="
    )
    ```
    ![Client Elasticsearch](https://github.com/andrewarnaud1/tp-elastic/blob/main/4_api_endpoint_and_key.png?raw=true)
