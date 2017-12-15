# MongoDB

Outil web : http://genghisapp.com/

## Installation

```
mongo # client
mongod # serveur
/etc/mongod.conf # fichier de configuration
service mongod start
service mongod stop

mongod --dbpath /mnt/libre/db # utilisation d'un répertoire de stockage (plutôt que celui en fichier de configuration)
use formation # création ou utilisation d'une base de données
db.movies.insert(doc) # ajout un document JSON dans une collection movies. Si la collection n'existe pas, il la crée
show dbs # affiche la liste des bases de données
show collections # affiche la liste des collections pour la base de données sélectionnée par use

```

## Read

```
db.movies.find()
db.movies.find().pretty()
# affiche le 1er élément et applique un pretty
db.movies.findOne()
# affiche les 5 premiers éléments
db.movies.find().limit(5).pretty()

```

## Update

Utiliser l'opérateur $set pour mettre à jour un champ. Sinon, l'objet sera mis exactement à l'état de l'objet envoyé à update

```
# changer l'année du film Vertigo
db.movies.update({title: "Vertigo"}, {$set: { year: 1959 }}) 

# changer en ajoutant s'il n'existe pas les infos du film Alien (marche aussi sans set)
db.movies.update({ title: "Alien" }, 
  {$set:
    { 
      year: 1979,
      country: "USA",
      genre: ["science-fiction"],
      director: 
        {
          "last-name": "Scott", 
          "first-name": "Ridley",
          "birth-date": "1937-11-30",
          "nationality": "GB"
        }
    }
  }, 
{ upsert: true })

# Ajouter pour tous les films avec la country USA un champ oscars
db.movies.update(
	{ country: "USA" },
  { $set: 
  	{
    	oscars: 1
    }
  },
  { multi: 1}
)

# incrémenter le nombre d'oscar de 3
db.movies.update(
	{ country: "USA" },
  { $inc: 
  	{
    	oscars: 3
    }
  },
  { multi: 1}
)

# supprimer le champ oscars
db.movies.update(
	{ country: "USA" },
  { $unset: 
  	{
    	oscars: ""
    }
  },
  { multi: 1}
)

# ajouter un tableau vide actors à Alien
db.movies.update(
	{ title: "Alien" },
  { $set: 
  	{
    	actors: []
    }
  },
  { multi: 1}
)

# ajouter un actor à Alien
db.movies.update(
	{ title: "Alien" },
  { $push: 
  	{ actors: 
		{
		"first-name": "Sigourney",
		"last-name": "Weaver",
		"birth-date": new Date("1949-10-08"),
		nationality: "GB",
		role: "Ellen Ripley"
	      }
    }
  }
)

# modifier la nationalité du 1er actor pour le film Alien
db.movies.update(
  { title: "Alien" },
	  { $set: 
		{ "actors.0.nationality" : "FR" }
	  }
)

# supprimer un élément de tableau actors
db.movies.update(
  { title: "Alien" },
	  { $pop: 
		{ "actors" : 1 }
	  }
)

+ opérateur $each pour ajouter un tableau à un tableau existant. Si on fait un push sans $each, cela ajoute un sous tableau au tableau !!
+ opérateur $pull pour supprimer un élément dans un tableau avec un critère

# supprimer l'actor qui a comme first-name Tom
db.movies.update(   { title: "Alien" },   { $pull:  { "actors" : { "first_name": "Tom" } }   } )

# pour restaurer un fichier bson (le serveur doit être lancé)
mongorestore -d formation -c movies movies.bson

# affiche les title (ainsi que _id par défaut)
db.movies.find({year: 1997}, { title: "" ).pretty()
# pour supprimer de l'affichage _id
db.movies.find({year: 1997}, { title: 1, _id: 0 }).pretty()


db.movies.find(
	{ year: {
  		$in: [1997, 1998, 1999, 2000]
  	}
  },
  { title: "" }
)

# trouver tous les films de 1997 à 2000
> db.movies.find({ year: { $gte: 1997, $lte: 2000 }}, { title: "", year: "", _id: 0 }).sort({ year: 1 })

# tri sur annee (rétroactivement) puis par titre
> db.movies.find({ year: { $gte: 1997, $lte: 2000 }}, { title: "", year: "", _id: 0 }).sort({ year: -1, title: 1 })

# retrouver les films de 1997 de genre drama
> db.movies.find({$and: [{ year: 1997}, { genre: "drama" }]}, { title: "" }).sort({ year: -1, title: 1 })

# ou bien 
> db.movies.find({ year: 1997, genre: "drama"}, {title: ""})

# trouver les films entre 1997 et 2000 de genre drama
> db.movies.find({ year: { $gte: 1997, $lte: 2000 }, genre: "drama"}, {title: ""})

# trouver tous les films après 2000
> db.movies.find({ year: { $gt: 2000 }}, {title: "", year: "", _id: 0}).sort({ year: 1, title: 1 })

# opérateur not
> db.movies.find({ year: { $not : { $gt: 2000 } }}, {title: "", year: "", _id: 0}).sort({ year: 1, title: 1 })

# trouver les films avec Bruce Willis en tant qu'acteur 
> db.movies.find({ "actors.last_name": "Willis", "actors.first_name": "Bruce"  }, { title: 1, year: 1}).sort(title: 1)

# avec regex implicite
> db.movies.find({ title: /matri*/i }, { title: 1 })
> db.movies.find({ title: /matrix^/i }, { title: 1 })

# avec regex explicite
db.movies.find({title: { $regex: "matrix", $options: 'i'}}, { title: 1 })

# opérateur exists permet ici de ne prendre que les documents qui ont bien un champ summary mais qu'il est null. 
# NB : après un unset, le champ est complètement supprimé (il n'est pas à null)
> db.movies.find({ summary: { $exists: 1, $eq: null }}, { title: 1, year: 1})

# récupère les documents dont le champ summary n'existe pas
> db.movies.find({ summary: { $exists: 0 }}, { title: 1, year: 1})

# ajoute un champ oscar sans valeur pour l'id 27 (on peut mettre aussi bien null que undefined)
> db.movies.update({ "_id": 27 }, { $set: { oscars: undefined } })

# ajoute un champ oscar à 1 pour l'id 31
> db.movies.update({ "_id": 31 }, { $set: { oscars: 1 } })

# incrémente oscar de 2 pour tous les films français qui ont déjà une valeur numérique ($ne: null prend bien en compte les valeurs null ou undefined)
> db.movies.update({ country: "FR", oscars: { $exists: true, $ne: null } }, { $inc: { oscars: 2 } })

# trouver les films qui commencent par le
> db.movies.find({ title: /^le/i }, { year: true, title: true, genre: true}) 

```

