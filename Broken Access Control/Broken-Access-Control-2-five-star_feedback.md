# Five-Star Feedback

## Description de la faille
Cette faille met en évidence la possibilité de supprimé des reviews.

# Méthodologie
1 - On va sur le site web\
2 - On rentre dans la section admin découvert précédement \
3 - Et supprime les feedback avec 5 étoiles\
4 - l'effet est irréversible

## Dangerosité
Cette faille est **de niveau low** car elle peut être exploitée qu'avec les permission admin.

## Conséquences pour une entreprise
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)

## Comment corriger la faille
- Mettre en place des contrôles d’accès stricts côté serveur
- Vérifier systématiquement les rôles et permissions
- Ne jamais exposer d’endpoint ou de fichier sensible publiquement
- Auditer régulièrement les configurations et journaux d’accès
- Appliquer le principe du moindre privilège

## Référence sécurité
- OWASP Top 10
- OWASP Broken Access Control
