# LOG430 - Lab 6 - Système POS Multi-Magasins
## Architecture microservices avec Saga Orchestration

Système de point de vente (POS) **multi-magasins** évoluant vers une **architecture microservices avec orchestration saga** pour la coordination de transactions distribuées. Le système gère **5 magasins + 1 centre logistique + 1 maison mère** avec 9 microservices indépendants, API Gateway Kong et saga orchestrator pour les transactions complexes.

## Evolution d'architecture

```
Lab 3: API REST → Lab 4: Optimisations → Lab 5: Microservices → Lab 6: Saga Orchestration
   ↓                    ↓                      ↓                        ↓
Interface API        Load Balancer         7 Services isolés      9 Services + Saga
documentée          + Cache Redis         + Kong Gateway         + Transactions distribuées
```

## Architecture microservices avec Saga

- **Kong API Gateway** : Point d'entrée unique sécurisé (port 8080)
- **9 Microservices** : Services indépendants avec database per service
- **Saga Orchestration** : Coordination synchrone de transactions distribuées
- **Compensation automatique** : Rollback intelligent en cas d'échec
- **Load balancing** : Distribution intelligente avec 3 instances Cart Service
- **Observabilité** : Monitoring Prometheus/Grafana avec métriques saga

## Architecture microservices (9 services)

### Services métier extraits
1. **Product Service (8001)** - Catalogue produits et catégories
2. **Inventory Service (8002)** - Gestion stocks par location
3. **Sales Service (8003)** - Transactions de vente magasins
4. **Reporting Service (8004)** - Analytique et tableaux de bord

### Services e-commerce
5. **Customer Service (8005)** - Comptes clients et authentification JWT
6. **Cart Service (8006)** - Panier d'achat avec sessions Redis
7. **Order Service (8007)** - Commandes et checkout e-commerce

### Nouveaux services Lab 6 - Saga Orchestration
8. **Saga Orchestrator (8008)** - Coordination saga et compensation
9. **Payment Service (8009)** - Traitement paiements et remboursements

### Infrastructure
- **Kong API Gateway (8080)** : Point d'entrée unique avec sécurité multicouche
- **Database per service** : 9 PostgreSQL + 1 Redis (isolation complète)
- **Saga state management** : In-memory avec possibilité de persistance
- **Load balancing** : 3 instances Cart Service avec least-connections
- **Monitoring** : Prometheus/Grafana avec métriques saga étendues

## Entités métier

- **5 Magasins** : Montréal Centre, Québec, Sherbrooke, Trois-Rivières, Gatineau
- **1 Centre Logistique** : Distribution et gestion des stocks
- **1 Maison Mère** : Supervision et rapports consolidés
- **E-commerce** : Boutique en ligne avec comptes clients

## Fonctionnalités microservices

### API Gateway Kong (point d'entrée unique)
- **Routage dynamique** : Distribution automatique vers 7 microservices
- **Sécurité multicouche** : API Keys + JWT pour Customer/Orders
- **Load balancing** : 3 instances Cart Service (least-connections)
- **Observabilité** : Métriques Prometheus + logging centralisé

### Services métier (extraits du monolithe)
- **Product Service** : Catalogue produits, catégories, recherche
- **Inventory Service** : Stocks par entité, mouvements, alertes
- **Sales Service** : Transactions POS, historique magasins
- **Reporting Service** : Analytics, KPIs, tableaux de bord

### Services e-commerce
- **Customer Service** : Comptes clients, authentification JWT, profils
- **Cart Service** : Panier d'achat, sessions Redis, calculs taxes
- **Order Service** : Commandes, checkout, workflow paiement

### Services saga orchestration (Lab 6)
- **Saga Orchestrator** : Coordination transactions distribuées, machine d'état saga
- **Payment Service** : Traitement paiements, remboursements, simulation d'échecs

### Interface Console (conservée)
- **Magasins** : Recherche produits, ventes, retours, approvisionnement
- **Centre Logistique** : Traitement demandes inter-magasins
- **Maison Mère** : Rapports consolidés, gestion globale

### Interface Web (évoluée)
- **Dashboards Grafana** : Kong Gateway + microservices
- **Métriques temps réel** : Performance, distribution, santé services
- **Comparaison architectures** : Lab 4 vs Lab 5

### API REST (distribuée)
- **7 microservices** : Chacun avec API REST et documentation Swagger
- **Point d'entrée unique** : Kong Gateway (port 8080)
- **Authentification** : API Keys + JWT selon le service
- **Standards RESTful** : Maintenus dans chaque microservice

## Capacités Saga Orchestration Lab 6

