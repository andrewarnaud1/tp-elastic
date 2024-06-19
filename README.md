# TP Elasticsearch avec Python

# Partie 1

### Partie 1 : Envoyer des demandes à Elasticsearch

1. ** Exécutez la requête API de test suivante dans la console**

Nous obtenons 3 réponses, pour chaque réponse est composée de trois parties : une ligne de statut, un corps de réponse et un code de statut HTTP.

La ligne de statut indique le type de requête effectuée (PUT, POST ou GET), l'URL de l'index Elasticsearch concerné et le code de statut HTTP renvoyé par le serveur Elasticsearch.

Le corps de réponse est un objet JSON qui contient des informations spécifiques à la requête effectuée. Les informations peuvent varier en fonction du type de requête et de l'action effectuée sur l'index Elasticsearch.

Par exemple, dans la première réponse, le corps de réponse indique que la requête PUT a été exécutée avec succès. L'objet JSON contient une clé "acknowledged" qui est définie sur "true", indiquant que la demande a été reconnue par Elasticsearch.

Dans la deuxième réponse, la requête POST a créé un nouveau document dans l'index Elasticsearch. L'objet JSON contient des informations telles que l'ID du document créé, la version du document, le résultat de l'action (dans ce cas, "created") et des informations sur les fragments (shards) utilisés pour stocker le document.

Enfin, dans la troisième réponse, la requête GET a été exécutée avec succès, mais aucun résultat n'a été trouvé pour la recherche spécifiée. L'objet JSON contient des informations sur le temps d'exécution de la requête, les fragments utilisés et les résultats de la recherche.

Ces réponses fournissent des informations sur le succès ou l'échec des requêtes effectuées, ainsi que des détails spécifiques à chaque requête.

2.  ** Ajout d'un document **

- Création d'un index
  PUT /books
- Ajout d'un document à l'index
  POST /books/_doc
  {
  "name":"Snow Crash",
  "author":"Neal Stephenson",
  "release_date":"1992-06-01",
  "page_count":470
  }

3.  ** Recherche des données **

- Recherche pour retrouver les document contenant snow
  GET /books/_search?q="snow"
- Recherche pour retrouver les document contenant snow en utilisant MATCH
  GET books/_search
  {
  "query": {
  "match": {
  "content": "snow"
  }
  }
  }
- Ajout de plusieurs documents

  ```
  POST /_bulk
  { "index" : { "_index" : "books" } }
  { "name":"Revelation Space", "author":"Alastair Reynolds", "release_date":"2000-03-15", "page_count":585 }
  { "index" : { "_index" : "books" } }
  { "name":"1984", "author":"George Orwell", "release_date":"1949-06-08", "page_count":328 }
  { "index" : { "_index" : "books" } }
  { "name":"Fahrenheit 451", "author":"Ray Bradbury", "release_date":"1953-10-15", "page_count":227 }
  { "index" : { "_index" : "books" } }
  { "name":"Brave New World", "author":"Aldous Huxley", "release_date":"1932-08-01", "page_count":268 }
  { "index" : { "_index" : "books" } }
  { "name":"The Handmaid's Tale", "author":"Margaret Atwood", "release_date":"1985-06-01", "page_count":311 }
  ```
- Manipuler les données depuis Kibana
  Documents (7) : Une liste de 7 documents de l'index books.
  Field statistics : Un onglet à côté de "Documents" pour voir les statistiques des champs.
  Table des Documents :
  Chaque ligne représente un document avec des informations sur l'auteur, le titre, le nombre de pages, la date de publication, l'ID du document, et l'index associé.
  Par exemple, le premier document est :
  Author : Alastair Reynolds
  Name : Revelation Space
  Page Count : 585
  Release Date : Mar 15, 2000
  ID : dC0nKpABWtlN-y5WAXAp
  Index : books
  Score : Non affiché (indiqué par un tiret)


# Partie 2

## Elasticsearch librairie Python

Cette commande :
```python
from elasticsearch import Elasticsearch
```

Renvoi :

```python
ModuleNotFoundError: No module named 'elasticsearch'
```

Pour résoudre cette erreur il faut utiliser cette commande :

```python
pip install elasticsearch
```

- Pour se connecter à l'endpoint :

```python
client = Elasticsearch(
  "https://704deaedc9a549a9ac4bc1be91310ab8.us-central1.gcp.cloud.es.io:443",  # Endpoint
  api_key="" # Clé API 
)
```

