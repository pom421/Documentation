##Commandes classiques Git & GitHub

**Références**
- http://git-scm.com/docs : Référence du site officiel
- http://sklise.com/2012/09/22/introduction-to-git/ : la base de git
- http://zachbruggeman.me/github-for-cats/ : la base de github
- http://gitref.org/index.html : référence git selon github (top!)
- https://www.grafikart.fr/formations/git/git-flow : Une vidéo issue du tutoriel de Grafikart sur Git
- https://www.atlassian.com/git/tutorials/setting-up-a-repository : Par Atlassian, créateur de BitBucket. De bonnes ressources pour migrer de SVN à Git (dont les workflow possibles)

**Convention**

	origin : nom du remote repo par convention
	master : branche par défaut
	upstream : nom de la remote d'un repo forké 
	origin/master : branche master du remote repo sur GH. git status se compare par rapport à elle

**Diagnostic**

- lister toutes les remotes : git remote -v

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
# éventuellement on a besoin de faire : git pull origin master
git push origin autre-fonctionnalite # On pousse au serveur la nouvelle branche sur la remote de GH
// pull request sur GH
git checkout master # on revient sur la branche master
git merge autre-fonctionnalite # on fusionne 
git branch -D autre-fonctionnalite # on supprime la branche en local
git push origin --delete autre-fonctionnalite # on pousse les modifications en remote et suppression de autre-fonctionnalite
```
**Divers**

```sh
# pour savoir les fichiers trackés (cad stage) et ceux qui ne le sont pas
git status
# pour voir l'historique des commits
git log
```

Permet d'avoir des infos importantes comme la branche en cours d'utilisation ou les fichiers non commits

**Pour créer un remote repo**

Si l'on veut créer un remote repo soi-même (plutôt que créer un repo sur Github puis git clone), on doit spécifier que ce dernier ne doit pas avoir de working tree. 
C'est à dire qu'on ne pourra pas faire de git add ou de git commit et le seul moyen de faire évoluer ce repo se fera par des git push. Pour cela il faut faire un bare repo. 

```sh
# par convention, on suffixe par .git
cd ../projet_remote.git
git --bare init
```

**Pour créer un site documentaire**

Par convention, on crée une branche gh_pages. Cette branche n'a aucun lien avec les autres branches (master, etc..) car elle ne contient pas d'artefacts de code mais des pages de documentation. Notez pour cela l'utilisation de --orphan.

```sh
git checkout --orphan gh-pages
# Creates our branch, without any parents (it's an orphan!)
Switched to a new branch 'gh-pages'
git rm -rf .
# Remove all files from the old working tree
rm '.gitignore'
echo "My Page" > index.html
git add index.html
git commit -a -m "First pages commit"
git push origin gh-pages
```

**Problème proxy**

```sh
git config --global http.proxy http://10.154.61.3:3128
git config --global https.proxy https://10.154.61.3:3128
```
