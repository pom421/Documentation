
- le storyboard regroupe les vues graphiques et la coordination entre elles. Par exemple
- tips : faire les liens avec son code et les éléments graphiques. Cliquer sur un bouton ou un label dans le ViewController à partir du Storyboard. 
Ctl-Clic dans ViewController.m pour récupérer un Outlet (prise, permettant de récupérer l'objet graphique pour faire un .text par exemple) ou un Action pour réagir à un évènement. 

- @property : 
  - permet de définir une propriété à la classe (get/set sur le champ). On accède à un champ par self.nom-du-champ ou par _nom-du-champ.
  - nonatomic permet de gérer le setter de manière non atomique (plus rapide mais pas transcationnelle)
  - copy permet de dire qu'on gère cette propriété par recopie
  - assign permet de dire qu'on gère cette propriété par référence

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

- IBOutlet, IBAction : objets récupérés de l'outil InterfaceBuilder (outil WYSIWIG pour dessiner les ViewController)

- NSxxx ou UIxxx préfixe des objets Objective-C (NS pour NextStep). NSObject est la classe de plus haut niveau

- invocation d'une méthode : 
  ```objective-c
  [beagle feedWithSnack:snack andWater:YES]
  ```
  
  ## Correspondance Java/Objectice-C
  - instance -> receiver
  - méthode -> selector
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
  
  ```objective-c
    // NSLog est une fonction (et pas une méthode de classe). C'est pour cela qu'elle ne respsecte pas la syntaxe crochet
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
    
    ```

### Cycle de vie
  
- viewDidLoad: initialiser les ressources
- viewWillAppear : reprendre le tracking GPS, spinner
- viewDidAppear : 
- viewWillDisappear : arrêt GPS
- viewDidDisappear : fin des animations
- viewDidUnload : suppression de la mémoire par le système

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




- (void) test {
NSString *exemple = @"Exemple";
NSString *exemple2 = @"Exemple2";
NSArray *exempleArray = @[exemple, exemple2];

NSArray *exempleArray2 = [[NSArray alloc] initWithObjects:exemple, exemple2, nil];

exemple = nil;
NSLog(@"%@", exempleArray);
}


#### Distribution en recette

- avec un compte développeur à 99e/an on a à disposition 100 iphones maximum autorisés pour déployer l'application (hors de l'App store). Chaque iPhone doit être référencé sur le compte (UDID). nKey UDID, application pour récupérer l'UDID de l'iPhone

- on peut déployer via iTunes ou plus simplement via une référence sur internet (ou intranet) via un serveur web sous HTTPS. Il faut que l'application soit signée avec le certificat attribué par Apple lors de la création de l'application. Les n° d'Iphone autorisés sont intégrés à l'application. 

- http://www.git-tower.com/blog/xcode-cheat-sheet

