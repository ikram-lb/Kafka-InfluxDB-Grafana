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
    
  <img width="464" alt="image" src="https://github.com/user-attachments/assets/7d86542a-0170-4cfc-81f4-7ed0d6d54a60" />

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
 #### Test kafka Producer-Consumer 
   <img width="554" alt="Topic-producer" src="https://github.com/user-attachments/assets/a02c3aed-ce84-41f7-b8c5-208ce14ca7c8" />

   <img width="630" alt="Topic-Consummer" src="https://github.com/user-attachments/assets/c4f26077-35c4-4d5b-b21f-ab022e339616" />

### Configuration de Grafana et InfluxDB in docker compose file 


3. **Monitorer dans Grafana**
   