## MapReduce

```
> var map = function() { emit(this.genre, 1) }
> var reduce = function(key, values) { return Array.sum(values) }
> db.movies.mapReduce(map, reduce, { out: "nb_movies_by_genre" })
> show collections
movies
nb_movies_by_genre

> db.nb_movies_by_genre.find().sort({ value: 1 })
{ "_id" : "romance", "value" : 1 }
{ "_id" : "Fantastique", "value" : 2 }
{ "_id" : "Guerre", "value" : 2 }
{ "_id" : "Suspense", "value" : 2 }
{ "_id" : "Western", "value" : 3 }
{ "_id" : "ComÃ©die", "value" : 4 }
{ "_id" : "Horreur", "value" : 4 }
{ "_id" : "Thriller", "value" : 5 }
{ "_id" : "crime", "value" : 6 }
{ "_id" : "Science-fiction", "value" : 8 }
{ "_id" : "Action", "value" : 11 }
{ "_id" : "drama", "value" : 17 }

# faire un mapReduce pour avoir les films par année et par genre
> var map = function() { emit({ year: this.year, genre: this.genre }, 1) }
> var reduce = function(key, values) { return Array.sum(values) }
> db.movies.mapReduce(map, reduce, { out: "nb_movies_by_year_and_genre" })
> show collections
movies
nb_movies_by_genre
nb_movies_by_year_and_genre
> db.nb_movies_by_year_and_genre.find().sort({ value: -1 })
{ "_id" : { "year" : 1988, "genre" : "drama" }, "value" : 3 }
{ "_id" : { "year" : 1990, "genre" : "drama" }, "value" : 3 }
{ "_id" : { "year" : 1994, "genre" : "Action" }, "value" : 2 }
{ "_id" : { "year" : 1997, "genre" : "crime" }, "value" : 2 }
{ "_id" : { "year" : 1999, "genre" : "Fantastique" }, "value" : 2 }
{ "_id" : { "year" : 2003, "genre" : "Science-fiction" }, "value" : 2 }
{ "_id" : { "year" : 1954, "genre" : "Suspense" }, "value" : 1 }
{ "_id" : { "year" : 1958, "genre" : "drama" }, "value" : 1 }
{ "_id" : { "year" : 1959, "genre" : "Suspense" }, "value" : 1 }

# Version avec aggregate
> db.movies.aggregate([ { $group: { "_id": "$genre", nb_movies_per_genre : { $sum: 1 } } }, { $out: "agg_nb_movies_per_genre" } ])
> show collections
agg_nb_movies_per_genre
movies
nb_movies_by_genre
nb_movies_by_year_and_genre
> db.agg_nb_movies_per_genre.find().sort({ "nb_movies_per_genre": -1 })[0]
{ "_id" : "drama", "nb_movies_per_genre" : 17 }

# idem en filtrant sur les films après 2000
> db.movies.aggregate([ { $match: { year: { $gt: 2000}  } }, { $group: { "_id": "$genre", nb_movies_per_genre : { $sum: 1 } } }, { $out: "agg_nb_movies_per_genre_match" } ])

# idem en ajoutant pour chaque ligne le tableau des titres de film
> db.movies.aggregate([ { $match: { year: { $gt: 2000}  } }, { $group: { "_id": "$genre", nb_movies_per_genre : { $sum: 1 }, films : { $push: "$title" }}} , { $out: "agg_nb_movies_per_genre_match_push" } ])

# trouver l'année moyenne d'un genre (l'âge d'or d'un genre)
> db.movies.aggregate([ { $group: { "_id": "$genre", avg_per_year__per_genre : { $avg : "$year" }} } , { $out: "agg_avg_year_genre" } ])


```
$lookup : pseudo jointure possible entre 2 collections. Possible uniquement pour les aggrégations.

