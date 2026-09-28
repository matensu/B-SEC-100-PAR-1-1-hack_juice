# Confidential Document

## Description de la faille
Cette faille est due a l'exposition de l'endpoint /ftp ce qui donne accès au fichier du file transfert protocol donc un accès a des fichier sensible sur le site web.

## Dangerosité
Cette faille est **critique** car elle peut être exploitée sans authentification forte ou avec des privilèges limités et permet d'avoir accès a des fichiers dont seul les administrateur pourrais avori l'accès.

## Méthodologie
- Allez sur le site 
- Allez a l'endpoint suivant https://ctf.juice.cyber.epitest.eu/ftp
- cliquez sur acquisition.md 
- On rentre dans un document confidentiel

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
- OWASP Sensitive Data Exposure
