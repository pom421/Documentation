Voir http://wiki.bash-hackers.org/start : montre toutes les syntaxes {}, [], [[ ]], etc.. 

### Quote

- ' est le quoting fort : rien n'est interprété
- " est le quoting faible : seules les expansions de paramètres $toto et de commande $(date) sont interprétées, tout le reste comme les espaces, tabulation, retour chariot.. sont préservés. Pas de SHELL GLOBBING effectué.
- ‘ la quote inversé ne permet pas de préserver les caractères mais de susbstituer une commande (mais il est préconisé maintenant de passer par la syntaxe $(date)

Il existe 3 façons de préserver ou échapper des caractères : 
- la quote ' et la double quote " définissent un début et une fin de zone où les caractères ne doivent pas être interprétés
- la backslash \ permet de préserver uniquement le caractère qui suit

### Les 8 sortes d'expansion (ou développement) du bash (dans l'ordre)

- Expansion d'accolade {1..10}, {1..10..2} ou {a..g} 
- Expansion de tilde ~ 
- **Expansion paramètre et variable du shell $var ou ${var}**
- **Expansion de commande $(commande)**
- Expansion arithmétique $(( 10 * 4 )) 
- Expansion de processus
- Expansion de coupure de mot (IFS par défaut = espace ou tab pour séparer les mots entre eux)
- Expansion de chemin ./toto/*.txt ou ./toto/file?.txt  == SHELL GLOBBING

En gras, ce qui est interprété par ".

Comme les expansions sont ordonnés, {0..$var} est impossible puisque l'opérateur accolade doit être étendu d'abord. Pour passer outre cette dernière limitation, il faut passer par le hack suivant : 
 
 ````sh
 eval echo {0..$var}
 ````
 
### Debug

Pour débuguer comment se déroule l'expansion : 
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

## Commandes

Les commandes ont toujours accès à 3 flux toujours ouverts : 
- /dev/stdin : avec le file descripteur 0
- /dev/stdout : avec le file descripteur 1
- /dev/stderr : avec le file descripteur 2

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
- attribut -ctime : la date de dernier changement des méta data de l'inode (ex: changement permission, etc..) Au début, la date de création du fichier. 
- attribut -atime : la date de dernier accès
- attribut -mtime : la date de dernière modification

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
 find . -iname "*.txt" -print | grep "toto.*.txt"
 
 # tous les fichiers dont les fichiers contiennent un certain motifs (-print0 et xargs -0 permettent de gérer les espaces dans les noms de fichier
 find . -name "*.txt" -print0 | xargs -0 grep "beau"
 
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

### Expression arithmétique et expansion arithmétique

L'opérateur (( )) dit à Bash qu'on passe sur des opérations numériques et non plus chaîne de caractères. 
(( )) permet d'évaluer une expression arithmétique et de rendre 1 en cas d'erreur et 0 en cas de succès. Il faut le voir comme une sortie de commande Linux. Il est souvent utilisé pour valoriser une variable (équivalent du let). Cette variable ne doit pas être préfixée par $. Attention, pour rendre une valeur arithmétique, il faut passer par $(( )). 

Voici un exemple :

```
flag=0
while read line; do
 if [[ $line == *err* ]]; then flag=1; fi
done < test.txt
if ((flag)); then echo "Erreur(s) trouvée(s)"; else echo "Pas d'erreur dans le fichier";fi

((abs = a > 0 ? a : -a)) # valorise la variable abs
ou
abs=$(( abs = a > 0 ? a : -a ))
```

Cela ne fonctionne que sur les nombres. On ne peut utliser une ternaire pour affecter une chaîne.

À noter qu'à l'intérieur de (( )), on se retrouve dans un environnement proche du C. On peut alors ajouter des espaces entre les opérateurs sans qu'ils soient pris en compte.

### Typeset

Typeset permet de définir une variable et son type (-a pour un tableu, -f pour une fonction).
Dans une fonction, définir une variable avec typeset rend la variable locale à la fonction. On peut aussi utiliser le mot clé local pour cela.


Référence :
- http://wiki.bash-hackers.org/commands/classictest
- getopts : http://wiki.bash-hackers.org/howto/getopts_tutorial
- récupérer les arguments : http://mywiki.wooledge.org/BashFAQ/035
- http://mywiki.wooledge.org/BashGuide/CompoundCommands#Arithmetic_Evaluation
