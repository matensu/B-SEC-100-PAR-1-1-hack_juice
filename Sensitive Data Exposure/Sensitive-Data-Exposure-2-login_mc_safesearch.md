# Login MC SafeSearch

## Description de la faille
Cette faille est due a la divulagation d'un mot de passe dans une musique

## Dangerosité
Cette faille est **medium** car elle peut être exploitée sans authentification forte ou avec des privilèges limités.

## Méthodologie 
- Dans la section admin découvert ici [admin-section](Broken-Access-Control-2-admin_section.md)
- Nous trouvons l'email mc.safesearch@juice-sh.op
- Si nous cherchons MC SafeSearch sur internet nous tombons sur une vidéo
- En écoutant attentivement les paroles nous Trouvons le nom de son chient Mr Noodles.
- En brute Force le password avec différente combinaison du nom, nous trouvons enfin le password qui est Mr. N00dles

## Conséquences pour une entreprise
- Fuite de données sensibles (clients, employés, finances)
- Exploitation en chaîne avec d’autres vulnérabilités

## Comment corriger la faille
- Demandé un changement de mot de passe régulier sans pouvoir remettre un mot de passe similaire au précédent. 


## Référence sécurité
- OWASP Top 10
- OWASP Sensitive Data Exposure
