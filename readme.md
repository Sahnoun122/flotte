# Fleet Management Platform

## README Technique du Projet

Architecture technique professionnelle pour une **Plateforme Digitale de Gestion de Flotte** construite avec :

* **Backend** : NestJS
* **Admin Dashboard** : React
* **Mobile Driver App** : React Native
* **Database** : MongoDB
* **ORM** : Prisma
* **Object Storage** : MinIO

---

## 1. Présentation du projet

La plateforme de gestion de flotte est un système digital conçu pour centraliser, automatiser et superviser toutes les opérations liées à une flotte de véhicules.

Le système couvre principalement :

* la gestion des utilisateurs
* la gestion des conducteurs
* la gestion des véhicules
* le suivi des sessions de travail
* le suivi du carburant
* la gestion des livraisons
* la déclaration des incidents
* le suivi de la maintenance
* les alertes automatiques
* les rapports et indicateurs
* l’audit des actions

L’objectif est de disposer d’une plateforme moderne, modulaire, sécurisée et scalable.

---

## 2. Stack technique principale

### Backend

* **NestJS**
* **Prisma ORM**
* **MongoDB**
* **JWT Authentication**
* **MinIO SDK**
* **Class Validator / DTO Validation**
* **Swagger**

### Admin Dashboard

* **React.js**
* **React Router**
* **Axios**
* **Redux Toolkit** ou **Context API**
* **React Hook Form**

### Mobile Driver App

* **React Native**
* **React Navigation**
* **AsyncStorage / Secure Storage**
* **Camera / GPS / Signature Capture**
* **Offline Sync Strategy**

### Infrastructure

* **MongoDB** pour les données métier
* **Prisma** comme couche d’accès aux données
* **MinIO** pour le stockage objet
* **REST API** pour la communication
* **Notification Services** pour alertes et événements

---

## 3. High-Level System Architecture

```text
Driver Mobile App (React Native)
           |
           | REST API
           v
Backend API (NestJS)
           |
           |--- Prisma ORM
           |
           |--- MongoDB (Operational Data)
           |
           |--- MinIO (Images, Documents, Proof Files)
           |
           |--- Notification Services
           v
Admin Dashboard (React)
```

Le backend NestJS représente le centre de l’architecture. Il orchestre la logique métier, la sécurité, l’accès aux données et les intégrations externes.

---

## 4. Architecture globale du monorepo

Une structure monorepo professionnelle permet d’organiser clairement toutes les parties du projet.

```text
fleet-management-platform/
│
├── backend/
├── admin-dashboard/
├── mobile-driver/
├── docs/
├── scripts/
├── docker-compose.yml
├── .gitignore
├── .editorconfig
├── .env.example
├── package.json
└── README.md
```

### Description des dossiers racine

