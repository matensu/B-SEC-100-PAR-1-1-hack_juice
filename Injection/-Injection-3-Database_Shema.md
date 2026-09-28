# Database shema

> **Category :** Injection  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Énumération
   - Outil : Navigateur, DevTools
   - Description : Chercher un point d'entrée potentiel. Découverte de la requete GET `/rest/products/search?q=`.

2. **Exploitation de la faille**
   - Technique : Injection SQL (UNION-based)
   - Outil : Navigateur
   - Description : Mis en place d'une injection UNION directement dans l URL pour extraire le schéma de la base de données depuis la table systeme `sqlite_master`.

| Partie | Rôle |
|--------|------|
| `'))` | Ferme la requête originale  |
| `UNION SELECT` | Ajoute a la requête originale une nouvelle requête |
| `sql,2,3,4,5,6,7,8,9` | Sélectionne 9 colonnes (correspond au nombre de colonnes de la requête). `sql` contient la définition des tables |
| `FROM sqlite_master` | Table système de SQLite qui contient le schéma de toutes les tables |
| `--` | Commente le reste de la requête originale  afin de l'ignorer 
## Vulnerabilities

**Vulnerability :** Injection SQL - Exfiltration de données

**Components :** Database 

**Severity level :** Critique

## Risks

- Fuite de données sensibles  ( informations personnelles ou confidentielle )
- Atteinte à la credibilite de l'entreprise 
- Baisse de confiance des investiseur et actionnaire 

## Actions

**Risks mitigation strategies  :**
 - Limiter les droits du compte de base de données
 - Utilise des requete paramétrées 
**Remediation fixes :**
Utiliser des requêtes paramétrées :
```javascript
const query = "SELECT * FROM Products WHERE name LIKE ?";
db.all(query, ['%' + req.query.q + '%'], callback);
```
**Security practice :**
- Faire des tests de sécurité avant la publication 
- Ne pas afficher les erreurs SQL 

## Evidences and artifacts

**Request :**
```http
https://ctf.juice.cyber.epitest.eu/rest/products/search?q=1'))union all select sql,2,3,4,5,6,7,8,9 from sqlite_master--
```
