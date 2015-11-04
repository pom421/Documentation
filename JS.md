## JS

**Réferences**

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide : Guide JS de Mozilla
- http://eloquentjavascript.net/ : Eloquent JS
- https://github.com/maxogden/art-of-node : La base de Node
- http://nodejs.org/api/ : API des modules intégrés à Node
- https://github.com/tdd/node-demo : Explication Node par C. Porteneuve + liens

**JS**

6 types JS :

- numbers       
- string
- Booleans
- object
- function
- undefined values : undefined (= void java), null

Les numbers ont 3 valeurs particulières :

- Infinity et -Infinity
- NaN, une valeur number qui ne rend pas de valeur précise ou qui ait du sens (ex : 0/0, Infinity - Infinity)
Attention : NaN est la seule valeur qui rend false à console.log(NaN == NaN)
    
console.log(typeof 4.5);

Idiome :

    // court-circuit
    console.log(null || "toto") // -> toto


**Node**

**Récupération des arguments du programme node**

process.argv
- 0 : l'exécutable node
- 1 : le chemin du fichier lancé
- 2 : le 1er argument
- etc..

**module**

dans le module monModule.js
````js
module.exports = function(arg1, arg2, callback) { ... };
````

dans le fichier main
````js
var fs = require('fs');
var monModule = require('./monModule');
````

**Callback**
idiome node : 
````js
fs.readdir(path, function(err, data) {
  if (err)
    return callback(err);
  // else ...
  ...
  callback(null, list);
  })
````

**Helper JS**
Dans node : tab.forEach(function(elt)), tab.filter(function(elt))
  
**Stream**

Contrairement aux autres callbacks, les callbacks des streams ne prennent qu'un argument. 
3 évènements sont alors interceptables : data, error et end. 
  ````js
  var http = require('http');
  http.get('http://www.lexpage.net', function(response) {
    response.setEncoding('utf-8');
    response.on('data', console.log);
    response.on('error', consol.error);
  });
  ````
  
**Scroller à un endroit de la page**

  ````js
  document.body.scrollTop = elementDom.offsetTop;
  ````

