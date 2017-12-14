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
