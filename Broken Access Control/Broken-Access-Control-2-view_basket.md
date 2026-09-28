# View Basket

## Description de la faille
Cette faille met en évidence une mauvaise gestion des requêtes qui impact la sécurité du site .

# Méthodologie
1 - On va sur le site web\
2 - On rentre dans la section admin découvert précédement dans [admin-section](Broken-authentication-2-admin_section.md)\
3 - Et supprime les feedback avec 5 étoiles\
4 - l'effet est irréversible

## Dangerosité
Cette faille est **de niveau low** car elle ne permet que de voir le panier d'un autre client

## Conséquences pour une entreprise
- Fuite de données sensibles (clients, employés, finances)
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)
- Exploitation en chaîne avec d’autres vulnérabilités

## Comment corriger la faille
- Mettre en place des contrôles d’accès stricts côté serveur
- Vérifier systématiquement les rôles et permissions
- Ne jamais exposer d’endpoint ou de fichier sensible publiquement
- Auditer régulièrement les configurations et journaux d’accès
- Appliquer le principe du moindre privilège

## Référence sécurité
- OWASP Top 10
- OWASP Broken Access Control
