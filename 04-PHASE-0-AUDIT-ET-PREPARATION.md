# 🔍 SECTION 4 : PHASE 0 - AUDIT ET PRÉPARATION

> **🎯 Objectif** : Inventorier tous vos datasets Power BI connectés à Snowflake et vérifier vos accès.
> **⏱️ Durée estimée** : 5 jours ouvrés

---

## 4.1 VÉRIFIER VOS DROITS SNOWFLAKE

### 📋 Droits nécessaires

Pour configurer OAuth dans Snowflake, vous avez besoin du rôle **ACCOUNTADMIN** ou **SECURITYADMIN**.

### ✅ Comment vérifier

**Étape 1 : Se connecter à Snowflake**

1. Ouvrez votre navigateur
2. Allez sur : `https://[votre-account].snowflakecomputing.com`
3. Connectez-vous avec vos identifiants actuels

**Étape 2 : Ouvrir un Worksheet**

1. Cliquez sur "Worksheets" dans le menu de gauche
2. Cliquez sur "+ Worksheet" (bouton en haut à droite)

**Étape 3 : Vérifier votre rôle actuel**

Copiez et exécutez cette requête :

```sql
-- Vérifier mon rôle actuel
SELECT CURRENT_ROLE() AS mon_role_actuel;
```

**Étape 4 : Lister tous vos rôles disponibles**

```sql
-- Voir tous les rôles que je peux utiliser
SHOW GRANTS TO USER CURRENT_USER();
```

**Étape 5 : Basculer vers ACCOUNTADMIN (si disponible)**

```sql
-- Passer en mode ACCOUNTADMIN
USE ROLE ACCOUNTADMIN;

-- Vérifier que ça a fonctionné
SELECT CURRENT_ROLE();
```

### ❌ Si vous n'avez PAS le rôle nécessaire

**Template d'email à votre admin Snowflake :**

```
Objet : Demande de droits ACCOUNTADMIN pour migration OAuth Power BI

Bonjour [Nom Admin],

Dans le cadre de la migration obligatoire vers OAuth 2.0 pour nos connexions 
Power BI-Snowflake (échéance novembre 2025), j'ai besoin des droits suivants :

- Rôle : ACCOUNTADMIN ou SECURITYADMIN
- Raison : Créer une Security Integration OAuth pour Microsoft Entra ID
- Durée : Temporaire (le temps de la configuration initiale - 2 jours)

Actions que je vais effectuer :
1. Créer une EXTERNAL OAUTH INTEGRATION
2. Configurer le mapping des utilisateurs (LOGIN_NAME)
3. Tester la connexion OAuth

Pouvez-vous me donner ces droits ou programmer un créneau pour le faire ensemble ?

Merci,
[Votre nom]
```

---

## 4.2 VÉRIFIER VOS DROITS AZURE AD

### 📋 Droits nécessaires

Pour créer une App Registration dans Microsoft Entra ID, vous avez besoin :
- **Application Administrator** OU
- **Cloud Application Administrator** OU
- **Global Administrator**

### ✅ Comment vérifier

**Étape 1 : Accéder au portail Azure**

1. Ouvrez : https://portal.azure.com
2. Connectez-vous avec votre compte Microsoft professionnel

**Étape 2 : Trouver Microsoft Entra ID**

1. Dans la barre de recherche en haut, tapez : "Entra"
2. Cliquez sur "Microsoft Entra ID"

**Étape 3 : Noter votre Tenant ID**

1. Dans la page d'accueil Entra ID, regardez "Overview"
2. Notez le **Tenant ID** (format UUID)
   - Exemple : `12345678-90ab-cdef-1234-567890abcdef`

**Créez un fichier texte** `oauth-config.txt` et notez :
```
TENANT ID : 12345678-90ab-cdef-1234-567890abcdef
```

**Étape 4 : Vérifier accès aux App Registrations**

1. Dans le menu de gauche, cliquez sur "App registrations"
2. Cherchez le bouton "+ New registration" en haut
3. **Si vous le voyez** ✅ : Vous avez les droits !
4. **Si vous ne le voyez pas** ❌ : Vous devez demander les droits

### ❌ Si vous n'avez PAS les droits

**Template d'email à votre admin Azure :**

```
Objet : Demande de droits App Registration pour migration OAuth Snowflake

Bonjour [Nom Admin Azure],

Pour la migration de nos 800 rapports Power BI vers OAuth 2.0 avec Snowflake,
j'ai besoin de créer une App Registration dans Microsoft Entra ID.

Droits nécessaires :
- Application Administrator (ou équivalent)
- Pour créer et gérer une App Registration

Que vais-je faire :
1. Créer une App Registration nommée "PowerBI-Snowflake-OAuth"
2. Générer un Client Secret
3. Configurer les permissions API pour Power BI et Snowflake

Alternative : Si vous préférez le faire vous-même, je peux vous fournir 
les instructions détaillées.

Pouvons-nous planifier cela cette semaine ?

Merci,
[Votre nom]
```

