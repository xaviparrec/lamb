

# Lambdes

## Què és una Lambda?
- És una manera molt compacta d'escriure una implementació d'una interfície funcional (una interfície que només té un mètode).
- És una funció anònima (sense nom).
- Permet passar comportaments com a paràmetres.


## Principals Interficies Funcionals
### ```Runnable``` Executar una acció.
```java
Runnable -> () -> { ... }
```
- No rep paràmetres, no retorna res.
- Mètode: ```run()```

### ```Supplier``` Proveir un valor.
```java 
Supplier<T> -> () -> T
```
- No rep paràmetres, retorna un valor de tipus ```T```.
- Mètode: ```get()```

### ```Consumer``` Rebre un valor i actuar
```java
Consumer<T> -> (T) -> { ... }
```
- Rep un valor de tipus ```T```, no retorna res.
- Mètode: ```accept(T t)```

### ```Function``` Transformar un valor en un altre.
```java
Function<T, R> -> (T) -> R
```
- Rep un valor de tipus ```T``` i retorna un valor de tipus ```R```.
- Mètode: ```apply(T t)```

### ```Predicate``` Avalua si un valor compleix una condició.
```java
Predicate<T> -> (T) -> boolean
```
- Rep un valor de tipus ```T``` i retorna un valor ```boolean```.
- Mètode: ```test(T)```

# Streams
## Què és un Stream?
- Una seqüència de dades que es processa declarativament.
##### Exemple
```java
List<String> noms = List.of("Joan", "Anna", "Pau");
noms.stream()
    .filter(nom -> nom.startsWith("J"))
    .forEach(System.out::println);
```

## Operacions intermèdies
### ```filter(Predicate)``` Filtra elements que compleixen una condició.
```java
Stream<T> -> Stream<T>
```
- Rep un element i retorna un ```booleà``` (true si vols conservar-lo).
- Utilitza una ```Predicate<T>```.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
noms.stream()
    .filter(nom -> nom.startsWith("A"))
    .forEach(System.out::println);
// Output: Anna, Albert
```

### ```map(Function)``` Transforma cada element del Stream en un altre.
```java
Stream<T> -> Stream<R>
```
- Rep un element i el transforma.
- Utilitza una ```Function<T, R>```.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan");
noms.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);
// Output: ANNA, JOAN
```
### ```sorted()``` Ordena els elements del Stream.
```java
Stream<T> -> Stream<T>
```
- Sense paràmetres ➔ ordena amb ```Comparable```.
- Amb ```Comparator``` ➔ ordena com tu decideixis.
##### Exemple
```java
List<Integer> numeros = List.of(3, 1, 4, 2);
numeros.stream()
    .sorted()
    .forEach(System.out::println);
// Output: 1, 2, 3, 4
```

### ```distinct()``` Elimina elements duplicats.
```java
Stream<T> -> Stream<T>
```
- Conserva només un exemplar de cada element
- Funciona amb el mètode ```equals()```.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Anna");
noms.stream()
    .distinct()
    .forEach(System.out::println);
// Output: Anna, Joan
```

### ```limit(n)``` Limita el Stream als primers ```n``` elements.
```java
Stream<T> -> Stream<T>
```
- Retorna només els primers ```n``` elements del flux.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
noms.stream()
    .limit(2)
    .forEach(System.out::println);
// Output: Anna, Joan
```

### ```skip(n)``` Salta els primers ```n``` elements del Stream.
```java
Stream<T> -> Stream<T>
```
- Ignora els primers ```n``` elements.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
noms.stream()
    .skip(1)
    .forEach(System.out::println);
// Output: Joan, Albert
```

### ```peek(Consumer)``` Executa una acció per cada element durant el processament (normalment per debugging).
```java
Stream<T> -> Stream<T>
```
- No transforma ni filtra, només permet "mirar" els elements.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan");
noms.stream()
    .peek(nom -> System.out.println("Processant: " + nom))
    .map(String::toUpperCase)
    .forEach(System.out::println);
// Output: Processant: Anna
//         ANNA
//         Processant: Joan
//         JOAN
```

## Operacions terminals bàsiques
### ```forEach(Consumer)``` Executa una acció per cada element del Stream.
```java
noms.stream().forEach(System.out::println);
```

### ```count()``` Compta el nombre d'elements.
```java
long total = noms.stream().count();
```

### ```findFirst()``` Torna el primer element (opcional).
```java
Optional<String> primer = noms.stream().findFirst();
``` 

### ```findAny()``` Torna qualsevol element del Stream (especialment útil en streams paral·lels).
```java
Optional<String> qualsevol = noms.stream().findAny();
```

### ```anyMatch(Predicate)``` Comprova si algun element compleix una condició.
```java
boolean hiHa = noms.stream().anyMatch(nom -> nom.startsWith("A"));
``` 

### ```allMatch(Predicate)``` Comprova si tots els elements compleixen una condició.
```java
boolean tots = noms.stream().allMatch(nom -> nom.length() > 2);
``` 

