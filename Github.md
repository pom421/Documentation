##Commandes classiques Git & GitHub

**Références**
- http://git-scm.com/docs : Référence du site officiel
- http://sklise.com/2012/09/22/introduction-to-git/ : la base de git
- http://zachbruggeman.me/github-for-cats/ : la base de github

**Convention**

	Origin : nom de la remote par défaut d'un repo
	Master : branche par défaut
	Upstream : nom de la remote d'un repo forké 

**Workflow personnel**


- git remote add origin https://github.com/pom421/hello-world.git
- git branch super-fonctionnalite
- git checkout super-fonctionnalite # faux-ami SVN : permet de se positionner sur la nouvelle branche
- touch toto.sh
- git add . # faux ami SVN : à faire même pour un fichier déjà connu du repo
- git commit -m "ajout du script toto" # ou git -a commit -m "blabla" pour éviter ligne précédente
- git checkout master # on se repositionne sur la branche master
- git merge super-fonctionnalite # merge des 2 branches
- git branch -D super-fonctionnalite # suppression en local
- git push --delete super-fonctionnalite # suppression en remote


------------
**Workflow Fork & pull**

- // Fork sur GH
- git clone https://github.com/pom421/patchwork.git
- git branch autre-fonctionnalite
- git checkout autre-fonctionnalite
- touch titi.sh
- git commit -a -m "ajout de fonctionnalite"
- git pull # pour récupérer les modifications du repo distant
- git push origin autre-fonctionnalite
- // pull request sur GH
- git checkout master
- git merge autre-fonctionnalite
- git branch -D autre-fonctionnalite
- git push origin --delete autre-fonctionnalite

**Divers**

	git status

Permet d'avoir des infos importantes comme la branche en cours d'utilisation ou les fichiers non commits
