# Change Bender's Password

> **Category :** Broken Authentication  
> **Difficulty :** 5  

## Methodology

1. **Accès au compte**
   - Technique : Injection SQL
   - Outil : Navigateur
   - Description : Connexion au compte `bender@juice-sh.op` via injection SQL (`bender@juice-sh.op'--`).

2. **Recherche de faille**
   - Technique : Analyse des requêtes
   - Outil : BurpSuite (Proxy, Interceptor)
   - Description : Accès à la page "Change Password", remplissage du formulaire avec un mot de passe actuel aléatoire et `slurmCl4ssic` comme nouveau mot de passe. Interception de la requête pour analyser les paramètres.

3. **Exploitation de la faille**
   - Technique : Manipulation de paramètres (Parameter Tampering)
   - Outil : BurpSuite
   - Description : Suppression du paramètre `current` de la requête. Le serveur accepte le changement de mot de passe sans vérifier l'ancien mot de passe.

| Partie | Rôle |
|--------|------|
| `GET /rest/user/change-password` | Endpoint de changement de mot de passe |
| `current` | Paramètre du mot de passe actuel (supprimé) |
| `new` | Nouveau mot de passe |
| `repeat` | Confirmation du nouveau mot de passe |

## Vulnerabilities

**Vulnerability :** Broken Authentication - Contournement de la ré-authentification

**Components :** Système de changement de mot de passe, API REST

**Severity level :** Critique

## Risks

- Prise de contrôle de compte (Account Takeover) permanente
- Contournement du mécanisme de ré-authentification
- Escalade depuis un vol de session (XSS) vers un accès persistant
- Verrouillage de l'utilisateur légitime hors de son compte

## Actions

**Risks mitigation strategies :**
- Envoyer une notification email immédiate lors de tout changement de mot de passe
- Implémenter une invalidation de toutes les sessions après changement de mot de passe

**Remediation fixes :**
Valider obligatoirement le mot de passe actuel côté serveur :
```javascript
app.get('/rest/user/change-password', async (req, res) => {
    const { current, new: newPass, repeat } = req.query;
    
    // Vérification stricte du paramètre current
    if (!current || current.trim() === '') {
        return res.status(400).json({ error: "Current password is required" });
    }
    
    // Vérifier que le mot de passe actuel est correct
    const user = await db.get("SELECT * FROM Users WHERE id = ?", [req.user.id]);
    if (!verifyPassword(current, user.password)) {
        return res.status(401).json({ error: "Current password is incorrect" });
    }
    
    // Procéder au changement
    await db.run("UPDATE Users SET password = ? WHERE id = ?", [hashPassword(newPass), req.user.id]);
    res.json({ status: "success" });
});
```

**Security practice :**
- Toujours exiger le mot de passe actuel pour les actions critiques
- Rejeter les requêtes avec des paramètres manquants ou vides
- Utiliser POST au lieu de GET pour les opérations sensibles
- Notifier l'utilisateur par email de tout changement de mot de passe

## Evidences and artifacts

**Request (paramètre `current` supprimé) :**
```http
GET /rest/user/change-password?new=slurmCl4ssic&repeat=slurmCl4ssic HTTP/2
Host: ctf.juice.cyber.epitest.eu
Authorization: Bearer [JWT_TOKEN]
```

**Response :**
```json
{
    "user": {
        "id": 3,
        "email": "bender@juice-sh.op",
        "password": "06b0c5c1922ed4ed62a5449dd209c96d",
        "role": "customer",
        "updatedAt": "2025-12-15T21:00:32.756Z"
    }
}
```