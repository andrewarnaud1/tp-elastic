# TP Elasticsearch avec Python

## Partie 2: Maîtriser Elasticsearch avec Python

### Elasticsearch librairie Python

Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

1. **Import de la librairie**

    ```python
    from elasticsearch import Elasticsearch
    ```
    ![Import Error](https://github.com/andrewarnaud1/tp-elastic/blob/main/1_erreur_module.png?raw=true)

2. **Création d'une clé API dans Elasticsearch**

   - Accédez à l'interface Elasticsearch et créez une clé d'API.
   - Sauvegardez cette clé car elle ne sera plus visible après sa création.
   ![API Endpoint and Key](https://github.com/andrewarnaud1/tp-elastic/blob/main/4_api_endpoint_and_key.png?raw=true)

3. **Connexion au client Elasticsearch depuis Databricks**

    ```python
    client = Elasticsearch(
      "endpoint",                       # endpoint
      api_key="clé_d_api"                # clé d'api
    )
    ```
    Elasticsearch Client est la classe qui représente le client Elasticsearch. Elle est utilisée pour établir une connexion avec un cluster Elasticsearch. Vous devez d'abord avoir votre propre cluster, que vous pouvez créer gratuitement sur [Elasticsearch essai](https://www.elastic.co/cloud/elasticsearch-service/signup).

4. **Vérification de la connexion**

    - **Commande et Résultat avec `ping()`**

      ```python
      response = client.ping()
      print(response)
      ```
      ![Résultat ping](https://github.com/andrewarnaud1/tp-elastic/blob/main/6_resultat_ping.png?raw=true)

    - **Commande et Résultat avec `info()`**

      ```python
      response = client.info()
      print(response)
      ```
      ![Résultat info](https://github.com/andrewarnaud1/tp-elastic/blob/main/7_resultat_info.png?raw=true)

### Gestion des Index depuis Python

5. **Création d'un index**

    Avant d'ajouter un index, il est souvent conseillé de vérifier l'existence de cet index.

    ```python
    def create_index_with_test(nom_index):
        success = False
        if not client.indices.exists(index=nom_index):
            response = client.indices.create(index=nom_index)
            success = response['acknowledged']
        return success
    ```
    ![Fonction création index](https://github.com/andrewarnaud1/tp-elastic/blob/main/8_fonction_creation_index.png?raw=true)
    ![Création index test](https://github.com/andrewarnaud1/tp-elastic/blob/main/9_creation_index_test.png?raw=true)

6. **Suppression d'un index**

    ```python
    def delete_index_with_test(nom_index):
        success = False
        if client.indices.exists(index=nom_index):
            response = client.indices.delete(index=nom_index)
            success = response['acknowledged']
        return success
    ```
    ![Fonction suppression index](https://github.com/andrewarnaud1/tp-elastic/blob/main/10_fonction_suppression_index.png?raw=true)
    ![Suppression index test](https://github.com/andrewarnaud1/tp-elastic/blob/main/11_suppression_index_test.png?raw=true)

### Indexation des documents

7. **Ajouter un document à `books` avec l'ID 565**

    ```python
    document = {
       "name":"The Handmaid's Tale",
       "author":"Margaret Atwood",
       "release_date":"1985-06-01",
       "page_count":311
    }

    response = client.index(index='books', id=565, document=document)
    print(response)
    ```
    ![Indexation document](https://github.com/andrewarnaud1/tp-elastic/blob/main/12_indexation_document.png?raw=true)
    ![Books](https://github.com/andrewarnaud1/tp-elastic/blob/main/13_books.png?raw=true)
    ![Ajout books](https://github.com/andrewarnaud1/tp-elastic/blob/main/14_ajout_books.png?raw=true)

### Récupération de Document

8. **Récupérer le document avec l'ID 565**

    ```python
    response = client.get(index='books', id=565)
    print(response)
    ```
    ![Récupération document](https://github.com/andrewarnaud1/tp-elastic/blob/main/15_fontion_recuperation_document.png?raw=true)
    ![Document books](https://github.com/andrewarnaud1/tp-elastic/blob/main/16_recuperation_document_books.png?raw=true)

9. **Récupérer seulement le contenu du document**

    ```python
    document_content = response['_source']
    print(document_content)
    ```

### Opérations avec Bulk

10. **Fonction d'ajout en vrac**

    ```python
    from elasticsearch.helpers import bulk

    def create_index_with_test(nom_index):
        if not client.indices.exists(index=nom_index):
            client.indices.create(index=nom_index)

    def bulk_index_data(index_name, data):
        actions = [
            {
                "_op_type": "index",
                "_index": index_name,
                "_id": entry['id'],
                "_source": entry
            }
            for entry in data
        ]
        response = bulk(client, actions)
        return response

    # Scénario
    create_index_with_test('py-libraries')

    data = [
      {'id': 1, 'title': 'NumPy', 'description': 'NumPy is a powerful python library with many functions for creating and manipulating multi-dimensional arrays and matrices.'}, 
      {'id': 2, 'title': 'Pandas', 'description': 'Pandas is a Python library for data manipulation and analysis. It provides data structures for efficient storage of data and high-level manipulations.'}, 
      {'id': 3, 'title': 'Scikit-Learn', 'description': 'Scikit-Learn is a popular library for machine learning in Python. It provides tools to build, train, evaluate, and deploy machine learning algorithms.'}, 
      {'id': 4, 'title': 'Matplotlib', 'description': 'Matplotlib is a Python plotting library for creating publication quality plots. It can produce line graphs, histograms, power spectra, bar charts, and more.'}, 
      {'id': 5, 'title': 'Seaborn', 'description': 'Seaborn is a graphical library in Python for drawing statistical graphics. It provides a high level interface for drawing attractive statistical graphics.'}
    ]

    response = bulk_index_data('py-libraries', data)
    print(response)
    ```
    ![Fonction ajout bulk](https://github.com/andrewarnaud1/tp-elastic/blob/main/17_fonction_ajout_bulk.png?raw=true)
    ![Création index libraries](https://github.com/andrewarnaud1/tp-elastic/blob/main/18_creation_index_libraries.png?raw=true)
    ![Data](https://github.com/andrewarnaud1/tp-elastic/blob/main/19_data.png?raw=true)
    ![Ajout data](https://github.com/andrewarnaud1/tp-elastic/blob/main/20_ajout_data.png?raw=true)

11. **Vérification de l'ajout**

    ```python
    response = client.search(
        index='py-libraries',
        body={
            "query": {
                "match": {
                    "description": "statistical graphics"
                }
            }
        }
    )
    print(response)
    ```
    ![Fonction recherche library](https://github.com/andrewarnaud1/tp-elastic/blob/main/21_fonction_recherche_library.png?raw=true)
    ![Résultat library](https://github.com/andrewarnaud1/tp-elastic/blob/main/22_resultat_library.png?raw=true)

### Agrégations

12. **Création d'un index `products` et ajout en vrac des documents**

    ```python
    products = [
        {'product': 'example0', 'price': 3.2},
        {'product': 'example1', 'price': 3.95},
        {'product': 'example2', 'price': 4.7},
        {'product': 'example3', 'price': 5.45},
        {'product': 'example4', 'price': 6.2},
        {'product': 'example5', 'price': 6.95},
        {'product': 'example6', 'price': 7.7},
        {'product': 'example7', 'price': 8.45},
        {'product': 'example8', 'price': 9.2},
        {'product': 'example9', 'price': 9.95},
        {'product': 'example10', 'price': 10.7},
        {'product': 'example11', 'price': 11.45},
        {'product': 'example12', 'price': 12.2},
        {'product': 'example13', 'price': 12.95},
        {'product': 'example14', 'price': 13.7},
        {'product': 'example15', 'price': 14.45},
        {'product': 'example16', 'price': 15.2},
        {‘product’: ‘example17’, ‘price’: 15.95},
        {‘product’: ‘example18’, ‘price’: 16.7},
        {‘product’: ‘example19’, ‘price’: 17.45}
    ]
    
    create_index_with_test('products')
    response = bulk_index_data('products', products)
    print(response)
    ```
    
![Création index product](https://github.com/andrewarnaud1/tp-elastic/blob/main/23_creation_index_product.png?raw=true)
![Data product](https://github.com/andrewarnaud1/tp-elastic/blob/main/24_data_product.png?raw=true)
![Ajout products](https://github.com/andrewarnaud1/tp-elastic/blob/main/25_ajout_products.png?raw=true)

12. **Requête d’agrégation pour trouver le prix moyen des produits**

aggregation_query = {
    "aggs": {                       # Définir les agrégations
        "avg_price": {              # Nom de l'agrégation
            "avg": {                # Type de l'agrégation
                "field": "price"     # Champ sur lequel l'agrégation est appliquée
            }
        }
    }
}

response = client.search(index='products', body=aggregation_query)
avg_price = response['aggregations']['avg_price']['value']
print(avg_price)
