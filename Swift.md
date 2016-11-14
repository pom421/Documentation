
Quand un type n'est pas explicitement défini, le type est forcément non nil (non Optionnel). 
Pour définir une variable optionnelle avec une valeur par défaut, il faut écrire :

```swift
var age: Int? = 10

```

L'affectation rend un booléen à false si l'affectation ne peut pas se faire.

```swift
var age: Int?

if let age2 = age {
  print(age2)
}
```

Pour avoir les méthodes de String qui viennent d'Objective C (par exemple Substring), il faut faire : 
```
import Foundation
```

Différence tuple et dictionnary:

Un tuple a éventuellement des propriétés nommés ce qui le rapproche des dictionnaires. Mais avec les différences : 
- les noms des propriétés ne peuvent pas être calculées contrairement au Dictionnary
- les tuples sont créés une fois pour toutes contrairement au Dictionnary qui peut évoluer au fil du temps

à la place de l'opérateur ternaire, on peut utiliser ??

a != nil ? a : b

est remplacé par 

a ?? b

- pas besoin de break à la fin de chaque cas. Mais si l'on veut éviter le break automatique, on peut utiliser fallthrough

