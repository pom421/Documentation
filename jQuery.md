##Tips jQuery

**Références**
- [jQuery Learning Center](http://learn.jquery.com/)

**Que peut-on mettre dans la partie $() ?**

- un sélecteur CSS 
- du code HTML (permettant par exemple d'ajouter ce code à un noeud du DOM par le biais d'append)
- du code js

**Comment gérer le mode noConflict du $ jQuery?**

Le raccourci $ de jQuery peut provoquer des conflits avec d'autres librairies qui utilisent également le $. 

Voici l'idiome qui permet de gérer ces cas de conflits : 

````js
<script type="text/javascript" src="other_lib.js"></script>
<script type="text/javascript" src="jquery.js"></script>
<script type="text/javascript">
$.noConflict();
jQuery(document).ready(function($) {
// Code that uses jQuery's $ can follow here.
});
// Code that uses other libraries $ can follow here.
</script>
````

**Comment récupérer un sous-élément à partir d'un élément?**

Ex : récupérer les images sous un div avec l'id main
````js
$('img', '#main)
````
