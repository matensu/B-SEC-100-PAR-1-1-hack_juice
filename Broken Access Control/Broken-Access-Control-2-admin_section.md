# Admin Section

## Description de la faille
Pour cette faille on c'est login en admin via injection SQL. Une fois cela fait  nous pouvons allez à l'endpoint suivant /administration ce qui nous donne accès a la section admin du site web.

# Méthodologie
1 - On va sur le site web dans la section login\
2 - On rentre dans ('OR TRUE,) dans le username et on rentre n'importe quoi dans le password on sera connecté en admin\
3 - Et on rentre l'endpoint suivant /administration\
4 - On arrive ensuite sur l'admin section

## Dangerosité
Cette faille est **de niveau medium** car il faut être log en admin avant de pouvoir accèdé a cela mais une fois cela fait nous pouvons voir tous les comptes et commentaires du site. 

## Conséquences pour une entreprise
- Fuite de données sensibles
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD)
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
