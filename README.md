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


### Gestion des Index depuis Python

5. **Création d'un index**

    Avant d'ajouter un index, il est souvent conseillé de vérifier l'existence de cet index. La fonction suivante permet de créer un index avec un test préalable pour vérifier s'il existe déjà.

    ```python
    def create_index_with_test(nom_index):
        success = False
        try:
            # Vérifier si l'index existe
            if not client.indices.exists(index=nom_index):
                # Créer l'index
                client.indices.create(index=nom_index)
                success = True
                print(f"Index '{nom_index}' créé avec succès.")
            else:
                print(f"L'index '{nom_index}' existe déjà.")
        except Exception as e:
            print(f"Erreur lors de la création de l'index '{nom_index}': {e}")
        return success
    ```
    ![Fonction création index](https://github.com/andrewarnaud1/tp-elastic/blob/main/8_fonction_creation_index.png?raw=true)

    - **Exemple de test**

      ```python
      # Exemple de test
      create_index_with_test("index_test")
      ```
      ![Création index test](https://github.com/andrewarnaud1/tp-elastic/blob/main/9_creation_index_test.png?raw=true)

6. **Suppression d'un index**

    La fonction suivante permet de supprimer un index avec un test préalable pour vérifier s'il existe.

    ```python
    def delete_index_with_test(nom_index):
        success = False
        try:
            # Vérifier si l'index existe
            if client.indices.exists(index=nom_index):
                # Supprimer l'index
                client.indices.delete(index=nom_index)
                success = True
                print(f"Index '{nom_index}' supprimé avec succès.")
            else:
                print(f"L'index '{nom_index}' n'existe pas.")
        except Exception as e:
            print(f"Erreur lors de la suppression de l'index '{nom_index}': {e}")
        return success
    ```
    ![Fonction suppression index](https://github.com/andrewarnaud1/tp-elastic/blob/main/10_fonction_suppression_index.png?raw=true)

    - **Exemple d'utilisation**

      ```python
      # Exemple d'utilisation
      delete_index_with_test("index_test")
      ```
      ![Suppression index test](https://github.com/andrewarnaud1/tp-elastic/blob/main/11_suppression_index_test.png?raw=true)
