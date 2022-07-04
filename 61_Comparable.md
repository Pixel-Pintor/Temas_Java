# Comparable
Mientras trabaja con datos, es probable que necesite ordenarlos de una manera conveniente. Por ejemplo, es posible que deba poner numeros en orden ascendente, agrupar lineas en orden alfabetico y organizar cualquier cosa con la que trabajo por fecha, precio u otras caracteristicas personalizadas. En Java es posible implementar varios algoritmos de clasificacion para cualquier tipo de datos con la interfaz `Comparable`.  

## Preparandose para Comparar
Veamos un ejemplo. Creamos una lista de `Integer`, agrego algunos elementos y luego los clasifico.
~~~java
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(55);
    list.add(13);
    list.add(47);

    Collections.sort(list);
    System.out.println(list);
}

// salida de lista organizada
// [13, 47, 55]
~~~
Ahora, vamos a crear una clase simple `Car` donde queremos ordenar los autos por sus numeros.
~~~java
public class Car {
    private int number;
    private String model;
    private String color;
    private int weight;

    // constructor
    // getters and setters
}

// Tratamos de usar sort() para ordenar instancias de Car
public static void main(String[] args) {
    List<Car> cars = new ArrayList<>();
    Car car1 = new Car(876, "BMW", "white", 1400);
    Car car2 = new Car(345, "Mercedes", "black", 2000);
    Car car3 = new Car(470, "Volvo", "blue", 1800);
    cars.add(car1);
    cars.add(car2);
    cars.add(car3);

    Collections.sort(cars);
    System.out.println(cars);
}

// Obtenemos un error de compilacion
The method sort(List<T>) in the type Collections 
  is not applicable for the arguments (ArrayList<Car>)
~~~
La razon de esto es que las clases estandar como `Integer` implementan una interfaz especial, para que podamos compararlos sin ningun problema. Pero es una clase personalizada como `Car` no funciona esa interfaz.

## Interfaz Comparable
`Comparable` proporciona el metodo `compareTo()` que permite comparar un objeto con otros objetos del mismo tipo. Tambien es importante cumplir con las condiciones: todos los objetos se pueden comparar con otros objetos del mismo tipo de la manera mas utilizada, lo que significa `compareTo()` debe ser consistente con el metodo `equals()`. Una secuencia de datos tiene un **ordenamiento natural**, si para cada 2 elementos **a** y **b**, donde **a** se encuentra a la izquierda de **b**, la condicion es verdadera: `a.compareTo(b) <= 0`.  
Es facil entender como comparar un `Integer` o un `String` porque ya implementan la interfaz `Comparable`. Pero para clases personalizadas debemos hacerlos por cualquier campo unico o varios campos.  
Para poder ordenar, debemos reescribir nuestra clase `Car` usando la interfaz `Comparable`. Por ejemplo, podemos comparar nuestros objetos `Car` por su `number`. Asi es como puedes implementarlo.
~~~java
public class Car implements Comparable<Car> {
    private int number;
    private String model; 
    private String color;
    private int weight;

    // constructor
    // getters and setters

    @Override
    public int compareTo(Car otherCar) {
        return Integer.valueOf(getNumber()).compareTo(otherCar.getNumber());
    }
}
~~~

## Implementando el metodo compareTo
El metodo `compareTo()` compara el objeto actual con el objeto enviado como parametro. Para implementarlo correctamente, debemos asegurarnos de que el metodo devuelva:
- Un entero positivo (por ejemplo 1), si el objeto actual es mayor
- Un entero negativo (por ejemplo -1), si el objeto actual es menor
- Cero, si son iguales  
A continuacion puede ver un ejemplo de como `compareTo()` se implementan en la clase `Integer`.
~~~java
@Override
public int compareTo(Integer anotherInteger) {
    return compare(this.value, anotherInteger.value);
}

public static int compare(int x, int y) {
    return (x < u) ? - 1 : ((x == y) ? 0 : 1);
}
~~~
Hay algunos otras reglas para implementar el metodo `compareTo()`. Para demostrarlos, imagina que tenemos una clase llamada `Coin`:
~~~java
class Coin implements Comparable<Coin> {
    private final int nominalValue;
    private final int mintYear;

    Coin(int nominalValue, int mintYear) {
        this.nominalValue = nominalValue;
        this.mintYear = mintYear;
    }

