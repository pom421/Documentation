# Environnement développement mobile Apple

- COCOA : moteur de rendu graphique
- UIKit : bouton, UICollectionView, 
- Foundation : NSString, etc.. (NS pour NextStep). NSObject est la classe de plus haut niveau. En type de retour, quand on ne connait pas le type de l'objet rendu on peut mettre id
- XCode éditeur
- InterfaceBuilder, outil WYSIWIG de XCode pour dessiner les ViewController et leurs interactions

### Éléments d'Objective-C


- NSNumber est une classe est permet de véhiculer des informations de type int, double ou même booléen. Ensuite, il faut caster par exemple [maval boolvalue]
- dans le .h on met l'interface (au sens java). Tout ce qui sera définit ici sera public. Les héritages se mettent ici
- dans le .m on met l'implémentation. Les propriétés définies dans la partie @interface du .m seront privées et donc inaccessibles à l'extérieur
- les propriétés qui commencent par "-" sont des variables d'instance
- les propriétés commençant par "+" sont des variables de classe
- les classes méres sont renseignés ainsi : 
  ```objective-c
  @interface QuestionListViewController : UIViewController <UITableViewDataSource, UITableViewDelegate>
  ```
- les classes mère peuvent avoir des méthodes optionnelles (@optionnal) ou obligatoires (@required) à remplir pour les classes filles

- @property : 
  - permet de définir une propriété à la classe (get/set sur le champ). On accède à un champ par self.nom-du-champ ou par _nom-du-champ.
  - nonatomic permet de gérer le setter de manière non atomique (plus rapide mais pas transactionnelle)
  - copy permet de dire qu'on gère cette propriété par recopie
  - assign permet de dire qu'on gère cette propriété par référence. À faire pour le struct (et ne pas mettre étoile pour défnir la variable)
  - weak permet de dire qu'en sortant de la méthode qui a valorisé cette propriété, elle pourra être supprimée par le système quand bon lui semble (à vérifier)
  - strong, à l'inverse permet de garder la valeur de la propriété jusqu'à la sortie de la vue (ou de l'application? question à poser)
  
-  #import "MonFichier.h" -> pour importer un fichier
-  @import CoreLocation; -> pour importer un fichier d'un framework

- invocation d'une méthode : 
  ```objective-c
  [beagle feedWithSnack:snack andWater:YES]
  ```
  
  ## Correspondance Java/Objective-C
  - instance -> receiver
  - méthode -> selector (en plusieurs "morceaux")
  - argument -> argument
  
  ```objective-c
  // vérifier que beagle est différent de nil
  if (beagle) {
    // faire quelque chose
  }
  
  beagle = nil;
  [beagle feedWithSnack:snack andWater:YES]
  // pas de crash
  ```

- NSLog permet d'afficher des choses dans la console
- @"Une chaine de caractère standard"

- NSArray  
  ```objective-c
    _questionsArray = [[NSArray alloc] initWithObjects:
    @"Quelle est la capitale de la Turquie?",
    @"Quelle est la capitale de Cuba?",
    @"Quelle est la capitale de l'Italie?",
    @"Quelle est la capitale du Brésil?",
    @"Quelle est la capitale de la Colombie?",
   nil];
   
   // ou 
   
   self.answersArray = @[@"Istanbul", @"La Havane", @"Rome", @"Brasilia", @"Bogotta"];

  ```
  
  ```objective-c
    // NSLog est une fonction (et pas une méthode de classe). C'est pour cela qu'elle ne respecte pas la syntaxe crochet
    NSLog(@"Ma question est : %@ et son nombre de point(s) est %d", question.title, question.points);
  ```
  ```objective-c
  [NSString stringWithFormat:@"(%d pts)", question.points];
  ```
  ```objective-c
   // 2 styles pour les boucles
    for (Question *question in _questionsArray) {
        NSLog(@"%@", [question description]);
    }
    
    for (int i = 0; i < _questionsArray.count; i++) {
        NSLog(@"%@", _questionsArray[i]);
    }
    ```
    
    ```objective-c
    // alloc est une méthode de classe (+) alors qu'init est une méthode d'instance (-)
    [[Question alloc] init]
    

  ```objective-c
  self.title = title
  // équivalent
  [self setTitle:title]
  
  // on peut surcharger le setTitle même si on a définit une property plus haut
  @property (nonatomic, copy) NSString *title;
  - (void) setTitle:(NSString *)title {
    _title = title; //impossible de passer par self.title sinon boucle infinie
  }
  // getter
  - (NSString *)title: {
    return _title;
  }
  ```
  
- Propriété par défaut : atomic, strong, readwrite
  
  ```objective-c
  NSString *title = [question title]; // pour récupérer le title de la question (= get)
  [question setTitle:@"Nouvelle question?"]; // question.title = @""Nouvelle question";
  ```

- pas de NullPointerException en Objective-C (puisqu'on utilise des messages)

  ```objective-c
  - (void) test {
  NSString *exemple = @"Exemple";
  NSString *exemple2 = @"Exemple2";
  NSArray *exempleArray = @[exemple, exemple2];
  
  NSArray *exempleArray2 = [[NSArray alloc] initWithObjects:exemple, exemple2, nil];
  
  exemple = nil;
  NSLog(@"%@", exempleArray);
  }
  ```

Gestion de bloc asynchrone en Objective-C : 
- de la forme ^(params) { code; };

  ```objective-c
    NSString *urlVelib = @"https://api.jcdecaux.com/vls/v1/stations?contract=PARIS&apiKey=50fa844d51fe89f00bd0e227c817728d35d5e6ee";
    
    NSURL *stationsWSURL = [NSURL URLWithString:urlVelib];
    NSURLSession *session = [NSURLSession sharedSession];
    NSURLSessionDataTask *task = [session dataTaskWithURL:stationsWSURL completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        // Appelé quand la tâche datatask est terminée
        NSLog(@"ici");
    }];
    [task resume];
    NSLog(@"Et là");
  ```
  
- comment filtrer un NSArray d'objet avec un prédicat
 
  ```objective-c
  // exemple avec des objets qui ont des propriété name et address
  NSPredicate *predicate = [NSPredicate predicateWithFormat:@"(name contains[c] %@) or (address contains[c] %@)", searchText, searchText];
    
  self.velibSearchedStationsArray = [self.velibStationsArray filteredArrayUsingPredicate:predicate];
  
