# Visual Geo Stalking

## Description de la faille
Cette faille est due a une erreur d'un utilisateur qui dévoile ses données sans le savoir via une image.

## Dangerosité
Cette faille est **élevé** car elle peut être exploité avec une image.

## Méthodologie
- On va sur le site 
- On Trouve l'adresse mail d' John et on vois que sa phrase secrète pour reset son password est son endroit préféré pour aller se baladé
- En allant dans la section Photo Wall nous trouvons une photo d'un sentier pour randonné
- On télécharge l'image 
- puis on utilise un outils appelé exiftool 
- cela nous permet d'avoir les coordonnées gps de l'image 
- On met les données sur google maps et nous avons un lieu approximatif on recherche un peu et nous trouvons le nom de la forêt
- nous pouvons dès a présent accédé au compte de John en utilisant sa phrase secrète pour changé son password.

## Conséquences pour une entreprise
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)
- Exploitation en chaîne avec d’autres vulnérabilités

## Comment corriger la faille
- Mettre en place des contrôles d’accès stricts côté serveur
- Vérifier systématiquement les rôles et permissions
- Ne jamais exposer d’endpoint ou de fichier sensible publiquement
- Auditer régulièrement les configurations et journaux d’accès
- Appliquer le principe du moindre privilège
- Ne pas pouvoir téléchargé les images

## Référence sécurité
- OWASP Top 10
- OWASP Sensitive Data Exposure