    @Override
    public int compareTo(Coin other) {
        // this method we have to implement
    }

    // we consider two coins equal if they have the same nominal value
    @Override
    public boolean equals(Object that) {
        if (this == that) return true;
        if (that == null || getClass() != that.getClass()) return false;
        Coin coin = (Coin) that;
        return nominalValue == coin.nominalValue;
    }

    // getters, setters, hashCode and toString
}

// vamos a crear algunos objetos Coin
public static void main(String[] args) {
    Coin big = new Coin(25, 2006);
    Coin medium1 = new Coin(10, 2016);
    Coin medium2 = new Coin(10, 2001);
    Coin small = new Coin(2, 2000);
}
~~~
Una de las reglas es mantener la implementacion de `compareTo()` consistente con la implementacion del metodo `equals()`. Por ejemplo:
- `medium1.compareTo(medium2) == 0` debe tener el mismo valor boolean que `medium2.equals(medium1)`  
Si comparamos nuestras monedas y `big` es mayor que `medium1` y `medium2` es mayor que `small`, despues de `big` es mayor que `small`:
- `(big(compareTo(medium1) > 0 && medium1.compareTo(small) > 0)` implica `big.compareTo(small) > 0`  
`big` es mayor que `small`, por eso `small` es mas pequeño que `big`:
- `big.compareTo(small) > 0` y `small.compareTo(big) < 0`  
Si `medium1` es igual a `medium2`, ambos deben ser mas grandes o mas pequeños que `small` y `big` respectivamente:
- `medium1.compareTo(medium2) == 0` implica que `big.compareTo(medium1) > 0` y `big.compareTo(medium2) > 0` o `small.compareTo(medium1) < 0` y `small.compareTo(medium2) < 0`  
Esto asegurara que podamos usar dichos objetos de manera segura en conjuntos ordenados y mapas ordenados.  
En este caso, podemos cumplir con todos estos requisitos si comparamos nuestras monedas por su valor nominal:
~~~java
class Coin implements Comparable<Coin> {

    // fields, constructor, equals, hashCode, getters and setters

    @Override
    public int compareTo(Coin other) {
        if (nominalValue == other.nominalValue) {
            return 0;
        } else if (nominalValue < other.nominalValue) {
            return -1;
        } else {
            return 1;
        }
    }

    @Override
    public String toString() {
        return "Coin{nominal=" + nominalValue + ", year=" + mintYear + "}";
    }
} 

// Ahora podemos agregar las monedas a una lista y ordenarlas
List<Coin> coins = new ArrayList<>();

coins.add(big);
coins.add(medium1);
coins.add(medium2);
coins.add(small);

Collections.sort(coins);
coins.forEach(System.out::println);

// en la salida podemos ver que las monedas se han clasificado correctamente
Coin{nominal=2, year=2000}
Coin{nominal=10, year=2016}
Coin{nominal=10, year=2001}
Coin{nominal=25, year=2006}
~~~

## Conclusion
En este tema, exploramos como usar la interfaz `Comparable` en nuestras clases personalizadas para definir algoritmos de orden natural. Tambien aprendimos como implementar adecuadamente el metodo `compareTo()`.
---

## Ejercicios
1. Un sitio web utiliza la clase `Rating` para almacenar informacion sobre las clasificaciones de las publicaciones. Tiene dos campos enteros: `upVotes` y `downVotes` que implementan la interfaz `Comparable`. Debe escribir el metodo `compareTo`. Dos objetos `Rating` deben compararse mediante una "calificacion neta" que se calcula como la diferencia entre el numero de votos a favor y el numero de votos en contra.
~~~java
class Rating implements Comparable<Rating> {
    private int upVotes;
    private int downVotes;

    public int getUpVotes() {
        return upVotes;
    }

    public void setUpVotes(int upVotes) {
        this.upVotes = upVotes;
    }

    public int getDownVotes() {
        return downVotes;
    }

    public void setDownVotes(int downVotes) {
        this.downVotes = downVotes;
    }

    @Override
    public int compareTo(Rating rating) {
        int thisRatingDiff = this.upVotes - this.downVotes;
        int otherRatingDiff = rating.upVotes - rating.downVotes;
        // Solution with Integer.compare()
        // return Integer.compare(thisRatingDiff, otherRatingDiff);
        if (thisRatingDiff == otherRatingDiff) {
            return 0;
        } else if (thisRatingDiff < otherRatingDiff) {
            return -1;
        } else {
            return 1;
        }
    }
}

