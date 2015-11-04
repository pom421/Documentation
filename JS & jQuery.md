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
