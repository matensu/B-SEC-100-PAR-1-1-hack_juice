# Easter Egg

## Description de la faille
Cette faille de sécurité viens directement depuis l'endpoint **/ftp** qui nous permet d'accédé au server de fichier du site-web puis nous pouvons exécuté un poison-null-byte pour téléchargé le fichier Eastere.gg en marquant a la fin de l'URL **%2500.md** Cela nous fera téléchargé le fichier en **.md**

# Méthodologie
1 - On va sur le site web\
2 - On rentre l'endpoint suivant /ftp \
3 - On découvre un fichier se nommant eastere.gg qu'on ne peu pas voir\
4 - On utilise un **Poison Byte** pour le téléchargé dans le bon format sois a la fin de l'URL **%2500.md**  sa nous télécharge l'easter egg en .md.

## Dangerosité
Cette faille est **critique** car elle peut être exploitée sans authentification.

## Conséquences pour une entreprise
- Fuite de données sensibles (clients, employés, finances)
- Atteinte à la réputation et perte de confiance
- Risques légaux (RGPD, sanctions financières)
- Exploitation en chaîne avec d’autres vulnérabilités.

## Comment corriger la faille
- Mettre en place des contrôles d’accès stricts côté serveur
- Vérifier systématiquement les rôles et permissions
- Ne jamais exposer d’endpoint ou de fichier sensible publiquement
- Auditer régulièrement les configurations et journaux d’accès
- Appliquer le principe du moindre privilège

## Référence sécurité
- OWASP Top 10
- OWASP Broken Access Control