- comment trier un NSArray avec NSSortDescriptor


  
En cas d'appel asynchrone, il y a création d'un nouveau thread. 
Il faut après récupération des résultats, demander à recharger la vue du TableView. Pour cela, on ne peut pas faire un simple [tableView reloadData] car on est sur 2 threads séparés. Plutôt, on peut faire : 

 [self.tableView performSelectorOnMainThread:@selector(reloadData) withObject:nil waitUntilDone:NO];

### Cycle de vie d'une application mobile
  
- viewDidLoad: initialiser les ressources + calculs des dimensions suivant les contraintes
- viewWillAppear : reprendre le tracking GPS, spinner
- viewDidAppear : 
- viewWillDisappear : arrêt GPS
- viewDidDisappear : fin des animations
- viewDidUnload : suppression de la mémoire par le système

info.plist = manifest en Android

#### Distribution en recette

- avec un compte développeur à 99e/an on a à disposition 100 iphones maximum autorisés pour déployer l'application (hors de l'App store). Chaque iPhone doit être référencé sur le compte (UDID). nKey UDID, application pour récupérer l'UDID de l'iPhone

- on peut déployer via iTunes ou plus simplement via une référence sur internet (ou intranet) via un serveur web sous HTTPS. Il faut que l'application soit signée avec le certificat attribué par Apple lors de la création de l'application. Les n° d'Iphone autorisés sont intégrés à l'application. Ou TestFlight qui permet de recetter. Télécharger une application pour iPhone ou iPad. 

- Tips : 
  - http://www.git-tower.com/blog/xcode-cheat-sheet
  - http://spin.atomicobject.com/2014/03/23/xcode-keyboard-shortcuts/

#### Comment ajouter une classe ViewController en association à un élément ViewController du Storyboard?

Faire clic sur le viewController dans le storyboard et ajouter dans le 3ème onglet la classe

Ex de convention de nommage pour la fermeture d'un ViewController : 
- IBAction nommé closeButtonPressed

## Vue d'ensemble du développement en XCode

- le storyboard regroupe les vues graphiques et la coordination entre elles. 
- tips : faire les liens avec son code et les éléments graphiques. Cliquer sur un bouton ou un label dans le ViewController à partir du Storyboard. 
- Ctl-Clic à partir d'un élément graphique (ex : un label) et le mettre dans la partie interface de ViewController.m (ou ViewController.h pour le mettre en public) pour récupérer un Outlet (prise, permettant de récupérer l'objet graphique pour faire un .text par exemple) ou un Action pour réagir à un évènement. 

### Concepts

