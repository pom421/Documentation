Docker

Référence Dockerfile : http://docs.docker.com/reference/builder/


- docker build : exécute les infos à partir du Dockerfile et télécharge les prérequis pour faire une image de container
- docker run : lance le container
- docker images : montre toutes les images crées (l'image chargée par un run plus celles héritées)
- docker ps

**Faux-ami**

- RUN lance une commande au moment du build
- CMD (ou ENTRYPOINT) lance une commande au moment du lancement de l'image
