# Access Log

## Description de la faille
Accès de la faille par l'URL suivant **https://ctf.juice.cyber.epitest.eu/support/logs** après s'être connecté au site 

## Dangerosité
Cette faille est **Critique** car elle peut être exploitée via un Url et les données trouvée sont très sensible.

## Méthodologie
- allez sur le site
- connectons-nous au administrateur via [admin-section](Broken-Access-Control-2-admin_section.md)
- puis on entre dans l'URL suivante **https://ctf.juice.cyber.epitest.eu/support/logs** 

## Conséquences pour une entreprise
- Fuite de données sensibles (clients, employés, finances)
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)
- Exploitation en chaîne avec d’autres vulnérabilités

## Comment corriger la faille
- Supprimé l'accès des logs sur le site web 

## Référence sécurité
- OWASP Top 10
- OWASP Sensitive Data Exposure
