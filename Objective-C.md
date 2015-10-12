# Environnement développement mobile Apple

- COCOA : moteur de rendu graphique
- UIKit : bouton, UICollectionView, 
- Foundation : NSString, etc.. (NS pour NextStep). NSObject est la classe de plus haut niveau. En type de retour, quand on ne connait pas le type de l'objet rendu on peut mettre id
- XCode éditeur
- InterfaceBuilder, outil WYSIWIG de XCode pour dessiner les ViewController et leurs interactions

### Éléments d'Objective-C

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
  - assign permet de dire qu'on gère cette propriété par référence

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
    ```

### Cycle de vie d'une application mobile
  
- viewDidLoad: initialiser les ressources + calculs des dimensions suivant les contraintes
- viewWillAppear : reprendre le tracking GPS, spinner
- viewDidAppear : 
- viewWillDisappear : arrêt GPS
- viewDidDisappear : fin des animations
- viewDidUnload : suppression de la mémoire par le système


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

- stackview : rassemble des composants pour appliquer un ensemble de contraintes à un lot d'éléments
- 3ème onglet : "Add new constraints" aligne les uns par rapport aux autres
- 4ème onglet : pour appliquer les contraintes à InterfaceBuilder (mais si pas appliqué, sera quand même fait dans le simulateur)

## Système de prototypage rapide d'une application mobile

- AppCooker (sur iPad) avec ergonomie iPhone
- insightly (invision) (png cliquable)
