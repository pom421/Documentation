# MongoDB

## Installation
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


