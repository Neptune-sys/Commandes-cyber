# 🕵️‍♂️ Reconnaissance Passive – Discrète mais Puissante 💡

📍 **Observer sans se faire remarquer.**  
Collecter des infos **sans interagir directement** avec la cible.

---

## 🔍 Outils en ligne de commande

| 🎯 Objectif                      | 💻 Commande CLI                                      |
|----------------------------------|------------------------------------------------------|
| 🔎 WHOIS d’un domaine           | `whois tryhackme.com`                               |
| 📛 Enregistrements DNS A        | `nslookup -type=A tryhackme.com`                    |
| 📬 Enregistrements DNS MX via serveur | `nslookup -type=MX tryhackme.com 1.1.1.1`         |
| 🧾 Enregistrements DNS TXT      | `nslookup -type=TXT tryhackme.com`                  |
| 📛 Enregistrements A (avec dig) | `dig tryhackme.com A`                               |
| 📬 MX via serveur (avec dig)    | `dig @1.1.1.1 tryhackme.com MX`                     |
| 🧾 TXT (avec dig)               | `dig tryhackme.com TXT`                             |

---

## 🌐 Services publics très utiles

| 🧰 Service         | 🔍 Description                                      |
|-------------------|-----------------------------------------------------|
| [DNSDumpster](https://dnsdumpster.com) | Cartographie DNS, sous-domaines, hôtes visibles |
| [Shodan.io](https://www.shodan.io)     | Moteur de recherche pour appareils connectés    |

---

💡 **Astuce :** Ces outils permettent de recueillir **beaucoup d'informations** sans jamais alerter la cible.  
Maîtrise les options de recherche et apprends à **interpréter les résultats** = 💣.

