# Práctica 1: Herencia, composición y polimorfismo

## Ejercicios propuestos

### Ejercicio 1

Dados los siguientes fragmentos de código, responder a las siguientes preguntas:

#### `ElementsSet.java`

```java
public class ElementsSet<E> extends HashSet<E> {
    //Number of attempted elements insertions using the "add" method
    private int numberOfAddedElements = 0;

    public ElementsSet() {}

    @Override
    public boolean add(E element) {
        numberOfAddedElements++; //Counting the element added
        return super.add(element);
    }

    @Override
    public boolean addAll(Collection<? extends E> elements) {
        numberOfAddedElements += elements.size(); //Counting the elements added
        return super.addAll(elements);
    }

    public int getNumberOfAddedElements() {
        return numberOfAddedElements;
    }
}
```

#### `Main.java`

```java
    ...
    ElementsSet<String> set = new ElementsSet<String>();
    set.addAll(Arrays.asList("One", "Two", "Three"));
    System.out.println(set.getNumberOfAddedElements());
    ...
```

#### Preguntas propuestas

a) ¿Es el uso de la herencia adecuado para la implementación de la clase `ElementsSet`? ¿Qué salida muestra la función `System.out.println` al invocar el método `getNumberOfAddedElements`, 3 o 6?
Es mejor usar la delegación en lugar de la herencia pues en este caso ElementsSets es del tipo HashSet, lo que provoca el problema del método que vemos en addAll. Lo ideal sería utilizar un HashSet como miembro de la clase e implementar la interfaz Set.

6. El método AddAll itera sobre la colección y llama al método Add sobre cada uno de los elementos de la colección específica. Como en este caso se invocaría al add del ElementsSet.

b) En el caso de que haya algún problema en la implementación anterior, proponga una solución alternativa usando composición/delegación que resuelva el problema.

### Ejercicio 2

Dado los siguientes fragmentos de código responder a las siguientes preguntas:

#### `Animal.java`

```java
public abstract class Animal {
    //Number of legs the animal holds
    protected int numberOfLegs = 0;

    public abstract String speak();
    public abstract boolean eat(String typeOfFeed);
    public abstract int getNumberOfLegs();
}
```

#### `Cat.java`

```java
public class Cat extends Animal {
    @Override
    public String speak() {
        return "Meow";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("fish")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Dog.java`

```java
public class Dog extends Animal {
    @Override
    public String speak() {
        return "Woof";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("meat")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Main.java`

```java
    ...
    Animal cat = new Cat();
    Animal dog = new Dog();
    System.out.println(cat.speak());
    System.out.println(dog.speak());
    ...
```

#### Preguntas propuestas

a) ¿Es correcto el uso de herencia en la implementación de las clases `Cat` y `Dog`? ¿Qué beneficios se obtienen?
No, se está representando aquí una herencia como estructura. Si queremos ampliar el comportamiento sería problemático si quisieramos añadir un animal que no hable, tendría que extender también la clase animal y acabaríamos con un método vacío.

b) En el caso de que el uso de la herencia no sea correcto, proponga una solución alternativa. ¿Cuáles son los beneficios de la solución propuesta frente a la original?

Propongo una solución con implementación de interfaces, CanEat y CanSpeak, las clases podrán implementar esta interfaz lo que permitirá que si un animal no emite ningún sonido no tiene porqué implementar la interfaz CanSpeak y podrá seguir implementando CanEat, además haciendo esto conseguimos un modelo de clases mucho más escalable porque, si una clase quiere implementar un nuevo comportamiento, por ejemplo, a un pez se le quiere implementar la función de nadar, lo único necesario es crear la interfaz de CanSwim y hacer que el pez la implemente sin necesidad de que la implementen todos los animales y solo teniendo que recompilar el código de la interfaz pez.

#### `CanEat.java`

```java
public interface CanEat {
    public boolean eat(String typeOfFeed);
}
```

#### `CanSpeak.java`

```java
public interface CanSpeak {
public String speak();
}

```

#### `Dog.java`

```java
public class Dog implements CanSpeak, CanEat {

    @Override
    public String speak() {
        return "Woof";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if (typeOfFeed.equals("meat")) {
            return true;
        }
        return false;
    }
}
```

#### `Cat.java`

``` java
public class Cat implements CanSpeak, CanEat {
    @Override
    public String speak() {
        return "meow";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if (typeOfFeed.equals("fish")) {
            return true;
        }
        return false;
    }

}
```