# Index

>Test sur la base sirenes de data.gouv

```
db.etablissements.createIndex({ siren: 1 })
# ne marche pas avec le fichier récupéré de data.gouv
db.etablissements.createIndex({ siren: 1, nic: 1 }, { unique: true, name: "index_siren_nic"}) 
> db.etablissements.createIndex({ "entreprise.nomen_long" : 1 }, { name: "index_nomen_long" })
> db.etablissements.find({ entreprise.nomen_long: "24EME" }).pretty()
# stats d'exécution
> db.etablissements.find({ "entreprise.nomen_long": "24EME" }).explain("executionStats")
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1,
                "executionTimeMillis" : 0,
                "totalKeysExamined" : 1, => parcourt d'un index
                "totalDocsExamined" : 1, => 
# création d'un index full text afin de rechercher (insensible à la casse, etc..)
> db.etablissements.createIndex({ "entreprise.nomen_long": "text", siren: "text" })

# l'index texte est unique par collection donc on doit supprimer l'index texte courant avant de le recréer
> db.etablissements.dropIndex("entreprise.nomen_long_text_siren_text")

# création d'un index full text sur plusieurs champs string avec un poids différent 
> db.etablissements.createIndex({ "entreprise.nomen_long": "text", "caracteristiques_economiques.libapet": "text" }, { "default_language": "french", weights: { "entreprise.nomen_long": 2 } })

> db.etablissements.find({ $text: { $search: "24eme" }}, { siren: true, nic: true, caracteristiques_economiques.libapen })

# recherche avec casse exacte
> db.etablissements.find({ $text: { $search: "24EME", $caseSensitive: true }}, { siren: true, nic: true, "entreprise.nomen_long": true, "caracteristiques_economiques.libapet": true })

# récupération d'un élément sur l'index text (comme il n'y en a qu'un, pas besoin de préciser le champ du document) et affichage du score trouvé
> db.etablissements.find({ $text: { $search: "24eme"}}, { "entreprise.nomen_long": 1, score: { $meta: "textScore" }}).sort({ score: { $meta: "textScore" } })

```

##Indexation géospatiale

API du gouvernement qui rend une cordonnée GPS à partir d'une adresse postale (utilise BANO)

http://api-adresse.data.gouv.fr/search/?q=11%20rue%20du%20faubourg%20poissini%C3%A8re%2075005%20paris

```
# création d'un index géospatial de type 2dsphere (amélioration par rapport à Mongo 2.2 qui utilisait 2d, coordonnées à plat)
# sparse à true veut dire qu'on n'indexe pas les documents qui ont des champs à null
> db.etablissements.createIndex({ localisation: "2dsphere", "caracteristiques_economiques.apet700"}, { sparse: true })

# récupération des restaurants les plus proche de notre coordonée GPS du 9ème arrondissement dans un rayon de 100m
# 5610A", "5610B", "5610C sont les champs activités définissant des restaurants de type fast food à gastronomique
> db.etablissements.find({ localisation: { $near: { $geometry: { type: "Point", coordinates: [2.348752, 48.876626] }, $maxDistance: 100 } }, "caracteristiques_economiques.apet700": { $in: [ "5610A", "5610B", "5610C" ]} }).pretty()

http://geojson.io => permet de voir sur une carte des points au formation GeoJSON
```
# Réplication

