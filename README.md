Lien du 
# TP - Elasticsearch

Elasticsearch est un moteur de recherche et d'analyse distribué conçu pour traiter des données à grande échelle. Il peut évoluer horizontalement en ajoutant des nœuds au cluster, ce qui lui permet de traiter de gros volumes de données et de fournir des réponses de recherche rapides. Il stocke et indexe les données sous forme de documents JSON.

## Objectif

1. Exécutez Elasticsearch et Kibana (en utilisant Elastic Cloud).
2. Ajoutez un jeu de données à Elasticsearch.
3. Exécuter des recherches basiques.
4. Interagir avec l'écosystème avec plusieurs interfaces.

## Partie 1: Envoyer des demandes à Elasticsearch

### 1. Création de compte, configuration et déploiement de Elasticsearch

- Cliquez sur "Obtenez un essai gratuit".
- Connectez-vous à Elastic Cloud.
- Cliquez sur "Créer un déploiement".
- Donnez un nom à votre déploiement.
- Cliquez sur "Créer un déploiement" et téléchargez le mot de passe pour l'utilisateur elastic.
- Cliquez sur "Continuer" pour ouvrir Kibana, l'interface utilisateur d'Elastic Cloud.
- Cliquez sur "Explorer par moi-même".

### 2. Envoyer des requêtes à Elasticsearch

#### Utilisation de Kibana

1. Ouvrir le menu principal de Kibana (“☰” près du logo Elastic) et allez à Dev Tools > Console.
2. Exécutez la requête API de test suivante dans la console:

```html
GET /_cluster/health
```

**Réponse obtenue :**

- La ligne de statut indique le type de requête (GET), l'URL de l'index et le code de statut HTTP.
- Le corps de réponse est un objet JSON qui contient des informations spécifiques à la requête effectuée.
- Par exemple, la requête GET retourne des informations sur l'état du cluster.

### 3. Ajouter des données à Elasticsearch

#### Ajouter un seul document

1. Créez un index `books`:

```html
PUT /books
```

2. Ajoutez un document à l'index `books`:

```html
POST /books/_doc
{
  "name":"Snow Crash",
  "author":"Neal Stephenson",
  "release_date":"1992-06-01",
  "page_count":470
}
```

### 4. Recherche des données

#### Recherche de tous les documents

1. Exécutez la commande suivante pour rechercher l'index `books` pour tous les documents:

```html
GET /books/_search
```

2. Faites une recherche pour retrouver le document contenant "snow":

```html
GET /books/_search?q="snow"
```

3. Faites une recherche avec une requête `match`:

```html
GET books/_search
{
  "query": {
    "match": {
      "name": "snow"
    }
  }
}
```

### 5. Ajout de plusieurs documents

1. Utilisez le `_bulk` endpoint pour ajouter plusieurs documents dans une seule requête à l'index `books`:

