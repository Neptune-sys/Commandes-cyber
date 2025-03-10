# Commandes Github

## Se joindre à 1 répo que l'on a créer
1. Cloner le répo `git clone <URL>`
2. Se placer dans le répo
3. Cliquer sur la bulle de notre avatar en haut à gauche
4. Cliquer sur Settings
5. Allez sur l'onglet developper settings
6. Générer un "Personal Access Token" (avec tous les droits)
7. Dans le terminal de l'ordinateur dans le dossier du répo, on lance la commande suivante : `git remote set-url origin https://<token>@github.com/<username>/<repo>`
8. Tapez ensuite la commande `git fetch origin`
