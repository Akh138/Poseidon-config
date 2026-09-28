# ⚙️ Poseidon - Centralized Configuration Repository

Ce dépôt constitue le **répertoire de configuration centralisé et externalisé** de l'écosystème microservices **Poseidon Trading Platform**.

Il est consommé dynamiquement au démarrage de l'infrastructure par le microservice **Spring Cloud Config Server** (port `8071`).

---

## 🎯 Rôle dans l'Architecture

Conformément aux principes des **12-Factor Apps (Facteur III : Configuration)**, la configuration applicative est strictement séparée du code source des microservices.

```text
  [ Dépôt GitHub : Poseidon-config ]
                 │
                 ▼ (Pull Git au boot)
    [ Spring Cloud Config Server : 8071 ]
                 │
                 ├─► user-service
                 ├─► bidlist-service
                 ├─► trade-service
                 ├─► curvepoint-service
                 ├─► rating-service
                 └─► rulename-service
```

### Avantages de cette approche :
* **Découplage total** : Modification des paramètres de base de données, timeouts HikariCP ou niveaux de logs sans avoir à recompiler le code Java ni reconstruire les images Docker.
* **Traçabilité & Versioning** : Chaque changement de configuration est historisé via les commits Git.
* **Sécurité & Cohérence** : Paramétrage unifié des accès MySQL, de la découverte Consul et de l'export des endpoints Actuator.

---

## 📁 Inventaire des Fichiers

| Fichier | Service Cible | Paramètres Clés Gérés |
| :--- | :--- | :--- |
| **`bidlist.properties`** | Service BidList | Datasource MySQL, Hibernate DDL, Consul Discovery |
| **`curvepoint.properties`** | Service CurvePoint | Datasource MySQL, pool HikariCP, Actuator Health |
| **`rating.properties`** | Service Rating | Connexion persistante MySQL, port d'écoute |
| **`rulename.properties`** | Service RuleName | Profils d'initialisation SQL, discovery Consul |
| **`trade.properties`** | Service Trade | Propriétés transactionnelles, dialecte MySQL |
| **`user.properties`** | Service User | Chiffrement, validation et découverte réseau |

---

## 🔗 Projet Principal

L'ensemble du code source des microservices, l'orchestration Docker Compose et l'interface de négociation sont disponibles sur le dépôt principal :

👉 **[PoseidonApplication (Dépôt Principal)](https://github.com/Akh138/PoseidonApplication)**