L'architecture microservices avec saga orchestration Lab 6 apporte une **coordination intelligente des transactions distribuées** avec compensation automatique.

### Nouvelles capacités transactionnelles

| Capacité | Lab 5 | Lab 6 Saga | Amélioration |
|----------|-------|-------------|-------------|
| **Transactions distribuées** | Manuelle | Automatique | **Coordination saga** |
| **Gestion d'échecs** | Manuelle | Compensation automatique | **0% perte de données** |
| **Cohérence état** | Eventually consistent | Coordonnée par saga | **Garanties ACID distribuées** |
| **Observabilité transactions** | Limitée | Métriques saga complètes | **Traçabilité end-to-end** |

### Métriques saga orchestration
- **Durée saga moyenne** : 2-5 secondes (succès)
- **Compensation automatique** : < 10 secondes en cas d'échec
- **Taux de succès** : 95%+ avec retry automatique
- **Observabilité** : Métriques Prometheus détaillées par étape

### Résilience et compensation

| Scenario d'échec | Lab 5 | Lab 6 Saga | Amélioration |
|------------------|-------|-------------|-------------|
| **Échec paiement** | Incohérence | Rollback automatique stock | **Cohérence garantie** |
| **Service indisponible** | Timeout | Compensation + retry | **Récupération automatique** |
| **Échec partiel** | État invalide | Machine d'état saga | **États cohérents** |
| **Monitoring échecs** | Manual | Métriques automatiques | **Observabilité complète** |

### Architecture recommandée
- **Transactions critiques** : Lab 6 (Saga) optimal  
- **Coordination distribuée** : Automatique avec compensation
- **Cohérence données** : Garanties ACID distribuées
- **Observabilité** : Traçabilité complète des transactions

## Démarrage rapide - Architecture microservices avec Saga

### Prérequis
- Docker Engine 20.10+
- Docker Compose v2.0+

### Lancement microservices avec saga orchestration

```bash
# Démarrer l'architecture microservices complète (9 services)
cd microservices/
docker-compose up -d

# Configuration automatique des clés API
./scripts/setup-api-keys.sh

# Tests de validation API Gateway
./scripts/test-api-gateway.sh

# Tests spécifiques saga orchestration
./scripts/test-saga.sh
```

### Points d'accès principaux

```bash
# Kong API Gateway - Point d'entrée unique
http://localhost:8080                 # API Gateway (toutes les requêtes)
http://localhost:8001                 # Kong Admin API

# Services microservices individuels
http://localhost:8001/docs            # Product Service (Swagger)
http://localhost:8002/health          # Inventory Service
http://localhost:8003/health          # Sales Service  
http://localhost:8004/health          # Reporting Service
http://localhost:8005/docs            # Customer Service (Swagger)
http://localhost:8006/docs            # Cart Service (Swagger)
http://localhost:8007/health          # Order Service
http://localhost:8008/docs            # Saga Orchestrator (Swagger)
http://localhost:8009/docs            # Payment Service (Swagger)

# Monitoring et observabilité
http://localhost:3000                 # Grafana (dashboards Kong + microservices)
http://localhost:9090                 # Prometheus (métriques)
```

### Utilisation API Gateway

```bash
# Utiliser l'API via Kong Gateway avec clé API
curl -H "X-API-Key: pos-web-app-2025-frontend-key" \
     http://localhost:8080/api/v1/products

# Authentification JWT pour Customer/Orders
curl -H "X-API-Key: pos-web-app-2025-frontend-key" \
     -X POST \
     -d '{"email": "user@example.com", "password": "password"}' \
     http://localhost:8080/api/v1/auth

# Test load balancing Cart Service (3 instances)
curl -H "X-API-Key: pos-mobile-2025-app-secure-key" \
     http://localhost:8080/api/v1/cart?session_id=test123
```

### Workflow microservices automatique avec saga

L'architecture microservices avec saga démarre automatiquement :
1. **9 bases PostgreSQL + 1 Redis** initialisées avec données de test
2. **9 microservices** démarrent avec health checks
3. **Kong Gateway** configure le routage automatiquement  
4. **API Keys** configurées par script d'initialisation
5. **Saga Orchestrator** initialise la machine d'état
6. **Payment Service** configure la simulation d'échecs
7. **Monitoring Prometheus/Grafana** déploie les dashboards avec métriques saga
8. **Load balancing** activé pour Cart Service (3 instances)
9. **Architecture prête** : Point d'entrée unique + coordination saga

### Dashboards d'observabilité

