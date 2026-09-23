# Inventaire des points d'entrée

Le livrable de l'atelier d'analyse. Il dit ce que l'API devra **permettre** — pas comment on
l'écrira.

Une règle : tout ce qui figure ici doit se justifier par la lettre de mission ou par un écran
des maquettes. Si vous ne savez plus d'où vient une ligne, c'est peut-être qu'elle n'en vient
pas. (Seule exception : la section bonus, si vous en ajoutez une.)

---

## Ce que l'utilisateur peut faire

Une action par ligne, en français. Pas encore de chemins.

- Compte utilisateur : un utilisateur peut créer un compte (mail, mot de passe, prénom et nom facultatifs) puis s'identifier (mail et mot de passe)
- Profil utilisateur : un utilisateur peut consulter son profil (mail, prénom, nom, date de création du profil)
- Rechercher un voyage : un utilisateur peut rechercher un voyage (ville de départ, ville d'arrivée, date, nombre de personnes à transporter) (utilisateur non connecté)
- Proposition de ville : au fur et à mesure de l'entrée de l'utilisateur, les villes disponibles lui sont proposées
- Modifier la recherche d'un voyage : un utilisateur peut modifier la recherche en cours en modifiant des données
- Visualiser la liste des voyages correspondant à une recherche : un utilisateur peut consulter la liste des voyages correspondant à sa recherche (nombre de correspondance à la recherche, date + pour chaque résultat : horaire, durée, prix, départ, arrivée)
- Visualisation du détail d'un voyage : un utilisateur peut consulter le détail d'un voyage (ville de départ, date/horaire de départ, ville d'arrivée, date/horaire d'arrivée, durée du trajet, franchise de masse, type de catapulte, consignes d'embarquement, prix du trajet total, nb de places souhaitées)
- Réserver un voyage : l'utilisateur peut enregistrer un voyage dans son panier en ajustant le nombre de voyageurs (authentification obligatoire)
- Visualisation du panier : un utilisareur peut visualiser son panier (les différents trajets réservé : ville départ, ville d'arrivée, date et heure de départ, nb de places, prix + prix total du panier)
- Suppression d'un voyage du panier : l'utilisateur peut supprimer une réservation de son panier
- Visualisation du récapitulatif du règlement : l'utilisateur peut consulter le récapitulatif d'un règlement et choisir son mode de payement (rappel des voyages avec le prix, total du prix)
- Réglement d'un ou plusieurs voyages : l'utilisateur peut régler les voyages présents dans son panier
- Visualisation de la confirmation de règlement : l'utilisateur peux visualiser la confirmation de son règlement (montant réglé, moyen de paiement, nombre de billets émis, date d'emission, détails des billets : code unique, ville de départ/arrivée, date/heure de départ, date de création, prix)
- Visualisation des billets : l'utilisateur peut consultés ses billets (code unique, ville de départ/arrivée, date/heure de départ, date de création, prix)

Règles : 
- une seule catapulte par ville
- authentification obligatoire pour la réservation d'un voyage
- chaque voyage est direct
- la masse embarquée conditionne le lancer
- un billet par voyageur (N voyageur émet N billets)
- un panier réglé n'est plus modifiable est est visible dans les billets émis
- le règlement est fictif

## Les points d'entrée

| Ce que ça fait                   | Chemin proposé                                      | Qui peut l'appeler                             |
|----------------------------------|-----------------------------------------------------|------------------------------------------------|
| Rechercher un voyage             | GET /                                               | tout le monde                                  |
| Récupérer les villes disponibles | GET /cities                                         | tout le monde                                  |
| Résultats de la recherche        | GET /search?departure=&arrival=&date=&nbPassengers= | tout le monde                                  |
| Détail d'un lancer               | GET /throws/{id}?nbPassengers=                      | tout le monde                                  |
| Connexion                        | POST /login                                         | tout le monde                                  |
| Inscription                      | POST /sign-in                                       | tout le monde                                  |
| Profil                           | GET /users/me/profils                               | utilisateur courant                            |
| Panier                           | GET /users/me/baskets                               | utilisateur courant                            |
| Paiement                         | POST /baskets/{id}/pay                              | utilisateur courant avec un panier             |
| Confirmation de paiement         | GET /purchase/{id}                                  | utilisateur courant après paiement d'un panier |
| Billets                          | GET /users/me/tickets                               | utilisateur courant                            |




## Les données qui circulent

Pour chaque point d'entrée : ce qu'il reçoit, ce qu'il renvoie. Nommez les données comme
l'Office les nomme — les traduire en identifiants techniques, c'est le travail de demain.

### GET /
requête
```json
{}
```
reponse
```json
{
  "departure" : ["Paris", "Lyon", "Marseille", "Bordeaux", "Lille", "Strasbourg", "Toulouse", "Nantes", "Dijon", "Brest"],
  "arrival" : ["Paris", "Lyon", "Marseille", "Bordeaux", "Lille", "Strasbourg", "Toulouse", "Nantes", "Dijon", "Brest"]
}
```

### GET /results?departure=&arrival=&date=&nbPassengers=
requête
```json
{
  "departure": "",
  "arrival": "",
  "date": "",
  "nbPassengers": ""
}
```

réponse
```json
{
  "0" : [
    "departure": "",
    "arrival": "",
    "time": "",
    "duration": ""
  ],
  "1" : [
    "departure": "",
    "arrival": "",
    "time": "",
    "duration": ""
  ],
  ...
}
```

### GET /throws/{id}?nbPassengers=
requête
```json
{
  "throwsId": "",
  "nbPassengers": ""
}
```
reponse
```json
{
  "departure" : "" ,
  "arrival" : "",
  "timeArrival" : "",
  "timeDeparture" : "",
  "duration" : "",
  "franchise" : "",
  "catapultName" : "",
  "throwOrder" : "",
  "price": ""
}
```

### GET /users/me/profils
requête
```json
{
  "userId": ""
}
```
reponse
```json
{
  "lastName" : "",
  "firstName" : "",
  "email" : "",
  "createdDate" : ""
}
```

## Ce dont on n'est pas sûrs

Les questions que la lettre et les maquettes ne tranchent pas. Une question notée vaut mieux
qu'une réponse inventée.

- 
