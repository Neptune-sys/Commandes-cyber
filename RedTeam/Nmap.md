# 📡 Nmap - Détection d’Hôtes Vivants (Host Discovery)

## 📍 Principe Général

> **Toute réponse d’un hôte indique qu’il est en ligne.**  
> Ces méthodes permettent d’identifier les machines actives sur un réseau sans forcément scanner les ports.

---

## 🔍 Types de Scans de Présence (Host Discovery)

| **Type de Scan**           | **Commande Exemple**                                           | **Description** |
|----------------------------|----------------------------------------------------------------|-----------------|
| **ARP Scan**               | `sudo nmap -PR -sn MACHINE_IP/24`                             | Scan rapide sur réseau local. Très fiable en LAN (IPv4 uniquement). |
| **ICMP Echo Scan**         | `sudo nmap -PE -sn MACHINE_IP/24`                             | Envoie une requête de type *ping* (ICMP Echo Request). |
| **ICMP Timestamp Scan**    | `sudo nmap -PP -sn MACHINE_IP/24`                             | Demande l'heure système (ICMP Timestamp Request). |
| **ICMP Address Mask Scan** | `sudo nmap -PM -sn MACHINE_IP/24`                             | Demande le masque de sous-réseau (ICMP Address Mask Request). |
| **TCP SYN Ping Scan**      | `sudo nmap -PS22,80,443 -sn MACHINE_IP/30`                    | Envoie un SYN sur les ports indiqués (22, 80, 443 ici). |
| **TCP ACK Ping Scan**      | `sudo nmap -PA22,80,443 -sn MACHINE_IP/30`                    | Envoie un paquet ACK → peut contourner certains pare-feux. |
| **UDP Ping Scan**          | `sudo nmap -PU53,161,162 -sn MACHINE_IP/30`                   | Envoie des paquets UDP aux ports souvent ouverts (ex: DNS, SNMP). |

---

## ⚙️ Options Importantes

| **Option**   | **But**                                                        |
|--------------|----------------------------------------------------------------|
| `-sn`        | Scan de découverte uniquement (pas de port scan).              |
| `-n`         | Désactive la résolution DNS → plus rapide.                     |
| `-R`         | Active la **résolution DNS inverse** pour tous les hôtes.     |

> **⚠️ Note :** Si tu omets `-sn`, **Nmap effectuera un scan de ports en plus** après avoir découvert les hôtes en ligne.

---

## 🧠 Résumé Rapide

- **LAN ?** Utilise **ARP Scan** pour plus de précision.
- **Hors LAN ?** Utilise les **scans ICMP, TCP, ou UDP**.
- **Cible protégée par pare-feu ?** Essaie le **TCP ACK Scan** ou le **UDP Ping Scan**.
- **Juste les hôtes actifs ?** Ajoute toujours `-sn`.

---

## ✅ Bonnes pratiques

- Combine plusieurs types de ping pour contourner les règles de filtrage.
- Utilise `-n` pour accélérer la découverte (évite les requêtes DNS inutiles).
- Utilise `/24`, `/30`, etc., pour cibler un sous-réseau.

---

# 🔎 Nmap - Fiche Mémo : Scans TCP/UDP & Contrôle de Vitesse

## 🚪 Types de Scans de Ports

| **Type de Scan**    | **Commande Exemple**                 | **Description** |
|---------------------|--------------------------------------|-----------------|
| **TCP Connect Scan**| `nmap -sT MACHINE_IP`                | Utilise la fonction connect() du système → détectable mais fiable. |
| **TCP SYN Scan**    | `sudo nmap -sS MACHINE_IP`           | Envoie des paquets SYN (semi-ouvert) → rapide et furtif. |
| **UDP Scan**        | `sudo nmap -sU MACHINE_IP`           | Détecte les services UDP → plus lent et souvent partiellement fiable. |

> ✅ **Ces scans permettent d’identifier les services TCP et UDP actifs sur la cible.**

---

## 🎯 Options de Sélection de Ports