Voici le résultat :

```python
<Elasticsearch(['https://iut-deployment.es.us-central1.gcp.cloud.es.io:443'])>
```

- Pour tester ping et pong :

```python
is_connected = client.ping()
print(f"Connected to Elasticsearch: {is_connected}")
```

```python
info = client.info()
print(info)
```
















# TP: Maîtriser Elasticsearch avec Python

## Partie 2: Elasticsearch librairie Python
Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

### Importation de la librairie
```python
from elasticsearch import Elasticsearch
```
Initialement, cette commande peut échouer avec l'erreur suivante :
![Import Error](https://github.com/votrecompte/votreprojet/images/import_error.png)

Pour résoudre cette erreur, vous devez installer la librairie `elasticsearch` en utilisant `pip` :
```python
pip install elasticsearch
```
Voici l'installation réussie :
![Pip Install](https://github.com/votrecompte/votreprojet/images/pip_install.png)

Ensuite, la commande d'importation fonctionne correctement :
![Import Success](https://github.com/votrecompte/votreprojet/images/import_success.png)

### Création d'une clé d'API dans Elasticsearch
Pour accéder à Elasticsearch, créez une clé d'API dans l'interface Elasticsearch et sauvegardez-la, car elle disparaîtra définitivement après sa création.

### Connexion à Elasticsearch
```python
client = Elasticsearch(
  "votre_endpoint",                       # endpoint
  api_key="votre_clé_d_api"               # clé d'api
)
```
`Elasticsearch Client` est la classe représentant le client Elasticsearch, utilisée pour établir une connexion avec un cluster Elasticsearch. 

### Vérification de la connexion
Pour vérifier la connexion, vous pouvez utiliser les fonctions `ping()` ou `info()` avec la variable `client`.

#### Commande avec ping()
```python
ping_response = client.ping()
print(ping_response)
```
#### Résultat
```json
True
```

#### Commande avec info()
```python
info_response = client.info()
print(info_response)
```
#### Résultat
```json
{
  "name": "...",
  "cluster_name": "...",
  ...
}
```

### Gestion des Index depuis Python

#### Création d'un index
```python
def create_index_with_test(nom_index):
    success = False
    if not client.indices.exists(index=nom_index):
        response = client.indices.create(index=nom_index)
        success = response['acknowledged']
    return success
```

#### Suppression d'un index
```python
def delete_index_with_test(nom_index):
    success = False
    if client.indices.exists(index=nom_index):
        response = client.indices.delete(index=nom_index)
        success = response['acknowledged']
    return success
```

### Indexation des documents
Pour indexer des documents dans Elasticsearch, utilisez la fonction `index`.

#### Ajouter un document à l'index `books` avec l'id 565
```python
document = {
   "name": "The Handmaids Tale",
   "author": "Margaret Atwood",
   "release_date": "1985-06-01",
   "page_count": 311
}

response = client.index(index="books", id=565, document=document)
print(response)
```

#### Résultat
```json
{
  "_index": "books",
  "_id": "565",
  ...
}
```

### Récupération de Document

#### Récupérer le document avec l'id 565
```python
get_response = client.get(index="books", id=565)
print(get_response)
```

#### Résultat
```json
{
  "_index": "books",
  "_id": "565",
  "_source": {
    "name": "The Handmaids Tale",
    "author": "Margaret Atwood",
    "release_date": "1985-06-01",
    "page_count": 311
  }
}
```

#### Récupérer seulement le contenu du document
```python
document_content = get_response['_source']
print(document_content)
```

#### Résultat
```json
{
  "name": "The Handmaids Tale",
  "author": "Margaret Atwood",
  "release_date": "1985-06-01",
  "page_count": 311
}
```

### Opérations avec Bulk
La fonction `bulk` permet d'effectuer des opérations groupées.

#### Fonction pour ajouter des données en vrac
```python
from elasticsearch.helpers import bulk

def bulk_add_data(index_name, data):
    actions = [
        {
            "_index": index_name,
            "_id": entry['id'],
            "_source": entry
        }
        for entry in data
    ]
    success, _ = bulk(client, actions)
    return success

# Créez un index py-libraries
create_index_with_test("py-libraries")

# Données à ajouter
data = [
  {'id': 1, 'title': 'NumPy', 'description': 'NumPy is a powerful python library with many functions for creating and manipulating multi-dimensional arrays and matrices.'}, 
  {'id': 2, 'title': 'Pandas', 'description': 'Pandas is a Python library for data manipulation and analysis. It provides data structures for efficient storage of data and high-level manipulations.'}, 
  {'id': 3, 'title': 'Scikit-Learn', 'description': 'Scikit-Learn is a popular library for machine learning in Python. It provides tools to build, train, evaluate, and deploy machine learning algorithms.'}, 
  {'id': 4, 'title': 'Matplotlib', 'description': 'Matplotlib is a Python plotting library for creating publication quality plots. It can produce line graphs, histograms, power spectra, bar charts, and more.'}, 
  {'id': 5, 'title': 'Seaborn', 'description': 'Seaborn is a graphical library in Python for drawing statistical graphics. It provides a high level interface for drawing attractive statistical graphics.'}
]

bulk_add_data("py-libraries", data)
```

### Vérification de l'ajout
Pour vérifier que les données ont bien été ajoutées:

#### Recherche de la librairie pour "l'élaboration de graphiques statistiques"
```python
search_response = client.search(
    index="py-libraries",
    query={
        "match": {
            "description": "statistical graphics"
        }
    }
)
print(search_response['hits']['hits'])
```

### Agrégations
Les agrégations permettent de faire des analyses et recueillir des informations à partir des données.

#### Création d'un index `products` et ajout en vrac des documents
```python
create_index_with_test("products")

product_data = [
    {'id': i, 'product': f'example{i}', 'price': 3.2 + i * 0.75} for i in range(20)
]

bulk_add_data("products", product_data)
```

#### Requête d'agrégation pour trouver le prix moyen
```python
aggregation_query = {
    "aggs": {
        "avg_price": {
            "avg": {
               "field": "price"
            }
        }
    }
}
```
- `aggs`: indique le début de la requête d'agrégation.
- `avg_price`: le nom de l'agrégation.
- `avg`: type de l'agrégation (moyenne).
- `field`: le champ sur lequel l'agrégation est effectuée.

#### Exécution de la requête d'agrégation
```python
response = client.search(index="products", body=aggregation_query)
avg_price = response['aggregations']['avg_price']['value']
print(avg_price)
```
#### Résultat
```json
9.45
```
Le prix moyen des produits est de 9.45.




## Partie 2: Maîtriser Elasticsearch avec Python

### Elasticsearch librairie Python

Le Python Elasticsearch library est le client Python officiel d'Elasticsearch. Il fournit une interface de haut niveau et de bas niveau pour interagir avec Elasticsearch.

1. **Import de la librairie**

    Tout d'abord, nous devons installer la librairie `elasticsearch` en utilisant pip. Dans un premier temps une erreur peut-être retournée, elle est due au fait que le module `elasticsearch` n'est pas installé.

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

### Indexation des documents

7. **Ajouter un document à `books` avec l'ID 565**

    Pour indexer un document dans Elasticsearch, nous utilisons la fonction `index()`. Voici une fonction pour indexer un document avec gestion des exceptions :

    ```python
    def index_document(index_name, document_id, document):
        try:
            response = client.index(
                index=index_name,
                id=document_id,
                document=document
            )
            print(f"Document indexed successfully: {response['result']}")
            return response
        except Exception as e:
            print(f"Error indexing document: {e}")
    ```
    ![Fonction indexation document](https://github.com/andrewarnaud1/tp-elastic/blob/main/12_indexation_document.png?raw=true)

    - **Exemple de document à ajouter**

      Voici le document que nous allons ajouter à l'index `books` avec l'ID 565.

      ```python
      document = {
          "name": "The Handmaid's Tale",
          "author": "Margaret Atwood",
          "release_date": "1985-06-01",
          "page_count": 311
      }
      ```
      ![Document à indexer](https://github.com/andrewarnaud1/tp-elastic/blob/main/13_books.png?raw=true)

    - **Indexer le document**

      Utilisez la fonction définie précédemment pour indexer le document.

      ```python
      # Indexer le document
      response = index_document("books", 565, document)
      ```
      ![Ajout books](https://github.com/andrewarnaud1/tp-elastic/blob/main/14_ajout_books.png?raw=true)

      Résultat :

      ```plaintext
      Document indexed successfully: created
      ```
      ![Résultat indexation](https://github.com/andrewarnaud1/tp-elastic/blob/main/12_indexation_document.png?raw=true)
