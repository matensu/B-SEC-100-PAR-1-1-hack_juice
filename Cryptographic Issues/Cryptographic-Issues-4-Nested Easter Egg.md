# Nested Easter Egg

## Description de la faille
Cette épreuve met en évidence une vulnérabilité de sécurité courante liée à **Confidential Document**. Elle illustre un mauvais contrôle d’accès ou une exposition involontaire de données, souvent due à une mauvaise configuration ou à un manque de validation côté serveur.
²   
## Dangerosité
Cette faille est **potentiellement critique** car elle peut être exploitée sans authentification forte ou avec des privilèges limités.

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