| **Option**     | **But**                                                |
|----------------|--------------------------------------------------------|
| `-p-`          | Scanne **tous les 65535 ports**.                       |
| `-p1-1023`     | Scanne les ports **bien connus (privileged)**.         |
| `-F`           | Scanne les **100 ports les plus courants** (rapide).   |
| `-r`           | Scanne les ports **dans l’ordre numérique** (pas par défaut). |

---

## ⚡ Contrôle de la Vitesse et Performance

| **Option**               | **But** |
|--------------------------|--------|
| `-T0` à `-T5`            | **Réglage de timing** (0 = le plus lent, 5 = le plus agressif). |
| `--max-rate 50`          | Limite à **50 paquets/sec max** → utile pour discrétion. |
| `--min-rate 15`          | Envoie **au moins 15 paquets/sec** → garantit un rythme minimum. |
| `--min-parallelism 100`  | Force **100 sondes parallèles minimum** → booste la rapidité. |

> ⚠️ Attention : les options trop agressives peuvent provoquer du **bruit** sur le réseau ou faire planter les services ciblés.

---

## 🧠 Recommandations

- Pour un scan **rapide** : `nmap -sS -F -T4`
- Pour un scan **complet** : `sudo nmap -sS -sU -p- -T3`
- Pour un scan **discret** : `sudo nmap -sS --max-rate 20 -T1`

---

## ✅ À retenir

- **TCP Connect (-sT)** = plus visible, pas besoin de root.
- **TCP SYN (-sS)** = plus furtif, nécessite sudo/root.
- **UDP (-sU)** = indispensable pour services comme DNS, SNMP, mais souvent lent.

---



# 🛠️ Nmap - Fiche de Mémo : Types de Scans TCP & Options Avancées

## 🔍 Types de Scans TCP

| **Type de Scan**        | **Commande Exemple**                               | **But / Particularité**                                                                 |
|-------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------|
| TCP Null Scan           | `sudo nmap -sN 10.10.212.211`                      | Aucun drapeau TCP → Contourne certains pare-feux.                                       |
| TCP FIN Scan            | `sudo nmap -sF 10.10.212.211`                      | Envoie un seul drapeau FIN → Réponse uniquement si port **fermé**.                      |
| TCP Xmas Scan           | `sudo nmap -sX 10.10.212.211`                      | Envoie FIN + URG + PSH (comme un "sapin de Noël") → Réponse si **fermé**.               |
| TCP Maimon Scan         | `sudo nmap -sM 10.10.212.211`                      | Variante rare, peut contourner certains IDS.                                             |
| TCP ACK Scan            | `sudo nmap -sA 10.10.212.211`                      | Vérifie la présence de pare-feu ; ne donne pas l'état du port (ouvert/fermé).           |
| TCP Window Scan         | `sudo nmap -sW 10.10.212.211`                      | Similaire à ACK, mais exploite la taille de fenêtre TCP pour identifier les ports.      |
| Custom TCP Scan         | `sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.212.211` | Définir manuellement les drapeaux TCP.                                                |

---

## 🎭 Techniques d'Évasion & D'anonymisation

| **Technique**            | **Commande Exemple**                                    | **Utilité**                                                           |
|--------------------------|---------------------------------------------------------|------------------------------------------------------------------------|
| Spoofed Source IP        | `sudo nmap -S SPOOFED_IP 10.10.212.211`                | Simule une autre IP source (nécessite contrôle du réseau).             |
| Spoofed MAC Address      | `--spoof-mac SPOOFED_MAC`                              | Change l’adresse MAC → Contourne certains contrôles réseau.            |
| Decoy Scan               | `nmap -D DECOY_IP,ME 10.10.212.211`                    | Ajoute des leurres pour masquer l'IP réelle de l'attaquant.            |
| Idle (Zombie) Scan       | `sudo nmap -sI ZOMBIE_IP 10.10.212.211`                | Scan furtif via un hôte tiers inactif (zombie).                        |
| Fragmentation - 8 bytes  | `-f`                                                   | Fragmente les paquets IP pour éviter la détection.                     |
| Fragmentation - 16 bytes | `-ff`                                                  | Fragmente davantage → Plus de furtivité, mais risque de défaillance.   |

---

## ⚙️ Options Avancées

