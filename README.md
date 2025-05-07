
# Projet Nextcloud en Docker Compose

## Description

Ce projet met en place un mini-cloud personnel basé sur **Nextcloud**, accompagné d’une base de données **MariaDB**, d’un outil d’administration de la base de données **Adminer** et d’un conteneur **Watchtower** pour la mise à jour automatique des images Docker.

Tout le projet est orchestré avec **Docker Compose**, et repose sur :
- Plusieurs conteneurs communicants via un réseau virtuel dédié.
- Des volumes persistants pour les données.
- Des variables d'environnement externes via un fichier `.env`.
- Un ordre de démarrage contrôlé grâce à `depends_on`.
- Des directives avancées comme des **healthchecks** et des **labels**.

---
```
## Architecture des services


+------------+ +------------------+
| Nextcloud | <--> | MariaDB |
+------------+ +------------------+
|
v
+------------+
| Adminer |
+------------+

+------------+
| Watchtower |
+------------+

```

---

## Services inclus

| Service    | Description                          | Port d’accès  |
|------------|--------------------------------------|---------------|
| `nextcloud` | Application de cloud personnel       | [localhost:8080](http://localhost:8080) |
| `db`        | Base de données MariaDB              | interne       |
| `adminer`   | Interface web pour gérer la base     | [localhost:8081](http://localhost:8081) |
| `watchtower`| Mise à jour automatique des images   | interne       |

---

## Structure du projet

```

.
├── docker-compose.yml
├── .env
├── .gitignore
├── README.md
├── nextcloud/
│   ├── Dockerfile
│   └── custom.ini

````

---

## Installation & utilisation

### 1️ Prérequis :
- Docker

---

### 2️ Cloner le projet

```bash
git clone https://github.com/Coco-LeRigolo/Nextcloud_docker.git
cd Nextcloud_docker
````

---

### 3️ Configurer les variables d’environnement

Modifier le fichier `.env` :

```env
MYSQL_ROOT_PASSWORD=supersecret
MYSQL_PASSWORD=secret
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextclouduser
```

---

### 4️ Lancer les services

```bash
docker compose up -d --build
```

---

### 5️ Accéder aux services

* Nextcloud : [http://localhost:8080](http://localhost:8080)
* Adminer : [http://localhost:8081](http://localhost:8081)

---

### 6️ Configuration de Nextcloud

Lors de la première connexion à **[http://localhost:8080](http://localhost:8080)**, il faut compléter la configuration :

* Choisir un nom d'utilisateur et mot de passe administrateur Nextcloud (libre)
* Renseigner les informations de la base de données comme suit :

| Champ                       | Valeur                                            |
| :-------------------------- | :------------------------------------------------ |
| **Type de base de données** | `MySQL/MariaDB`                                   |
| **Identifiant**             | `nextclouduser`                                   |
| **Mot de passe**            | celui renseigné dans `.env` sous `MYSQL_PASSWORD` |
| **Nom de la base**          | `nextcloud`                                       |
| **Hôte**                    | `db`                                              |

⚠ **Important** :
Le champ **Hôte** doit impérativement être `db` (nom du service Docker Compose) et non `localhost`.

Une fois ces informations renseignées, cliquer sur **Terminer l’installation**.

### 7 Connexion à Adminer

Accède à Adminer via **[http://localhost:8081](http://localhost:8081)**

| Champ                       | Valeur                                            |
| :-------------------------- | :------------------------------------------------ |
| **Type de base de données** | `MySQL/MariaDB`                                   |
| **Serveur**                 | `db`                                              |
| **Utilisateur**             | `nextclouduser`                                   |
| **Mot de passe**            | celui renseigné dans `.env` sous `MYSQL_PASSWORD` |
| **Base de données**         | `nextcloud`                                       |
Puis cliquer sur Connexion.

---

## Fonctionnalités avancées

*  **Volumes nommés** pour persister les données MariaDB et Nextcloud.
*  **Réseau bridge personnalisé** (`cloudnet`) avec sous-réseau dédié.
*  **Healthcheck** sur la base MariaDB pour contrôler son état.
*  **Labels** pour faciliter la gestion et la documentation des conteneurs.
*  **Watchtower** qui surveille et met à jour automatiquement les conteneurs.

---

## Commandes utiles

Afficher les logs d’un service :

```bash
docker compose logs -f nextcloud
```

Arrêter et supprimer les services :

```bash
docker compose down
```

Lister les conteneurs :

```bash
docker ps
```

