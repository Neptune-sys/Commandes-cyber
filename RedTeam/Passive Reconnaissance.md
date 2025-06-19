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



# 🕵️‍♂️ Search Engine Cheat Sheet

## 📌 Search Modifiers

| Syntax | Description |
| ------ | ------------ |
| `"search phrase"` | Cherche une phrase exacte. |
| `filetype:pdf` | Cherche uniquement des fichiers PDF (remplace `pdf` par `doc`, `ppt`, `xls`, etc. pour d’autres formats). |
| `site:example.com` | Limite la recherche à un site précis. |
| `-site:example.com` | Exclut un site précis des résultats. |
| `intitle:keyword` | Cherche des pages avec un mot-clé dans le titre de la page. |
| `inurl:keyword` | Cherche des pages avec un mot-clé dans l’URL. |

---

## 🗂️ Exemples pratiques (GHDB)

| Catégorie | Exemple | Fonction |
| --------- | ------- | -------- |
| **Footholds** | `intitle:"index of" "nginx.log"` | Cherche des logs Nginx mal configurés. |
| **Files Containing Usernames** | `intitle:"index of" "contacts.txt"` | Débusque des fichiers de contacts exposés. |
| **Sensitive Directories** | `inurl:/certs/server.key` | Vérifie si une clé privée RSA est accessible. |
| **Web Server Detection** | `intitle:"GlassFish Server - Server Running"` | Identifie des serveurs GlassFish actifs. |
| **Vulnerable Files** | `intitle:"index of" "*.php"` | Liste des fichiers PHP qui pourraient être vulnérables. |
| **Vulnerable Servers** | `intext:"user name" intext:"orion core" -solarwinds.com` | Localise des consoles web SolarWinds Orion. |
| **Error Messages** | `intitle:"index of" errors.log` | Trouve des fichiers de logs d’erreurs. |

🔑 **Note :** Ces requêtes trouvent des infos sensibles **indexées publiquement**. Toujours vérifier la légalité avant d’exploiter ces résultats.

---

## 📱 Sources d'infos passives supplémentaires

### 🔗 Réseaux Sociaux
- **Pourquoi ?** : Identifier employés, technologies utilisées, réponses possibles aux questions de récupération de mots de passe.
- **À surveiller :** LinkedIn, Twitter, Facebook, Instagram.
- **Indice :** Les publications techniques peuvent trahir des infos système.

### 💼 Offres d'emploi
- **Pourquoi ?** : Révéler technologies, systèmes internes, emails de contact.
- **À faire :** Chercher sur sites d’emploi locaux et pages carrières du site cible.
- **Astuce :** Utiliser la [Wayback Machine](https://archive.org/web/) pour voir d’anciennes offres.

---

## 🏁 Ressources utiles

- [Google Advanced Search](https://www.google.com/advanced_search)
- [Google Refine Web Searches](https://support.google.com/websearch/answer/2466433)
- [DuckDuckGo Search Syntax](https://duckduckgo.com/advanced)
- [Bing Advanced Search Options](https://www.bing.com/search?q=bing+advanced+search)
- [Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database)

---

✅ **Résumé :**
Combine ces opérateurs et sources pour faire du **reconnaissance passive** efficacement et sans bruit inutile.


