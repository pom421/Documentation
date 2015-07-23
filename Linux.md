### Quote

3 quote : 
- ' est le quoting fort : rien n'est interprété
- " est le quoting faible : seules les expansions de paramètres ($toto) et de commande ($(date)) sont interprétés, tout le reste comme les espaces, tabultation, retour chariot.. sont préservés
- ‘ la quote inversé ne permet pas de préserver les caractères mais de susbstituer une commande (mais il est préconisé maintenant de passer par la syntaxe $(date)

Il existe 3 façons de préserver des caractères : 
- la quote ' et la double quote " définissent un début et une fin de zone où les caractères ne doivent pas être interprétés
- la backslash \ permet de préserver uniquement le caractère qui suit

### Les 8 sortes d'expansion (ou développement) du bash (dans l'ordre)

- Expansion d'accolade {1..10}, {1..10..2} ou {a..g} (pas interprété par le caractère d'échappement ")
- Expansion de tilde ~ (pas interprété par ")
- **Expansion paramètre et variable du shell $var ou ${var} (interprété par ")**
- **Expansion de commande $(commande) (interprété par ")**
- Expansion arithmétique $(( 10 * 4 )) (pas interprété par ")
- Expansion de processus
- Expansion de coupure de mot (IFS par défaut = espace ou tab pour séparer les mots entre eux) (par interprété par " c'est à dire que les espaces sont laissés tels quels)
- Expansion de nom de fichier ./toto/*.txt ou ./toto/file?.txt (par interprété par ")

L'ordre veut dire que {0..$var} est impossible puisque l'opérateur accolade doit être étendu d'abord.  
Pour passer outre cette dernière limitation, il faut passer par le hack suivant : 

 eval echo {0..$var}

### Debug

Pour bien comprendre comment se déroule l'expansion : 
- soit lancer le script avec les commandes sh -x ou sh -v (-x permet de voir toutes les transformations)
- soit faire une démarcation dans le script set -x ... set +x
- soit dans le shebang ajouter l'otpion -x ou -v

### Expansion de parametres 

http://wiki.bash-hackers.org/syntax/pe

- ${#PARAMETER} : longueur de la valeur
- ${PARAMETER^} : pour mettre le 1er caractère en majuscule (^^ pour toute chaîne)
- ${PARAMETER,} : pour mettre le 1er caractère en minuscule (,, pour toute la chaîne)
- ${PARAMETER~} : pour inverser la casse (~~ pour toute la chaîne)
- ${PARAMETER/PATTERN/STRING} : cherche et remplace (// pour l'ensemble de la chaîne)
- ${PARAMETER:=WORD} : valeur par défaut si variable inexistante ou vide et assigné à la variable
- ${PARAMETER-WORD} : renvoie la valeur par défaut si la variable est inexistante ou vide
- ${PARAMETER:OFFSET:LENGTH} : renvoie la chaîne à partir de OFFSET (1er caractère = 0)
- ${PARAMETER:OFFSET:LENGTH} : idem sur x caractères

 

##Commandes

Les commandes ont toujours accès à 3 flux toujours ouverts : 
- /dev/stdin : avec le file descripteur 0
- /dev/stdout : 1
- /dev/stderr : 2

Si l'on veut envoyer le retour d'une commande et que les erreurs soient ignorées : 

  ````sh
  ma-commande > resultat.txt 2>/dev/null
  ````

Si l'on veut que les erreurs soient dans le fichier final : 
  
  ````sh
  ma-commande >& resultat.txt
  ou 
  ma-commande > resultat.txt 2>&1
  ````

Les caractères de recherches sont ?, * et la parenthèse droite []. 
  ````
  ls pierre-[a-q]livier.txt
  ls pierre-[!stuv]livier.txt
  ````
La manière la plus concise de créer un fichier :
  ````
  > monfichier.txt
  ````


  
**Find**

  ````sh
  find . -iname "*.swp" -exec rm {} ';'
  ou pour avoir une confirmation
  find . -iname "*.swp" -ok rm {} ';' 
  ````
-ctime : la date de dernier changement des méta data de l'inode (ex: changement permission, etc..) Au début, la date de création du fichier. 
-atime : la date de dernier accès
-mtime : la date de dernière modification

-size : avec date en byte c, k, M ou G. Avec + ou - pour donner des intervalles
-type f ou d : pour le type soit fichier soit répertoire
-iname : comme -name mais insensible à la casse

  ````
  find / -size +10M -exec command {} ';'
  
  # les fichiers modifiés aujourd'hui
  find / -mtime 0
  
  # les fichiers accédés depuis moins d'une heure
  find / -atime -1h
  ````

 ````sh
 # compter les répertoires
 find . -type d -maxdepth 0 | wc -l
 # compter les fichiers 
 find . -type f -maxdepth 0 | wc -l

 # compter les répertoires récursivement 
 find . -type d | wc -l
 # compter les fichiers récursivement
 find . -type f | wc -l
 
 # taille d'un répertoire 
 du -sh . 
 # taille de tous les sous répertoires 
 du -sh *
 
 # retrouver les fichiers de plus de 150M avec xargs
 find . -size +150M -type f -print0 | xargs -0 ls -al # noter le -print0 et le -0 permettant d'éviter les pb avec les fichiers ayant un caractère quote
 
 # tous les fichiers qui contiennent un certain motifs (-iname pour case insensitive)
 mac:tmp pom$ find . -iname "*.txt" -print | grep "toto.*.txt"
 
 # tous les fichiers dont les fichiers contiennent un certain motifs (-print0 et xargs -0 permettent de gérer les espaces dans les noms de fichier
 mac:tmp pom$ find . -name "*.txt" -print0 | xargs -0 grep "beau"
 
 # tous les fichiers qui ont un certains motifs et contiennent un certain motif
 find . -print0 | grep  -z '\.txt' | xargs -0 grep 'beau'
 ````
Le -z permet de mettre un octet null à la fin de chaque fichier (plutôt qu'un saut de ligne)
Le -print0 / xargs -0 permet de gérer les espaces ou saut de ligne éventuel dans le nom du fichier

Grep recherche sur des expressions régulières. 
Le bash utilise des shell globbing, i.e. *.txt est étendu par le shell comme n'importe quoi qui finit par .txt. 
Alors que pour grep, le caractère * est zéro ou n fois le caractère précédent. 

Exemple : 

````sh
find . -name "toto*.txt" # récupère toto.txt, toto1.txt, etc..
ls | grep "toto.*.txt" fichier # pour chercher le motif toto.txt, toto1.txt dans le fichier
````