---

## 4.3 VÉRIFIER VOS DROITS POWER BI

### 📋 Droit nécessaire

**Power BI Administrator** (pour activer le SSO au niveau tenant)

### ✅ Comment vérifier

**Étape 1 : Se connecter à Power BI Service**

1. Ouvrez : https://app.powerbi.com
2. Connectez-vous

**Étape 2 : Vérifier accès au portail admin**

1. Cliquez sur l'icône ⚙️ (Settings) en haut à droite
2. Cherchez "Admin portal" dans le menu déroulant
3. **Si vous le voyez** ✅ : Vous êtes admin !
4. **Si absent** ❌ : Vous n'êtes pas admin

**Étape 3 : Vérifier le paramètre SSO Snowflake**

Si vous avez accès :
1. Admin portal → Tenant settings
2. Cherchez "Azure AD Single Sign-On (SSO) for Snowflake"
3. Notez si c'est activé ou non

---

## 4.4 INVENTAIRE DES DATASETS SNOWFLAKE

### 🎯 Objectif

Identifier **TOUS** les datasets Power BI connectés à Snowflake.

### 📊 MÉTHODE 1 : Manuelle via Power BI Service (RECOMMANDÉE)

**Temps estimé** : 2-4 heures pour 50 workspaces

**Template Excel à créer** : `inventaire-datasets-snowflake.xlsx`

**Colonnes** :

| Workspace | Dataset | Serveur Snowflake | Warehouse | Mode | Auth Actuelle | Rapports | Criticité | Propriétaire | Refresh Planifié |
|-----------|---------|-------------------|-----------|------|---------------|----------|-----------|--------------|------------------|

**Procédure étape par étape :**

**1. Lister tous les workspaces**

1. Sur app.powerbi.com
2. Menu de gauche : "Workspaces"
3. Notez tous les workspaces

**2. Pour CHAQUE workspace :**

1. Ouvrez le workspace
2. Identifiez les **datasets** (icône 📊 avec 3 barres)
3. Pour chaque dataset :
   - Cliquez sur "..." (More options)
   - Cliquez sur "Settings"
   - Section "Data source credentials"
   - **Regardez si "Snowflake" est mentionné**

**3. Si c'est un dataset Snowflake :**

Notez dans votre Excel :
- Nom du dataset
- Serveur Snowflake (ex: abc123.snowflakecomputing.com)
- Warehouse utilisé
- Mode : DirectQuery ou Import
- Méthode auth actuelle : Basic (username/pwd) ou OAuth2
- Nombre de rapports associés (voir "Related reports")
- Criticité : 🔴 Critique / 🟡 Important / 🟢 Standard
- Propriétaire (qui a créé)
- Refresh planifié ? (Oui/Non + fréquence)

**4. Évaluer la criticité**

| Critère | 🔴 Critique | 🟡 Important | 🟢 Standard |
|---------|-------------|--------------|-------------|
| Utilisateurs | >50 | 10-50 | <10 |
| Fréquence refresh | Horaire | Quotidien | Hebdo |
| Impact métier | Direction | Managers | Équipes |

---

### 📊 MÉTHODE 2 : Via Power BI REST API (AVANCÉ)

Si vous maîtrisez PowerShell, utilisez ce script :

```powershell
# Script d'inventaire automatique (optionnel)
# Voir scripts/powerbi-inventory.ps1
```

---

## 4.5 PRIORISATION

### 🎯 Créer 3 groupes

**Groupe PILOTE** (3-5 datasets)
- Critères : Non-critique, peu d'utilisateurs, facile à tester
- Objectif : Valider la configuration OAuth
- Timing : Semaine 1 après Phase 1

**Groupe PRIORITAIRE** (20-30% des datasets)
- Critères : Critique, beaucoup d'utilisateurs
- Objectif : Migrer les plus importants rapidement
- Timing : Semaines 2-4

**Groupe STANDARD** (70% restants)
- Critères : Tout le reste
- Objectif : Migration progressive
- Timing : Semaines 5-12

---

## 4.6 CHECKLIST PHASE 0

```
☐ Vérifié droits Snowflake (ACCOUNTADMIN)
☐ Vérifié droits Azure AD (App Administrator)
☐ Vérifié droits Power BI (Admin portal)
☐ Noté Tenant ID Azure
☐ Créé fichier oauth-config.txt
☐ Créé Excel inventaire-datasets-snowflake.xlsx
☐ Inventorié tous les workspaces Power BI
☐ Identifié tous les datasets Snowflake
☐ Évalué la criticité de chaque dataset
☐ Créé les 3 groupes (Pilote, Prioritaire, Standard)
☐ Validé l'inventaire avec l'équipe
```

---

**⏱️ DURÉE TOTALE PHASE 0 : 5 jours**

**➡️ [Suivant : Section 5 - Phase 1 Configuration OAuth](05-PHASE-1-CONFIGURATION-OAUTH.md)**