```
# simulation sur un même ordi de 3 noeuds faisant partie du replica set formation (non encore créé)
mongod --port 27017 --replSet formation --dbpath /mnt/libre/db1
mongod --port 27018 --replSet formation --dbpath /mnt/libre/db2
mongod --port 27019 --replSet formation --dbpath /mnt/libre/db3

# connexion avec un client sur le 1er noeud
mongo --port 27017
# création du replica set
> rs.initiate()
formation:SECONDARY>  # changement du prompt. Il se met par défaut en tant que noeud secondaire
# après le hearbeat, il se rend compte qu'il est seul et il se met donc en tant que noeud primaire
formation:PRIMARY> 
# ajout des autres noeuds en tant que réplicat
formation:PRIMARY> rs.add("poste102:27018")
formation:PRIMARY> rs.add("poste102:27019")

formation:PRIMARY> rs.status()
{
        "set" : "formation",
        "date" : ISODate("2017-12-15T13:14:01.422Z"),
        "myState" : 1,
        "term" : NumberLong(1),
        "heartbeatIntervalMillis" : NumberLong(2000),
        "optimes" : {
                "lastCommittedOpTime" : {
                        "ts" : Timestamp(1513343634, 1),
                        "t" : NumberLong(1)
                },
                "appliedOpTime" : {
                        "ts" : Timestamp(1513343634, 1),
                        "t" : NumberLong(1)
                },
                "durableOpTime" : {
                        "ts" : Timestamp(1513343634, 1),
                        "t" : NumberLong(1)
                }
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "poste102:27017",
                        "health" : 1,
                        "state" : 1,
                        "stateStr" : "PRIMARY",
                        "uptime" : 993,
                        "optime" : {
                                "ts" : Timestamp(1513343634, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2017-12-15T13:13:54Z"),
                        "electionTime" : Timestamp(1513342932, 2),
                        "electionDate" : ISODate("2017-12-15T13:02:12Z"),
                        "configVersion" : 3,
                        "self" : true
                },
                {
                        "_id" : 1,
                        "name" : "poste102:27018",
                        "health" : 1,
                        "state" : 2,
                        "stateStr" : "SECONDARY",
                        "uptime" : 195,
                        "optime" : {
                                "ts" : Timestamp(1513343634, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDurable" : {
                                "ts" : Timestamp(1513343634, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2017-12-15T13:13:54Z"),
                        "optimeDurableDate" : ISODate("2017-12-15T13:13:54Z"),
                        "lastHeartbeat" : ISODate("2017-12-15T13:14:00.246Z"),
                        "lastHeartbeatRecv" : ISODate("2017-12-15T13:14:00.241Z"),
                        "pingMs" : NumberLong(0),
                        "syncingTo" : "poste102:27017",
                        "configVersion" : 3
                },
	...

# connexion sur un noeud secondaire
mongo --port 27019
formation:SECONDARY> show dbs
2017-12-15T14:31:03.465+0100 E QUERY    [thread1] Error: listDatabases failed:{
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotMasterNoSlaveOk"
} :
formation:SECONDARY> show dbs
admin      0.000GB
formation  0.000GB
local      0.000GB

# il faut activer la lecture sur le noeud secondaire
formation:SECONDARY> rs.slaveOk()

# création d'un arbitre
root@poste102:~# mongod --port 27020 --replSet formation --dbpath /mnt/libre/dbArbitre/
# sur le primaire, ajout de l'arbitre (boolean isArbitre en dernière position de add)
formation:PRIMARY> rs.add("poste102:27020", true)
formation:PRIMARY> rs.status()
                {
                        "_id" : 3,
                        "name" : "poste102:27020",
                        "health" : 1,
                        "state" : 7,
                        "stateStr" : "ARBITER",
                        "uptime" : 12,
                        "lastHeartbeat" : ISODate("2017-12-15T13:39:06.139Z"),
                        "lastHeartbeatRecv" : ISODate("2017-12-15T13:39:03.153Z"),
                        "pingMs" : NumberLong(0),
                        "configVersion" : 4
                }


```
