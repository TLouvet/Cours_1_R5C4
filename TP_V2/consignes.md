# Consignes pour le TP Flask : API de Gestion de Tâches

## Objectifs

Votre objectif est de développer une API avec Flask qui permette de gérer une liste de tâches (Todo List) via les différentes opérations CRUD (Create, Read, Update, Delete). Vous allez également mettre en place des statistiques sur ces tâches.

## Prérequis

1. L'application doit être développée en utilisant Flask.
2. Toutes les requêtes doivent pouvoir être effectuées depuis le frontend fourni (fichiers HTML et JavaScript).
3. Vous devez autoriser toutes les applications à se connecter et permettre les méthodes **GET / POST / PUT / DELETE et OPTIONS**.

## Représentation d'une tâche en Base de données

- "id_tache": chaine de caractères
- "nom": chaîne de caractères
- "description": chaîne de caractères
- "statut" : chaine de caractères, énumération entre TODO, DONE et IN_PROGRESS. Tout autre valeur doit amené un refus par Requête erronnée
- "priorite": nombre entier
- "categorie": chaine de caractères
- "utilisateur": chaîne de caractères
- "date_creation": chaine de caractères

## Tâches à Implémenter

### 1. **Lancer l'Application**

- L'application doit se lancer sans erreur.
- Le serveur doit écouter sur `http://localhost:5000`.

### 2. **Routes à Implémenter**

Pour chaque endpoint, vous devez vous baser sur les définitions fournies dans le fichier `routes.js`. N'inventez pas d'endpoint, ils sont déjà prêts.

#### a. **Liste de toutes les tâches** ()

- **Format de retour attendu** : Une liste de tâches contenant des dictionnaires `[{tache1}, {tache2}, ...]`
- **Pagination** : La pagination doit être gérée à l'aide de deux paramètres de requête : `offset` et `limit`.
  - **Exemple** : `/tasks?offset=0&limit=10` doit retourner les 10 premières tâches.
  - **Remarque** : `offset` et `limit` doivent être convertis en nombre entier avant utilisation, le site d'exemple envoie systématiquement une query

#### b. **Trouver une tâche par ID** ()

- **Format de retour attendu** : Un dictionnaire contenant les propriétés de la tâche.
- **Gestion des erreurs** : Si la tâche n'est pas trouvée, retourner une réponse avec le code HTTP approprié et un message d'erreur sous le format `{"error": "Tâche non trouvée"}`.

#### c. **Créer une nouvelle tâche** ()

- **Format de retour attendu** : La nouvelle tâche créée avec ses attributs (y compris `id` et `date de création`).
- **Gestion de l'ID et de la date de création** :
  - L'ID (`id_tache`) et la date de création (`date_creation`) doivent être gérés par le backend **et ne font jamais partie du payload de création**. Respectez bien la nomenclature
  - Une variable globale `next_id` est fournie pour gérer l'attribution des IDs. Pensez à utiliser le mot-clé `global` pour manipuler cette variable. Si vous préférez une autre solution, assurez vous simplement que la clé soit unique pour ne pas casser votre base. (uuid par exemple)
  - Vous pouvez générer la date de création avec datetime.now par exemple, nous ne travaillerons pas avec ce champ.
- **Validation** : La validation des inputs doit être faite côté backend. Le modèle de données à suivre se situe dans le fichier "grand_dataset_taches.py" qui contient l'entièreté de la base. Vous devez donc avoir validé les champs suivants avant enregistrement dans la base de données:
  - "nom": chaîne de caractères
  - "description": chaîne de caractères
  - "statut" : chaine de caractères, énumération entre TODO, DONE et IN_PROGRESS. Tout autre valeur doit amené un refus par Requête erronnée
  - "priorite": nombre entier
  - "categorie": chaine de caractères
  - "utilisateur": chaîne de caractères

#### d. **Modifier une tâche existante** ()

- **Format de retour attendu** : La tâche modifiée sous forme de dictionnaire.
- **Validation** : La validation des inputs doit être faite côté backend.
- **Gestion des erreurs** :
  - Si la tâche n'est pas trouvée, retourner une réponse avec le code HTTP approprié et un message d'erreur sous le format `{"error": "Tâche non trouvée"}`.

#### e. **Supprimer une tâche** ()

- **Comportement attendu** : Supprimer la tâche si elle existe et renvoyer **dans tous les cas** une réponse indiquant **l'absence de contenu.**
- **Gestion des erreurs** : L'appli ne doit pas planter si on essaie de supprimer une tâche qui n'existe pas.

#### f. **Statistiques sur les tâches** ()

- **Format de retour attendu** : Un dictionnaire avec le nombre de tâches par statut.
  - 3 status existent pour l'application = 'TODO', 'IN_PROGRESS', 'DONE'
  - **Exemple de retour** :
  ```json
  {
    "En attente": 1002,
    "En cours": 102,
    "Terminées": 1500
  }
  ```

Une façon possible de réaliser la logique est de créer un dictionnaire avec les 3 valeurs à 0, itérer sur les valeurs du dictionnaire et en fonction du statut de la tâche, d'incrémenter la clé liée.

### 3. **Contours du projet**

#### a. **Documentation**

Veillez à laisser à disposition les informations nécessaires pour installer le projet

- dépendances à installer
- procédure d'installation du projet

#### b. **Code**

Vous pouvez coder en français ou en anglais, mais restez cohérents sur vos choix
Attention aux fichiers/dossiers que vous versionnez, certains n'ont pas vocation à quitter votre PC

#### c. **GPT & co**

Merci de ne pas l'utiliser pour ce projet, il n'y a pas de complexité qui nécessite son utilisation
Vous avez en revanche droit de consulter la doc Python, Flask, StackOverflow ...

### d. Architecture

Nous avons vu quelques notions d'archtitecture de code, une partie de la note tiendra compte de son respect

## Conseils pour la réalisation

- Déclarez votre application, assurez vous qu'une route hello world est fonctionnelle
- Ouvrez votre domaine au front avec les configurations de sécurité
- Commencer par déclarer toutes vos routes sans logique, afin de visualiser votre api globalement
- Pour chaque route, renvoyez le minimum nécessaire pour formater une réponse correcte avec le code attendu
- Implémentez pour chaque route la logique attendue
- Commencez par les routes "faciles" -> Récupération et suppression de tâches
- Vérifiez systématiquement le bon fonctionnement de vos routes selon les spécifications, que ça soit avec le front ou avec une extension de test type Chrome Talend Api Tester
- Faites les routes qui ajoutent et modifient la base de données
- Testez encore
- Introduisez la validation de payload, testez encore
- Une fois arrivés là, votre application fonctionne sauf peut-être la page statistiques, faites un peu d'architecture
- Faites la route pour les statistiques, testez encore
- Complétez la doc avec un Readme m'expliquant comment installer votre projet sur mon PC, éventuelement listez vos routes dans ce document
- Regardez si vous pouvez améliorer votre code (respect de la PEP 8 - entre autres pas de camelCase en python -, fonctions avec un nom significatif, idem variables, le code doit être compréhensible en un coup d'oeil)
- Si vous arrivez ici en ayant respecté tous les points au-dessus, vous avez 20.
