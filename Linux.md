##Commandes

Les commandes ont toujours accès à 3 flux toujours ouverts : 
- stdin : avec le file descripteur 0
- stdout : 1
- stderr : 2

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

  ````
  find / -size +10M -exec command {} ';'
  
  # les fichiers modifiés aujourd'hui
  find / -mtime 0
  
  # les fichiers accédés depuis moins d'une heure
  find / -atime -1h
  ````
  
