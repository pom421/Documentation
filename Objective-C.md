
- le storyboard regroupe les vues graphiques et la coordination entre elles. Par exemple
- tips : faire les liens avec son code et les éléments graphiques. Cliquer sur un bouton ou un label dans le ViewController à partir du Storyboard. 
Ctl-Clic dans ViewController.m pour récupérer un Outlet (prise, permettant de récupérer l'objet graphique pour faire un .text par exemple) ou un Action pour réagir à un évènement. 

- @property : 
  - permet de définir une propriété à la classe (get/set sur le champ). On accède à un champ par self.nom-du-champ ou par _nom-du-champ.
  - nonatomic permet de gérer le setter de manière non atomique (plus rapide mais pas transcationnelle)
  - copy permet de dire qu'on gère cette propriété par recopie
  - assign permet de dire qu'on gère cette propriété par référence
  
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
