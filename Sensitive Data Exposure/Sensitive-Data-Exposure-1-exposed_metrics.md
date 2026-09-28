# Exposed Metrics

## Description de la faille
Cette faille en évidence l'exposition de l'endpoint /metrics qui donne accès a tout les donnée metric du site web .

## Dangerosité
Cette faille est **élevé** car elle peut être exploitée sans authentification forte ou avec des privilèges limités et permet d'accédé a des données.

## Méthodologie 
- Allez sur le site 
- rentré l'endpoint suivant /metrics
- vous accédé au donnée metric du site web.

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
