# Payback Time

> **Category :** Improper Input Validation  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Analyse des requêtes
   - Outil : BurpSuite (Proxy, HTTP History)
   - Description : Ajout d'un produit au panier et augmentation de la quantité. Interception de la requête PUT pour analyser les paramètres envoyés au serveur.

2. **Exploitation de la faille**
   - Technique : Manipulation de paramètres (Parameter Tampering)
   - Outil : BurpSuite (Repeater)
   - Description : Modification du paramètre `quantity` avec une valeur négative. Le serveur accepte cette valeur sans validation, ce qui génère un prix total négatif (crédit) dans le panier, permettant d'acheter des produits gratuitement.

| Partie | Rôle |
|--------|------|
| `PUT /api/BasketItems/{id}` | Endpoint de modification de la quantité d'un article |
| `quantity` | Paramètre définissant la quantité du produit |
| `-123456789` | Valeur négative créant un "crédit" dans le panier |

## Vulnerabilities

**Vulnerability :** Improper Input Validation - Acceptation de quantités négatives

**Components :** API REST, Système de panier, Logique métier de calcul des prix

**Severity level :** Critique

## Risks

- Fraude financière (acquisition de produits gratuitement en compensant avec des quantités négatives)
- Contournement de la logique métier (possibilité de "vendre" des articles au magasin sans retour physique)
- Corruption de l'inventaire (les stocks peuvent être faussés par des valeurs négatives)
- Pertes financières massives pour l'entreprise

## Actions

**Risks mitigation strategies :**
- Implémenter une vérification de cohérence lors du checkout (total > 0)
- Configurer des contraintes de base de données (unsigned integers pour les quantités)

**Remediation fixes :**
Valider côté serveur que la quantité est strictement positive :
```javascript
app.put('/api/BasketItems/:id', async (req, res) => {
    const { quantity } = req.body;
    
    // Validation stricte de la quantité
    if (!Number.isInteger(quantity) || quantity <= 0) {
        return res.status(400).json({ error: "Quantity must be a positive integer" });
    }
    
    // Mettre à jour la quantité
    await db.run("UPDATE BasketItems SET quantity = ? WHERE id = ?", [quantity, req.params.id]);
    res.json({ status: "success" });
});
```

**Security practice :**
- Ne jamais faire confiance aux données envoyées par le client
- Implémenter une validation côté serveur pour tous les paramètres numériques
- Ajouter des contraintes au niveau de la base de données (CHECK quantity > 0)
- Effectuer des tests de sécurité sur toutes les fonctionnalités de paiement

## Evidences and artifacts

**Request :**
```http
PUT /api/BasketItems/9 HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json
Authorization: Bearer [JWT_TOKEN]

{
    "quantity": -123456789
}
```

**Response :**
```json
{
    "status": "success",
    "data": {
        "ProductId": 1,
        "BasketId": 6,
        "id": 9,
        "quantity": -123456789,
        "createdAt": "2025-12-14T01:01:27.232Z",
        "updatedAt": "2025-12-14T01:03:50.466Z"
    }
}
```
