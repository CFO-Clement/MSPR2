# MSPR2 - Solution de Surveillance et de Télémaintenance

## Objectifs

Ce projet vise à mettre en œuvre une solution complète de surveillance et de télémaintenance des systèmes informatiques à travers deux sous-composants principaux :
- **L'Harvester** : un client qui collecte des métriques des machines clientes de manière sécurisée.
- **Le Nester** : un serveur qui supervise les clients et facilite la télémaintenance.

## Architecture

### Harvester (Client)
Le Harvester est responsable de la collecte des métriques système à l'aide de Prometheus et de `node_exporter`. Il utilise également `remote_write` pour centraliser les données dans une base de données TimeScaleDB/Postgres12.

#### Fonctionnalités principales
- Collecte de métriques avec Prometheus.
- Télémaintenance avec VNC et ShellInABox pour l'accès distant et sécurisé.

### Nester (Serveur)
Le Nester fournit une interface web pour la supervision des Harvesters et réalise la télémaintenance grâce à l'intégration de services comme Grafana et NoVNC.

#### Fonctionnalités principales
- Supervision des Harvesters via des tableaux de bord Grafana.
- Interface web avec Flask pour le contrôle et la supervision.
- Intégration de NoVNC pour l'accès VNC dans le navigateur.

## Configuration et Déploiement

Les deux sous-systèmes utilisent Ansible pour automatiser le déploiement et la configuration des services nécessaires. Chaque sous-système comprend des playbooks Ansible spécifiques qui gèrent l'installation, la configuration, et le lancement des services.

### Harvester
- Répertoire `prometheus_ansible/` pour la configuration de Prometheus et `node_exporter`.
- Répertoire `novnc_ansible/` pour la configuration de NoVNC et VNC.
- Script `install_prometheus.sh` et `install_novnc.sh` pour l'installation automatique.

### Nester
- Répertoire `grafana_ansible/` pour l'installation de Grafana.
- Répertoire `nester_ansible/` pour le déploiement de l'interface web Flask et l'intégration de NoVNC.
- Script `install_grafana.sh` et `install_nester.sh` pour l'installation automatique.

## Intégration avec TimescaleDB

Pour une gestion efficace et performante des données de métriques collectées par Prometheus, TimescaleDB est configuré pour fonctionner de concert avec ce dernier. Le processus d'installation et de configuration détaillé est disponible dans le fichier `SGDB.md`. 

### Résumé de l'installation
- Installation de TimescaleDB sur un serveur PostgreSQL.
- Configuration pour accepter les connexions Prometheus.
- Installation de l'adaptateur Prometheus pour TimescaleDB.

## Résumé des Ports Utilisés
- **Harvester** : Ports 9090 (Prometheus), 9100 (Node Exporter), 6082 (NoVNC), 6175 (ShellInABox), 5902 (VNC).
- **Nester** : Ports 3000 (Grafana), 6080 (NoVNC), 8080 (interface web), 5000 (reverse TCP pour Flask).
- **SGDB** : Ports 5432 (TimeScalesPG), 9201 (Adaptateur)

## Comment Contribuer

Les contributions sont bienvenues ! Veuillez consulter les fichiers `README.md` individuels des sous-projets pour plus de détails sur le développement et la configuration.

---

Pour plus d'informations sur le déploiement, la configuration, et l'utilisation des différents services et outils, veuillez vous référer aux `README.md` de chaque sous-repo ainsi que le fichier `SGDB.md`