- Une View est un composant graphique. On entend par là que c'est un artefact à dessinner à un moment donné qui contient donc une largeur et une hauteur
- une view peut posséder d'autre vue à l'intérieur d'elle-même. Dans ce cas et si on laisse Clip subviews dans l'onget Attributes de InterfaceBuilder, on voit qu'une sous vue qui dépasserait d'une vue mère (appelée Superview) n'affiche pas l'éventuel résidu en dehors de l'espace de la super vue. Si on décoche "Clip Subviews", cependant ce qui dépasse éventuellement est affiché, ceci pour des raisons de performance. Mais aucun évènement ne sera détecté au niveau de ce qui dépasse (même si Clip subviews est décoché)
- en particulier un Label, un EditView sont des vues. Un TableViewCell est une vue!!
- un contrôleur est le traitement qui est lancé au démarrage. Il peut piloter une ou plusieurs vues (par exemple un TableView d'un côté et un SearchBar de l'autre)
- une vue spéciale (Container view) peut contenir un controller...
- il existe une superview si une vue (une sous-vue donc) appartient à une vue. Cf. contrainte.


### View et Controller référence

View
- UIViewController : Contrôleur standard
- UITableView : liste d'élements. 1 élément par ligne qui peuvent être de type différent (différencié par u n cellIdentifier)
- UITableViewCell
- UICollectionView : liste d'élements + colonnes. Il peut y avoir des colonnes différentes par ligne. Permet de simplifier un UITableView et permet de mettre un nombre de colonnes différentes en fonction de l'orientation

Delegate 
- UITextFieldDelegate pour gérer les évènements d'un TextField (entrée de texte)
- UITableViewDelegate pour gérer les évènements d'une TableView

Datasource
- UITableViewDataSource gère le modèle qui servira de données pour les éléments graphiques

Navigation et presenter
- NavigationController (ajoute la NavigationBar)
- PushViewController (navigation horizontale) pour aller vers toujours plus de détails
- PresentViewController (navigation verticale) interruption de l'activité sur une activité annexe (à vérifier)

Action et Segue
- IBOutlet, IBAction : objets récupérés de l'outil InterfaceBuilder
- Segue : pour faire une simple transition d'un VC à un autre VC. Ajouté visuellement via un Ctl-clic puis relier un bouton à un VC puis "present modally"
  - faire Show sous iPhone (Show detail le fait sous iPad, on garde la liste à gauche et affiche le détail à droite)
- becomeFirstResponded : pour donner le focus au TextField (shouldBeginEditing) + afficher le clavier
- resignFirstResponder : pour sortir du champ, éteindre le clavier (shouldReturn) 

#### Contraintes

Sur un élément graphique, on peut cliquer en bas à droite (3ème icone) pour ajouter des contraintes qui seront respectées pour calculer les dimensions. Ensuite, cliquer dans le 4ème icone à droite pour faire update frames.

Taille des icones à faire par le designer : 156*90 compatible iPhone 6 plus

Dans Interface Builder, on gère par pixel old school alors qu'à partir d'iPhone on est en retina (donc haute résolution avec 2x plus de pixels). Donc sur un ImageView de 52px de largeur, un iPhone 4 (et supérieur) utilisera une image de 104 pixels. Dans les faits, un iPhone 4 et 6 utilisera une image png x2. Seul le 6 plus utilisera le x3 (qui est un faux x3 au passage)

On peut mettre des contraintes par rapport à une Superview ou à un autre composant. Par exemple, quand on veut mettre un composant sous un autre. Ainsi le composant du dessous se place relativement par rapport à celui du dessus et on gagne en souplesse. self.view n'a pas de superview. 

- stackview : rassemble des composants pour appliquer un ensemble de contraintes à un lot d'éléments
- 3ème onglet : "Add new constraints" aligne les uns par rapport aux autres
- 4ème onglet : pour appliquer les contraintes à InterfaceBuilder (mais si pas appliqué, sera quand même fait dans le simulateur)

## Système de prototypage rapide d'une application mobile

- AppCooker (sur iPad) avec ergonomie iPhone
- insightly (invision) (png cliquable)

### Divers

