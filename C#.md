# Formation C#

### Visual Studio

- Pour lancer un programme et voir la console : Ctl-F5
- Pour lancer un programme avec un paramètre d'entrée : Projet > Propriété de la console puis modifier Arguments de la ligne de commande

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello");
        }
    }
}
```
---

### Catégories de types

- Value type

ex : int, struct, enum

Uniquement dans la pile. Donc un int par exemple ne peut pas être null.

Un struct est aussi un value type.

Ex :
````cs
struct Point {int x; int y}
````

Bénéfices
- pas d'allocation dans le tas, moins de travail pour le GC

- Reference type


- 1 ou plusieurs types dans un fichier
- 1 ou plusieurs fichiers dans un module
- 1 ou plusieurs modules font un assemblage (assembly)


### using

Equivalent de import .* en java.

Permet d'avoir aussi des synonymes.

````cs
using C1 = N1.N2.C1
using N2 = N1.N2;

C1 a;
N2.C1 b;
````
### Accès

- private
- protected
- internal = membre accessible par tous les fichiers du modules

### Constantes

- const = constante au moment de la compilation
- readonly = initialisée au moment de l'exécution et plus modifiable ensuite

### exception

- pas de throws obligatoire dans la déclaration de la méthode
- pas obligé de catcher une exception ??
- catch ne spécifie pas forcément une exception

### opérateurs sur les types

- is pour savoir si une instance est du type (ou classe mère)
- as pour faire un cast dans une classe compatible (null sinon)
- typeof (rend le System.Type de l'objet)


### méthodes virtuelles en C#

Par défaut, les méthodes en C# ne sont pas polymorphes.

Si on a une classe Personne avec une fonction f(). Et si une classe Client dérive de Personne et a une méthode f().

Personne aux = new Client();
aux.f() => en Java utilise la méthode de Client.
En C#, on appelle la méthode f() de Personne.

````cs
# pour rendre des méthodes virtuelles
class Shape {
  public virtual void Draw() {}

}

class Box : Shape {
  public override void Draw() {}
}

class Sphere : Shape {
  public override void Draw() {}
}
````

# pour rendre des méthodes virtuelles
````cs
class Shape {
  public virtual void Draw() {}
}

class Box : Shape {
  public override void Draw() {}
}

class Sphere : Shape {
  new public void Draw() {}
}

class Sphere2 : Sphere {
  public override void Draw() {}
}

````
Pour une classe mère qui définit une méthode virtual (qui pourra donc être redéfinit) on a 2 options :

- soit mettre sur la méthode de la classe fille override pour appliquer cette méthode pour toute instance de
