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


