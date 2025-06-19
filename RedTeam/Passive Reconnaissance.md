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

# 🌐 WHOIS, DNS & Specialized Search Cheat Sheet

## 🗂️ WHOIS History

- **But :** Obtenir l’historique des propriétaires de domaines.
- **Outil :** [WHOIS History](https://whois-history.whoisxmlapi.com/)
  - 🔍 Utile si le propriétaire initial n’a pas utilisé la confidentialité WHOIS.

---

## 🧩 Services DNS Avancés

### 🔍 **ViewDNS.info**
- **Fonction principale :** Reverse IP Lookup.
  - ✅ À partir d’un **IP** ou domaine ➜ trouve **tous les domaines** utilisant la même IP.
  - 💡 Intérêt : découvrir des sites hébergés sur le même serveur (utile pour du shared hosting).

### 🧑‍💻 **Threat Intelligence Platform**
- **Fonction principale :** Rapport enrichi WHOIS + DNS + sécurité.
  - ✅ Fournit :
    - Résolution de domaines en IPv4/IPv6.
    - Vérification de malware.
    - Liste des autres domaines sur la même IP.
  - 💡 Plus lisible qu’un simple `whois` ou `dig`.

---

## 🕵️ Specialized Search Engines

### 🔬 **Censys**
- **But :** Obtenir des infos détaillées sur IPs & domaines.
  - ✅ Montre :
    - Ports ouverts.
    - Certificats SSL.
    - Organisation propriétaire de l’IP.
  - ⚠️ Attention à bien vérifier que l’IP appartient à la cible pour éviter de scanner hors périmètre.

### 🚀 **Shodan**
- **But :** Scanner l’Internet pour trouver des services exposés.
  - ✅ Depuis la ligne de commande :
    1. **Configurer l’API :**  
       ```bash
       shodan init API_KEY
       ```
    2. **Chercher un host :**  
       ```bash
       shodan host IP_ADDRESS
       ```
       Affiche :
       - Localisation géographique.
       - Organisation propriétaire.
       - Ports ouverts + versions SSL/TLS.
  - 📌 Exemple :
    ```bash
    shodan host 172.67.212.249
    ```
    **Résultat typique :**
    ```
    City: San Francisco
    Country: United States
    Organisation: Cloudflare, Inc.
    Ports:
      80/tcp
      443/tcp (TLSv1.2, TLSv1.3)
      2086/tcp
      2087/tcp
      8080/tcp
    ```

---

## ✅ Points Clés

- 🗝️ **WHOIS History** : retrace les anciens propriétaires.
- 🗝️ **ViewDNS.info** : trouve d’autres sites partageant une IP.
- 🗝️ **Threat Intelligence Platform** : fait WHOIS + DNS + vérification sécurité de façon visuelle.
- 🗝️ **Censys** : détaille IP/domaines + ports + certificats.
- 🗝️ **Shodan** : liste ports ouverts & services exposés sur Internet.

---

## 🔗 Ressources

- [WHOIS History](https://whois-history.whoisxmlapi.com/)
- [ViewDNS.info](https://viewdns.info/)
- [Threat Intelligence Platform](https://threatintelligenceplatform.com/)
- [Censys](https://censys.io/)
- [Shodan](https://www.shodan.io/)

---

⚠️ **Conseil :** Toujours rester dans le cadre légal et contractuel lors de l’utilisation de ces outils.



