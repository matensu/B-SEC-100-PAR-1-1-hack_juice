# Visual Geo Stalking

## Description de la faille
Cette faille est due a une erreur d'un utilisateur qui dévoile ses données sans le savoir.

## Dangerosité
Cette faille est **élevé** car elle peut être exploité avec une image.

## Méthodologie
- On va sur le site 
- On Trouve l'adresse mail d'Emma et on vois que sa phrase secrète pour reset son password est son ancien lieu de Travail 
- En allant dans la section Photo Wall nous trouvons une photo de son ancien lieu de travail 
- On télécharge l'image 
- puis on utilise un outils appelé exiftool 
- cela nous permet d'avoir les coordonnées gps de l'image 
- On met les données sur google maps et nous avons le nom du lieu
- nous pouvons dès a présent accédé au compte d'Emma.

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