* **backend/** : API NestJS et logique métier.
* **admin-dashboard/** : interface web d’administration en React.
* **mobile-driver/** : application mobile du conducteur en React Native.
* **docs/** : documentation technique, architecture, API, database, déploiement.
* **scripts/** : scripts utilitaires de setup, seed, backup, quality et déploiement.

---

## 5. Architecture détaillée du Backend (NestJS)

Le backend doit être structuré en modules NestJS par domaine métier.

### 5.1 Structure complète du backend

```text
backend/
│
├── prisma/
│   ├── schema.prisma
│   ├── seed.ts
│   └── migrations/
│
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   ├── app.controller.ts
│   ├── app.service.ts
│   │
│   ├── config/
│   │   ├── app.config.ts
│   │   ├── database.config.ts
│   │   ├── jwt.config.ts
│   │   ├── minio.config.ts
│   │   ├── swagger.config.ts
│   │   └── env.validation.ts
│   │
│   ├── common/
│   │   ├── decorators/
│   │   ├── dto/
│   │   ├── enums/
│   │   ├── exceptions/
│   │   ├── filters/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── middlewares/
│   │   ├── pipes/
│   │   ├── types/
│   │   ├── utils/
│   │   └── constants/
│   │
│   ├── database/
│   │   ├── prisma.module.ts
│   │   ├── prisma.service.ts
│   │   └── repositories/
│   │
│   ├── integrations/
│   │   ├── minio/
│   │   │   ├── minio.module.ts
│   │   │   ├── minio.service.ts
│   │   │   └── minio.types.ts
│   │   │
│   │   ├── notifications/
│   │   │   ├── notifications.module.ts
│   │   │   ├── notifications.service.ts
│   │   │   └── channels/
│   │   │
│   │   ├── gps/
│   │   └── qr/
│   │
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── dto/
│   │   │   ├── strategies/
│   │   │   ├── guards/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.module.ts
│   │   │   └── auth.repository.ts
│   │   │
│   │   ├── users/
│   │   │   ├── dto/
│   │   │   ├── users.controller.ts
│   │   │   ├── users.service.ts
│   │   │   ├── users.module.ts
│   │   │   └── users.repository.ts
│   │   │
│   │   ├── drivers/
│   │   ├── vehicles/
│   │   ├── work-sessions/
│   │   ├── fuel-logs/
│   │   ├── deliveries/
│   │   ├── incidents/
│   │   ├── maintenance/
│   │   ├── alerts/
│   │   ├── reports/
│   │   ├── analytics/
│   │   └── audit-logs/
│   │
│   ├── jobs/
│   │   ├── cron/
│   │   ├── queues/
│   │   ├── processors/
│   │   └── schedulers/
│   │
│   └── docs/
│       └── swagger/
│
├── test/
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   └── fixtures/
│
├── .env
├── .env.example
├── nest-cli.json
├── package.json
├── tsconfig.json
├── tsconfig.build.json
└── README.md
```

---

## 6. Rôle des dossiers Backend

### `prisma/`

Contient toute la couche Prisma :

* `schema.prisma` pour le modèle de données
* `seed.ts` pour injecter les données initiales
* `migrations/` pour l’évolution contrôlée du schéma

### `src/config/`

Contient les configurations techniques de l’application :

* variables d’environnement
* connexion base de données
* configuration JWT
* configuration MinIO
* validation d’environnement
* documentation Swagger

### `src/common/`

Contient les éléments réutilisables dans tout le backend :

* decorators personnalisés
* guards d’authentification
* filters de gestion d’erreurs
* interceptors
* pipes de validation
* enums et constantes
* utilitaires partagés

### `src/database/`

Contient la couche d’accès à la base via Prisma :

* `prisma.module.ts`
* `prisma.service.ts`
* repositories génériques ou spécifiques

### `src/integrations/`

Contient les connexions aux services externes :

* MinIO
* notifications
* GPS
* QR code

### `src/modules/`

Contient chaque domaine métier sous forme de module NestJS isolé.

### `src/jobs/`

Contient les traitements différés et planifiés :

* tâches CRON
* jobs asynchrones
* traitements lourds
* alertes automatiques
* génération de rapports

---

## 7. Structure recommandée d’un module NestJS

Chaque module métier doit garder la même structure pour assurer la cohérence du projet.

### Exemple générique

```text
modules/
└── vehicles/
    ├── dto/
    │   ├── create-vehicle.dto.ts
    │   ├── update-vehicle.dto.ts
    │   └── vehicle-query.dto.ts
    │
    ├── entities/
    │   └── vehicle.entity.ts
    │
    ├── vehicles.controller.ts
    ├── vehicles.service.ts
    ├── vehicles.module.ts
    ├── vehicles.repository.ts
    ├── vehicles.mapper.ts
    ├── vehicles.constants.ts
    └── vehicles.policy.ts
```

### Rôle de chaque fichier

* **dto/** : définit les payloads d’entrée et leurs validations.
* **entities/** : représente les objets exposés ou la forme métier.
* **controller** : reçoit les requêtes HTTP.
* **service** : contient la logique métier principale.
* **module** : déclare les providers et dépendances NestJS.
* **repository** : centralise l’accès Prisma à la base.
* **mapper** : transforme les données base → réponse API.
* **constants** : constantes du module.
* **policy** : règles d’accès spécifiques au module.

### Exemple pour `auth`

```text
modules/
└── auth/
    ├── dto/
    │   ├── login.dto.ts
    │   ├── register.dto.ts
    │   └── refresh-token.dto.ts
    │
    ├── strategies/
    │   ├── jwt.strategy.ts
    │   └── local.strategy.ts
    │
    ├── guards/
    │   ├── jwt-auth.guard.ts
    │   └── local-auth.guard.ts
    │
    ├── auth.controller.ts
    ├── auth.service.ts
    ├── auth.module.ts
    ├── auth.repository.ts
    ├── auth.tokens.ts
    └── auth.constants.ts
```

### Exemple pour `deliveries`

```text
modules/
└── deliveries/
    ├── dto/
    │   ├── create-delivery.dto.ts
    │   ├── update-delivery-status.dto.ts
    │   └── confirm-delivery.dto.ts
    │
    ├── entities/
    │   └── delivery.entity.ts
    │
    ├── deliveries.controller.ts
    ├── deliveries.service.ts
    ├── deliveries.module.ts
    ├── deliveries.repository.ts
    ├── deliveries.mapper.ts
    ├── delivery-proof.service.ts
    └── deliveries.constants.ts
```

---

## 8. Modules métier recommandés côté Backend

Le backend doit être séparé en modules clairs.

### Authentication Module

Responsable de :

* login
* refresh token
* logout
* sécurisation JWT
* rôles et permissions

### Users Module

Responsable de :

* gestion des administrateurs
* gestion des gestionnaires flotte
* activation / désactivation comptes

### Drivers Module

Responsable de :

* profils conducteurs
* permis de conduire
* documents du conducteur
* affectation du véhicule

### Vehicles Module

Responsable de :

* informations véhicule
* immatriculation
* statut opérationnel
* documents véhicule
* kilométrage

### Work Sessions Module

Responsable de :

* check-in / check-out
* scan QR code
* géolocalisation début / fin
* durée de session

### Fuel Logs Module

Responsable de :

* déclaration carburant
* ticket carburant photo
* kilométrage
* détection d’anomalies

### Deliveries Module

Responsable de :

* missions de livraison
* confirmation de livraison
* preuve photo
* signature client
* géolocalisation

### Incidents Module

Responsable de :

* déclaration incident
* photos incident
* niveau de gravité
* suivi de résolution

### Maintenance Module

Responsable de :

* maintenance préventive
* maintenance corrective
* historique interventions
* coûts

### Alerts Module

Responsable de :

* génération d’alertes
* priorisation
* statut d’alerte

### Reports Module

Responsable de :

* export PDF / Excel
* génération de rapports filtrés

### Analytics Module

Responsable de :

* KPI globaux
* métriques carburant
* performance livraison
* statistiques incidents

### Audit Logs Module

Responsable de :

* historique des actions
* traçabilité sécurité
* logs administratifs

---

## 9. Database Design avec Prisma + MongoDB

Le contenu explicatif reste en français, mais les noms techniques de base de données restent en anglais.

### 9.1 Prisma Schema Overview

Le fichier `schema.prisma` centralise toute la définition des modèles.

Exemple de configuration :

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mongodb"
  url      = env("DATABASE_URL")
}
```

---

## 10. Database Models (English naming)

### `User`

```prisma
model User {
  id           String   @id @default(auto()) @map("_id") @db.ObjectId
  fullName     String
  email        String   @unique
  passwordHash String
  role         String
  status       String
  phoneNumber  String?
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
}
```

### `Vehicle`

```prisma
model Vehicle {
  id                          String   @id @default(auto()) @map("_id") @db.ObjectId
  registrationNumber          String   @unique
  brand                       String
  model                       String
  vehicleType                 String
  fuelType                    String
  status                      String
  odometer                    Float?
  year                        Int?
  vin                         String?
  insuranceExpiryDate         DateTime?
  technicalInspectionExpiryDate DateTime?
  createdAt                   DateTime @default(now())
  updatedAt                   DateTime @updatedAt
}
```

### `Driver`

```prisma
model Driver {
  id                String   @id @default(auto()) @map("_id") @db.ObjectId
  userId            String   @db.ObjectId
  licenseNumber     String
  licenseCategory   String?
  licenseExpiryDate DateTime?
  assignedVehicleId String?  @db.ObjectId
  status            String
  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
}
```

### `WorkSession`

```prisma
model WorkSession {
  id              String   @id @default(auto()) @map("_id") @db.ObjectId
  driverId        String   @db.ObjectId
  vehicleId       String   @db.ObjectId
  startTime       DateTime
  endTime         DateTime?
  startLatitude   Float?
  startLongitude  Float?
  endLatitude     Float?
  endLongitude    Float?
  qrCodeScanned   Boolean  @default(false)
  durationMinutes Int?
  status          String
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
}
```

### `FuelLog`

```prisma
model FuelLog {
  id              String   @id @default(auto()) @map("_id") @db.ObjectId
  vehicleId       String   @db.ObjectId
  driverId        String   @db.ObjectId
  amount          Float
  litres          Float
  odometer        Float
  receiptImageUrl String?
  latitude        Float?
  longitude       Float?
  anomalyFlag     Boolean  @default(false)
  notes           String?
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
}
```

### `Delivery`

```prisma
model Delivery {
  id                String   @id @default(auto()) @map("_id") @db.ObjectId
  driverId          String   @db.ObjectId
  vehicleId         String   @db.ObjectId
  clientId          String?  @db.ObjectId
  deliveryStatus    String
  proofImageUrl     String?
  signatureImageUrl String?
  latitude          Float?
  longitude         Float?
  deliveredAt       DateTime?
  notes             String?
  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
}
```

### `Incident`

```prisma
model Incident {
  id          String   @id @default(auto()) @map("_id") @db.ObjectId
  vehicleId   String   @db.ObjectId
  driverId    String   @db.ObjectId
  type        String
  description String
  imageUrl    String?
  latitude    Float?
  longitude   Float?
  severity    String
  status      String
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

### `MaintenanceLog`

```prisma
model MaintenanceLog {
  id                  String   @id @default(auto()) @map("_id") @db.ObjectId
  vehicleId           String   @db.ObjectId
  interventionType    String
  cost                Float?
  date                DateTime
  notes               String?
  serviceProvider     String?
  nextMaintenanceDate DateTime?
  createdAt           DateTime @default(now())
  updatedAt           DateTime @updatedAt
}
```

### `Alert`

```prisma
model Alert {
  id              String   @id @default(auto()) @map("_id") @db.ObjectId
  type            String
  message         String
  level           String
  status          String
  relatedEntity   String?
  relatedEntityId String?  @db.ObjectId
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
}
```

### `AuditLog`

```prisma
model AuditLog {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  userId    String   @db.ObjectId
  action    String
  entity    String
  entityId  String?  @db.ObjectId
  timestamp DateTime @default(now())
  details   String?
}
```

---

## 11. Relations métier recommandées

Même si MongoDB avec Prisma ne fonctionne pas exactement comme une base SQL relationnelle, il faut garder une logique claire de liaison applicative.

* `Driver.userId` → `User.id`
* `Driver.assignedVehicleId` → `Vehicle.id`
* `WorkSession.driverId` → `Driver.id`
* `WorkSession.vehicleId` → `Vehicle.id`
* `FuelLog.driverId` → `Driver.id`
* `FuelLog.vehicleId` → `Vehicle.id`
* `Delivery.driverId` → `Driver.id`
* `Delivery.vehicleId` → `Vehicle.id`
* `Incident.driverId` → `Driver.id`
* `Incident.vehicleId` → `Vehicle.id`
* `MaintenanceLog.vehicleId` → `Vehicle.id`
* `AuditLog.userId` → `User.id`

---

## 12. Structure détaillée du Admin Dashboard (React)

Le dashboard d’administration doit être construit de manière modulaire, orientée fonctionnalités, avec séparation claire entre UI, logique métier, appels API et gestion d’état.

### 12.1 Structure complète du dashboard admin

```text
admin-dashboard/
│
├── public/
│   ├── favicon.ico
│   ├── logo.png
│   └── index.html
│
├── src/
│   ├── api/
│   │   ├── client.js
│   │   ├── endpoints.js
│   │   └── interceptors.js
│   │
│   ├── app/
│   │   ├── store.js
│   │   ├── providers.jsx
│   │   └── router.jsx
│   │
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── illustrations/
│   │
│   ├── components/
│   │   ├── common/
│   │   ├── layout/
│   │   ├── forms/
│   │   ├── tables/
│   │   ├── charts/
│   │   ├── modals/
│   │   └── feedback/
│   │
│   ├── features/
│   │   ├── auth/
│   │   ├── dashboard/
│   │   ├── users/
│   │   ├── drivers/
│   │   ├── vehicles/
│   │   ├── work-sessions/
│   │   ├── fuel-logs/
│   │   ├── deliveries/
│   │   ├── incidents/
│   │   ├── maintenance/
│   │   ├── alerts/
│   │   ├── reports/
│   │   ├── analytics/
│   │   └── settings/
│   │
│   ├── hooks/
│   │   ├── useAuth.js
│   │   ├── usePagination.js
│   │   ├── useFilters.js
│   │   └── usePermissions.js
│   │
│   ├── layouts/
│   │   ├── DashboardLayout.jsx
│   │   ├── AuthLayout.jsx
│   │   └── MinimalLayout.jsx
│   │
│   ├── pages/
│   │   ├── LoginPage.jsx
│   │   ├── DashboardPage.jsx
│   │   ├── UsersPage.jsx
│   │   ├── DriversPage.jsx
│   │   ├── VehiclesPage.jsx
│   │   ├── WorkSessionsPage.jsx
│   │   ├── FuelLogsPage.jsx
│   │   ├── DeliveriesPage.jsx
│   │   ├── IncidentsPage.jsx
│   │   ├── MaintenancePage.jsx
│   │   ├── AlertsPage.jsx
│   │   ├── ReportsPage.jsx
│   │   ├── AnalyticsPage.jsx
│   │   └── NotFoundPage.jsx
│   │
│   ├── routes/
│   │   ├── index.jsx
│   │   ├── ProtectedRoute.jsx
│   │   └── routeNames.js
│   │
│   ├── services/
│   │   ├── auth.service.js
│   │   ├── users.service.js
│   │   ├── drivers.service.js
│   │   ├── vehicles.service.js
│   │   ├── work-sessions.service.js
│   │   ├── fuel-logs.service.js
│   │   ├── deliveries.service.js
│   │   ├── incidents.service.js
│   │   ├── maintenance.service.js
│   │   ├── alerts.service.js
│   │   ├── reports.service.js
│   │   └── analytics.service.js
│   │
│   ├── store/
│   │   ├── slices/
│   │   ├── selectors/
│   │   └── middleware/
│   │
│   ├── styles/
│   │   ├── globals.css
│   │   ├── variables.css
│   │   └── theme.css
│   │
│   ├── utils/
│   │   ├── date.js
│   │   ├── formatters.js
│   │   ├── validators.js
│   │   └── export.js
│   │
│   ├── App.jsx
│   └── main.jsx
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── .env
├── package.json
├── vite.config.js
└── README.md
```

---

## 13. Structure interne recommandée d’une feature React Admin

Exemple pour `vehicles` :

```text
features/
└── vehicles/
    ├── components/
    │   ├── VehicleTable.jsx
    │   ├── VehicleForm.jsx
    │   ├── VehicleFilters.jsx
    │   └── VehicleStatusBadge.jsx
    │
    ├── hooks/
    │   ├── useVehicles.js
    │   └── useVehicleDetails.js
    │
    ├── pages/
    │   ├── VehiclesListPage.jsx
    │   └── VehicleDetailsPage.jsx
    │
    ├── services/
    │   └── vehicles.api.js
    │
    ├── store/
    │   ├── vehicles.slice.js
    │   └── vehicles.selectors.js
    │
    ├── utils/
    │   └── vehicles.helpers.js
    │
    ├── constants.js
    └── index.js
```

---

## 14. Structure détaillée du Mobile Driver App (React Native)

L’application mobile du conducteur doit être pensée pour les usages terrain : GPS, photo, signature, offline mode, synchronisation différée et simplicité d’exécution.

### 14.1 Structure complète du mobile driver app

```text
mobile-driver/
│
├── src/
│   ├── api/
│   │   ├── client.js
│   │   ├── endpoints.js
│   │   └── interceptors.js
│   │
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── fonts/
│   │
│   ├── components/
│   │   ├── common/
│   │   ├── forms/
│   │   ├── cards/
│   │   ├── feedback/
│   │   └── modals/
│   │
│   ├── features/
│   │   ├── auth/
│   │   ├── profile/
│   │   ├── work-sessions/
│   │   ├── fuel-logs/
│   │   ├── deliveries/
│   │   ├── incidents/
│   │   └── maintenance/
│   │
│   ├── hooks/
│   │   ├── useAuth.js
│   │   ├── useCurrentLocation.js
│   │   ├── useCamera.js
│   │   ├── useSignatureCapture.js
│   │   └── useOfflineQueue.js
│   │
│   ├── navigation/
│   │   ├── AppNavigator.js
│   │   ├── AuthNavigator.js
│   │   ├── MainNavigator.js
│   │   └── routeNames.js
│   │
│   ├── screens/
│   │   ├── auth/
│   │   │   └── LoginScreen.js
│   │   │
│   │   ├── home/
│   │   │   └── HomeScreen.js
│   │   │
│   │   ├── profile/
│   │   ├── work-sessions/
│   │   ├── fuel-logs/
│   │   ├── deliveries/
│   │   ├── incidents/
│   │   └── maintenance/
│   │
│   ├── services/
│   │   ├── auth.service.js
│   │   ├── location.service.js
│   │   ├── camera.service.js
│   │   ├── signature.service.js
│   │   ├── sync.service.js
│   │   └── storage.service.js
│   │
│   ├── storage/
│   │   ├── async-storage.js
│   │   ├── secure-storage.js
│   │   ├── offline-queue.js
│   │   └── cache.js
│   │
│   ├── styles/
│   │   ├── colors.js
│   │   ├── spacing.js
│   │   ├── typography.js
│   │   └── theme.js
│   │
│   ├── utils/
│   │   ├── formatters.js
│   │   ├── validators.js
│   │   ├── permissions.js
│   │   └── network.js
│   │
│   ├── App.js
│   └── index.js
│
├── android/
├── ios/
├── .env
├── package.json
└── README.md
```

---

## 15. Structure interne recommandée d’une feature mobile

Exemple pour `deliveries` :

```text
features/
└── deliveries/
    ├── components/
    │   ├── DeliveryCard.js
    │   ├── DeliveryStatusBadge.js
    │   ├── DeliveryProofForm.js
    │   └── SignaturePad.js
    │
    ├── hooks/
    │   ├── useDeliveries.js
    │   └── useConfirmDelivery.js
    │
    ├── screens/
    │   ├── DeliveriesListScreen.js
    │   ├── DeliveryDetailsScreen.js
    │   └── ConfirmDeliveryScreen.js
    │
    ├── services/
    │   └── deliveries.api.js
    │
    ├── storage/
    │   └── deliveries.queue.js
    │
    ├── utils/
    │   └── deliveries.helpers.js
    │
    ├── constants.js
    └── index.js
```

---

## 16. Structure du dossier `docs`

```text
docs/
├── architecture/
│   ├── system-overview.md
│   ├── backend-nestjs-architecture.md
│   ├── admin-react-architecture.md
│   ├── mobile-driver-architecture.md
│   └── security-architecture.md
│
├── api/
│   ├── endpoints.md
│   ├── authentication.md
│   └── error-handling.md
│
├── database/
│   ├── prisma-schema.md
│   ├── collections.md
│   ├── indexes.md
│   └── relationships.md
│
├── diagrams/
│   ├── high-level-architecture.png
│   ├── delivery-flow.png
│   └── database-schema.png
│
└── deployment/
    ├── local-setup.md
    ├── staging.md
    └── production.md
```

---

## 17. Structure du dossier `scripts`

```text
scripts/
├── setup/
│   ├── init-project.sh
│   └── install-deps.sh
│
├── database/
│   ├── seed.ts
│   ├── reset.ts
│   └── backup.ts
│
├── deployment/
│   ├── build-admin.sh
│   ├── deploy-backend.sh
│   └── deploy-mobile.sh
│
└── quality/
    ├── lint.sh
    ├── test.sh
    └── format.sh
```

---

## 18. Organisation recommandée dans MinIO

```text
fleet-files/
├── drivers/
│   ├── licenses/
│   ├── identities/
│   └── profile-images/
│
├── vehicles/
│   ├── documents/
│   ├── inspections/
│   └── insurance/
│
├── fuel/
│   └── receipts/
│
├── deliveries/
│   ├── proof-images/
│   └── signatures/
│
└── incidents/
    └── images/
```

Exemples de nommage :

```text
fuel/receipts/{vehicleId}/{timestamp}-receipt.jpg
deliveries/signatures/{deliveryId}/{timestamp}-signature.png
incidents/images/{vehicleId}/{timestamp}-incident.jpg
```

---

## 19. API Routes recommandées

```text
/api/v1/auth
/api/v1/users
/api/v1/drivers
/api/v1/vehicles
/api/v1/work-sessions
/api/v1/fuel-logs
/api/v1/deliveries
/api/v1/incidents
/api/v1/maintenance
/api/v1/alerts
/api/v1/reports
/api/v1/analytics
/api/v1/audit-logs
```

Exemple CRUD :

```text
GET    /api/v1/vehicles
POST   /api/v1/vehicles
GET    /api/v1/vehicles/:id
PATCH  /api/v1/vehicles/:id
DELETE /api/v1/vehicles/:id
```

---

## 20. Bonnes pratiques de sécurité

* JWT access token + refresh token
* hash mot de passe avec bcrypt
* DTO validation avec class-validator
* guards NestJS pour routes protégées
* rôles et permissions
* sanitization des entrées
* rate limiting
* audit logs sur actions sensibles
* validation stricte des fichiers uploadés
* protection des accès fichiers MinIO

---

## 21. Recommandations de scalabilité

* séparer les modules par domaine métier
* garder les controllers légers
* centraliser la logique métier dans les services
* centraliser Prisma dans repositories ou services dédiés
* paginer toutes les grandes listes
* indexer les champs critiques MongoDB
* utiliser des jobs asynchrones pour tâches lourdes
* préparer une séparation future en microservices si la flotte grossit fortement

---

## 22. Indexes MongoDB recommandés

* `User.email`
* `Vehicle.registrationNumber`
* `Vehicle.status`
* `Driver.userId`
* `Driver.assignedVehicleId`
* `WorkSession.driverId + startTime`
* `FuelLog.vehicleId + createdAt`
* `Delivery.driverId + deliveryStatus`
* `Incident.vehicleId + createdAt`
* `Alert.status + level`
* `AuditLog.userId + timestamp`

---

## 23. Naming conventions recommandées

### Backend (NestJS)

* fichiers : `users.service.ts`, `vehicles.controller.ts`
* DTO : `create-user.dto.ts`
* modules : `users.module.ts`
* classes : `PascalCase`
* variables : `camelCase`
* constantes : `UPPER_SNAKE_CASE`

### Admin Dashboard (React)

* composants : `PascalCase`
* hooks : `useSomething`
* pages : `SomethingPage.jsx`
* services : `vehicles.service.js`

### Mobile Driver App

* écrans : `SomethingScreen.js`
* composants : `PascalCase`
* hooks : `useSomething`
* fichiers storage : `offline-queue.js`, `secure-storage.js`

---

## 24. Ordre professionnel de développement

### Phase 1 — Foundation

* initialiser monorepo
* créer backend NestJS
* connecter Prisma à MongoDB
* configurer MinIO
* créer admin React
* créer mobile React Native

### Phase 2 — Security Core

* Authentication Module
* Users Module
* rôles et permissions
* protection routes backend et frontend

### Phase 3 — Core Fleet Data

* Vehicles Module
* Drivers Module
* Work Sessions Module

### Phase 4 — Operational Modules

* Fuel Logs Module
* Deliveries Module
* Incidents Module
* Maintenance Module

### Phase 5 — Supervision

* Alerts Module
* Reports Module
* Analytics Module
* Audit Logs Module

### Phase 6 — Production Readiness

* tests unitaires
* tests intégration
* logs
* monitoring
* CI/CD
* optimisation performances
* sécurité avancée

---

## 25. Conclusion

Cette architecture permet :

* une organisation professionnelle du projet
* une séparation claire backend / admin / mobile
* une scalabilité correcte dès le départ
* une base solide pour le développement en équipe
* une meilleure maintenabilité du code
* une intégration propre entre NestJS, Prisma, MongoDB, React et React Native

---

## 26. Titre recommandé du README racine

**Fleet Management Platform – Technical Architecture, Prisma Database Design, Backend Structure, Admin Dashboard Structure, and Mobile Driver App Structure**
