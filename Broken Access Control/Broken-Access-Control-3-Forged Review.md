# Forged Review

## Description de la faille
Cette faille montre une mauvaise gestion des requêtes sans vérification ce qui permet d'ajouté des éléments dans le panier des autres utilisateurs. 

# Méthodologie
1 - On va sur le site web\
2 - On ouvre Burp et on lance une requête en ajoutant un objet dans notre panier\
3 - Puis on forward la requête et on l'edit pour rajouté un id basket dans la requête\
4 - L'objet sera ajouté dans les 2 paniers

## Dangerosité
Cette faille est **de niveau medium** car elle peut être exploitée .

## Conséquences pour une entreprise
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)

## Comment corriger la faille
- Mettre en place des contrôles d’accès stricts côté serveur
- Vérifier systématiquement les rôles et permissions
- Appliquer le principe du moindre privilège

## Référence sécurité
- OWASP Top 10
- OWASP Broken Access Control
