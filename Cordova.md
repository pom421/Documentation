

### Installation

- installer git
- installer nodesj
- installer ripple-emulator : environnement pour tester une application (gestion de l'évènement deviceready, géoloc, etc..)
- Android studio : (optimisation) dans Android SDK > SDK Tools, cochez HAXM + dans AVD > Edit > Use Host GPU
- cordova create hello
- cd hello
- cordova platform add android
- (cordova run android) pour lancer sur un smartphone
- cordova platform add browser
- cordova emulate browser # lance un serveur sur le port 8000
- cordova serve # Idem. Juste pour tester rapidement (l'app cordova n'est qu'un simple navigateur mais n'est pas précis sur le rendu final natif) -> permet de tester en android, iOs, windowsPhone ou browser
- npm install -g ripple-emulator
- ripple emulate # lance un serveur sur le port 4000

### Proxies

- git config --global http.proxy http://...
- npm config set proxy http://...
- npm config set https-proxy http://...

````dos
npm init
npm install -g cordova
npm install -g ripple-emulator

npm install -g http-server
cd Formation/angular
http-server
````
### Divers
- cordova.js est le script qui permet de déclencher l'évènement deviceready qui est spécifique au device. Le fichier cordova.js se retrouve par platform dans platform_www. Dans le fichier html il faut ajouter la ligne 

````html
<script src="cordova.js"></script>
````

- telnet : Panneau de config> Programme > Telnet
  - telnet localhost 5554 -> point d'arrêt pour l'émulateur (geo, power, sms, gsm, etc..)
  - power ac off
  - power capacity 15

- ionic : angular + cordova
- ratchett: 
- http://mean.io/ : full stack MongodDB Express Angular NodeJS

- https://github.com/doedje/jquery.soap : SOAP pour JS
- http://deployd.com/ : moyen simple pour construire une API (WS REST)

### Cordova plugin

cordova plugin add cordova-plugin-device (on pourrait ajouter un plugin en référençant un chemin en local)

10.0.2.2:8000 -> pour aller à partir de l'émulateur sur la machine hôte (car il y a une VM pour l'émulateur)

### Angular

$scope est un singleton injecté par Angular. Cela est étonnant car le nom du paramètre est important. 
Du coup, cela peut poser problème si l'on passe par des outils de minifications/mochisation. 

Dans ce cas, remplacer la syntaxe par la suivante : 

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

Des attributs sont remplacés par Angular comme src (ng-src) ou href (ng-href). Ceci permet de ne pas avoir un "moment de flottement" pour le navigateur où l'image ne serait pas encore disponible tant que le contenu de l'attribut n'a pas été évalué par Angular.
