# Système de Détection de Fraudes en Temps Réel

Ce projet implémente un système de détection de fraudes en temps réel utilisant Kafka Streams, InfluxDB et Grafana. Le système analyse les transactions financières en continu et détecte les transactions suspectes basées sur des règles prédéfinies.

## Architecture du Système

L'architecture du système se compose des éléments suivants :

1. **Kafka & Kafka Streams**
   - Topic `transactions-input` : Reçoit les transactions brutes
   - Topic `fraud-alerts` : Stocke les transactions détectées comme suspectes
   - Application Kafka Streams : Traite les transactions en temps réel

2. **InfluxDB**
   - Stockage des transactions suspectes
   - Optimisé pour les données temporelles
   - Permet des requêtes performantes pour l'analyse

3. **Grafana**
   - Tableaux de bord en temps réel
   - Visualisation des métriques de fraude
   - Actualisation automatique des données

## Règles de Détection de Fraudes

Actuellement, le système implémente les règles suivantes :

- **Montant Élevé** : Transactions dépassant 10 000 unités
- Structure de données des transactions :
  ```json
  {
    "userId": "12345",
    "amount": 15000,
    "timestamp": "2024-12-04T15:00:00Z"
  }
  ```
## Structure du Projet
flowchart LR
    subgraph Input["Sources de données"]
        DP["Data Producer\n(Transactions JSON)"]
        style DP fill:#f9f,stroke:#333,stroke-width:2px
    end

    subgraph Kafka["Ecosystem Kafka"]
        TI["Topic:\ntransactions-input"] --> KS["Kafka Streams\nApplication"]
        KS --> FA["Topic:\nfraud-alerts"]
        
        subgraph KSDetails["Kafka Streams Processing"]
            direction TB
            FD["Détection de Fraude\n(amount > 10000)"]
            ST["State Store\n(Agrégations)"]
        end
    end

    subgraph Storage["Stockage des données"]
        IDB[("InfluxDB\n(Time Series DB)")]
    end

    subgraph Visualisation["Monitoring & Visualisation"]
        GD["Grafana Dashboards"]
        subgraph Metrics["Métriques en temps réel"]
            M1["Transactions suspectes\npar utilisateur"]
            M2["Montant total\npar période"]
        end
    end

    subgraph Deploy["Déploiement"]
        DC["Docker Compose:\n- Kafka\n- InfluxDB\n- Grafana"]
    end

    DP --> TI
    KS --> FD
    FD <--> ST
    FA --> IDB
    IDB --> GD
    GD --> M1
    GD --> M2
    
    classDef kafka fill:#232F3E,stroke:#232F3E,color:#fff
    classDef storage fill:#22ADF6,stroke:#22ADF6,color:#fff
    classDef monitoring fill:#F46800,stroke:#F46800,color:#fff
    classDef deployment fill:#2496ED,stroke:#2496ED,color:#fff
    
    class TI,KS,FA,FD,ST kafka
    class IDB storage
    class GD,M1,M2 monitoring
    class DC deployment



### Étapes de Déploiement


2. **Lancer l'environnement**
   ```bash
   docker-compose up build
   docker-compose up -d
   ```

3. **Créer les topics Kafka**
   ```bash
     kafka-topics --create --bootstrap-server localhost:9092 --topic transactions-input --partitions 3 --replication-factor 1
     kafka-topics --create --bootstrap-server localhost:9092 --topic fraud-alerts --partitions 3 --replication-factor 1
   ```

### Configuration de Grafana et InfluxDB in docker compose file 


3. **Monitorer dans Grafana**
   


