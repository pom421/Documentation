Docker

Référence Dockerfile : http://docs.docker.com/reference/builder/
Guide pour OS X : http://blog.javabien.net/2014/06/05/a-better-boot2docker-on-osx/


- docker build : exécute les infos à partir du Dockerfile et télécharge les prérequis pour faire une image de container
- docker run : lance le container
- docker images : montre toutes les images crées (l'image chargée par un run plus celles héritées)
- docker ps

**Faux-ami**

- RUN lance une commande au moment du build
- CMD (ou ENTRYPOINT) lance une commande au moment du lancement de l'image

**OS X**

Boot2docker est un utilitaire pour OS X. C'est un outil qui lance une VM sous Virtual Box avec une image Linux de quelques dizaine de Mo. 

Workflow

  # pour lancer la VM
  boot2docker up
  
  # pour avoir le status running ou pas
  boot2docker status
  
  # pour pouvoir voir les fichiers se trouvant dans la VM
  boot2docker ssh
  
  # si problème # /var/run/docker.sock: no such file or directory. 
  $(boot2docker shellinit) # permet d'écrire les variables d'environnement DOCKER_HOST, etc..
  
  # voir les différents containers en cours d'exécution
  docker ps
  
  # Attention : il faut pointer vers la VM boot2docker à partir de son Mac (indirection)
  curl $(boot2docker ip):49154 # 49154 est un exemple, prendre celui trouvé avec docker ps

  
