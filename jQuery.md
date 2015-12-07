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
$('img', '#main')
````

  **Remplacer tous les H2 par des H3**
  ````js
  $("h2").each(function() {
    var elt = $(this);
    elt.replaceWith("<h3>" + elt.text() +"</h3>");
  });
  ````
  
  **Modifier le lien d'un élement a**
 ```js  
  $("#lien").attr("href", "http://www.yahoo.fr");
  ```

  **Ajouter une ligne modèle d'un tableau à ce tableau**
  
  ````js
  function ajouterLigne() {
    var modele = $("#ma-table>tfoot>tr").clone();
      $("#ma-table>tbody").append(modele);
  }
  $("span").wrap("<b><i></i></b>");
  ````
  
  **Supprimer la ligne du tableau**
  
  ```js
  $(element).parent().remove();
  $(element).closest("tr").remove(); // mieux
  ```

  **Récupérer le 1er td de tableau avec clic sur un autre td de ce tableau**
  
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
  
  **Comment mettre des gestionnaire d'évènement sur du contenu HTML dynamique?**

  Un corrolaire de l'item précédent est que cela permet d'ajouter un gestionnaire sur des éléments non encore existants dans le DOM. Par exemple : 

  ```js
  $(staticAncestors).on(eventName, dynamicChild, function() {});
  ```
  
  Dans cet exemple, seul staticAncestors a besoin d'être présent au moment où le gestionnaire d'évènement a été installé. 
  
  **Empêcher le fonctionnement nominal pour lien, input texte, etc..**
  
  ````js  
  // pour un lien
  $("#monUl").on("click", "a", function(e) {
      if($("#toggle").hasClass("disabled")) {
          e.preventDefault();
      }
  });
  // pour un champ texte
  $("#mon-form").on("keypress", "input", function(e) {
      if(e.charCode > 48 && e.charCode < 58) {
          console.log("on trouve ", e.charCode);
      }
      else {
          e.preventDefault(); 
      } 
          
  });
 ````
L'environnement d'exécution est général. Si 2 scripts dans la même page, ils partagent le même environnement (même variables, etc..). 
L'environnement est détruit à chaque sortie de page.

### Sélecteurs

  ````js
    $(".urgent").css("background-color", "red");
    $("span.urgent").css("background-color", "grey");
    
    $("h1,h2").css("color", "white");
    
    # descendant indirect
    $("ul.urgent>li").css("background-color", "blue");
    # descendant direct
    $("ul.urgent>li").css("background-color", "blue");
    # tous les frères de même niveau
    $("#ici~li").css("background-color", "purple");
    # le frère suivant de type li
    $("#ici+li").css("background-color", "yellow");
    
    # tous les li sous le ul sauf le 1er
    $("ul>li+li").css("background-colo", "red"); // ou $("ul>li+li").css("background-colo", "red");
    
    # sélectionne une balise avec un attribut et une valeur précise de cet attribut
    $("span[title='Alert!']").css("background-color", "green");

    # sélectionne les éléments qui contiennent le mot jour (mot = avec des espaces avant et après)
    $("span[title~='jour']").css("background-color", "blue");

    # sélectionne les éléments qui contiennent la sous-chaîne jour
    $("span[title*='jour']").css("background-color", "yellow");
    
        $("span[title!='Bonjour']").css("background-color", "grey");
    
    
    // liens
    $("a[href^='http://']").css("background-color", "red");
    $("a[href^='file://']").css("background-color", "green");
    $("a[href$='.pdf']").css("background-color", "yellow");
    $("a[href$='.jpg'],a[href$='.gif'],a[href$='.png']").css("background-color", "red");
    
   <form>
        <input type="checkbox" checked />
        <label>Ok</label>
   </form>
   
   <script>
   $("input:checked+label").css("background-color", "red");
   </script>
   
   // le 1er enfant sous #debut (ET qui doit être un li)
    $("ul#debut>li:first-child").css("background-color", "red");
    
    // le dernier enfant de #debut (ET qui doit être un li)
    $("ul#debut>:last-child").css("background-color", "yellow");

    // si l'on veut le premier td de la ligne (et que le 1er enfant est un th par exemple)
    $("#ma-table>tbody>tr>td:first-of-type").css("background-color", "green");
    
    # pour avoir les 3 1ers li en jaune, du 5 au 7 en violet et du 8 à la fin en vert
    $("#ma-liste li").css("background-color", "yellow");
    $("#ma-liste li:nth-child(4)~li").css("background-color", "purple");
    $("#ma-liste li:nth-child(7)~li").css("background-color", "green");
    
    # n = n° itération dans la boucle
    $("#ma-liste-pyjama :nth-child(2n)").css("background-color", "lightgrey");
    
    $("#liste-rainbow :nth-child(3n)").css("background-color", "blue");
    $("#liste-rainbow :nth-child(3n+1)").css("background-color", "gray");
    $("#liste-rainbow :nth-child(3n+2)").css("background-color", "purple");
 ````
 
 ````js
 
// ajoute un fils au sélecteur au début
$("monselecteur").prepend()
// ajoute un fils au sélecteur à la fin
$("monselecteur").append()
// ajoute un frère avant le sélecteur
$("monselecteur").before()
// ajoute un frère après le sélecteur
$("monselecteur").after()
 
 ````

### Evènements

### Ajax

Pour permettre à Chrome de pouvoir faire du Ajax avec le protocole file : 

Dans le raccourci Chrome, ajouter à la commande --allow-file-access-from-files.

  **Pour charger des scripts à la demande**
  $.getScript("script.js", function callback() { ..}
  
  **Appel Ajax**
  
   $.getJSON(url, function(data) {
      $("#meteo").text(data.query.results.channel.item.forecast[0].text);
  });
  
  **Méthode chaînées utilitaire en Ajax**
  
  .success().fail().always();
  
