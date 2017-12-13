# MongoDB

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

# changer en ajoutant s'il n'existe pas les infos du film Alien
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
```

NB: marche aussi sans set