### ```noneMatch(Predicate)``` Comprova si cap element compleix una condició.
```java
boolean cap = noms.stream().noneMatch(nom -> nom.contains("z"));
```
## Operació terminal especial
```collect(Collector)``` Permet recollir el resultat del Stream en una col·lecció o resultat.

### ```toList()``` Recull tots els elements del Stream dins una ```List``` mutable.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
List<String> resultat = noms.stream()
    .collect(Collectors.toList());
```
- Ús de ```.collect(Collectors.toList())``` → ```.toList()``` correcte (des de Java 16+). **Llista inmutable**

### ```toSet()``` Recull tots els elements del Stream dins un ```Set``` (no permet duplicats).
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Anna");
Set<String> resultat = noms.stream()
    .collect(Collectors.toSet());
```

### ```toMap(clauFunction, valorFunction)``` Recull els elements en un ```Map<Clau, Valor>```, segons dues funcions: una per calcular la clau, i una per calcular el valor.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan");
Map<String, Integer> resultat = noms.stream()
    .collect(Collectors.toMap(
        nom -> nom,             // Clau = el mateix nom
        nom -> nom.length()     // Valor = longitud del nom
    ));
```
Es podria donar el cap que dues entrades tenguin la mateixa clau donant un ```IllegalStateException```.
```java
List<String> noms = List.of("Anna", "Alba");
Map<Character, String> resultat = noms.stream()
    .collect(Collectors.toMap(
        nom -> nom.charAt(0),    // Clau = primera lletra ('A')
        nom -> nom               // Valor = el nom
    ));
// Error en executar!
// Perquè tant "Anna" com "Alba" tenen la mateixa clau 'A'.
```
El ```toMap``` té una versió amb 3 paràmetres on pots indicar com resoldre conflictes:
```java
List<String> noms = List.of("Anna", "Alba");
Map<Character, String> resultat = noms.stream()
    .collect(Collectors.toMap(
        nom -> nom.charAt(0),
        nom -> nom,
        (nom1, nom2) -> nom1 // En cas de conflicte, queda el primer
    ));
```

### ```joining(delimitador)``` Ajunta tots els elements del Stream en un sol ```String```, separats pel delimitador que indiquis.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
String resultat = noms.stream()
    .collect(Collectors.joining(", "));
// Output: "Anna, Joan, Albert"
```

### ```counting()``` Compta el nombre d'elements presents al Stream.
##### Exemple
```java
List<String> noms = List.of("Anna", "Joan", "Albert");
Long resultat = noms.stream()
    .collect(Collectors.counting());
// Output: 3
```

### ```groupingBy(clauFunction)``` Agrupa elements segons una clau que triïs.
##### Exemple
```java
List<String> noms = List.of("Anna", "Albert", "Joan");
Map<Character, List<String>> resultat = noms.stream()
    .collect(Collectors.groupingBy(nom -> nom.charAt(0)));
// Output: Agrupa per la primera lletra
```

### ```mapping(transformFunction, collector)``` Transforma cada element abans de recollir-lo dins un altre collector (normalment dins un groupingBy).
##### Exemple
```java
Map<Character, List<String>> resultat = noms.stream()
    .collect(Collectors.groupingBy(
        nom -> nom.charAt(0),
        Collectors.mapping(nom -> nom.toUpperCase(), Collectors.toList())
    ));
// Output: Agrupa per la primera lletra i converteix en majúscules.
```

### ```partitioningBy(Predicate)``` Divideix els elements en dos grups (```true``` / ```false```) segons una condició.
##### Exemple
```java
Map<Boolean, List<String>> resultat = noms.stream()
    .collect(Collectors.partitioningBy(nom -> nom.length() > 4));
// Output: Separació entre noms curts i llargs
```

### ```reducing()``` Fa una reducció manual d'elements (sumar, concatenar, etc.).
##### Exemple
```java
Integer suma = List.of(1, 2, 3).stream()
    .collect(Collectors.reducing(0, num -> num, Integer::sum));
// Output: Suma de tots els números.
```

### ```teeing(collector1, collector2, mergeFunction)``` Aplica dos collectors alhora i combina els resultats en un sol objecte.
##### Exemple
```java
var resultat = noms.stream()
    .collect(Collectors.teeing(
        Collectors.counting(),
        Collectors.joining(", "),
        (count, joined) -> count + " noms: " + joined
    ));
// Output: Exemple que compta i concatena noms en una sola frase.
```

### ```collectingAndThen(collector, funcióFinal)``` Aplica una funció final després de recollir..
##### Exemple
```java
List<String> paraules = List.of("casa", "pont", "casa", "bosc", "pont", "muntanya");
Integer nombreUnics = paraules.stream()
    .collect(Collectors.collectingAndThen(
        Collectors.toSet(),  // Primer: eliminam duplicats
        Set::size            // Després: comptam quants valors únics hi ha
    ));
// Output: 4
```

# Conclusió

- Lambda = funció sense nom que podem passar i executar.
- Stream = seqüència de dades processades declarativament.
- Collector = estratègia per recollir els elements del Stream.
- Primera regla del Stream: Configura amb intermèdies → Executa amb terminal.
