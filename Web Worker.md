###Exemple de web worker 

But : donner un aperçu de l'avancement d'un traitement backoffice

Référence : 
- [Lexpage](http://www.lexpage.net/board/thread/183-ide-php/7/) 
- [MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/Performance/Using_web_workers?redirectlocale=en-US&redirectslug=DOM%2FUsing_web_workers)

----

index.html
````html
<meta charset="UTF-8"  />
<script src="lib/jquery.js"></script>

<h1>Avancement Traitement</h1>
<h2 style=color:red><span id="avancement"></span> %</h2>

<script>
var worker = new Worker("worker.js");

worker.addEventListener("message", function(event) {
	console.log("CLIENT : ", event.data);
	$("#avancement").text(event.data["avancement"]);
});

// on lance l'appel au traitement backoffice
worker.postMessage({action : "run"});
</script>
````
worker.js
````js
addEventListener("message", function(event) {
	if (event.data["action"] === "run") {
		// lancer traitement
		console.log("WORKER : traitement en cours...");
		traitement();
	}
	else {
		console.log("WORKER : demande inconnue");
	}
});

// simulation d'un traitement lourd
var NB_BOUCLES_MAX = 17;
var nbBoucles = 0;
var traitement = function traitement() {

	postMessage({avancement : Math.ceil(nbBoucles / NB_BOUCLES_MAX * 100)});
	if (nbBoucles == NB_BOUCLES_MAX ) {
		console.log("WORKER : Fin du traitement lourd");
		return;
	}
	console.log("WORKER : simulation de traitement : " + nbBoucles);
	nbBoucles++;
	setTimeout("traitement()", 1000);
};
````
