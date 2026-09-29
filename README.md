# ⚙️ Poseidon - Centralized Configuration Repository

Ce dépôt constitue le **répertoire de configuration centralisé et externalisé** de l'écosystème microservices **Poseidon Trading Platform**.

Il est consommé dynamiquement au démarrage de l'infrastructure par le microservice **Spring Cloud Config Server** (port `8071`) pour injecter les réglages d'environnement dans chaque microservice métier.

---

## 🎯 Rôle dans l'Architecture

Conformément aux principes des **12-Factor Apps (Facteur III : Configuration)**, la configuration applicative est strictement séparée du code source Java et des images Docker :

```text
  [ Dépôt GitHub : Poseidon-config ]
                 │
                 ▼ (Pull Git au démarrage)
    [ Spring Cloud Config Server : 8071 ]
                 │
                 ├─► user-service (Port 8086)
                 ├─► bidlist-service (Port 8081)
                 ├─► trade-service (Port 8085)
                 ├─► curvepoint-service (Port 8082)
                 ├─► rating-service (Port 8083)
                 └─► rulename-service (Port 8084)
```

### Avantages de cette approche :
* **Découplage & Agilité** : Possibilité de modifier les paramètres réseau, dialectes de base de données ou sondes de santé sans recompiler le code Java ni reconstruire les images Docker.
* **Polyglot Persistence** : Déclaration unifiée des moteurs de stockage adaptés à chaque besoin (MySQL persistant pour les comptes, H2 ultra-rapide en mémoire pour les modules de cotation).
* **Traçabilité & Versioning** : Chaque changement de paramètre d'infrastructure est historisé via les commits Git.

---

## 📁 Inventaire des Fichiers de Configuration

| Fichier | Service Cible | Moteur de Base de Données | Port & Paramètres Clés |
| :--- | :--- | :--- | :--- |
| **`bidlist.properties`** | Service BidList | **H2 In-Memory** (`bidlistdb`) | Port `8081`, Consul Discovery, Actuator Health |
| **`curvepoint.properties`** | Service CurvePoint | **H2 In-Memory** (`curvepointdb`) | Port `8082`, Formatage SQL, Consul Discovery |
| **`rating.properties`** | Service Rating | **H2 In-Memory** (`ratingdb`) | Port `8083`, Initialisation SQL, Consul Discovery |
| **`rulename.properties`** | Service RuleName | **H2 In-Memory** (`rulenamedb`) | Port `8084`, Defer-init SQL, Consul Discovery |
| **`trade.properties`** | Service Trade | **H2 In-Memory** (`tradedb`) | Port `8085`, Dialecte H2, Consul Discovery |
| **`user.properties`** | Service User | **MySQL 8.0** (`poseidon_db`) | Port `8086`, Persistance MySQL, Consul Discovery |

---

## 🔗 Projet Principal

L'ensemble du code source des microservices, l'orchestration Docker Compose et l'interface de négociation sont disponibles sur le dépôt principal :

👉 **[PoseidonApplication (Dépôt Principal)](https://github.com/Akh138/PoseidonApplication)**
