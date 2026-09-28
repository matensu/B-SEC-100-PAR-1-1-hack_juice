# Login Jim

> **Category :** Injection  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Reconnaissance
   - Outil : Navigateur
   - Description : Identification du formulaire de connexion comme point d'entrée potentiel. Analyse du comportement de l'application lors de la soumission des identifiants.

2. **Exploitation de la faille**
   - Technique : Injection SQL (Authentication Bypass)
   - Outil : Navigateur
   - Description : Injection d'un payload SQL dans le champ email pour contourner l'authentification et se connecter en tant que l'utilisateur Bender sans connaître son mot de passe.

| Partie | Rôle |
|--------|------|
| `jim@juice-sh.op` | Adresse email valide de l'utilisateur ciblé |
| `'` | Ferme la chaîne de caractères du champ email dans la requête SQL |
| `--` | Commente le reste de la requête SQL (notamment la vérification du mot de passe) |

## Vulnerabilities

**Vulnerability :** Injection SQL - Contournement d'authentification

**Components :** Système d'authentification, Base de données

**Severity level :** Critique

## Risks

- Accès non autorisé aux comptes utilisateurs sans mot de passe
- Escalade de privilèges si un compte administrateur est ciblé
- Fuite de données personnelles (historique de commandes, moyens de paiement, informations personnelles)
- Atteinte à la réputation de l'entreprise

## Actions

**Risks mitigation strategies :**
- Implémenter un WAF (Web Application Firewall) pour détecter les patterns d'injection SQL
- Limiter les tentatives de connexion échouées

**Remediation fixes :**
Utiliser des requêtes paramétrées (Prepared Statements) :
```javascript
const query = "SELECT * FROM Users WHERE email = ? AND password = ?";
db.get(query, [req.body.email, req.body.password], callback);
```

**Security practice :**
- Échapper ou rejeter les caractères spéciaux (`'`, `--`) dans les champs de saisie
- Ne jamais afficher les erreurs SQL détaillées à l'utilisateur
- Effectuer des audits de sécurité réguliers sur les formulaires d'authentification

## Evidences and artifacts

**Payload utilisé :**
```
Email: jim@juice-sh.op'--
Password: [n'importe quelle valeur]
```
