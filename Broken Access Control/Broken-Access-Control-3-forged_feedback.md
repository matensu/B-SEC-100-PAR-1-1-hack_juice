# Forged Feedback

## Description de la faille
Cette faille se révèle car nous pouvons post un message avec l'email d'un autre utilisateur grâce a Burp.\
Ceci est due a une vérification de Token null ou inexistante.

# Méthodologie
1 - On va sur le site web\
2 - On ouvre Burp et on lance une requête dans la review en postant un message\
3 - Puis on forward la requête et on l'edit pour changé l'author du message\
4 - la faille est exploité car le message est permanent.

## Dangerosité
Cette faille est **de niveau élevé** car elle peut être exploitée sans authentification.

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
