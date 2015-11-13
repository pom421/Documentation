
### Installation

- installer git
- installer nodesj
- installer ripple-emulator : environnement pour tester une application (gestion de l'évènement deviceready, géoloc, etc..)
- Android studio : (optimisation) dans Android SDK > SDK Tools, cochez HAXM + dans AVD > Edit > Use Host GPU

- npm init
- npm install -g cordova
- npm install -g ripple-emulator # émulateur de smartphone
- npm install -g http-server # installation d'un serveur http light pour tester appels XHR
- cd Formation/angular
- http-server

#### Utilisation de Cordova

- cordova create hello
- cd hello
- cordova platform add android
- (cordova run android) pour lancer sur un smartphone
- cordova platform add browser # environnement browser mais ne permet pas de tester des plugins natifs
- cordova emulate browser # lance un serveur sur le port 8000
- cordova serve # Idem. Juste pour tester rapidement (l'app cordova n'est qu'un simple navigateur mais n'est pas précis sur le rendu final natif) -> permet de tester en android, iOs, windowsPhone ou browser
- npm install -g ripple-emulator
- ripple emulate # lance un serveur sur le port 4000
- telnet : Panneau de config> Programme > Telnet
  - telnet localhost 5554 -> point d'arrêt pour l'émulateur (geo, power, sms, gsm, etc..)
  - power ac off
  - power capacity 15 # mettre le niveau de batterie à 15% pour voir la réaction de l'application

### Proxies

- git config --global http.proxy http://...
- npm config set proxy http://...
- npm config set https-proxy http://...

### Évènement deviceready

- cordova.js est le script qui permet de déclencher l'évènement deviceready qui est spécifique au device. Le fichier cordova.js se retrouve par platform dans platform_www. Dans le fichier html il faut ajouter la ligne 

````html
<script src="cordova.js"></script>
````

Pour afficher une page faite pour cordova, il est nécessaire de lancer cordova emulate, cordova serve ou ripple emulate !!!

#### Autres frameworks/librairies

- ionic : angular + cordova + CSS
- ratchett: "framework" CSS pour mobile émulant le mode iOs ou Android
- http://mean.io/ : full stack MongodDB Express Angular NodeJS
- https://github.com/doedje/jquery.soap : SOAP pour JS
- http://deployd.com/ : moyen simple pour construire une API (WS REST)

### Cordova plugin

cordova plugin add cordova-plugin-device (on pourrait ajouter un plugin en référençant un chemin en local)

10.0.2.2:8000 -> pour aller à partir de l'émulateur sur la machine hôte (car il y a une VM pour l'émulateur)

### Angular

$scope est un singleton injecté par Angular. Cela n'est pas naturel car ici le nom du paramètre est important. 
Du coup, cela peut poser problème si l'on passe par des outils de minifications qui va remplacer un nom de paramètre signifiant par un 'a'. 

Dans ce cas, remplacer cette syntaxe par la suivante : 

````
phonecatApp.controller('PhoneListCtrl', function ($scope, $http) {
  $http.get('phones/phones.json').success(function(data) {
    $scope.phones = data;
  });

  $scope.orderProp = 'age';
});
````

````js
phonecatApp.controller('PhoneListCtrl', ['$scope', '$http',
  function ($scope, $http) {
    $http.get('phones/phones.json').success(function(data) {
      $scope.phones = data;
    });

    $scope.orderProp = 'age';
  }]);

````
- angular-ui -> liste de modules disponibles pour Angular. Voir en particulier le Router qui devrait être intégré à Angular 2

Des attributs sont remplacés par Angular comme src (ng-src) ou href (ng-href). Ceci permet de ne pas avoir un "moment de flottement" pour le navigateur où l'image ne serait pas encore disponible tant que le contenu de l'attribut n'a pas été évalué par Angular.
