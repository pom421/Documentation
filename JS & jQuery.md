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
  ````

### Evènements

### Ajax