```html
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

### 6. Manipuler les données depuis Kibana

- Rendez-vous dans l'onglet "Discover" d'Elasticsearch Kibana.
- Cliquez sur "Discover".
- Cliquez sur "Create data view".
- Ajoutez un nom à la vue et choisissez l'index `books`.
- Vous verrez une liste de documents indexés et des statistiques sur les champs.

## Partie 2: Maîtriser Elasticsearch avec Python

### 1. Importation de la librairie Elasticsearch

Lors de la tentative d'importation de la librairie `elasticsearch`, une erreur s'est produite car le module n'était pas installé.

![Erreur module non trouvé](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/1_erreur_module.png)

Pour résoudre ce problème, nous avons installé la librairie en utilisant la commande `pip install elasticsearch`.

![Installation du module](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/2_pip_install.png)

Après l'installation, l'importation du module a réussi.

![Importation réussie](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/3_import_elastic.png)

### 2. Connexion à Elasticsearch depuis Databricks

#### Création de la clé d'API dans Elasticsearch

Pour accéder à l'API depuis Databricks, il faut créer une clé d'API dans Elasticsearch et la sauvegarder car elle disparaîtra définitivement.

#### Connexion à Elasticsearch

Nous avons configuré le client Elasticsearch avec l'endpoint et la clé d'API.

```python
client = Elasticsearch(
    "https://iut-deployment.es.us-central1.gcp.cloud.es.io",
    api_key="enLHNEs1Q...7BqQQ=="
)
```

![Configuration du client](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/4_api_endpoint_and_key.png)

#### Vérification de la connexion

Pour vérifier la connexion, nous avons utilisé la fonction `ping()`.

```python
is_connected = client.ping()
print(f"Connected to Elasticsearch: {is_connected}")
```

**Résultat :**

```plaintext
Connected to Elasticsearch: True
```

![Résultat de ping](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/5_test_api.png)

#### Vérification des informations du cluster

Pour obtenir des informations détaillées sur le cluster Elasticsearch, nous avons utilisé la fonction `info()`.

```python
info = client.info()
print(info)
```

**Résultat :**

```python
{
    'name': 'instance-000000001', 
    'cluster_name': '704deaedc9a549a9ac4bc1be91310ab8', 
    'cluster_uuid': 'WQ_hUhpWS0anpTZxgRCucA', 
    'version': {
        'number': '8.14.1', 
        'build_flavor': 'default', 
        'build_type': 'docker', 
        'build_hash': '93a57a1a76f56d8aee6a90d1a95b06187501310', 
        'build_date': '2024-06-10T23:35:17.114581191Z', 
        'build_snapshot': False, 
        'lucene_version': '9.10.0', 
        'minimum_wire_compatibility_version': '7.17.0', 
        'minimum_index_compatibility_version': '7.0.0'
    }, 
    'tagline': 'You Know, for Search'
}
```

![Résultat de info](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/7_resultat_info.png)

### 3. Création et Suppression d'Index dans Elasticsearch

#### Fonction de création d'index avec test d'existence

Il est souvent conseillé, avant d'ajouter un index, de faire un test d'existence de ce dernier. Voici une fonction qui crée un index après avoir vérifié s'il n'existe pas déjà.

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

#### Exemple de test de création d'index

```python
# Exemple de test
create_index_with_test("index_test")
```

**Résultat :**

```plaintext
Index 'index_test' créé avec succès.
Out[17]: True
```

![Création de l'index](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/8_fonction_creation_index.png)

![Résultat de la création de l'index](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/9_creation_index_test.png)

#### Fonction de suppression d'index avec test d'existence

De même, il est utile de vérifier l'existence d'un index avant de le supprimer.

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

#### Exemple d'utilisation de la suppression d'index

```python
# Exemple d'utilisation
delete_index_with_test("index_test")
```

**Résultat :**

```plaintext
Index 'index_test' supprimé avec succès.
Out[19]: True
```

![Fonction de suppression d'index](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/10_fonction_suppression_index.png)

![Résultat de la suppression d'index](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/11_suppression_index_test.png)

### 4. Indexation des documents dans Elasticsearch

Pour indexer des documents dans Elasticsearch, nous utilisons la fonction `index()`. Voici une fonction pour indexer un document.

#### Fonction d'indexation de document

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

![Fonction d'indexation de document](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/12_indexation_document.png)

#### Création du document à indexer

Voici le document que nous allons indexer dans l'index `books` avec l'ID `565`.

```python
document = {
    "name": "The Handmaid's Tale",
    "author": "Margaret Atwood",
    "release_date": "1985-06-01",
    "page_count": 311
}
```

![Création du document](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/13_books.png)

#### Indexation du document

Nous indexons le document dans l'index `books` avec l'ID `565`.

```python
# Indexer le document
response = index_document("books", 565, document)
```

**Résultat :**

```plaintext
Document indexed successfully: created
```

![Résultat de l'indexation](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/14_ajout_books.png)

### 5. Récupération de Document depuis Elasticsearch

Pour récupérer des documents à partir d'Elasticsearch, nous pouvons utiliser diverses méthodes comme `get()`, `search()`, etc. Voici une fonction pour récupérer un document par son ID.

#### Fonction de récupération de document

```python
def get_document(index_name, document_id):
    try:
        response = client.get(
            index=index_name,
            id=document_id
        )
        return response
    except Exception as e:
        print(f"Error retrieving document: {e}")
```

![Fonction de récupération de document](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/15_fontion_recuperation_document.png)

#### Récupérer le document avec l'ID `565`

Nous allons maintenant récupérer le document que nous avons indexé précédemment dans l'index `books`.

```python
# Récupérer le document
response = get_document("books", 565)
print(response)
```

**Résultat :**

```python
{
    '_index': 'books',
    '_id': '565',
    '_version': 1,
    '_seq_no': 7,
    '_primary_term': 1,
    'found': True,
    '_source': {
        'name': "The Handmaid's Tale",
        'author': 'Margaret Atwood',
        'release_date': '1985-06-01',
        'page_count': 311
    }
}
```

![Résultat de la récupération](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/16_recuperation_document_books.png)

### 6. Recherche dans Elasticsearch

Pour effectuer une recherche dans un index Elasticsearch, nous pouvons utiliser la méthode `search()`. Voici une fonction pour rechercher des documents dans un index basé sur une description.

#### Fonction de recherche de bibliothèque avec description

```python
def search_library_with_description(index_name, query_text):
    try:
        response = client.search(
            index=index_name,
            body={
                "query": {
                    "bool": {
                        "must": [
                            {"match": {"description": "graphical"}},
                            {"match": {"description": "library"}},
                            {"match": {"description": "drawing"}},
                            {"match": {"description": "statistical"}},
                            {"match": {"description": "graphics"}}
                        ]
                    }
                }
            }
        )
        return response['hits']['hits']
    except Exception as e:
        print(f"Erreur lors de la recherche: {e}")
        return []
