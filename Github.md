Commandes classiques

**Workflow personnel**

- git remote add origin https://github.com/pom421/hello-world.git
- git branch super-fonctionnalite
- git checkout super-fonctionnalite
- touch toto.sh
- git add .
- git commit -m "ajout du script toto"
- git checkout master
- git merge super-fonctionnalite
- git branch -D super-fonctionnalite
- git push --delete super-fonctionnalite


------------
**Workflow Fork & pull**

- // Fork sur GH
- git clone https://github.com/pom421/patchwork.git
- git branch autre-fonctionnalite
- git checkout autre-fonctionnalite
- touch titi.sh
- git commit -a -m "ajout de fonctionnalite"
- git push origin autre-fonctionnalite
- // pull request sur GH
- git checkout master
- git merge autre-fonctionnalite
git branch -D autre-fonctionnalite
git push origin --delete autre-fonctionnalite