| **Option**              | **Description**                                          |
|-------------------------|----------------------------------------------------------|
| `--source-port PORT`    | Définit un port source personnalisé (ex: `--source-port 53`) |
| `--data-length NUM`     | Ajoute des données aléatoires jusqu'à atteindre cette taille |
| `--reason`              | Affiche la raison de la détection (ex: RST, SYN-ACK, etc.) |
| `-v` / `-vv`            | Mode verbeux / très verbeux → plus de détails sur le scan |
| `-d` / `-dd`            | Mode debug / debug poussé → utile pour déboguer un scan    |

---

## 🧠 Notes Importantes

- Les scans **Null**, **FIN**, et **Xmas** sont conçus pour détecter les **ports fermés**.
- Les scans **Maimon**, **ACK** et **Window** peuvent détecter les **comportements des pare-feux** ou les **ports ouverts/fermés indirectement**.
- Les options comme `--spoof-mac`, `-D`, ou `-sI` sont utiles pour **éviter la détection** ou **masquer l'identité de l'attaquant**.

---
# 🧠 Nmap Avancé – Scripts NSE, Détection & Sauvegarde des Résultats 🔍

📍 **Nmap ne fait pas que scanner des ports...**  
Il peut **détecter des services, des failles, exécuter des scripts**, etc.

---

## 📜 Scripts Nmap (NSE – Nmap Scripting Engine)

📂 Les scripts sont stockés dans : `/usr/share/nmap/scripts`

🔢 Il en existe **près de 600**, dont **130+ liés à HTTP** !

📌 Utilisation :
```bash
nmap --script=default target.com      # Scripts par défaut
nmap -sC target.com                   # Équivalent à --script=default
nmap --script=http-* target.com      # Tous les scripts http
nmap --script=vuln target.com        # Scripts de détection de vulnérabilités
```

---

## 📂 Catégories de Scripts

| 🔠 Catégorie     | 📖 Description                                                  |
|------------------|------------------------------------------------------------------|
| `auth`           | Scripts liés à l'authentification                                |
| `broadcast`      | Découverte d’hôtes via messages broadcast                        |
| `brute`          | Attaques par force brute sur des logins                          |
| `default`        | Scripts par défaut (équivalent à `-sC`)                          |
| `discovery`      | Infos accessibles (tables BDD, noms DNS, etc.)                   |
| `dos`            | Détection de vulnérabilités DoS                                  |
| `exploit`        | Tentatives d’exploitation de services                            |
| `external`       | Utilisation de services tiers (GeoPlugin, VirusTotal, etc.)      |
| `fuzzer`         | Fuzzing de services                                              |
| `intrusive`      | Scripts agressifs (force brute, exploits)                        |
| `malware`        | Recherche de portes dérobées                                     |
| `safe`           | Scripts sûrs, non destructifs                                    |
| `version`        | Détection de versions de services                                |
| `vuln`           | Recherche de vulnérabilités connues                              |

---

## 🕵️ Détection système et services

| ⚙️ Option                    | 📖 Signification                                           |
|-----------------------------|------------------------------------------------------------|
| `-sV`                       | Déterminer les services/versions sur les ports ouverts     |
| `--version-light`           | Tester les sondes les plus probables                       |
| `--version-all`             | Tester toutes les sondes disponibles                       |
| `-O`                        | Détection du système d’exploitation                        |
| `--traceroute`              | Exécuter un traceroute vers la cible                       |
| `--script=SCRIPTS`          | Exécuter des scripts NSE spécifiques                       |
| `-sC` ou `--script=default` | Exécuter les scripts de la catégorie "default"             |
| `-A`                        | Tout en un : `-sV -O -sC --traceroute`                     |

---

## 💾 Sauvegarde des résultats de scan

| 💾 Option  | 📄 Format de sortie                          |
|-----------|----------------------------------------------|
| `-oN`     | Format normal lisible                        |
| `-oG`     | Format grepable (pour parsers/regex)        |
| `-oX`     | Format XML (pour outils automatisés)        |
| `-oA`     | Génère les trois formats ci-dessus en un coup |

---

💡 **Tips :** Combine `-A -oA scan_result` pour un scan complet **+** sauvegarde propre 🔥


