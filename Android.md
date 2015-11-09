## Formation Android 21/09

#### Considérations à prendre en compte quand on développe pour smartphone

On code avec un n° de version d'API Android. Ex : android 5.0 Lollipop -> 21.

On précise à partir de quelle version on est compatible. Ex: on compile à partir de la version 21 et on veut être compatible depuis la version 15 (Android 4.0.3) car la version 4.0 est la plus répandue dans le monde.

#### Différence entre  version smartphone & tablette

Exemple pour un parc d'attraction

- version smartphone : géolocalisation dans le parc
- version tablette : présentation du parc pour préparer et réserver ses places en amont

-innovation de la 4.4 Kit Kat : utilisation de web view en Chromium plutôt que Webkit => il faut donc avoir une cible de construction en 4.4 si l'on veut avoir le debug avec les Dev Tools


- avant la 2.2 => pas d'action bar
- avec la version 6.0 => l'action bar se retrouve dans le layout et pas configurable par le style
- la compatibilité avec des vieilles versions est assurée par AppCompatActivity pour les activités. Normalement, ça marche bien. 
- mais pour vérifier la compatibilité sur des anciens équipements : 
	- vérifier le minSdkVersion dans build.gradle
	- lancer genymotion sur un vieux téléphone (ex: samsung galaxy s2 pour une version 2.3 d'Android)
	- tester sur un téléphone physique

- émulateur Genymotion (plus rapide que celui d'Android studio)	

- pas d'utilisation de pixel mais des dp proportionnel à la résolution
ldpi, mdpi, hdpi, xhdpi => résolution

- taille maximum des applications sous Plays store < 50 Mo. Au delà, il faut télécharger le reste à partir d'une ressource sur internet (pour les jeux par exemple)

- en fonction des résolutions, on crée des répertoires différents pour les images (cf. http://developer.android.com/guide/practices/screens_support.html)

- zxing : lecture de code barre

#### Installation

- jdk 8
- android studio
- genymotion + terminal virtuel pour Nexus 4

- android studio
  - extra
    - Google Play services : notifications push
    - Google apk extension library : si son appli > 50 Mo
    - Google billing library : achat in app
    - Google USB driver : pour connecter son téléphone à android studio pour débug (faire : adb driver huawei pour le récupérer) ADB = android debug bridge

- png 24 bits transparents, gif. SVG ne fonctionne pas sous Android mais WebP

- utilisation du mécanisme 9 patch pour les fichiers png. Utilisation d'un pixel noir en haut et à gauche pour la partie étirable. Et à droite et en bas pour la partie emplacement du contenu. 
Aller dans le fichier sous drawable, faire Create 9-patch file, puis utiliser l'éditeur (cocher show patch et show content)
Bien noter les différents emplacements des points quand on a plusieures fichiers pour un bouton pressé/non pressé pour éviter les décalages

- pour les boutons pressé/non pressé il faut faire un fichier xml dans drawable qui orchestre ces 2 états. Ensuite, le layout qui va utiliser ce bouton fera juste une référence au fichier xml

- android:screenOrientation. Souvent, on met en mode portrait pour un téléphone et en mode paysage pour une tablette. Sinon, le risque est de perdre des informations d'un formulaire quand l'utilisateur fait pivoter l'appareil (pour l'éviter, il faut le gérer programmatiquement)

- android:noHistory pour une activité, permet de désactiver le retour arrière. À faire par exemple pour éviter de revenir sur le splash screen

- activity : une page
- service : en tâche de fond
- content provider : pour fournir des données à une autre application
- widget

#### Cycle de vie d'une activity 

- onCreate -> onStart -> onResume
- onResume -> onPause -> onStop (appui sur home)
- onStop -> onDestroy (appui sur retour arrière)

Les plus utilisées : onResume et onStop

- mettre l'initialisation des ressources (ex: findViewById) dans onCreate

Le système peut tuer n'importe quelle application suivant ses besoins (en commençant par l'application lancée il y a le plus longtemps)

- problème code barre : ça peut être un appareil qui n'a pas d'appareil photo sur le dos comme une tablette...

- apk : suffixe application android (ipa pour iPhone)


#### Système de remontée d'erreurs (= crash report)

- crashlytics
- crittercism
- splunk mint (ex bug sens)

- cloud test lab : permet de mettre des versions de test sur le playstore (mais moins pratique qu'envoyer au client l'application) -> équivalent de Test flight d'Apple, seul moyen de diffuser une app pour recette côté Apple!

- screenshot à faire pour le play store : cliquer sur Android device monitor (bonhomme android à droite)

#### sp et dp

- dp pour dpi est une unité de mesure qui prend en compte la résolution de l'écran
- sp est la même chose que dp + la prise en compte des préférences utilisateurs pour afficher les textes en petit, normal ou grand. 
- au final, pour les problématiques d'image, de margin, on utilise dp. Pour les problématiques de taille de texte, etc.. on prend sp. 


#### Intents & services

- un intent permet de passer d'une activité à une autre, ou d'une activité à un autre traitement (ex: envoyer un mail, télécharger un pdf, lancer l'application téléphone, etc..)
- possibilité de transférer des données entre activity ou entre activity/app par le biais de extra et bundle
- doc pour passer par l'application appareil photo pour récupérer une photo http://developer.android.com/training/camera/photobasics.html
- service : traitement en tâche de fond (ex : musique, GPS, etc..). Si la lecture de musique se faisait dans l'activité, il y aurait rupture à chaque changement d'activity


- Toast : info bulle en bas de l'écrans

- API Google developer console : ajouter un projet. On peut faire un projet test pour tester tous ses appels aux API de Google pour toutes ses applications mobiles (clé API)

- Volley android en remplacement de AsyncTask (plus rapide). À intégrer à gradle
- Picasso pour gérer les images. À intégrer à gradle
- Gson pour gérer le JSON. À intégrer à gradle

Références
- performance tips android (avoid getter setter)
- JSON View : plugin Firefox pour voir de façon formatée le JSON		
