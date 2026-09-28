# NFT Takeover

## Description de la faille
Cette faille est due a un utilisateur qui met sa phrase de récupération dans un commentaire


## Dangerosité
Cette faille est **critique** car elle peut être sans aucune authentification et peut nuir a la personne 

## Méthodologie
- On va sur le site 
- Dans la section About us
- Nous trouvons un message avec l'endpoint suivant /juicy-nft avec une suite de caractère
- En essaye la phrase dans la page de l'endpoint cela ne focntionne pas 
- Donc on cherche comment transformé la phrase en une suite valable 
- On trouve et on utilise BIP39 
- On essaye et cela fonctionne avec la private key

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
