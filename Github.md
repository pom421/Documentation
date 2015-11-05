##Commandes classiques Git & GitHub

**Références**
- http://git-scm.com/docs : Référence du site officiel
- http://sklise.com/2012/09/22/introduction-to-git/ : la base de git
- http://zachbruggeman.me/github-for-cats/ : la base de github
- http://gitref.org/index.html : référence git selon github (top!)

**Convention**

	Origin : nom de la remote par défaut d'un repo
	Master : branche par défaut
	Upstream : nom de la remote d'un repo forké 
	origin/master : branche principale de la remote sur GH. git status se compare par rapport à elle

**Workflow initial via un nouveau repo Github**

```sh
touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/pom421/FormationJS.git
git push -u origin master
```

**Workflow personnel**

```sh
git remote add origin https://github.com/pom421/hello-world.git
git branch super-fonctionnalite
git checkout super-fonctionnalite # faux-ami SVN : permet de se positionner sur la nouvelle branche
touch toto.sh
git add . # faux ami SVN : à faire même pour un fichier déjà connu du repo
git commit -m "ajout du script toto" # ou git -a commit -m "blabla" pour éviter ligne précédente
git checkout master # on se repositionne sur la branche master
git merge super-fonctionnalite # merge des 2 branches
git branch -D super-fonctionnalite # suppression en local
git push origin --delete super-fonctionnalite # suppression en remote
```
------------
**Workflow Fork & pull**

```sh
// Fork sur GH via le bouton Fork en haut à droite
git clone https://github.com/pom421/patchwork.git
git branch autre-fonctionnalite # création d'une nouvelle branche
git checkout autre-fonctionnalite # autre-fonctionnalite devient la branche courante
touch titi.sh
git commit -a -m "ajout de fonctionnalite"
git pull # pour récupérer les modifications du repo distant
git push origin autre-fonctionnalite # On pousse au serveur la nouvelle branche sur la remote de GH
// pull request sur GH
git checkout master # on revient sur la branche master
git merge autre-fonctionnalite # on fusionne 
git branch -D autre-fonctionnalite # on supprime la branche en local
git push origin --delete autre-fonctionnalite # on pousse les modifications en remote et suppression de autre-fonctionnalite
```
**Divers**

```sh
git status
```

Permet d'avoir des infos importantes comme la branche en cours d'utilisation ou les fichiers non commits

**Problème proxy**

```sh
git config --global http.proxy http://10.154.61.3:3128
git config --global https.proxy https://10.154.61.3:3128
```
