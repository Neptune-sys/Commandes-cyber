# 🛠️ Scanner Réseau & Système – Mode Script Shell 🐚

📍 **Outils simples mais puissants, souvent déjà installés !**

---

## 🌐 Détection & Traçage de la cible

| 🧰 Outil        | 💻 Commande Linux/macOS         | 💻 Commande Windows         | 🔍 Usage                              |
|----------------|-----------------------------|----------------------------|----------------------------------------|
| `ping`         | `ping -c 10 MACHINE_IP`     | `ping -n 10 MACHINE_IP`    | Vérifie si la cible répond à l'ICMP    |
| `traceroute`   | `traceroute MACHINE_IP`     | `tracert MACHINE_IP`       | Montre les routes réseau vers la cible |

---

## 🔓 Détection de ports ouverts

| 🧰 Outil         | 💻 Commande                                | 🔍 Usage                                     |
|-----------------|---------------------------------------------|----------------------------------------------|
| `telnet`        | `telnet MACHINE_IP PORT`                   | Tente une connexion sur un port spécifique   |
| `netcat` client | `nc MACHINE_IP PORT`                       | Idem, en version plus flexible               |
| `netcat` serveur| `nc -lvnp PORT`                            | Ouvre un port local pour écouter             |

---

## 🌍 Et n'oublie pas... le navigateur web !

🕵️‍♂️ Les **Outils de Développement** intégrés aux navigateurs sont des alliés précieux en reconnaissance :

| 💻 OS               | ⚡ Raccourci Dev Tools     |
|--------------------|---------------------------|
| Linux / Windows    | `Ctrl + Shift + I`        |
| macOS              | `Option + Command + I`    |

👀 *Explore, inspecte, observe les requêtes réseau, cookies, en-têtes HTTP, etc.*

---
