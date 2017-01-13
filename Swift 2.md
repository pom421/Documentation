### Swift - Romain Asnar - 13 janvier 2017

La résolution des types se fait à la compilation (et augmente le temps de compilation par rapport à Objective C)

Gestion des librairies : Pod ou Carthage (l'enfer à gérer à la main). Fichier podfile (~= package.json)

- AppDelegate.swift : le main de l'app
- ViewController.swift : le premier contrôleur
- storyboard : la partie visuelle
- Assets.xcassets : les images, etc..
- LaunchScreen.storyboard : storyboard pour le 1er écran (branding, afficher très rapidement un écran à l'utilisateur)
- info.plist : fichier de configuration
- Test.xcdatamodeld : "PhpMyAdmin" like pour Core Data 

- fichier "projet" (en cliquant directement dans le navigateur XCode sur le projet)
  - Deployment target : version minimum sur lequel on veut que ça tourne. On peut utiliser des fonctionnalités de 10 et mettre
  en Deployment target 7. Il faudra dans le code ajouter des conditions pour ne pas lancer des fonctionnalités si l'OS du
  périphérique ne les permettent pas.

- cycle de vie de l'application : quand l'application rentre en background, etc..

États : active, background, foreground ("zombie“) qui nécessite de redémarrer l'app, terminée

Sandboxing : les données de chaque app sont compartimentées. On peut faire communiquer des app entre elles par des app groups.

Les ViewController en iOS font du travail de contrôleur mais aussi une grosse partie de la vue.

MVC => problème les ViewController deviennent massifs

MVC => MVVM : le contrôleur contient un Model-View qui se limite au formatage des données du model. Cela permet également de
factoriser ce formatage entre plusieurs contrôleurs.

Viper : Nouvelle architecture pour iOS

Playground avec MVVM :
https://gist.github.com/romsi/9dee6214b06db33ac02fde0464ba0c5d

## Localization

- clic sur le projet
- en haut à gauche, clic sur la liste de choix et se positionner sur le nom du projet
- dans localizations, ajouter fr (dans Main.storyboard)

Pour externaliser même en anglais, sur le storyboard, à droite clic sur le 1er onglet et cliquer sur Localization english avec localizable strings.
tab

Interface

- Tab bar : menu du bas avec les onglets. Avantage : on sait toujours où l'on est (icone sélectionné)
- Navigation bar : menu en haut, lié à un Navigation Controller particulier
- Navigation Controller : un début de navigation où le système va gérer les retour en arrière avec l'icone <
- CollectionView : plus compliqué à gérer que TableView mais permet de faire des animations sexys comme Google Photos

## Storyboard

- Content hugging priority/Content compression resistance priority : permet de savoir qui gagne en priorité sur le contenu textuel
(par exemple 3 boutons qui s'affichent horizontalement)
- TableView & CollectionView : le CollectionView n'oblige pas à prendre toute la largeur du périphérique
- les liens (IBOutlet, segue, root view controller pour se relier à un NavigationViewController) se fait avec Ctl clic d'une view sur un VC
- il faut penser à **ajouter la classe Swift au ViewContoller** du Storyboard
- pour les **items de TableViewController** il faut penser à mettre le nom de l'item

Ajout d'un Navigation Controller 
- on crée un début de navigation, i.e. une suite d'écran qui vont naviguer les uns après les autres et la possibilité de revenir. 
Les classes Swift qu'on va coder vont correspondre aux écrans, pas au Navigation Controller lui même qui est géré par le système.
La flêche (entry point) pourrait très bien être placé directement sur le premier écran et pas sur le Navigation Controller mais 
on perdrait alors la navigation.

Il faut ajouter un segue de type "root view controller" entre le Navigation controller et le Table view controller" (clic sur le bouton 
jaune du controller dans Interface builder puis à droite, dernier onglet "flêche" pour voir les segues)

## TableViewController

Surcharge des fonctions
    
    # dit au système le nombre de cellules à afficher
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    # donne les informations de la celulle à afficher pour l'index
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    # donne la hauteur que doit prendre la celulle+
    override func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {

##Swift

**switch**

Un switch permet de switch sur le type d'une instance

```swift
switch cell {
case let postCell as PostTableViewCell: 
  postCell.nameLabel.text = "Test 1"
```

**Closure**

Closure : équivalent callback
Ref : http://fuckingclosuresyntax.com/

**Éviter les nil en Swift**

Pour éviter les nil en Swift, on peut utiliser : 
- object = String?, utilisation avec object! => on assure au compilateur que ce ne sera jamais utilisé (à éviter si possible)

- guard let data = data, error == nill else { ...} => le nouveau data est forcément non nil dans le else et errror est aussi non nil

guard let et if let permettent de faire des comparaisons et éventuellement d'affecter des variables qui seront forcément différents de nil

**kXXX**

Tout ce qui commence par k est l'ancienne notation pour les constantes en objective C (ex. kUTTypeMovie)

## Accessibilité et UX 

- ne pas faire de menu hamburger. La Tab bar est à préférer. De plus au niveau développeur, c'est beaucoup plus simple.
- zone de clic minimum : 44x44 px
- global tint pour colorer d'une même façon tous les boutons
- adapter les contraintes, hugging et compression pour prévoir un item qui puisse être diminué (par exemple une image). Mais
ce n'est pas aux développeurs de dire être minimisé mais à la MOA (ou designers).

## NSCache

NSCache permet de mettre en cache n'importe quel type d'objet. Par exemple, pour stocker des images récupérées via une requête réseau.

**Debugger**

Dans les variables, faire print "var". Ensuite, on peut faire po error! pour avoir le contenu de l'erreur

Le Main thread ou Queue main est celui qui est dédié pour la partie graphique. Il faut faire des opérations IO dans un autre thread
(par exemple URLSession.shared.dataTask fait l'opération dans un autre thread) puis revenir au main thread pour modifier l'UI
en conséquence (avec **DispatchQueue.main.async**).

ObjectMapper : pour mapper un objet JSON en objet swift

self.tableView.reloadData => pour recharger l'ensemble des données de la table. Il existe des méthodes qui recharge uniquement un ou 
une liste de row.

## Core Data

Notion de contexte. On peut faire un contexte par écran mais il est plus simple de faire un contexte unique pour toute l'application.

NSEntityDescription 
- name
- context

NSManagedObject
- entity
- context

**Pour débuguer Core data**

- télécharger https://github.com/ChristianKienle/Core-Data-Editor
- avant de run, choisir Core data editor, dans les préférences choisir le répertoire 
/Users/formation/Library/Developer/CoreSimulator/Devices

Possibilité d'utiliser setValue forKey ou setValue forKeyPath. Dans le 1er cas, on modifie un champ direct de la classe. Dans le 2ème cas, on peut modifier des objets en relation 

Ex: 
- post.setValue(postJSON.name, forKey: "name")
- post.setValue(postJSON.name, forKeyPath: "group.user.name")


##CloudKit
Sauver dans le cloud des informations. Une sorte de base de données sous forme de clé/valeur.
Record types => Définition d'un type de document
Default zone => là où se trouve les documents par défaut. Possibilité de créer des zones spécifiques

Public data/private data => les données sont publiques ou privées au sens de l'application, il n'y a pas d'API qui permettraient de récupérer ces données "publiques"

UUID.uuidString() => génération d'un UID


### Raccourcis et tips XCode

- Cmd-shift-o : pour ouvrir n'importe quel fichier ou méthode, attribut, etc..
- Cmd-shift-/ : pour commenter/décommenter
- scheme : pour configurer différemment dev, staging, prod
- 2 éditeurs : sur la partie droite, on peut choisir classes mères, appelants, etc.. ou bien passer en manuel. Attention l'explorateur
de gauche ne permet pas d'ouvrir l'éditeur de droite
- blame GIT en cliquant sur le 4ème bouton en haut à droite (en partant de la droite)
- ajouter des scripts dans Build phases
- Ctl-Shift-F : pour rechercher un terme
- Cmd-Alt-Shift-) : pour indenter sur un clavier AZERTY

### MapKit

- ajouter la capability MapKit (clic sur projet, choisir le bon target)
- création d'une classe Cocoa qui dérive de UIViewContoller => MapViewController
- dans la classe 
  - import MapKit
  - internal class MapViewController : UIViewController, MKMapViewDelegate, CLLocationManagerDelegate {

- création d'un ViewContoller + ajout d'un MapKitView à l'intérieur
- control sur le TabBarController vers le nouveau ViewController + IBOutlet

- associer dans le story board le ViewContrller avec cette classe
- clic sur le MapKitView et cliquer sur un onglet sur User Location (pour pouvoir afficher une annotation pour la position de l'utilisateur)
  
- CLLocationManager permet de gérer les demandes de permission d'accès au GPS

```swift

    override func viewDidLoad() {
        locationManager.delegate = self
        locationManager.desiredAccuracy = kCLLocationAccuracyBest
        locationManager.requestWhenInUseAuthorization()
    }
    
    override func viewDidAppear(_ animated: Bool) {
        let annotation = MKPointAnnotation()
        annotation.title = "Autre bar"
        annotation.subtitle = "À ne pas manquer"
        let lat = CLLocationDegrees(48.91)
        let long = CLLocationDegrees(2.5)
        
        annotation.coordinate = CLLocationCoordinate2DMake(lat2, long2)
        
        self.mapView.addAnnotation(annotation)
        //self.mapView.addAnnotations([annotation, annotation2])
   }
   
   func locationManager(_ manager: CLLocationManager, didChangeAuthorization status: CLAuthorizationStatus) {
        switch status {
        case .authorizedAlways, .authorizedWhenInUse:
            self.mapView.showsUserLocation = true
        default:
            break
        }
    }
    
    func mapView(_ mapView: MKMapView, didUpdate userLocation: MKUserLocation) {
        print("nouvelle localisation de l'utilisateur")
        
        let center = userLocation.coordinate
        let region = MKCoordinateRegion(center: center, span: MKCoordinateSpan(latitudeDelta: 0.03, longitudeDelta: 0.03))
        self.mapView.setRegion(region, animated: true)
    }

```

## Delegate

Delegate = Listner en java. Exemple : UIImagePickerController fait du code et à un moment délègue son traitement à notre delegate.


class PictureViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate

- création d'une BarButton avec une action
@IBAction func openCamera(sender: UIBarButtonItem!)

- dans info.plist : NSPhotoLibraryUSageDescription => ajouter une description

## Simulateur

- Debug > Location > Custom location pour simuler sa position quelque part
- alt souris pour faire les actions à 2 doigts (p.ex. pour zoomer/dézoomer sur une carte)


