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

## Updage

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
```