**Kong API Gateway Dashboard :** http://localhost:3000/d/kong-api-gateway-dashboard
- Distribution de charge temps réel (Cart Service 3 instances)
- Métriques performance par microservice
- Comparaison Lab 4 vs Lab 5
- Alerting sur déséquilibre et pannes de service

**Prometheus Targets :** http://localhost:9090/targets
- Santé des 7 microservices
- Métriques Kong Gateway
- Monitoring infrastructure Docker

## Utilisation microservices

### Kong API Gateway (point d'entrée unique)

**Base URL :** `http://localhost:8080`

**Authentification requise :**
```bash
# Web application
X-API-Key: pos-web-app-2025-frontend-key

# Mobile application  
X-API-Key: pos-mobile-2025-app-secure-key

# Tests automatisés
X-API-Key: pos-test-automation-dev-key-2025
```

### Services microservices disponibles

**Product Service** : `http://localhost:8080/api/v1/products`
- Catalogue produits et catégories
- Documentation: `http://localhost:8001/docs`

**Customer Service** : `http://localhost:8080/api/v1/customers`  
- Comptes clients et authentification JWT
- Documentation: `http://localhost:8005/docs`

**Cart Service** : `http://localhost:8080/api/v1/cart`
- Panier d'achat avec sessions (3 instances load-balancées)
- Documentation: `http://localhost:8006/docs`

**Order Service** : `http://localhost:8080/api/v1/orders`
- Commandes et checkout e-commerce
- Nécessite authentification JWT

**Saga Orchestrator** : `http://localhost:8080/api/v1/sagas`
- Coordination transactions distribuées
- Machine d'état saga et compensation
- Documentation: `http://localhost:8008/docs`

**Payment Service** : `http://localhost:8080/api/v1/payment`
- Traitement paiements et remboursements
- Simulation d'échecs configurable
- Documentation: `http://localhost:8009/docs`

**Inventory/Sales/Reporting** : Services legacy conservés
- Endpoints disponibles via Kong Gateway
- Fonctionnalités console maintenenues

### Tests saga orchestration et simulation d'échecs

```bash
# Test saga orchestration complète (succès)
cd microservices/
./scripts/test-saga.sh

# Test saga avec échec de paiement simulé
./scripts/test-saga-payment-failure.sh

# Test saga avec échec de stock
./scripts/test-saga-stock-failure.sh

# Test load balancing Cart Service (3 instances)
./scripts/test-load-balancing.sh
```

### Simulation d'échecs pour tests de compensation

```bash
# Configuration échecs paiement (Payment Service)
curl -X POST http://localhost:8009/admin/configure-failure \
  -H "Content-Type: application/json" \
  -d '{"failure_rate": 100, "failure_amounts": [99.99]}'

# Test saga avec compensation automatique
curl -H "X-API-Key: pos-web-app-2025-frontend-key" \
  -X POST http://localhost:8080/api/v1/sagas/orders \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "test_failure_123",
    "customer_id": "customer_1",
    "shipping_address": "123 Test St",
    "payment_info": {"amount": 99.99, "card": "4111111111111111"}
  }'

# Vérification état saga et compensation
curl http://localhost:8008/api/v1/sagas/{saga_id}/status

# Reset configuration échecs
curl -X POST http://localhost:8009/admin/reset-failure-config
```

### Monitoring en temps réel

**Grafana Kong Dashboard :** http://localhost:3000/d/kong-api-gateway-dashboard
- Performance des 7 microservices
- Distribution de charge Cart Service
- Comparaison architectures Lab 4 vs Lab 5
- Métriques de santé et alerting

## Tests et validation saga orchestration

```bash
# Tests de validation architecture microservices avec saga
cd microservices/
./scripts/test-api-gateway.sh

# Tests saga orchestration complète
./scripts/test-saga.sh

# Tests de compensation automatique
./scripts/test-saga-payment-failure.sh
./scripts/test-saga-stock-failure.sh

# Tests de load balancing multi-instances
./scripts/test-load-balancing.sh
```

### Exemples d'utilisation saga orchestrator

```bash
# Créer une saga de commande (succès)
curl -H "X-API-Key: pos-web-app-2025-frontend-key" \
  -X POST http://localhost:8080/api/v1/sagas/orders \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "cart_session_123",
    "customer_id": "customer_1",
    "shipping_address": "123 Main St, Montreal",
    "payment_info": {"amount": 159.99, "card": "4111111111111111"}
  }'

# Vérifier état d'une saga
curl http://localhost:8008/api/v1/sagas/{saga_id}/status

# Consulter métriques saga
curl http://localhost:8008/metrics

# Lister toutes les sagas actives
curl http://localhost:8008/api/v1/sagas/active
```

