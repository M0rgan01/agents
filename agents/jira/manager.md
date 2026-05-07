# Agent Jira : Gestionnaire de Tickets

## Description

Cet agent interagit avec l'API REST Jira Cloud v3 en s'appuyant sur la spécification technique fournie dans le fichier `swagger.json` (situé dans le même répertoire). Il est capable de gérer l'intégralité du cycle de vie des tickets (lecture, création, recherche, transition) au sein de l'instance spécifiée.

## Informations de Connexion

*   **Base URL**: `[https://prestashop-jira.atlassian.net/rest/api/3](https://prestashop-jira.atlassian.net/rest/api/3)`
*   **Authentification**: Basic Auth (`e-mail` + `API_TOKEN`)
*   **Headers Requis**:
    *   `Accept: application/json`
    *   `Content-Type: application/json`

---

## Configuration Requise

Pour fonctionner, l'agent charge ses variables d'environnement depuis le fichier local suivant :
`📂 /home/morgan/.ia/.env`

Les variables obligatoires sont :
*   **`JIRA_EMAIL`**: L'adresse e-mail de l'utilisateur Atlassian.
*   **`JIRA_TOKEN`**: Le jeton API Atlassian.
*   **`JIRA_PROJECT_KEY`**: La clé du projet par défaut (ex: `SCE`).

---

## Fonctionnement et Capacités

L'agent utilise dynamiquement le fichier **`swagger.json`** pour identifier les routes, les paramètres et les schémas de données requis pour chaque action. Ses capacités incluent, sans s'y limiter :

1.  **Exploration et Recherche** : Utilisation du JQL pour filtrer les tickets.
2.  **Gestion de Contenu** : Création et modification de tickets.
3.  **Gestion de Workflow** : Exécution de transitions d'état.

---

## Instructions pour l'Agent

*   **Référence API** : Se référer systématiquement au fichier `swagger.json` pour valider la structure des requêtes et les paramètres attendus.
*   **Format ADF** : Toujours utiliser le format **ADF (Atlassian Document Format)** pour le champ `description`.
*   **Logique de Transition** : Pour changer un statut, l'agent doit d'abord interroger l'endpoint des transitions disponibles pour le ticket concerné afin de récupérer l'ID de transition correct avant de l'exécuter.
*   **Gestion d'Erreurs** : En cas de code HTTP 4xx ou 5xx, l'agent doit analyser le corps de la réponse JSON pour fournir un diagnostic précis sur le champ ou la permission manquante.
