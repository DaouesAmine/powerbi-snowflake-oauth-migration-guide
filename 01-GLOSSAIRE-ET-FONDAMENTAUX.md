# 📚 SECTION 1 : GLOSSAIRE ET FONDAMENTAUX

> **🎯 Objectif** : Comprendre TOUS les termes techniques de ce guide.

---

## 1.1 CONCEPTS D'AUTHENTIFICATION DE BASE

### 🔑 Authentification (Authentication)

**Définition simple** : Prouver qui vous êtes.

**Analogie** : Montrer votre carte d'identité à l'entrée d'un bâtiment sécurisé.

**Dans notre contexte** : Lorsque Power BI se connecte à Snowflake, il doit prouver qu'il est bien autorisé à accéder aux données.

```
┌─────────────────────────────────────────────────────────┐
│  AVANT (Username/Password)                              │
├─────────────────────────────────────────────────────────┤
│  Power BI dit : "Je suis Jean Dupont"                  │
│  Snowflake demande : "Quel est ton mot de passe ?"     │
│  Power BI répond : "MonMotDePasse123"                  │
│  Snowflake vérifie et dit : "OK, tu peux entrer"       │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  APRÈS (OAuth 2.0)                                      │
├─────────────────────────────────────────────────────────┤
│  Power BI dit : "Je veux accéder aux données"          │
│  Snowflake dit : "Va demander à Microsoft"             │
│  Power BI va voir Microsoft                            │
│  Microsoft demande : "Qui es-tu ?" (+ MFA)             │
│  Microsoft donne un jeton temporaire à Power BI        │
│  Power BI montre le jeton à Snowflake                  │
│  Snowflake vérifie le jeton et dit : "OK"              │
└─────────────────────────────────────────────────────────┘
```

---

### 🎫 OAuth 2.0

**Définition simple** : Protocole qui permet à une application d'accéder à vos données SANS connaître votre mot de passe.

**Analogie** : Un hôtel donne une carte temporaire au service de ménage au lieu de votre clé principale.

---

### 🎟️ Token (Jeton)

**Définition simple** : Badge numérique temporaire qui prouve que vous êtes autorisé.

| Type | Durée | Rôle |
|------|-------|------|
| Access Token | 15-60 min | Accéder aux données |
| Refresh Token | Jours/mois | Renouveler l'Access Token |

---

### 🏢 Microsoft Entra ID

**Définition simple** : Service de gestion d'identité Microsoft dans le cloud.

---

### 🔐 SSO (Single Sign-On)

**Définition simple** : Se connecter UNE FOIS pour accéder à TOUT.

---

### 🛡️ MFA (Multi-Factor Authentication)

**Définition simple** : Vérifier votre identité avec PLUSIEURS preuves.

**Les 3 facteurs** :
1. Ce que vous SAVEZ (mot de passe)
2. Ce que vous AVEZ (smartphone)
3. Ce que vous ÊTES (empreinte)

---

## 1.2 GLOSSAIRE EXPRESS

| Terme | Définition |
|-------|------------|
| Access Token | Badge temporaire donnant accès aux données |
| App Registration | Déclaration de votre app dans Azure AD |
| Client ID | Identifiant public de l'app (UUID) |
| Client Secret | Mot de passe secret de l'app |
| Dataset | Modèle de données Power BI |
| OAuth 2.0 | Protocole d'autorisation sans password |
| SSO | Single Sign-On |
| Tenant ID | ID unique de votre organisation Azure |
| Token | Badge numérique temporaire |
| UPN | User Principal Name (email pro) |

---

**➡️ [Suivant : Section 2](02-COMPRENDRE-LE-CHANGEMENT.md)**