// do not change the code below
class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        List<Rating> list = new ArrayList<>();
        while (sc.hasNextLine()) {
            int[] votes = Arrays.stream(sc.nextLine().split(" "))
                    .mapToInt(Integer::parseInt)
                    .toArray();
            Rating rating = createRating(votes[0], votes[1]);
            list.add(rating);
        }
        Collections.sort(list);
        Checker.check(list);
    }

    private static Rating createRating(int up, int down) {
        Rating rating = new Rating();
        rating.setUpVotes(up);
        rating.setDownVotes(down);
        return rating;
    }
}
~~~
2. Imagina que implementas el metodo `compareTo()` en su clase. Como regla, `x.compareTo(y) == 0` tiene el mismo valor boolean que `x.equals(y)`.
- Si deberia tener el mismo valor booleano
3. Debe implementar la comparacion de articulos por su tamaño, y si sus tamaños son iguales, comparalos por titulo (segun el orden lexicografico)
~~~java
class Article implements Comparable<Article> {
    private final String title;
    private final int size;

    public Article(String title, int size) {
        this.title = title;
        this.size = size;
    }

    public String getTitle() {
        return this.title;
    }

    public int getSize() {
        return this.size;
    }

    @Override
    public int compareTo(Article otherArticle) {
        if (this.getSize() == otherArticle.getSize()) {
            return this.getTitle().compareTo(otherArticle.getTitle());
        } else if (this.getSize() < otherArticle.getSize()) {
            return -1;
        } else {
            return 1;
        }
    }
}
~~~
4. Imagina que escribes una clase simple llamada `Cake` y desea definir el orden por nombre ¿Cual es la implementacion correcta?
~~~java
public class Cake implements Comparable<Cake> {
    private String name;
    private String descriptions;
    private int weight;

    // constructor
    // getters and setters

    @Override
    public int compareTo(Cake otherCake) {
        return getName().compareTo(otherCake.getName());
    }
}
~~~
5. Cambie el codigo para que el ordenamiento natural clasifique los elementos por sus numeros y tambien por sus modelos.
~~~java
public class Car implements Comparable<Car> {
    private int number;
    private String model;
    private String color;
    private int weight;

    // constructor
    // getters and setters

    @Override
    public int compareTo(Car otherCar) {
        int res = getModel().compareTo(otherCar.getModel());
        return res;
    }
}

public static void main(String[] args) {
    List<Car> cars = new ArrayList<>();
    Car car1 = new Car(876, "Volvo", "white", 1400);
    Car car2 = new Car(345, "Mercedes", "black", 2000);
    Car car3 = new Car(470, "BMW", "blue", 1800);
    cars.add(car1);
    cars.add(car2);
    cars.add(car3);

    Collections.sort(cars);
    System.out.println(cars);
}
~~~
6. Implemente el metodo `compareTo()`, debe comparar personas por nombre, y si tienen el mismo, compararlas por edad.
~~~java
class Person implements Comparable<Person> {
    private final String name;
    private final int age;
    private final int height;
    private final int weight;

    public Person(String name, int age, int height, int weight) {
        this.name = name;
        this.age = age;
        this.height = height;
        this.weight = weight;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }

    public int getHeight() {
        return this.height;
    }

    public int getWeight() {
        return this.weight;
    }

    @Override
    public int compareTo(Person otherPerson) {
        if (Objects.equals(this.getName(), otherPerson.getName())) {
            return Integer.compare(this.getAge(), otherPerson.getAge());
        } else if (this.getName().compareTo(otherPerson.getName()) < 1) {
            return -1;
        } else {
            return 1;
        }
    }
}
~~~
7. Cambie la clase para que sea posible comparar objetos `Age`
~~~java
class Age implements Comparable<Age> {
    private final int value;

    public Age(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }


    @Override
    public int compareTo(Age age) {
        return this.value > age.getValue() ? 1 : this.value == age.getValue() ? 0 : -1;
    }
}

class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        List<Age> list = Arrays.stream(sc.nextLine().split(" "))
                .mapToInt(Integer::parseInt)
                .mapToObj(Age::new)
                .sorted()
                .collect(Collectors.toList());

        Checker.check(list);
    }
}
~~~
