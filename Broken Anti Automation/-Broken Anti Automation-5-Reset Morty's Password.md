# Reset Morty's Password

> **Category :** Broken Anti Automation  
> **Difficulty :** 5  

## Methodology

1. **Recherche d'informations (OSINT)**
   - Technique : OSINT, Analyse des avis utilisateurs
   - Outil : Navigateur, Moteur de recherche
   - Description : Découverte de la mention "Interdimensional Cable" dans un avis de Morty. Recherche révélant qu'il s'agit du dessin animé Rick and Morty. Identification des noms de l'animal de compagnie favori de Morty : `Snuffles` et `Snowball`.

2. **Préparation de l'attaque**
   - Technique : Brute Force (Cluster Bomb)
   - Outil : BurpSuite (Proxy, Intruder)
   - Description : Accès à la page "Forgot Password", remplissage du formulaire et interception de la requête. Configuration d'une attaque Cluster Bomb avec 8 payloads pour chaque lettre de la réponse.

3. **Exploitation de la faille**
   - Technique : Brute Force automatisé sur question de sécurité
   - Outil : BurpSuite Intruder
   - Description : Lancement de l'attaque avec toutes les combinaisons de casse possibles pour chaque lettre. Absence de rate limiting permettant des milliers de requêtes automatisées. Identification de la réponse correcte via le code de réponse 200.

| Partie | Rôle |
|--------|------|
| `POST /rest/user/reset-password` | Endpoint de réinitialisation de mot de passe |
| `Cluster Bomb Attack` | Type d'attaque testant toutes les combinaisons |
| `§§§§§§§§` | 8 positions de payload (une par lettre) |
| `Code 200` | Indicateur de réponse correcte |

## Vulnerabilities

**Vulnerability :** Broken Anti Automation - Absence de protection contre les attaques automatisées

**Components :** Système de récupération de mot de passe, Endpoint reset-password

**Severity level :** Critique

## Risks

- Prise de contrôle de compte (Account Takeover) via attaque automatisée
- Perte d'accès permanente pour l'utilisateur légitime
- Exploitation massive par des bots (milliers de tentatives en quelques secondes)
- Énumération des comptes utilisateurs
- Déni de service potentiel par surcharge du serveur

## Actions

**Risks mitigation strategies :**
- Implémenter un rate limiting strict (ex: 3 tentatives/heure par IP et par compte)
- Ajouter un délai exponentiel entre les tentatives échouées
- Envoyer une notification email à l'utilisateur lors de chaque tentative de reset

**Remediation fixes :**
Implémenter un CAPTCHA et rate limiting :
```javascript
const rateLimit = require('express-rate-limit');
const captcha = require('express-recaptcha');

const resetLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 heure
    max: 3, // 3 tentatives max
    message: { error: "Too many reset attempts, please try again later" }
});

app.post('/rest/user/reset-password', resetLimiter, captcha.middleware.verify, async (req, res) => {
    if (!req.recaptcha.error) {
        // Procéder à la réinitialisation
    } else {
        return res.status(400).json({ error: "CAPTCHA verification failed" });
    }
});
```

**Security practice :**
- Implémenter un CAPTCHA après plusieurs tentatives échouées
- Utiliser des tokens de réinitialisation à usage unique et durée limitée
- Bloquer temporairement les IPs suspectes
- Journaliser et alerter sur les patterns d'attaques automatisées
- Abandonner les questions de sécurité au profit d'OTP ou liens temporaires

## Evidences and artifacts

**OSINT :**
- Source : Avis utilisateur mentionnant "Interdimensional Cable"
- Recherche : "Morty favorite pet" → `Snuffles` / `Snowball`

**Request (Brute Force) :**
```http
POST /rest/user/reset-password HTTP/1.1
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json

{
    "email": "morty@juice-sh.op",
    "answer": "§§§§§§§§",
    "new": "reset",
    "repeat": "reset"
}
```

**Configuration Intruder :**
- Attack type : Cluster Bomb
- Payloads : 8 positions (une par lettre)
- Character set : Majuscule/Minuscule pour chaque lettre