```

![Fonction de recherche de bibliothèque](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/21_fonction_recherche_library.png)

#### Recherche de la bibliothèque avec la description "graphical library for drawing statistical graphics"

Nous allons maintenant rechercher une bibliothèque dans l'index `py-libraries` avec la description spécifiée.

```python
# Rechercher la librairie avec la description
resultats = search_library_with_description("py-libraries", "graphical library for drawing statistical graphics")

# Afficher les résultats
for resultat in resultats:
    print(f"Voici les résultats {resultat['_source']}")
```

**Résultat :**

```plaintext
Voici les résultats {'title': 'Seaborn', 'description': 'Seaborn is a graphical library in Python for drawing statistical graphics. It provides a high level interface for drawing attractive statistical graphics.'}
```

![Résultat de la recherche](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/22_resultat_library.png)

### 7. Création de l'index `products`

Pour effectuer des opérations sur les documents de produits, nous devons d'abord créer un index nommé `products`.

#### Création de l'index `products`

Nous utilisons la fonction `create_index_with_test` pour créer l'index après avoir vérifié s'il n'existe pas déjà.

```python
# Création de l'index 'products'
create_index_with_test("products")
```

**Résultat :**

```plaintext
Index 'products' créé avec succès.
Out[36]: True
```

![Création de l'index products](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/23_creation_index_product.png)

### 8. Ajout de données en vrac à l'index `products`

Nous allons ajouter plusieurs documents à l'index `products` en utilisant la méthode `bulk()` pour améliorer les performances d'indexation.

#### Données à ajouter

```python
# Données à ajouter
data = [
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
    {'product': 'example17', 'price': 15.95},
    {'product': 'example18', 'price': 16.7},
    {'product': 'example19', 'price': 17.45}
]
```

![Données à ajouter](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/24_data_product.png)

#### Ajout des données en vrac

Nous utilisons la méthode `bulk()` pour ajouter les documents en vrac à l'index `products`.

```python
from elasticsearch.helpers import bulk

# Fonction pour ajouter des données en vrac
def bulk_add_data(index_name, data):
    actions = [
        {
            "_index": index_name,
            "_source": d
        }
        for d in data
    ]
    success, _ = bulk(client, actions)
    print(f"Ajouté {len(actions)} documents à l'index '{index_name}'.")
    # Ajout des données en vrac à l'index 'products'
    bulk_add_data("products", data)
```

**Résultat :**

```plaintext
Ajouté 20 documents à l'index 'products'.
```

![Ajout des données en vrac](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/24_data_product.png)

### 9. Agrégations pour calculer le prix moyen

ElasticSearch prend en charge les agrégations pour effectuer des analyses et recueillir des informations à partir des données. Nous allons créer une requête d'agrégation pour calculer le prix moyen de nos produits.

#### Requête d'agrégation

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

**Commentaire :**

- La clé `aggs` est utilisée pour définir les agrégations.
- `avg_price` est le nom de l'agrégation que nous définissons.
- `avg` indique que nous voulons calculer la moyenne.
- `field` spécifie le champ sur lequel nous voulons effectuer l'agrégation, ici le champ `price`.

![Requête d'agrégation](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/26_requete_aggregation.png)

#### Fonction pour obtenir le prix moyen

```python
def get_average_price(index_name):
    aggregation_query = {
        "aggs": {
            "avg_price": {
                "avg": {
                    "field": "price"
                }
            }
        }
    }
    try:
        response = client.search(
            index=index_name,
            body=aggregation_query
        )
        avg_price = response['aggregations']['avg_price']['value']
        print(f"Le prix moyen des produits est: {round(avg_price, 2)}")
        return avg_price
    except Exception as e:
        print(f"Erreur lors de l'exécution de la requête d'agrégation: {e}")
        return None
```

![Fonction pour obtenir le prix moyen](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/27_fonction_aggregation_product.png)

#### Exécution de la requête d'agrégation

```python
# Exécution de la requête d'agrégation pour l'index 'products'
get_average_price("products")
```

**Résultat :**

```plaintext
Le prix moyen des produits est: 10.32€
Out[54]: 10.324999928474426
```

![Résultat de l'agrégation](https://raw.githubusercontent.com/andrewarnaud1/tp-elastic/main/28_moyenne_prix.png)

Nous avons calculé avec succès le prix moyen des produits dans l'index `products` en utilisant une requête d'agrégation.
