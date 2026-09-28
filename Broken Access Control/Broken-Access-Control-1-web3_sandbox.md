# Web3 Sandbox

## Description de la faille
Cette faille met en évidence un problème de sécurité au niveau des routes du site web.

# Méthodologie
1 - On va sur le site web\
2 - On rentre dans l'URL\
3 - Et on rentre l'endpoint suivant /web3-sandbox\
4 - On arrive ensuite sur le sandbox web3

## Dangerosité
Cette faille est **de niveau élevé** car elle peut être exploitée sans authentification forte ou avec des privilèges limités.

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
