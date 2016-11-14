
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
