# Mon aide-mémoire curl

Une entrée par quête. Trois lignes chacune, écrites avec mes mots.

- **La situation** — ce que la quête demandait
- **La commande** — celle qui a validé, recopiée telle quelle
- **Le piège** — ce qui m'a coincé, ou rien si tout a coulé

---

## 1 — Day 1: Inventory Check

**La situation** : Récupérer la liste des articles disponibles dans l'inventaire.

**La commande** :

```bash
curl "http://localhost:8080/inventory"
```

**Le piège** :

---

## 2 — Day 2: Adding Items

**La situation** : Ajouter un article à l'inventaire.

**La commande** :

```bash
curl -s -X POST -H "Content-Type: application/json" -d '{"name":"Oranges"}' "http://localhost:8080/inventory"
```

**Le piège** :
 * -s = met en silence la barre de progression de curl
 * -X = impose la méthode HTTP, sans elle, curl envoie un GET
 * -H = ajoute un header à la requete
 * -d = transmet le corps de la requête
---

## 3 — Day 3: Maintain and Update

**La situation** : Mettre à jour des données partielles, totales et en supprimer une.

**La commande** :

```bash
# faire une modification partielle
curl "http://localhost:8080/inventory/1" -s -X PATCH -H "Content-Type: application/json" -d '{"name": "Organic Bananas"}'

# faire une modification totale
curl "http://localhost:8080/inventory/2" -s -X PUT -H "Content-Type: application/json" -d '{"name": "In stock Watermelon", "price": "5.00"}'

# supprimer un élément
curl "http://localhost:8080/inventory/3" -s -X DELETE 
```

**Le piège** :
La commande de suppression (DELETE) ne renvoie aucune réponse, c'est normal !
---

## 4 — The Elemental Search

**La situation** : Rechercher des éléments, filtrer, trier et ajouter une pagination.

**La commande** :

```bash
# faire une recherche avec un filtre
curl "http://localhost:8080/pokemon/search?type=fire"

# faire une recherche avec plusieurs possibilités pour un même filtre (OR)
curl "http://localhost:8080/pokemon/search?type=water&type=grass"

# faire une recherche cumulant plusieurs filtres (AND)
curl "http://localhost:8080/pokemon/search?type=electric&region=kanto"

# faire une recherche avec des filtres contenant des caractère à encoder
curl "http://localhost:8080/pokemon/search?role=special+attacker"

# trier par ordre descendant une liste
curl "http://localhost:8080/pokemon/search?sort=base_stat_desc"

# recherche avec tous les filtres combinées
curl "http://localhost:8080/pokemon/search?type=fire&type=grass&region=kanto&role=special+attacker&sort=base_stat_desc"
```

**Le piège** :
 * Toujours placer les url dans des quotes, sinon les query parameters seront passés sous silence.
 * Utiliser `+` ou `%20` pour encoder les caractère particuliers dans une url
---

## 5 — Payslip Uploader

**La situation** : Télécharger et transmettre des fichiers lourds

**La commande** :

```bash
# télécharger la réponse sous le fichier payslip.json
curl "http://localhost:8080/files/payslip" -o payslip.json 

# uploader un fichier
curl "http://localhost:8080/payslips" -X POST -F "file=@payslip.json" -s
```

**Le piège** :
 * -o <filename> = enregistre la réponse dans un fichier local au lieu de l'imprimer dans le terminal
 * -F "field=@filename" = envoie le fichier en formulaire multipart (par défault le field = file pour un fichier)
 * ATTENTION : un fichier contenant du json n'est pas du content-type : application/json
---

## 6 — Strict API Contracts

**La situation** :

**La commande** : utiliser les headers

```bash
# récupérer la liste des élément en ajoutant son token en header
curl "http://localhost:8080/groceries" -H "x-api-key: secret123"

# utiliser le format application/json
curl -X POST -H "x-api-key: secret123" -H "Content-Type: application/json" -d '{"name": "Butter"}' "http://localhost:8080/groceries"

#utiliser le format application/x-www-form-urlencoded = formulaire
curl -X POST -H "x-api-key: secret123" -H "Content-Type: application/x-www-form-urlencoded" -d "name=Apples" "http://localhost:8080/groceries"

```

**Le piège** :

---

## 7 — The Manager's Secret

**La situation** : Visualiser les headers et changer d'agent utilisateur

**La commande** :

```bash
# obtenir les headers et le body
curl "http://localhost:8080/employee-portal" -i

# obtenir uniquement le header
curl "http://localhost:8080/staff-inventory?token=Manager-Access-99" -I

# changer le user-agent
curl "http://localhost:8080/manager-vault?token=Manager-Access-99" -I -A "The-Bosses-iPad"

```

**Le piège** :
 * -i = retourne les headers et body de la réponse
 * -I = retourne uniquement les headers
 * -v = retourne entièrement la requête/réponse pour une vue en débug
 * -A = pour changer le User-Agent (prétendre d'être quelqu'un d'autre)
---

## 8 — The Galactic Relay

**La situation** : Naviguer à travers les codes d'erreur

**La commande** :

```bash

```

**Le piège** :
- 1xx (Informational): La requête à été reçue
- 2xx (Success): La requpete a fonctionnée
- 3xx (Redirection): La ressource a été déplacée
- 4xx (Client Error): La requête est invalide
- 5xx (Server Error): Le server à un problème


## Bonus 1

`-L` : indique à `curl` d'automatiquement suivre toutes les redirections jusqu'à obtenir un 200 OK.

## Bonus 2
JWT (JSON Web Token)
```
xxxxx.yyyyy.zzzzz
│       │       └─ Signature : HMAC proof the server issued this
│       └───────── Payload  : your claims (who you are, when it expires)
└───────────────── Header   : algorithm (HS256) and type
```
à injecter en header : -H "Authorization: Bearer $TOKEN"

## Bonus 3
Le JSON peut être difficile à lire dans un terminal.

- `| jq`    : permet de filtrer et extraire les données voulues
- `.`       : The JSON input.
- `.[]`     : Unpacks an array to process elements individually.
- `select`  : Filters for objects where `.name == "Alice"`.
- `.uuid`   : Extracts the `uuid` field.
- `-r`      : Outputs raw text (no quotes). 