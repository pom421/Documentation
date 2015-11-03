##Tips jQuery

**Références**
- [jQuery Learning Center](http://learn.jquery.com/)

----

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

  **Remplacer tous les H2 par des H3**
  ````js
  $("h2").each(function() {
    var elt = $(this);
    elt.replaceWith("<h3>" + elt.text() +"</h3>");
  });
  ````
  **Modifier le lien d'un élement a**
  $("#lien").attr("href", "http://www.yahoo.fr");
  
  **Ajouter une ligne modèle d'un tableau à ce tableau**
  
  ````js
  function ajouterLigne() {
    var modele = $("#ma-table>tfoot>tr").clone();
      $("#ma-table>tbody").append(modele);
  }
  $("span").wrap("<b><i></i></b>");
  ````
  
  **Supprimer la ligne du tableau**
  
  ````js
    $(element).parent().remove();
    // ou mieux 
    $(element).closest("tr").remove();
    ````

  **Récupérer le 1er td d'un tableau avec clic sur un autre td de ce tableau**
  
  ````js
  var cell = $(element).closest("tr").find("td:first-of-type");
  cell.css("background-color", "red");
  ````
  
  **Ajouter un gestionnaire d'évènement click**
  
  ````js
  boutons.on("click", function supprimerLigne(event) {
   $(this).closest("tr").remove();
  });
  ````
  
  **Cloner un élément invisible du tableau et cloner son gestionnaire d'évènement**
  
  ````js
  $("#ajouterLigne").on("click", function ajouterLigne(evenement) {
      // clone(true) permet de copier le gestionnaire d'évènements en plus des éléments HTML
      $("#maTable").append($("#maTable>tfoot>tr").clone(true));
  });
  ````
  
  **Toggle de class**
  
  ````js
  // il est possible de toggler sur plusieurs classes. Penser à initialiser l'élément avec une des 2 classes
  cell.toggleClass("red green");
  ````
  
  **Centralisation du gestionnaire d'évènement**
  
  ````js
  // placer le gestionnaire d'évènement ni trop bas (sur chaque ligne, pas performant) ni trop haut (bordélique?)
  // on filtre uniquement sur les éléments avec la classe del
  tbody.on("click", ".del", function supprimerLigne(e) {
     $(e.target).closest("tr").remove();
  });
  
  ````
