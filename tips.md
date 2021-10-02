# Check liste si t'es en pls

## WEB

- Affiche le code source et cherche des commentaires --> CTRL-U --> CTRL-F (html : '-->' css: '/\*')
- Scan les fichiers cachés --> gobuster
- Regarde si l'adresse d'une page contien une route du type: 'http://IP/index.php?page=accueil' --> LFI
- Analyze les fichiers javascripts et cherche des fonctions utiles
- Regarde si tu as un cookie et ce que tu peux en faire
- Scan les fichers cachés des fichiers cachés que tu as déjà trouvé (recursivement)