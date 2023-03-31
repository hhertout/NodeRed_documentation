# Documentation Node-Red (FR)

## Créer un environnement de développement Node-Red avec docker
<code>docker run -it -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red</code>

Une fois le container lancer. Allez sur <a href="http://localhost:1880/">http://localhost:1880/</a>

## Variables d'environnement

Pour définir une variable d'environnement dans Node-RED, vous pouvez suivre les étapes suivantes :

Ouvrez le fichier de configuration de votre projet Node-RED. Ce fichier est généralement situé dans le répertoire principal de votre projet et s'appelle settings.js.

Recherchez la section "Environment variable configuration" dans le fichier settings.js.

Dans cette section, vous pouvez définir des variables d'environnement en utilisant la syntaxe suivante :


<code>process.env.NOM_DE_LA_VARIABLE = 'valeur_de_la_variable';</code><br>
Remplacez "NOM_DE_LA_VARIABLE" par le nom de votre variable d'environnement et "valeur_de_la_variable" par la valeur que vous souhaitez définir.

Enregistrez les modifications apportées au fichier settings.js et redémarrez votre serveur Node-RED.

Vous pouvez accéder à la valeur de votre variable d'environnement à partir de n'importe quel nœud dans votre flow en utilisant la syntaxe suivante :

<code>process.env.NOM_DE_LA_VARIABLE</code><br>
Remplacez "NOM_DE_LA_VARIABLE" par le nom de votre variable d'environnement.

Il est important de noter que les variables d'environnement définies dans le fichier settings.js ne sont visibles que pour l'instance de Node-RED exécutant votre projet. Si vous avez besoin de définir des variables d'environnement pour l'ensemble du système, vous devez les définir dans le système d'exploitation sous-jacent.

## Créer une route API qui renvoie du JSON

Pour créer une route API qui renvoie du JSON sur Node-RED, vous pouvez utiliser les nœuds "HTTP In", "function" et "HTTP Response".

Voici les étapes pour créer une route API qui renvoie du JSON sur Node-RED :

Ajoutez un nœud "HTTP In" à votre flux Node-RED et configurez-le pour écouter les requêtes HTTP entrantes sur un chemin d'URL spécifique (par exemple, "/api/data").

Ajoutez un nœud "function" à votre flux Node-RED et connectez-le au nœud "HTTP In". Dans le nœud "function", utilisez le code JavaScript suivant pour générer les données JSON que vous souhaitez renvoyer :


### Générer les données JSON
<br><code>var data = {
    "name": "John Doe",
    "age": 30,
    "city": "Paris"
};
</code>

### Enregistrer les données JSON dans le message pour une utilisation ultérieure
<code>msg.payload = data;</code>

### Passer le message au nœud suivant dans le flux Node-RED
<code>return msg;</code>
Dans ce code, un objet JSON est créé en utilisant des paires clé-valeur pour les données que vous souhaitez renvoyer. Cet objet est ensuite enregistré dans le message Node-RED à l'aide de la propriété msg.payload pour une utilisation ultérieure.

Ajoutez un nœud "HTTP Response" à votre flux Node-RED et connectez-le au nœud "function". Dans le nœud "HTTP Response", configurez le type de contenu de la réponse à "application/json" en utilisant la propriété "Content-Type" dans le champ "Headers", puis utilisez le code JavaScript suivant pour renvoyer les données JSON :


### Récupérer les données JSON du message
<br>
<code>var data = msg.payload;
</code>

### Envoyer les données JSON dans la réponse HTTP
msg.payload = JSON.stringify(data);
return msg;
Dans ce code, les données JSON enregistrées dans la propriété msg.payload sont récupérées et converties en chaîne JSON à l'aide de la fonction JSON.stringify(). Cette chaîne JSON est ensuite envoyée dans le corps de la réponse HTTP.

Démarrez votre flux Node-RED, puis envoyez une requête HTTP à l'URL que vous avez configurée dans le nœud "HTTP In" (par exemple, "/api/data"). Le flux Node-RED doit générer les données JSON et les renvoyer dans une réponse HTTP avec le type de contenu "application/json".

En suivant ces étapes, vous pouvez créer une route API qui renvoie des données JSON sur Node-RED. Vous pouvez personnaliser les données JSON que vous renvoyez en modifiant le code JavaScript dans le nœud "function".

## Gestion des Querystring

Pour gérer une query string dynamique sur Node-RED, vous pouvez utiliser le nœud "HTTP In" pour récupérer la requête HTTP entrante, et le nœud "function" pour manipuler la query string en JavaScript.

Voici un exemple de flux Node-RED qui récupère une query string dynamique et la manipule pour renvoyer une réponse HTTP en utilisant le nœud "HTTP Response" :

Ajoutez un nœud "HTTP In" à votre flux Node-RED, puis configurez-le pour écouter les requêtes HTTP entrantes sur un chemin d'URL spécifique (par exemple, "/api").