- http://www.raywenderlich.com/ : tutoriels
- https://developer.apple.com : swift, objective-c
- simulateur : pour avoir le clavier Mac en plus, faire Hardware > Keyboard > Connect hardware keyboard
- JS webview : get UIWebview Height dans stack overflow. Permet de récupérer la hauteur de la webview pour ensuite avoir un scrollbar sur l'ensemble d'une page même si notre écran se compose de natif et de webview
- faire une webview quand le contenu est dynamique, quand on a des liens. Faire une page web et des images dans l'application et peupler les parties dynamiques de l'application par un ws qui contient juste le html de la div dynamique
- une webview permet aussi de simuler un navigateur (pour ne pas sortir de l'application) SFSafariVC
- une webview a un delegate à lui
- une webview peut avoir des contraintes mais à l'intérieur on retrouve bien du HTML, du CSS, etc..
- pour ne pas surcharger les applis mobiles, toutes les librairies ne sont pas automatiquement ajoutées. Il faut ajouter en import Mapkit pour les plans, CoreLocation pour le GPS, Social pour l'intégration Facebook/Twitter, lecteur audio, vidéo, etc..
- bug de XCode : le @import Mapkit ne suffit pas pour avoir la carte. Il faut aller sur le projet dans l'éditeur (double clic sur le nom du projet qui ouvre une fenêtre de configuration, General, Linked frameworks and libraries) Clic + puis Mapkit.frameworks
- recherche d'icônes : font awesome
- delegate Reachability pour gérer des évènements quand on perd la connexion data ou qu'on la récupère...
- AnnotationView, callOutAccessory : annotation dans la map et bulle qui s'affiche quand on clique dessus. il est possible de créer ses propres vues pour avoir un affichage particulier
- les imports en objective-C ne comportent pas de sous "répertoires" comme des packages en java. Tout est au même niveau, même si on a mis des .h et .m dans des sous-groupes

### GPS

- pour tester une position GPS (par exemple sur un ordinateur qui n'a pas de puce GPS), on peut utiliser un fichier .gpx. Lancer le simulateur puis cliquer sur l'icône en bas à droite qui ressemble à un avion en papier
- on doit activer le GPS quand on arrive dans la View et le supprimer quand on en sort. Apple vérifie qu'il n'est pas utilisé à outrance
- degré de précision à fixer entre 3 km et quelques mètres. Plus la précision est grande et plus l'autonomie est impactée
- kCLAuthorizationStatusAuthorizedWhenInUse, ne met à jour le GPS que quand l'application est ouverte
- kCLAuthorizationStatusAuthorizedAlways, tout le temps (ex : pour RunKeeper)
- dans info.plist ajouter la variable NSLocationWhenInUseUsageDescription pour renseigner la raison d'utilisation

### Mapkit

- il faut ajouter en 1x toutes les annotations qui seront visibles à l'échelle actuelle demandée par l'utilisateur
- si l'utilisateur dézoome beaucoup, il y a trop d'annotations donc on utilise la technique de clustering qui utilise des petites loupes pour rassembler une zone d'annotations. Le clustering n'est pas standar, il faut utiliser une librairire github
- 

### CoreData 

- CoreData = persistence d'objets via une base Sqlite
- Managed object context
- î
- Persistente store coordinator -> Managed object model (NSManagedObject)
- î
- Persistant object store -> connexion au fichier sqlite

- clic sur le fichier coredataexemple.xcdatamodeld
- ajouter les entités. Clic sur les relationship pour pouvoir ajouter la cardinalité avec le type à To Many ou To One 
- pour générer les classes correspondantes, sélectionner l'entité puis Editor > Create NSManagedObject subclass
- faire ok pour le bridging header (si l'on a choisit Swift)
- dans le fichier faire les #import des .h des ressources Objectives-C pour les rendre disponibles à Swift
- une extension permet d'ajouter des propriétés ou des méthodes à une classe existante (ex: extension de UIController). Fait par @extension en Swift ou par @interface(Classe a étendre)
- pour qu'une classe O-C accède à swift, il faut faire #import "nomprojet-Swift.h"
- pour qu'une classe Swift accède à une ressources O-C, il faut ajouter les .h des ressources Objective-C dans coredataexemple-Bridging-Header.h,  

- id ne peut pas être le nom d'un attribut dans sqlite car c'est un mot clé dans Objective-C (utiliser identifier ou serialNumber à la place en O-C)
- fichier .momd = structure de la base de données sqlite
- modifier le AppDelegate.m pour modifier le nom de le base sqlite et du .momd

### AppDelegate

Il y a un objet transverse à tous nos VC, c'est AppDelegate. 

Il est accessible par : 
  AppDelegate *appDelegate = [[UIApplication sharedApplication]delegate];

- applicationDocumentsDirectory : donne le répertoire que l'utilisateur a définit pour stocker des fichiers

### Ajout d'un ViewController au storyboard

- dans la bibli copier-coller un ViewController
- créer une Cocoa classe dérivant de UIViewController et lier dans le storyboard le VC graphique et la nouvelle classe

### Ajout d'un NavigationController à un TabBar existant

- ajouter par drag and drop à partir de la bibliothèque
- supprimer le rootViewController
- ctl-clic du NavigationController vers FirstViewController puis Relationship segue : root ViewController
- suprimer le segue existant entre le TabBarController et FirstViewController
- Ctl-clic entre TabBatController et le NaviationController puis RelationShip segue > view Controllers
- le NavigationController self.navigationController.navigationBar.translucent = NO
