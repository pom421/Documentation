##Commandes

Les commandes ont toujours accès à 3 flux toujours ouverts : 
- stdin : avec le file descripteur 0
- stdout : 1
- stderr : 2

Si l'on veut envoyer le retour d'une commande et que les erreurs soient ignorées : 
ma-commande > resultat.txt 2>/dev/null

Si l'on veut que les erreurs soient dans le fichier final : 
ma-commande >& resultat.txt
ou 
ma-commande > resultat.txt 2>&1
