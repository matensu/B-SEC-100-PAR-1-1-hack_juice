# Login Amy

> **Category :** Sensitive Data Exposure  
> **Difficulty :** 3  

## Methodology

1. **Recherche d'informations**
   - Technique : OSINT, Recherche
   - Outil : Navigateur, Moteur de recherche
   - Description : Recherche de "One Important Final Note" menant au site [Password Haystack](https://www.grc.com/haystack.htm?1). Découverte du pattern de mot de passe `D0g.....................` comme exemple.

2. **Préparation de l'attaque**
   - Technique : Brute Force (Cluster Bomb)
   - Outil : BurpSuite (Proxy, Intruder)
   - Description : Interception de la requête de connexion et envoi vers Intruder. Configuration d'une attaque Cluster Bomb avec 3 payloads pour les 3 premiers caractères du mot de passe.

3. **Exploitation de la faille**
   - Technique : Brute Force
   - Outil : BurpSuite Intruder
   - Description : Lancement de l'attaque avec l'alphabet complet (majuscules/minuscules) et les chiffres pour chaque position. Identification du mot de passe valide via le code de réponse 200.

| Partie | Rôle |
|--------|------|
| `Cluster Bomb Attack` | Type d'attaque permettant de tester toutes les combinaisons de payloads |
| `§§§.....................` | 3 positions de payload suivies du pattern connu |
| `a-z, A-Z, 0-9` | Jeu de caractères pour chaque payload |
| `Code 200` | Indicateur de connexion réussie |

## Vulnerabilities

**Vulnerability :** Sensitive Data Exposure - Pattern de mot de passe exposé / Absence de rate limiting

**Components :** Système d'authentification, Endpoint de connexion

**Severity level :** Élevé

## Risks

- Prise de contrôle de comptes utilisateurs par force brute
- Perte de confidentialité (adresses, numéros de téléphone, informations de paiement)
- Fraude et pertes financières (commandes frauduleuses avec moyens de paiement enregistrés)
- Épuisement des ressources serveur (DoS potentiel par flood de requêtes)

## Actions

**Risks mitigation strategies :**
- Implémenter un rate limiting strict sur `/rest/user/login` (ex: 5 tentatives échouées/minute/IP)
- Mettre en place un mécanisme de verrouillage temporaire de compte après X tentatives échouées

**Remediation fixes :**
Implémenter un rate limiting avec express-rate-limit :
```javascript
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
    windowMs: 60 * 1000, // 1 minute
    max: 5, // 5 tentatives max
    message: { error: "Too many login attempts, please try again later" }
});

app.post('/rest/user/login', loginLimiter, (req, res) => {
    // Logique de connexion
});
```

**Security practice :**
- Implémenter un CAPTCHA après plusieurs tentatives échouées
- Appliquer une politique de mot de passe forte (rejeter les patterns prévisibles, exiger une haute entropie)
- Journaliser les tentatives de connexion échouées pour détecter les attaques
- Utiliser l'authentification multi-facteurs (MFA)

## Evidences and artifacts

**Request (Brute Force) :**
```http
POST /rest/user/login HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json

{"email":"amy@juice-sh.op", "password":"§§§....................."}
```

**Configuration Intruder :**
- Attack type : Cluster Bomb
- Payloads : 3 positions (caractères 1, 2, 3)
- Character set : a-z, A-Z, 0-9

