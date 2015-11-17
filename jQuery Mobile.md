#### Divers

- theme roller : https://themeroller.jquerymobile.com/


- data-theme : permet de préciser la nuance (entre a et z) du thème créé par 
- font squirrel : pour transfrormer une police en police css

- font awesome ou glyphicons (mais ce dernier n'est pas SVG en mode libre de droit sauf si intégré avec Bootstrap)

- https://build.phonegap.com : gratuit si projet public

- phone gap developer : app smarpthone pour récupérer des app de Phone gap build

- chrome inspect (faire chrome://inspect) pour faire du remote debugging (utiliser Safari pour une app iPhone)

- native droid : material design Android pour jQuery Mobile

- framework 7 : "successeur" de jQuery Mobile
 
- codiqa : pour faire un prototype rapide en jQuery Mobile (payant)

- class=ui-state-persist pour conserver le focus quand on clique sur un navbar (ne pas intégrer un navbar dans un footer...)

- Nested view : ne fonctionnent plus depuis la version 1.4

- JQM prend la main après l'affichage pour y mettre son traitement. Par exemple, cela a pour effet de faire perdre le focus à un input. Pour le fixer à nouveau, soit récupérer après l'évènement pagecreate, cad pageshow. Si ça ne marche pas : 

````
  setTimeout(function(){
    $(".ui-filterable input").focus();
  },0);
````

- table : data-role="table" data-mode="reflow" class="ui-responsive">

####Ionic

- ionic package : permet de créer une app pour Android ou iOs
- ionic creator : outil pour faire des maquettes (// codiqa)
- ionic app view : app servant de container sur ses apps dans le cloud
- ngcordova : plugins cordova wrappés en angular
