# Forgotten Developer Backup
**%2500.md**
## Description de la faille
Cette faille provient d'un fichier laissé dans **/ftp** sois le file transfert Protocol nous pouvons le téléchargé avec un Poison null byte en mettant **%2500.md** a la fin de l'url 

## Dangerosité
Cette faille est **élevé** car elle peut être exploitée sans authentification forte et elle permet d'exploité des fichier potentiellement sensible.

## Méthodologie
- Allez sur le site 
- rentré cette URL https://ctf.juice.cyber.epitest.eu/ftp
- cliqué sur **package-lock.json.bak**
- puis rajouté ceci a la fin de l'URL **%2500.md**

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
- retiré l'endpoint /ftp

## Référence sécurité
- OWASP Top 10
- OWASP Sensitive Data Exposure
