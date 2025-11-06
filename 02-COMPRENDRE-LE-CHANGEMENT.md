# 📊 SECTION 2 : COMPRENDRE LE CHANGEMENT

> **🎯 Objectif** : Visualiser EXACTEMENT ce qui change entre l'ancienne et la nouvelle méthode.

---

## 2.1 SCHÉMA : ANCIENNE MÉTHODE (USERNAME/PASSWORD)

```
╔═══════════════════════════════════════════════════════════╗
║  FLUX D'AUTHENTIFICATION : USERNAME/PASSWORD              ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║   👤 UTILISATEUR → 💻 POWER BI → ❄️ SNOWFLAKE            ║
║                                                           ║
║   1. Ouvre le rapport                                     ║
║   2. Power BI lit les credentials stockés                 ║
║      (Username: jdupont, Password: MonMdp123)             ║
║   3. Power BI envoie à Snowflake                          ║
║   4. Snowflake vérifie le password                        ║
║   5. Si OK → Session établie                              ║
║   6. Power BI exécute les requêtes SQL                    ║
║   7. Snowflake retourne les données                       ║
║                                                           ║
║  ⚠️ PROBLÈMES :                                           ║
║  ❌ Password stocké dans Power BI                         ║
║  ❌ Si password change → Mise à jour manuelle             ║
║  ❌ Pas de MFA                                            ║
║  ❌ BLOQUÉ en Novembre 2025                               ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

---

## 2.2 SCHÉMA : NOUVELLE MÉTHODE (OAUTH 2.0)

```
╔═══════════════════════════════════════════════════════════╗
║  FLUX D'AUTHENTIFICATION : OAUTH 2.0                      ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║   👤 USER → 💻 POWER BI → 🔐 MICROSOFT → ❄️ SNOWFLAKE   ║
║                                                           ║
║   1. Utilisateur ouvre le rapport                         ║
║   2. Power BI vérifie : Token valide ?                    ║
║   3. Si expiré → Redirection vers Microsoft               ║
║   4. Microsoft demande : Login + MFA                      ║
║   5. Utilisateur s'authentifie (email + code SMS)         ║
║   6. Microsoft génère un Access Token (60 min)            ║
║   7. Power BI reçoit le token                             ║
║   8. Power BI envoie token à Snowflake                    ║
║   9. Snowflake vérifie le token auprès de Microsoft       ║
║  10. Si valide → Session établie                          ║
║  11. Power BI exécute les requêtes                        ║
║  12. Snowflake retourne les données                       ║
║                                                           ║
║  ✅ AVANTAGES :                                           ║
║  ✅ Pas de password stocké                                ║
║  ✅ MFA obligatoire                                       ║
║  ✅ Token auto-renouvelé                                  ║
║  ✅ Conforme Snowflake 2025                               ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

---

## 2.3 TABLEAU COMPARATIF DÉTAILLÉ

| Critère | Username/Password | OAuth 2.0 |
|---------|-------------------|-----------|
| **Sécurité** | ❌ Faible (password stocké) | ✅ Élevée (tokens temporaires) |
| **MFA** | ❌ Optionnel | ✅ Obligatoire |
| **Gestion utilisateurs** | ❌ Complexe (changer partout) | ✅ Centralisée (Azure AD) |
| **Audit** | ❌ Limité | ✅ Complet (tous les accès loggés) |
| **Expiration** | ⚠️ Manuelle | ✅ Automatique |
| **Conformité** | ❌ Non-conforme 2025 | ✅ Conforme standards |
| **SSO** | ❌ Non | ✅ Oui (une connexion = tout) |
| **Setup initial** | ✅ Simple (2 min) | ⚠️ Moyen (1-2 jours) |
| **Maintenance** | ❌ Élevée | ✅ Faible |
| **Snowflake Nov 2025** | ❌ BLOQUÉ | ✅ AUTORISÉ |

---

## 2.4 POURQUOI SNOWFLAKE IMPOSE CE CHANGEMENT

### 📅 Échéance : Novembre 2025

Snowflake rendra le **MFA obligatoire** pour toutes les connexions.

**Conséquences si non-migration :**

```
╔═══════════════════════════════════════════════════════════╗
║  SCÉNARIO : Novembre 2025 sans migration                  ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║  ❌ 800 rapports Power BI cessent de fonctionner          ║
║  ❌ Rafraîchissements automatiques échouent               ║
║  ❌ Dashboards affichent "Erreur de connexion"            ║
║  ❌ Perte d'accès aux données Snowflake                   ║
║                                                           ║
║  💰 Impact business :                                     ║
║  • Perte de productivité                                  ║
║  • Décisions business retardées                           ║
║  • Migration en urgence = coûts élevés                    ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

### 🔒 Raisons de Snowflake

1. **Sécurité** : 80% des violations impliquent des passwords faibles
2. **Conformité** : RGPD, SOC2, ISO 27001 exigent MFA
3. **Standard** : Microsoft, AWS, Google ont déjà migré
4. **Responsabilité** : Protection des données clients

---

## 2.5 IMPACT BUSINESS

### ✅ Scénario : Migration AVANT novembre 2025

- ✅ Transition planifiée et contrôlée
- ✅ Formation des utilisateurs
- ✅ Tests approfondis
- ✅ Aucune interruption de service
- ✅ Sécurité renforcée
- 🏆 Reconnaissance de l'anticipation

### ❌ Scénario : Pas de migration

- ❌ Panne majeure en novembre 2025
- ❌ Migration en urgence (2-3 semaines)
- ❌ Coûts exponentiels
- ❌ Perte de confiance
- 📉 Impact sur carrière/équipe

---

**➡️ [Suivant : Section 3 - Comptes Personnels vs Service](03-COMPTES-PERSONNELS-VS-SERVICE.md)**