Ajoutez un nœud "function" à votre flux Node-RED, puis connectez-le au nœud "HTTP In". Dans le nœud "function", utilisez le code JavaScript suivant pour extraire la query string de la requête HTTP :

### Récupérer la query string de la requête HTTP
<code>var queryString = msg.req._parsedUrl.query;</code>

### Afficher la query string dans les logs Node-RED
<code>console.log("Query string : " + queryString);</code>

### Enregistrer la query string dans le message pour une utilisation ultérieure
<code>msg.queryString = queryString;</code>

### Passer le message au nœud suivant dans le flux Node-RED
<code>return msg;</code><br>
Dans ce code, la variable queryString est extraite de la requête HTTP entrante à l'aide de la propriété _parsedUrl.query de l'objet msg.req. Cette variable est ensuite enregistrée dans le message Node-RED à l'aide de la propriété msg.queryString pour une utilisation ultérieure.

Ajoutez un nœud "HTTP Response" à votre flux Node-RED, puis connectez-le au nœud "function". Dans le nœud "HTTP Response", utilisez le code JavaScript suivant pour renvoyer une réponse HTTP contenant la query string extraite :


### Récupérer la query string du message
<code>var queryString = msg.queryString;</code>

### Construire la réponse HTTP avec la query string
<code>var responseBody = "Query string : " + queryString;</code>

### Envoyer la réponse HTTP au client
<code>msg.payload = responseBody;
return msg;</code><br>
Dans ce code, la variable queryString est récupérée à partir de la propriété msg.queryString du message Node-RED. La réponse HTTP renvoyée contient la query string extraite dans le corps de la réponse.

Démarrez votre flux Node-RED, puis envoyez une requête HTTP à l'URL que vous avez configurée dans le nœud "HTTP In", en incluant une query string dans l'URL (par exemple, "/api?param1=value1&param2=value2"). Le flux Node-RED doit extraire la query string de la requête HTTP entrante, la stocker dans le message Node-RED et renvoyer une réponse HTTP contenant la query string extraite.

Cet exemple montre comment vous pouvez utiliser le nœud "function" pour manipuler une query string dynamique dans Node-RED. Vous pouvez utiliser cette technique pour extraire des paramètres de requête de la query string, les utiliser pour effectuer des opérations dans votre flux Node-RED et renvoyer une réponse HTTP contenant les résultats.

## Configuration des cookies

Pour récupérer le cookie envoyé par la requête HTTP, vous pouvez utiliser le champ "msg.headers" dans Node-RED.

Tout d'abord, assurez-vous que la réponse de l'API inclut un en-tête "Set-Cookie" avec une valeur de cookie valide. Vous pouvez le vérifier en utilisant un outil tel que Postman ou cURL.

Dans le nœud "http request", cochez la case "Store cookies" dans l'onglet "Advanced". Cela permettra au nœud "http request" de stocker automatiquement les cookies renvoyés par l'API.

Connectez le nœud "http request" à un nœud "debug" pour afficher la réponse.

Dans le nœud "debug", cliquez sur "Configure" pour afficher les options avancées.

Cochez la case "Output to console" pour afficher la sortie de débogage dans la console Node-RED.

Ensuite, ajoutez un nœud "function" après le nœud "debug". Dans le champ "Function", saisissez le code suivant pour extraire le cookie :

<code>var cookie = msg.headers['set-cookie'][0];
msg.payload = cookie;
return msg;</code><br>

Ce code extrait le premier cookie stocké dans le tableau "set-cookie" du champ "msg.headers" et l'affecte au champ "msg.payload". Enfin, il renvoie l'objet "msg".

Connectez le nœud "function" à un autre nœud "debug" pour afficher le cookie extrait.

## Requête CURL

Pour lancer une requête CURL sur Node-RED, vous pouvez utiliser le nœud "exec" de Node-RED. Ce nœud vous permet d'exécuter des commandes Shell, y compris des commandes CURL, directement depuis votre flux de travail Node-RED.

Voici les étapes pour lancer une requête CURL sur Node-RED :

Ajoutez un nœud "inject" à votre flux de travail pour déclencher la requête CURL.
Connectez le nœud "inject" à un nœud "exec".
Double-cliquez sur le nœud "exec" pour configurer la commande à exécuter. Entrez la commande CURL avec les options et les paramètres appropriés. Par exemple, pour effectuer une requête GET sur une URL donnée :
<br>
<code>curl -X GET https://api.example.com/data</code><br>
Cliquez sur "Done" pour enregistrer la configuration du nœud "exec".
Déployez votre flux de travail en cliquant sur le bouton "Deploy".
Le nœud "exec" exécutera la commande CURL chaque fois que le nœud "inject" déclenche le flux de travail.<br><br>
Vous pouvez également utiliser le nœud "http request" de Node-RED pour effectuer des requêtes HTTP à la place de CURL. Ce nœud est plus spécifique à l'envoi de requêtes HTTP et offre une interface de configuration plus conviviale pour les paramètres de la requête.





