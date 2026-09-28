# Login Bjoern

> **Category :** Broken Authentication  
> **Difficulty :** 4  

## Methodology

1. **Recherche d'informations**
   - Technique : Injection SQL, Énumération
   - Outil : Navigateur
   - Description : Connexion au compte admin via injection SQL (`admin@juice-sh.op'--`) puis accès à la page Administration pour identifier le compte Gmail de Bjoern (`bjoern.kimminich@gmail.com`).

2. **Analyse du code source**
   - Technique : Analyse de code, Reverse Engineering
   - Outil : DevTools (Inspecteur, Sources)
   - Description : Inspection du fichier `main.js` et recherche du mot-clé `email` pour identifier la logique de génération/validation des mots de passe exposée côté client.

3. **Exploitation de la faille**
   - Technique : Exécution de code côté client
   - Outil : Console du navigateur
   - Description : Exécution de la fonction découverte dans la console avec l'email `bjoern.kimminich@gmail.com` pour obtenir le mot de passe valide, puis connexion au compte.

| Partie | Rôle |
|--------|------|
| `main.js` | Fichier JavaScript contenant la logique de génération de mot de passe |
| `"email"` | Recherche du code lié à l'authentification |
| `Console` | Exécution du code pour générer le mot de passe |

## Vulnerabilities

**Vulnerability :** Broken Authentication - Exposition de secrets côté client

**Components :** Code JavaScript frontend, Système d'authentification

**Severity level :** Critique

## Risks

- Prise de contrôle complète du compte utilisateur avec des identifiants légitimes
- Fuite de secrets (logique sensible, clés API, algorithmes internes exposés à tous les utilisateurs)
- Escalade de privilèges administrateur (Bjoern étant un utilisateur à hauts privilèges)

## Actions

**Risks mitigation strategies :**
- Ne jamais inclure de logique de génération de mot de passe côté client
- Appliquer l'obfuscation du code JavaScript (défense en profondeur, non suffisant seul)

**Remediation fixes :**
Déplacer toute la logique d'authentification côté serveur :
```javascript
// Côté serveur uniquement
const bcrypt = require('bcrypt');

async function validatePassword(email, password) {
    const user = await db.get("SELECT * FROM Users WHERE email = ?", [email]);
    if (!user) return false;
    return await bcrypt.compare(password, user.passwordHash);
}
```

**Security practice :**
- Stocker les mots de passe sous forme de hash cryptographique fort (bcrypt)
- Valider les mots de passe uniquement côté serveur
- Auditer régulièrement le code frontend pour détecter les fuites de secrets
- Ne jamais exposer d'algorithmes sensibles dans les fichiers JavaScript publics

## Evidences and artifacts

**Étapes :**
1. Login admin via SQLi → Page Administration → Récupération email `bjoern.kimminich@gmail.com`
2. Inspection de `main.js` → Recherche "email" → Découverte de la logique de mot de passe
3. Exécution dans la console → Obtention du mot de passe
4. Connexion réussie au compte Bjoern

