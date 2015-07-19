
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
- soit faire une démarcation dans le script set +x ... set -x
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
  