## Commandes utiles microservices

```bash
# Arrêter tous les microservices
cd microservices/
docker-compose down

# Reset complet (suppression volumes)
docker-compose down -v

# Logs par service
docker-compose logs -f kong-gateway
docker-compose logs -f product-service
docker-compose logs -f cart-service-1

# État des services
docker-compose ps

# Monitoring santé services
curl http://localhost:8080/health          # Kong Gateway
curl http://localhost:8001/health          # Product Service
curl http://localhost:8006/health          # Cart Service
curl http://localhost:8008/health          # Saga Orchestrator
curl http://localhost:8009/health          # Payment Service

# Monitoring saga orchestration
curl http://localhost:8008/api/v1/sagas/metrics    # Métriques saga
curl http://localhost:8008/api/v1/sagas/active     # Sagas actives

# Kong Admin API
curl http://localhost:8001/services        # Services configurés
curl http://localhost:8001/upstreams       # Load balancing status
```

## Technologies

### Stack microservices
- **Kong Gateway** : API Gateway avec load balancing et sécurité
- **Python 3.11 + Flask** : Microservices avec Flask-RESTX
- **PostgreSQL** : 7 bases indépendantes (Database per Service)
- **Redis 7** : Cache sessions Cart Service
- **Docker Compose** : Orchestration microservices

### Observabilité et monitoring
- **Prometheus** : Métriques Kong + microservices
- **Grafana** : Dashboards temps réel avec comparaison Lab 4 vs Lab 5
- **Kong Metrics** : Observabilité API Gateway native
- **k6** : Tests de charge et performance

### Sécurité et authentification
- **API Keys Kong** : 4 consumers avec clés différenciées
- **JWT (HS256)** : Authentification Customer/Order services
- **CORS** : Configuration pour applications web
- **Health checks** : Monitoring automatique avec failover

### Résilience et performance
- **Load balancing** : Least-connections pour Cart Service (3 instances)
- **Circuit breakers** : Isolation des pannes via Kong
- **Database isolation** : Pattern Database per Service
- **Distributed tracing** : Correlation IDs pour traçabilité

## Structure du projet microservices avec saga

```
.
├── microservices/                    # Architecture microservices Lab 6 avec saga
│   ├── product-service/              # Service catalogue produits
│   ├── inventory-service/            # Service gestion stocks avec réservations
│   ├── sales-service/                # Service transactions POS
│   ├── reporting-service/            # Service analytics
│   ├── customer-service/             # Service comptes clients
│   ├── cart-service/                 # Service panier e-commerce (3 instances)
│   ├── order-service/                # Service commandes
│   ├── saga-orchestrator/            # Service orchestration saga (nouveau)
│   ├── payment-service/              # Service paiements avec simulation (nouveau)
│   ├── api-gateway/                  # Configuration Kong Gateway
│   ├── monitoring/                   # Prometheus/Grafana avec métriques saga
│   ├── scripts/                      # Scripts saga et tests de compensation
│   └── docker-compose.yml            # Orchestration 9 microservices
├── docs/                             # Documentation et diagrammes UML
│   ├── uml/                          # Diagrammes saga state machine
│   └── adr/                          # Architecture Decision Records
├── load_tests/k6/                    # Tests de performance et load balancing
├── src/                              # Code legacy monolithique (conservé)
└── RAPPORT_LAB6.md                   # Rapport saga orchestration complet
```

## Documentation

### Architecture et décisions
- **[Rapport d'architecture Arc42](lab5-architecture-report.md)** - Vue d'ensemble complète
- **[Comparaison Lab 4 vs Lab 5](LAB4-vs-LAB5-COMPARISON.md)** - Tests équitables et métriques
- **[ADR Kong Gateway](lab5-architecture-report.md#adr-001)** - Justification choix API Gateway
- **[ADR Database per Service](lab5-architecture-report.md#adr-002)** - Isolation des données

### Guides techniques
- **[Étape 1 - Microservices](microservices/README-Etape1.md)** - Décomposition en 7 services
- **[Étape 2 - API Gateway](microservices/README-Etape2.md)** - Kong avec sécurité
- **[Étape 3 - Load Balancing](microservices/README-Etape3.md)** - Multi-instances et tests

### Observabilité et monitoring
- **[Comparaison observabilité](microservices/lab5-observabilite-comparaison.md)** - Métriques détaillées
- **Grafana Kong Dashboard** : http://localhost:3000/d/kong-api-gateway-dashboard
- **Prometheus Metrics** : http://localhost:9090/targets
