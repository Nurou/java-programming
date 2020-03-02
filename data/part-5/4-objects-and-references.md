---
#path: '/part-5/4-oliot-ja-viitteet'
path: '/part-5/4-objects-and-references'
## title: 'Oliot ja viitteet'
title: 'Objects and references'
hidden: false
---

<!-- <text-box variant='learningObjectives' name='Oppimistavoitteet'> -->

<text-box variant='learningObjectives' name='Learning objectives'>


<!-- - Kertaat luokkien ja olioiden toimintaa. -->

- You brush up on how to use classes and objects.

<!-- - Tiedät mikä on `null`-viite ja tiedät mistä virhe NullPointerException johtuu. -->

- You know what a `null` reference is, and what causes a NullPointerException error.

<!-- - Osaat käyttää olioita oliomuuttujana ja metodin parametrina. -->

- You know how to use an object as an instance variable and a method parameter.

<!-- - Osaat luoda metodin, joka palauttaa olion. -->

- You know how to create a method that returns an object.

<!-- - Osaat luoda equals-metodin, jolla voi tarkastaa onko kaksi samantyyppistä oliota sisällöllisesti samat. -->

- You know how to create an equals method that can be used to check if two objects of the same type are also internally equal.

</text-box>


<!-- Jatketaan olioiden ja viitteiden parissa. Oletetaan, että käytössämme on alla oleva henkilöä kuvaava luokka. Henkilöllä on oliomuuttujat nimi, ikä, paino ja pituus, jonka lisäksi se tarjoaa metodin mm. painoindeksin laskemiseen. -->

Let's continue working with objects and references, and assume that we Person class shown below available to us. Person has the instance variables name, age, weight and height. It also has methods to calculate, among other things, body mass index.


<!-- ```java
public class Henkilo {

    private String nimi;
    private int ika;
    private int paino;
    private int pituus;

    public Henkilo(String nimi) {
        this(nimi, 0, 0, 0);
    }

    public Henkilo(String nimi, int ika, int pituus, int paino) {
        this.nimi = nimi;
        this.ika = ika;
        this.paino = paino;
        this.pituus = pituus;
    }

    // muita konstruktoreja ja metodeja

    public String getNimi() {
        return this.nimi;
    }

    public int getIka() {
        return this.ika;
    }

    public int getPituus() {
        return this.pituus;
    }

    public void vanhene() {
        this.ika = this.ika + 1;
    }

    public void setPituus(int uusiPituus) {
        this.pituus = uusiPituus;
    }

    public void setPaino(int uusiPaino) {
        this.paino = uusiPaino;
    }

    public double painoindeksi() {
        double pituusPerSata = this.pituus / 100.0;
        return this.paino / (pituusPerSata * pituusPerSata);
    }

    @Override
    public String toString() {
        return this.nimi + ", ikä " + this.ika + " vuotta";
    }
}
``` -->

```java
public class Person {

    private String name;
    private int age;
    private int weight;
    private int height;

    public Person(String name) {
        this(name, 0, 0, 0);
    }

    public Person(String name, int age, int height, int weight) {
        this.name = name;
        this.age = age;
        this.weight = weight;
        this.height = height;
    }

    // other constructors and methods

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }

    public int getHeight() {
        return this.height;
    }

    public void growOlder() {
        this.age = this.age + 1;
    }

    public void setHeight(int newHeight) {
        this.height = newHeight;
    }

    public void setWeight(int newWeight) {
        this.weight = newWeight;
    }

    public double bodyMassIndex() {
        double heightPerHundred = this.height / 100.0;
        return this.weight / (heightPerHundred * heightPerHundred);
    }

    @Override
    public String toString() {
        return this.name + ", age " + this.age + " years";
    }
}
```


<!-- Mitä oikein tapahtuu kun olio luodaan? -->

What precisely happens when a new object is created?


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
``` -->

```java
Person joan = new Person("Joan Ball");
```


<!-- Konstruktorikutsun `new` yhteydessä tapahtuu monta asiaa. Ensin tietokoneen muistista varataan tila oliomuuttujille. Tämän jälkeen oliomuuttujiin asetetaan oletus- tai alkuarvot (esimerkiksi `int`-tyyppisten muuttujien arvoksi tulee 0). Lopulta suoritetaan konstruktorissa oleva lähdekoodi. -->

A constructor call with `new` causes several things to happen. Firstly, space is reserved in computer memory for instance variables. Default or initial values are then assigned to the instance variables (for instance, an `int` type variable receives an initial value of 0). Finally, the source code in the constructor is executed.


<!-- Konstruktorikutsu palauttaa viitteen olioon. **Viite** on tieto olioon liittyvien tietojen paikasta. -->

A constructor call returns a reference to an object. This **reference** contains information about the location of data related to the object.


<img src="../img/drawings/olio-joan.png"/>


<!-- Muuttujan arvoksi asetetaan siis viite, eli tieto olioon liittyvien tietojen paikasta. Yllä oleva kuva paljastaa myös sen, että merkkijonot -- kuten henkilömme nimi -- ovat myös olioita. -->

As such, the value that gets assigned to the variable is a reference, i.e., a piece of information pointing to the data related to the given object. The image above also reveals to us that strings -- such as the name of our person -- are objects as well.

<!-- ## Viittaustyyppisen muuttujan arvon asettaminen kopioi viitteen -->

## Copying of Reference When Assigning to a Reference Variable



<!-- Lisätään ohjelmaan `Henkilo`-tyyppinen muuttuja `ball` ja annetaan sille alkuarvoksi `joan`. Mitä nyt tapahtuu? -->

Let's introduce a `Person` type variable called `ball` into our program and assign `joan` as its initial value. What happens then?


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

Henkilo ball = joan;
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
```

<!-- Lause `Henkilo ball = joan;` luo uuden henkilömuuttujan `ball`, jonka arvoksi kopioidaan muuttujan `joan` arvo. Tämä saa aikaan sen, että `ball` viittaa samaan olioon kuin `joan`. -->

The statement `Person ball = joan;` creates a new Person variable `ball` and copies the value of the variable `joan` as its value. As a result, `ball` refers to the same object as `joan`.


<img src="../img/drawings/olio-joan-ja-ball.png"/>


<!-- Tarkastellaan samaa esimerkkiä hieman pidemmälle. -->

Let's inspect the same example more thoroughly.


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

Henkilo ball = joan;
ball.vanhene();
ball.vanhene();

System.out.println(joan);
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
ball.growOlder();
ball.growOlder();

System.out.println(joan);
```

<!-- <sample-output>

Joan Ball, ikä 0 vuotta
Joan Ball, ikä 2 vuotta

</sample-output> -->

<sample-output>

Joan Ball, age 0 years
Joan Ball, age 2 years

</sample-output>

<!-- Joan Ball -- eli henkilöolio, johon viite muuttujassa `joan` osoittaa -- on alussa 0-vuotias. Tämän jälkeen muuttujaan `ball` asetetaan (eli kopioidaan) muuttujan `joan` arvo. Henkilöoliota `ball` vanhennetaan kaksi vuotta ja sen seurauksena Joan Ball vanhenee! -->

Joan Ball -- i.e., the Person object that the reference in the `joan` variable points to -- starts as 0 years old. The value of the `joan` variable is then assigned (i.e., copied) to the `ball` variable. The Person object `ball` is aged by two years and Joan Ball ages as a consequence!


<!-- Olion sisäinen tila ei kopioidu muuttujan arvoa asetettaessa. Lauseessa `Henkilo ball = joan;` ei luoda uutta oliota -- muuttujan ball arvoksi asetetaan kopio muuttujan joan arvosta, eli viite olioon. -->

An object's internal state isn't copied during variable assignment. The `Person ball = joan;` statement doesn't create a new object -- a copy of `joan`'s value, i.e., a reference to an object, is assigned as the value of the `ball` variable


<img src="../img/drawings/olio-joan-ja-ball-2.png"/>


<!-- Seuraavassa esimerkkiä on jatkettu siten, että `joan`-muuttujaa varten luodaan uusi olio, jonka viite asetetaan muuttujan arvoksi. Muuttuja `ball` viittaa yhä aiemmin luotuun olioon. -->

Next, the example is extended so that a new object is created for the variable `joan`  and a reference to it is assigned to the variable. The variable `ball` still refers to the object that we created earlier.

<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

Henkilo ball = joan;
ball.vanhene();
ball.vanhene();

System.out.println(joan);

joan = new Henkilo("Joan B.");
System.out.println(joan);
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
ball.growOlder();
ball.growOlder();

System.out.println(joan);

joan = new Person("Joan B.");
System.out.println(joan);
```


<!-- Tulostuu: -->

The following is printed:


<!-- <sample-output>

Joan Ball, ikä 0 vuotta
Joan Ball, ikä 2 vuotta
Joan B., ikä 0 vuotta

</sample-output> -->

<sample-output>

Joan Ball, age 0 years
Joan Ball, age 2 years
Joan B., age 0 years

</sample-output>


<!-- Muuttujassa `joan` on siis alussa viite yhteen olioon, mutta lopussa sen arvoksi on kopioitu toisen muuttujan viite. Seuraavassa kuva tilanteesta viimeisen koodirivin jälkeen. -->

Initially, the variable `joan` contains a reference to one object, but another variable's reference is assigned to it in the end. Here's an image of the situation after the last line of code.

<img src="../img/drawings/olio-joan-ja-ball-3.png"/>


<!-- ## Viittaustyyppisen muuttujan arvo `null` -->

## The `null` Value of a Reference Variable


<!-- Jatketaan vielä esimerkkiä asettamalla viittaustyyppisen muuttujan `ball` arvoksi `null`, eli viite "ei mihinkään". Viitteen "ei mihinkään" (eli `null`-viitteen voi asettaa minkä tahansa viittaustyyppisen muuttujan arvoksi. -->

Let's extend the example even further by setting the value of the reference variable `ball` to `null`, i.e., to point "to nothing". Any reference variable can be set to point at nothing by assigning `null` to them.


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

Henkilo ball = joan;
ball.vanhene();
ball.vanhene();

System.out.println(joan);

joan = new Henkilo("Joan B.");
System.out.println(joan);

ball = null;
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
ball.growOlder();
ball.growOlder();

System.out.println(joan);

joan = new Person("Joan B.");
System.out.println(joan);

ball = null;
```

<!-- Viimeisen rivin jälkeen ohjelman tila on seuraavanlainen. -->

The situation of the program after the last line is depicted below.


<img src="../img/drawings/olio-joan-ja-ball-null.png"/>

<!-- Olioon, jonka nimi on Joan Ball, ei enää viittaa kukaan. Oliosta on siis tullut "roska". Java-ohjelmointikielessä ohjelmoijan ei tarvitse huolehtia ohjelman käyttämästä muistista. Javan automaattinen roskienkerääjä käy siivoamassa roskaksi joutuneet oliot aika ajoin. Jos automaattista roskien keruuta ei tapahtuisi, jäisivät roskaksi joutuneet oliot varaamaan muistia ohjelman suorituksen loppuun asti. -->

The object named Joan Ball is no longer reference by nobody. In other words, it has become "garbage". In Java, the programmer doesn't need to worry about the program's memory use. Java's automatic garbage collector cleans up the objects that have become garbage every now and then. If garbage collection did not happen, garbage objects would continue occupying space in memory until the program finishes executing.


<!-- Kokeillaan vielä mitä käy kun yritämme tulostaa muuttujaa, jonka arvona on viite "ei mihinkään" eli `null`. -->

Let's see what happens when we try to print a variable that references "nothing" i.e., `null`.


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

Henkilo ball = joan;
ball.vanhene();
ball.vanhene();

System.out.println(joan);

joan = new Henkilo("Joan B.");
System.out.println(joan);

ball = null;
System.out.println(ball);
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
ball.growOlder();
ball.growOlder();

System.out.println(joan);

joan = new Person("Joan B.");
System.out.println(joan);

ball = null;
System.out.println(ball);
```

<!-- <sample-output>

Joan Ball, ikä 0 vuotta
Joan Ball, ikä 2 vuotta
Joan B., ikä 0 vuotta
null

</sample-output> -->

<sample-output>

Joan Ball, age 0 years
Joan Ball, age 2 years
Joan B., age 0 years
null

</sample-output>


<!-- Viitteen `null` tulostus tulostaa "null". Entäpä jos yritämme kutsua ei mihinkään viittaavan olion metodia, esimerkiksi metodia `vanhene`: -->

Printing a `null` reference prints "null". What if we were to try and call a method, say `growOlder`, on an object that doesn't refer to anything:


<!-- ```java
Henkilo joan = new Henkilo("Joan Ball");
System.out.println(joan);

joan = null;
joan.vanhene();
``` -->

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

joan = null;
joan.growOlder();
```

<!-- Tulos: -->

The result:


<sample-output>

<!-- Joan Ball, age 0 vuotta
**Exception in thread "main" java.lang.NullPointerException
  at Main.main(Main.java:(rivi))
  Java Result: 1** -->

Joan Ball, age 0 years
**Exception in thread "main" java.lang.NullPointerException
  at Main.main(Main.java:(row))
  Java Result: 1**

</sample-output>


<!-- Käy huonosti. Tämä on ehkä ensimmäinen kerta kun näet tekstin **NullPointerException**. Ohjelmassa tapahtuu virhe, joka liittyy siihen, että olemme kutsuneet ei mihinkään viittaavan muuttujan metodia. -->

It doesn't go well. This may be the first time you've encountered the text **NullPointerException**. An error occurred in the program that relates to calling a  method on a variable that doesn't point to anything.


<!-- Voimme luvata, että tulet näkemään edellisen virheen vielä uudelleen. Tällöin ensimmäinen askel on etsiä muuttujia, joiden arvona saattaisi olla `null`. Virheilmoitus on onneksi myös hyödyllinen: se kertoo millä rivillä virhe tapahtuu. Kokeile vaikka itse! -->

We can promise that this is not the last time you'll encounter the previous error. When you do, first look for variables whose value could be `null`. Fortunately for us, the error message is useful - it tells us which row caused the error. Try it out for yourself!


<!-- <programming-exercise name='NullPointerException' tmcname='osa05-Osa05_07.NullPointerException'> -->

<programming-exercise name='NullPointerException' tmcname='part05-Part05_07.NullPointerException'>


<!-- Toteuta ohjelma, jonka suorittaminen aiheuttaa virheen NullPointerException. Virheen tulee tapahtua heti kun ohjelma suoritetaan -- älä siis esimerkiksi lue käyttäjältä syötettä. -->

Implement a program that causes a NullPointerException error. The error should happen straight after the program has started -- i.e., don't wait until you've read user input, for example.

</programming-exercise>



<!-- ##  Olio metodin s to use a Date class. We could use the classname Date, but for the sake of avoiding confusion with the similarly named existing Java class, we will use SimpleDate here.parametrina -->

## Object as a Method Parameter


<!-- Olemme nähneet että metodien parametrina voi olla alkeis- ja viittaustyyppisiä muuttujia. Koska oliot ovat viittaustyyppisiä muuttujia, voi metodin parametriksi määritellä minkä tahansa tyyppisen olion. Demonstroidaan tätä esimerkillä. -->

We've seen both primitive and reference variables act as method parameters. Since objects are reference variables, we can define any type of objects as a method parameter. Let's take a look at a practical demonstration.

<!-- Huvipuiston laitteisiin hyväksytään henkilöitä, joiden pituus ylittää annetun rajan. Kaikissa laitteissa raja ei ole sama. Tehdään huvipuiston laitetta vastaava luokka. Olioa luotaessa konstruktorille annetaan parametriksi laitteen nimi sekä pienin pituus, jolla laitteeseen pääsee. -->

Amusement park rides only permit people on them whose heights exceed a given requirement, which is not the same across all rides. Let's create a class that represents an amusement ride. When a new object is instantiated, the constructor receives as its parameters both the ride's name, and the minimum height that permits entry to the ride.



<!-- ```java
public class Huvipuistolaite {
    private String nimi;
    private int alinPituus;

    public Huvipuistolaite(String nimi, int alinPituus) {
        this.nimi = nimi;
        this.alinPituus = alinPituus;
    }

    public String toString() {
        return this.nimi + ", pituusalaraja: " + this.alinPituus;
    }
}
``` -->

```java
public class AmusementRide {
    private String name;
    private int minHeight;

    public AmusementRide(String name, int minHeight) {
        this.name = name;
        this.minHeight = minHeight;
    }

    public String toString() {
        return this.name + ", minimum height: " + this.minHeight;
    }
}
```

<!-- Tehdään seuraavaksi metodi, jonka avulla voidaan tarkastaa pääseekö tietty henkilö laitteen kyytiin, eli onko henkilö tarpeeksi pitkä. Metodi palauttaa `true` jos parametrina annettu henkilö hyväksytään, `false` jos ei. -->

Let's now write a method that can be used to check whether or not a person is allowed to enter the ride, i.e., whether they're tall enough. The method returns `true` if the person given as a parameter is permitted, and `false` otherwise.

<!-- Alla oletetaan, että henkilöllä metodi ``public int getPituus()`, joka palauttaa henkilön pituuden. -->

We assume below that Person has the method `public int getHeight()`, which returns the height of the person.


<!-- ```java
public class Huvipuistolaite {
    private String nimi;
    private int alinPituus;

    public Huvipuistolaite(String nimi, int alinPituus) {
        this.nimi = nimi;
        this.alinPituus = alinPituus;
    }

    public boolean paaseeKyytiin(Henkilo henkilo) {
        if (henkilo.getPituus() < this.alinPituus) {
            return false;
        }

        return true;
    }

    public String toString() {
        return this.nimi + ", pituusalaraja: " + this.alinPituus;
    }
}
``` -->

```java
public class AmusementRide {
    private String name;
    private int minHeight;

    public AmusementRide(String name, int minHeight) {
        this.name = name;
        this.minHeight = minHeight;
    }

    public boolean allowedOn(Person person) {
        if (person.getHeight() < this.minHeight) {
            return false;
        }

        return true;
    }

    public String toString() {
        return this.name + ", minimum height: " + this.minHeight;
    }
}
```

<!-- Huvipuistolaite-olion metodille `paaseeKyytiin` annetaan siis parametrina `Henkilo`-olio. Kuten aiemmin, muuttujan arvo -- eli tässä viite -- kopioituu metodin käyttöön. Metodissa käsitellään kopioitua viitettä ja kutsutaan parametrina saadun henkilön metodia `getPituus`. -->

The `allowedOn` method of an AmusementRide object is passed a `Person` object as a parameter. Just as earlier, the value of the variable -- in this case, a reference -- is copied for the method's use. The copied reference is processed within the method, and the `getHeight` method of the person passed as a parameter is called.

<!-- Seuraavassa testipääohjelma jossa huvipuistolaitteen metodille annetaan ensin parametriksi henkilöolio `matti` ja sen jälkeen henkilöolio `juhana`: -->

Below we see an example of a main program where the amusement ride's method is called twice: first with the person object `matti` as an arugment, followed by the person object `juhana`:


<!-- ```java
Henkilo matti = new Henkilo("Matti");
matti.setPaino(86);
matti.setPituus(180);

Henkilo juhana = new Henkilo("Juhana");
juhana.setPaino(34);
juhana.setPituus(132);

Huvipuistolaite hurjakuru = new Huvipuistolaite("Hurjakuru", 140);

if (hurjakuru.paaseeKyytiin(matti)) {
    System.out.println(matti.getNimi() + " pääsee laitteeseen");
} else {
    System.out.println(matti.getNimi() + " ei pääse laitteeseen");
}

if (hurjakuru.paaseeKyytiin(juhana)) {
    System.out.println(juhana.getNimi() + " pääsee laitteeseen");
} else {
    System.out.println(juhana.getNimi() + " ei pääse laitteeseen");
}

System.out.println(hurjakuru);
``` -->

```java
Person matti = new Person("Matti");
matti.setWeight(86);
matti.setHeight(180);

Person juhana = new Person("Juhana");
juhana.setWeight(34);
juhana.setHeight(132);

AmusementRide waterTrack = new AmusementRide("Water track", 140);

if (waterTrack.allowedOn(matti)) {
    System.out.println(matti.getName() + " may get on the ride");
} else {
    System.out.println(matti.getName() + " may not get on the ride");
}

if (waterTrack.allowedOn(juhana)) {
    System.out.println(juhana.getName() + " may get on the ride");
} else {
    System.out.println(juhana.getName() + " may not get on the ride");
}

System.out.println(waterTrack);
```

<!-- Ohjelma tulostaa: -->

The output of the program is:

<!-- <sample-output>

Matti pääsee laitteeseen
Juhana ei pääse laitteeseen
Hurjakuru, pituusalaraja: 140

</sample-output> -->

<sample-output>

Matti may get on the ride
Juhana may not get on the ride
Water track, minimum height: 140

</sample-output>

<!-- Entäpä jos haluaisimme tietää kuinka moni on päässyt laitteen kyytiin? -->

What if we wanted to know how many people have gotten on the ride?

<!-- Lisätään huvipuistolaitteelle oliomuuttuja, joka pitää kirjaa kyytiin päässeiden henkilöiden lukumäärästä. -->

Let's add an instance variable to the amusement ride that keeps track of the number of people that have been allowed on.


<!-- ```java
public class Huvipuistolaite {
    private String nimi;
    private int alinPituus;
    private int kavijoita;

    public Huvipuistolaite(String nimi, int alinPituus) {
        this.nimi = nimi;
        this.alinPituus = alinPituus;
        this.kavijoita = 0;
    }

    public boolean paaseeKyytiin(Henkilo henkilo) {
        if (henkilo.getPituus() < this.alinPituus) {
            return false;
        }

        this.kavijoita++;
        return true;
    }

    public String toString() {
        return this.nimi + ", pituusalaraja: " + this.alinPituus +
            ", kävijöitä: " + this.kavijoita;
    }
}
``` -->

```java
public class AmusementRide {
    private String name;
    private int minHeight;
    private int visitors;

    public AmusementRide(String name, int minHeight) {
        this.name = name;
        this.minHeight = minHeight;
        this.visitors = 0;
    }

    public boolean allowedOn(Person person) {
        if (person.getHeight() < this.minHeight) {
            return false;
        }

        this.visitors++;
        return true;
    }

    public String toString() {
        return this.name + ", minimum height: " + this.minHeight +
            ", visitors: " + this.visitors;
    }
}
```

<!-- Nyt aiemmin käyttämässämme esimerkkiohjelmassa pidetään kirjaa myös laitteen kävijöiden määrästä. -->

The previously introduced example program now also keeps track of the number of people who've experienced the ride.

<!-- ```java
Henkilo matti = new Henkilo("Matti");
matti.setPaino(86);
matti.setPituus(180);

Henkilo juhana = new Henkilo("Juhana");
juhana.setPaino(34);
juhana.setPituus(132);

Huvipuistolaite hurjakuru = new Huvipuistolaite("Hurjakuru", 140);

if (hurjakuru.paaseeKyytiin(matti)) {
    System.out.println(matti.getNimi() + " pääsee laitteeseen");
} else {
    System.out.println(matti.getNimi() + " ei pääse laitteeseen");
}

if (hurjakuru.paaseeKyytiin(juhana)) {
    System.out.println(juhana.getNimi() + " pääsee laitteeseen");
} else {
    System.out.println(juhana.getNimi() + " ei pääse laitteeseen");
}

System.out.println(hurjakuru);
``` -->

```java
Person matti = new Person("Matti");
matti.setWeight(86);
matti.setHeight(180);

Person juhana = new Person("Juhana");
juhana.setWeight(34);
juhana.setHeight(132);

AmusementRide waterTrack = new AmusementRide("Water track", 140);

if (waterTrack.allowedOn(matti)) {
    System.out.println(matti.getName() + " may get on the ride");
} else {
    System.out.println(matti.getName() + " may not get on the ride");
}

if (waterTrack.allowedOn(juhana)) {
    System.out.println(juhana.getName() + " may get on the ride");
} else {
    System.out.println(juhana.getName() + " may not get on the ride");
}

System.out.println(waterTrack);
```

<!-- Ohjelma tulostaa: -->

The output of the program is:

<!-- <sample-output>

Matti pääsee laitteeseen
Juhana ei pääse laitteeseen
Hurjakuru, pituusalaraja: 140, kävijöitä: 1

</sample-output> -->

<sample-output>

Matti may get on the ride
Juhana may not get on the ride
Water track, minimum height: 140, visitors: 1

</sample-output>


<!-- <text-box variant='hint' name='Konstruktorien, getterien ja setterien avustettu luominen'> -->

<text-box variant='hint' name='Tool-Assisted Creation of Constructors, Getters, and Setters'>


<!-- Ohjelmointiympäristöt osaavat auttaa ohjelmoijaa. Jos luokalle on määriteltynä oliomuuttujat, onnistuu konstruktorien, getterien ja setterien luominen lähes automaattisesti. -->

Development environments know how to help the programmer. Once instance variables have been created for a class, constructors, getters, and setters can be created almost automatically.


<!-- Mene luokan koodilohkon sisäpuolelle mutta kaikkien metodien ulkopuolelle ja paina yhtä aikaa ctrl ja välilyönti. Jos luokallasi on esim. oliomuuttuja `saldo`, tarjoaa NetBeans mahdollisuuden generoida oliomuuttujalle getteri- ja setterimetodit sekä konstruktorin joka asettaa oliomuuttujalle alkuarvon. -->

Go inside the code block of the class, but outside of all the methods, and simultaneously press ctrl and space. If your class has an instance variable `balance` for example, NetBeans offers the option to generate the getter and setter methods for the instance variable, and a constructor that assigns an initial value for that variable.


<!-- Joillain Linux-koneilla, kuten Kumpulassa olevilla koneilla, tämä saadaan aikaan painamalla yhtä aikaa ctrl, alt ja välilyönti. -->

On some Linux machines, such as the ones at the Kumpula campus (University of Helsinki), this feature is triggered by simultaneously pressing ctrl, alt, and space.


</text-box>


<!-- <programming-exercise name='Kasvatuslaitos (3 osaa)' tmcname='osa05-Osa05_09.Kasvatuslaitos'> -->

<programming-exercise name='Health Station (3 parts)' tmcname='part05-Part05_09.HealthStation'>



<!-- Tehtäväpohjassasi on valmiina jo tutuksi tullut luokka `Henkilo` sekä runko luokalle `Kasvatuslaitos`. Kasvatuslaitosoliot käsittelevät ihmisiä eri tavalla, esim. punnitsevat ja syöttävät ihmisiä. Rakennamme tässä tehtävässä kasvatuslaitoksen. Luokan Henkilö koodiin ei tehtävässä ole tarkoitus koskea! -->

In the exercise base there is a class `Person` that we're already quite familiar with. It also comes with a frame of the `HealthStation` class. Health station objects process people in different ways, e.g., by weighing and feeding them. In this exercise, we'll construct a health station. The code of the Person class should not be modified in this exercise!


<!-- <h2>Henkilöiden punnitseminen</h2> -->

<h2>Weighing people</h2>

<!-- Kasvatuslaitoksen luokkarungossa on valmiina runko metodille `punnitse`: -->

In the class body of the Health station there is an outline for the method `weigh`:

<!-- ```java
public class Kasvatuslaitos {

    public int punnitse(Henkilo henkilo) {
        // palautetaan parametrina annetun henkilön paino
        return -1;
    }
}
``` -->

```java
public class HealthStation {

    public int weigh(Person person) {
        // return the weight of the person passed as the parameter
        return -1;
    }
}
```

<!-- Metodi saa parametrina henkilön ja metodin on tarkoitus palauttaa kutsujalleen parametrina olevan henkilön paino. Paino selviää kutsumalla parametrina olevan henkilön `henkilo` sopivaa metodia. **Eli täydennä metodin koodi!** -->

The method receives a person as a parameter, and is meant to return the weight of that person to the method's caller. The weight can be found by calling a suitable method of the `person` parameter. **In other words, complete the method code!**

<!-- Seuraavassa on pääohjelma jossa kasvatuslaitos punnitsee kaksi henkilöä: -->

Here is a main program where a health station weighs two people:

<!-- ```java
public static void main(String[] args) {
    // esimerkkipääohjelma tehtävän ensimmäiseen kohtaan

    Kasvatuslaitos haaganNeuvola = new Kasvatuslaitos();

    Henkilo eero = new Henkilo("Eero", 1, 110, 7);
    Henkilo pekka = new Henkilo("Pekka", 33, 176, 85);

    System.out.println(eero.getNimi() + " paino: " + haaganNeuvola.punnitse(eero) + " kiloa");
    System.out.println(pekka.getNimi() + " paino: " + haaganNeuvola.punnitse(pekka) + " kiloa");
}
``` -->

```java
public static void main(String[] args) {
    // example main program for the first section of the exercise

    HealthStation childrensHospital = new HealthInstitution();

    Person eero = new Person("Eero", 1, 110, 7);
    Person pekka = new Person("Pekka", 33, 176, 85);

    System.out.println(eero.getName() + " weight: " + childrensHospital.weigh(eero) + " kilos");
    System.out.println(pekka.getName() + " weight: " + childrensHospital.weigh(pekka) + " kilos");
}
```


<!-- Tulostuksen pitäisi olla seuraava: -->

The output should be the following:

<sample-output>

Eero's weight: 7 kilos
Pekka's weight: 85 kilos

</sample-output>


<!-- <h2>Syöttäminen</h2> -->

<h2>Feeding</h2>


<!-- Parametrina olevan olion tilaa on mahdollista muuttaa. Tee kasvatuslaitokselle metodi `public void syota(Henkilo henkilo)` joka kasvattaa parametrina olevan henkilön painoa yhdellä. -->

It's possible to modify the state of the object received as a parameter. Write a method called `public void feed(Person person)` for the health station. It should increment the weight of the parameter person by one.


<!-- Seuraavassa esimerkki, jossa henkilöt ensin punnitaan, ja tämän jälkeen neuvolassa syötetään eeroa kolme kertaa. Tämän jälkeen henkilöt taas punnitaan: -->

The following is an example where the people are weighed first, after which Eero is fed three times in the children's hospital. The people are then weighed again:

<!-- ```java
public static void main(String[] args) {
    Kasvatuslaitos haaganNeuvola = new Kasvatuslaitos();

    Henkilo eero = new Henkilo("Eero", 1, 110, 7);
    Henkilo pekka = new Henkilo("Pekka", 33, 176, 85);

    System.out.println(eero.getNimi() + " paino: " + haaganNeuvola.punnitse(eero) + " kiloa");
    System.out.println(pekka.getNimi() + " paino: " + haaganNeuvola.punnitse(pekka) + " kiloa");

    haaganNeuvola.syota(eero);
    haaganNeuvola.syota(eero);
    haaganNeuvola.syota(eero);

    System.out.println("");

    System.out.println(eero.getNimi() + " paino: " + haaganNeuvola.punnitse(eero) + " kiloa");
    System.out.println(pekka.getNimi() + " paino: " + haaganNeuvola.punnitse(pekka) + " kiloa");
}
``` -->

```java
public static void main(String[] args) {
    HealthStation childrensHospital = new HealthStation();

    Person eero = new Person("Eero", 1, 110, 7);
    Person pekka = new Person("Pekka", 33, 176, 85);

    System.out.println(eero.getName() + " weight: " + childrensHospital.weigh(eero) + " kilos");
    System.out.println(pekka.getName() + " weight: " + childrensHospital.weigh(pekka) + " kilos");

    childrensHospital.feed(eero);
    childrensHospital.feed(eero);
    childrensHospital.feed(eero);

    System.out.println("");

    System.out.println(eero.getName() + " weight: " + childrensHospital.weigh(eero) + " kilos");
    System.out.println(pekka.getName() + " weight: " + childrensHospital.weigh(pekka) + " kilos");
}
```

<!-- Tulostuksen pitäisi paljastaa että Eeron paino on noussut kolmella: -->

The output should reveal that Ethan's weight has increased by three:

<!-- <sample-output>

Eero paino: 7 kiloa
Pekka paino: 85 kiloa

Eero paino: 10 kiloa
Pekka paino: 85 kiloa

</sample-output> -->

<sample-output>

Eero weight: 7 kilos
Pekka weight: 85 kilos

Eero weight: 10 kilos
Pekka weight: 85 kilos

</sample-output>


<!-- <h2>Punnitusten laskeminen</h2> -->

<h2>Counting Weighings</h2>



<!-- Tee kasvatuslaitokselle metodi `public int punnitukset()` joka kertoo kuinka monta punnitusta kasvatuslaitos on ylipäätään tehnyt. *Huom! Tarvitset uuden oliomuuttujan punnitusten lukumäärän laskemiseen!* Testipääohjelma: -->

Create a new method called `public int weighings()` for the health station. It should tell how many weighings the health station has performed. *NB! You will need a new instance variable for counting the number of weighings!*. Here's the test main program:


<!-- ```java
public static void main(String[] args) {
    // esimerkkipääohjelma tehtävän ensimmäiseen kohtaan

    Kasvatuslaitos haaganNeuvola = new Kasvatuslaitos();

    Henkilo eero = new Henkilo("Eero", 1, 110, 7);
    Henkilo pekka = new Henkilo("Pekka", 33, 176, 85);

    System.out.println("punnituksia tehty " + haaganNeuvola.punnitukset());

    haaganNeuvola.punnitse(eero);
    haaganNeuvola.punnitse(pekka);

    System.out.println("punnituksia tehty " + haaganNeuvola.punnitukset());

    haaganNeuvola.punnitse(eero);
    haaganNeuvola.punnitse(eero);
    haaganNeuvola.punnitse(eero);
    haaganNeuvola.punnitse(eero);

    System.out.println("punnituksia tehty " + haaganNeuvola.punnitukset());
}
``` -->

```java
public static void main(String[] args) {

    HealthStation childrensHospital = new HealthInstitution();

    Person eero = new Person("Eero", 1, 110, 7);
    Person pekka = new Person("Pekka", 33, 176, 85);

    System.out.println("weighings performed: " + childrensHospital.weighings());

    childrensHospital.weigh(eero);
    childrensHospital.weigh(pekka);

    System.out.println("weighings performed: " + childrensHospital.weighings());

    childrensHospital.weigh(eero);
    childrensHospital.weigh(eero);
    childrensHospital.weigh(eero);
    childrensHospital.weigh(eero);

    System.out.println("weighings performed: " + childrensHospital.weighings());
}
```

<!-- Tulostuu: -->

The output is:

<!-- <sample-output>

punnituksia tehty 0
punnituksia tehty 2
punnituksia tehty 6

</sample-output> -->

<sample-output>

weighings performed: 0
weighings performed: 2
weighings performed: 6

</sample-output>

</programming-exercise>


<!-- <programming-exercise name='Maksukortti ja Kassapääte (4 osaa)' tmcname='osa05-Osa05_10.MaksukorttiJaKassapaate'> -->

<programming-exercise name='Card Payments (4 sections)' tmcname='part05-Part05_10.CardPayments'>



<!-- <h2>"Tyhmä" Maksukortti</h2> -->

<h2>"Dumb" Payment Card</h2>



<!-- Teimme edellisessä osassa luokan Maksukortti. Kortilla oli metodit edullisesti ja maukkaasti syömistä sekä rahan lataamista varten. -->

In the previous part we created a PaymentCard class. The card had methods for eating affordably and heartily, and also for adding money to the card.

<!-- Edellisen osan tyylillä tehdyssä Maksukortti-luokassa oli kuitenkin ongelma. Kortti tiesi lounaiden hinnan ja osasi sen ansiosta vähentää saldoa oikean määrän. Entä kun hinnat nousevat? Tai jos myyntivalikoimaan tulee uusia tuotteita? Hintojen muuttaminen tarkoittaisi, että kaikki jo käytössä olevat kortit pitäisi korvata uusilla, uudet hinnat tuntevilla korteilla. -->

However, there was a problem with the previous part's implementation of the PaymentCard class. The card knew the prices of the lunches, and could therefore decrease the balance by the correct amount. What if the prices were raised? Or new items were added to the list of products on offer? A change in the pricing would mean that all the existing cards would have to be replaced with new cards that know of the new prices.

<!-- Parempi ratkaisu on tehdä kortit "tyhmiksi", hinnoista ja myytävistä tuotteista tietämättömiksi pelkän saldon säilyttäjiksi. Kaikki äly kannattaakin laittaa erillisiin olioihin, kassapäätteisiin. -->

An improved solution would be to make the cards "dumb"; unaware of the prices and offerings, only keeping track of the balance. All intelligent logic should be placed in separate objects - payment terminals.

<!-- Toteutetaan ensin Maksukortista "tyhmä" versio. Kortilla on ainoastaan metodit saldon kysymiseen, rahan lataamiseen ja rahan ottamiseen. Täydennä alla (ja tehtäväpohjassa) olevaan luokkaan metodin `public boolean otaRahaa(double maara)` ohjeen mukaan: -->

Let's first implement a "dumb" version of the PaymentCard. The card only has methods for requesting the balance, depositing money and withdrawing money. Complete the `public boolean withdraw(double amount)` method in the class below (and found in the exercise template), using the following as a guide:


<!-- ```java
public class Maksukortti {
    private double saldo;

    public Maksukortti(double saldo) {
        this.saldo = saldo;
    }

    public double saldo() {
        return this.saldo;
    }

    public void lataaRahaa(double lisays) {
        this.saldo = this.saldo + lisays;
    }

    public boolean otaRahaa(double maara) {
        // toteuta metodi siten että se ottaa kortilta rahaa vain jos saldo on vähintään maara
        // onnistuessaan metodi palauttaa true ja muuten false
    }
}
``` -->

```java
public class PaymentCard {
    private double balance;

    public PaymentCard(double balance) {
        this.balance = balance;
    }

    public double balance() {
        return this.balance;
    }

    public void deposit(double amount) {
        this.balance = this.balance + amount;
    }

    public boolean withdraw(double amount) {
        // implement the method so that it only takes money from the card if
        // the balance is at least the amount parameter.
        // returns true if successful and false otherwise
    }
}
```

<!-- Testipääohjelma: -->

Test main program:

<!-- ```java
public class Paaohjelma {
    public static void main(String[] args) {
        Maksukortti pekanKortti = new Maksukortti(10);

        System.out.println("rahaa " + pekanKortti.saldo());
        boolean onnistuiko = pekanKortti.otaRahaa(8);
        System.out.println("onnistuiko otto: " + onnistuiko);
        System.out.println("rahaa " + pekanKortti.saldo());

        onnistuiko = pekanKortti.otaRahaa(4);
        System.out.println("onnistuiko otto: " + onnistuiko);
        System.out.println("rahaa " + pekanKortti.saldo());
    }
}
``` -->

```java
public class MainProgram {
    public static void main(String[] args) {
        PaymentCard pekkasCard = new PaymentCard(10);

        System.out.println("money " + pekkasCard.balance());
        boolean wasSuccessful = pekkasCard.withdraw(8);
        System.out.println("withdrawal successful: " + wasSuccessful);
        System.out.println("money " + pekkasCard.balance());

        wasSuccessful = pekkasCard.withdraw(4);
        System.out.println("withdrawal successful: " + wasSuccessful);
        System.out.println("money " + pekkasCard.balance());
    }
}
```

<!-- Tulostuksen kuuluisi olla seuraavanlainen -->

The output should resemble the following

<!-- <sample-output>

rahaa 10.0
onnistuiko otto: true
rahaa 2.0
onnistuiko otto: false
rahaa 2.0

</sample-output> -->

<sample-output>

money 10.0
withdrawal successful: true
money 2.0
withdrawal successful: false
money 2.0

</sample-output>


<!-- <h2>Kassapääte ja käteiskauppa</h2> -->

<h2>Payment Terminal and Cash</h2>


<!-- Unicafessa asioidessa asiakas maksaa joko käteisellä tai maksukortilla. Myyjä käyttää kassapäätettä kortin velottamiseen ja käteismaksujen hoitamiseen. Tehdään ensin kassapäätteestä käteismaksuihin sopiva versio. -->

At the student cafeteria, the customer either pays with cash or card. The cashier uses a payment terminal to charge the card or to process the cash payment. Let's begin by creating a terminal that's suitable for cash payments.


<!-- Kassapäätteen runko. Metodien kommentit kertovat halutun toiminnallisuuden: -->

The outline of the payment terminal. The comments inside the methods lay out the desired functionality:


<!-- ```java
public class Kassapaate {
    private double rahaa;  // kassassa olevan käteisen määrä
    private int edulliset; // myytyjen edullisten lounaiden määrä
    private int maukkaat;  // myytyjen maukkaiden lounaiden määrä

    public Kassapaate() {
        // kassassa on aluksi 1000 euroa rahaa
    }

    public double syoEdullisesti(double maksu) {
        // edullinen lounas maksaa 2.50 euroa.
        // kasvatetaan kassan rahamäärää edullisen lounaan hinnalla ja palautetaan vaihtorahat
        // jos parametrina annettu maksu ei ole riittävän suuri, ei lounasta myydä ja metodi palauttaa koko summan
    }

    public double syoMaukkaasti(double maksu) {
        // maukas lounas maksaa 4.30 euroa.
        // kasvatetaan kassan rahamäärää maukkaan lounaan hinnalla ja palautetaan vaihtorahat
        // jos parametrina annettu maksu ei ole riittävän suuri, ei lounasta myydä ja metodi palauttaa koko summan
    }

    public String toString() {
        return "kassassa rahaa " + rahaa + " edullisia lounaita myyty " + edulliset + " maukkaita lounaita myyty " + maukkaat;
    }
}
``` -->

```java
public class PaymentTerminal {
    private double cash;  // amount of cash
    private int affordableMeals; // number of affordable meals sold
    private int heartyMeals;  // number of hearty meals sold

    public PaymentTerminal() {
        // register initially has 1000 euros of cash
    }

    public double eatAffordably(double payment) {
        // an affordable meal costs 2.50 euros
        // increase the amount of cash by the price of an affordable mean and return the change
        // if the payment parameter is not large enough, no meal is sold and the method should return the whole payment
    }

    public double eatHeartily(double payment) {
        // a hearty meal costs 4.30 euros
        // increase the amount of cash by the price of a hearty mean and return the change
        // if the payment parameter is not large enough, no meal is sold and the method should return the whole payment
    }

    public String toString() {
        return "cash: " + cash + ", number of afforable meals sold: " + affordableMeals + ", number of hearty meals sold: " + heartyMeals;
    }
}
```

<!-- Kassapäätteessä on aluksi rahaa 1000 euroa. Toteuta yllä olevan rungon metodit ohjeen ja alla olevan pääohjelman esimerkkitulosteen mukaan toimiviksi. -->

The terminal starts with 1000 euros in it. Implement the methods correctly based on the instructions and the example prints of the main program below.

<!-- ```java
public class Paaohjelma {
    public static void main(String[] args) {
        Kassapaate unicafeExactum = new Kassapaate();

        double vaihtorahaa = unicafeExactum.syoEdullisesti(10);
        System.out.println("vaihtorahaa jäi " + vaihtorahaa);

        vaihtorahaa = unicafeExactum.syoEdullisesti(5);
        System.out.println("vaihtorahaa jäi " + vaihtorahaa);

        vaihtorahaa = unicafeExactum.syoMaukkaasti(4.3);
        System.out.println("vaihtorahaa jäi " + vaihtorahaa);

        System.out.println(unicafeExactum);
    }
}
``` -->

```java
public class MainProgram {
    public static void main(String[] args) {
        PaymentTerminal unicafeExactum = new PaymentTerminal();

        double change = unicafeExactum.eatAffordably(10);
        System.out.println("remaining change " + change);

        change = unicafeExactum.eatAffordably(5);
        System.out.println("remaining change " + change);

        change = unicafeExactum.eatHeartily(4.3);
        System.out.println("remaining change " + change);

        System.out.println(unicafeExactum);
    }
}
```

<!-- <sample-output>

vaihtorahaa jäi 7.5
vaihtorahaa jäi 2.5
vaihtorahaa jäi 0.0
kassassa rahaa 1009.3 edullisia lounaita myyty 2 maukkaita lounaita myyty 1

</sample-output> -->

<sample-output>

remaining change: 7.5
remaining change: 2.5
remaining change: 0.0
cash: 1009.3, number of affordable meals sold: 2, number of hearty meals sold: 1

</sample-output>


<!-- <h2>Kortilla maksaminen</h2> -->

<h2>Card payments</h2>


<!-- Laajennetaan kassapäätettä siten että myös kortilla voi maksaa. Teemme kassapäätteelle siis metodit joiden parametrina kassapääte saa maksukortin jolta se vähentää valitun lounaan hinnan. Seuraavassa uusien metodien rungot ja ohje niiden toteuttamiseksi: -->

Let's extend our payment terminal to also support card payments. We are going to create new methods for the terminal. It receives a payment card as a parameter, and decreases its balance by the price of the meal that was purchased. Here are the outlines for the methods, and instructions for completing them.

<!-- ```java
public class Kassapaate {
    // ...

    public boolean syoEdullisesti(Maksukortti kortti) {
        // edullinen lounas maksaa 2.50 euroa.
        // jos kortilla on tarpeeksi rahaa, vähennetään hinta kortilta ja palautetaan true
        // muuten palautetaan false
    }

    public boolean syoMaukkaasti(Maksukortti kortti) {
        // maukas lounas maksaa 4.30 euroa.
        // jos kortilla on tarpeeksi rahaa, vähennetään hinta kortilta ja palautetaan true
        // muuten palautetaan false
    }

    // ...
}
``` -->

```java
public class PaymentTerminal {
    // ...

    public boolean eatAffordably(PaymentCard card) {
        // an affordable meal costs 2.50 euros
        // if the payment card has enough money, the balance of the card is decreased by the price, and the method returns true
        // otherwise false is returned
    }

    public boolean eatHeartily(PaymentCard card) {
        // a hearty meal costs 4.30 euros
        // if the payment card has enough money, the balance of the card is decreased by the price, and the method returns true
        // otherwise false is returned
    }

    // ...
}
```


<!-- **Huom:** kortilla maksaminen ei lisää kassapäätteessä olevan käteisen määrää. -->

**NB:** card payments don't increase the amount of cash in the register


<!-- Seuraavassa testipääohjelma ja haluttu tulostus: -->

Below is a main program to test the classes, and the output that is desired:


<!-- ```java
public class Paaohjelma {
    public static void main(String[] args) {
        Kassapaate unicafeExactum = new Kassapaate();

        double vaihtorahaa = unicafeExactum.syoEdullisesti(10);
        System.out.println("vaihtorahaa jäi " + vaihtorahaa);

        Maksukortti antinKortti = new Maksukortti(7);

        boolean onnistuiko = unicafeExactum.syoMaukkaasti(antinKortti);
        System.out.println("riittikö raha: " + onnistuiko);
        onnistuiko = unicafeExactum.syoMaukkaasti(antinKortti);
        System.out.println("riittikö raha: " + onnistuiko);
        onnistuiko = unicafeExactum.syoEdullisesti(antinKortti);
        System.out.println("riittikö raha: " + onnistuiko);

        System.out.println(unicafeExactum);
    }
}
``` -->

```java
public class MainProgram {
    public static void main(String[] args) {
        PaymentTerminal unicafeExactum = new PaymentTerminal();

        double change = unicafeExactum.eatAffordably(10);
        System.out.println("remaining change: " + change);

        Maksukortti annesCard = new PaymentCard(7);

        boolean wasSuccessful = unicafeExactum.eatHeartily(annesCard);
        System.out.println("there was enough money: " + wasSuccessful);
        wasSuccessful = unicafeExactum.eatHeartily(annesCard);
        System.out.println("there was enough money: " + wasSuccessful);
        wasSuccessful = unicafeExactum.eatAffordably(annesCard);
        System.out.println("there was enough money: " + wasSuccessful);

        System.out.println(unicafeExactum);
    }
}
```

<!-- <sample-output>

vaihtorahaa jäi 7.5
riittikö raha: true
riittikö raha: false
riittikö raha: true
kassassa rahaa 1002.5 edullisia lounaita myyty 2 maukkaita lounaita myyty 1

</sample-output> -->

<sample-output>

remaining change: 7.5
there was enough money: true
there was enough money: false
there was enough money: true
money: 1002.5, number of affordable meals sold: 2, number of hearty meals sold: 1

</sample-output>


<!-- <h2>Rahan lataaminen</h2> -->

<h2>Depositing Money</h2>


<!-- Lisätään vielä kassapäätteelle metodi jonka avulla kortille voidaan ladata lisää rahaa. Muista, että rahan lataamisen yhteydessä ladattava summa viedään kassapäätteeseen. Metodin runko: -->

Let's create a method for the terminal that can be used to add money to a payment card. Remember that when money is deposited it goes in the register. The template for the method:


<!-- ```java
public void lataaRahaaKortille(Maksukortti kortti, double summa) {
    // ...
}
``` -->

```java
public void depositToCard(PaymentCard card, double sum) {
    // ...
}
```


<!-- Testipääohjelma ja esimerkkisyöte: -->

A main program to illustrate:


<!-- ```java
public class Paaohjelma {
    public static void main(String[] args) {
        Kassapaate unicafeExactum = new Kassapaate();
        System.out.println(unicafeExactum);

        Maksukortti antinKortti = new Maksukortti(2);

        System.out.println("kortilla rahaa " + antinKortti.saldo() + " euroa");

        boolean onnistuiko = unicafeExactum.syoMaukkaasti(antinKortti);
        System.out.println("riittikö raha: " + onnistuiko);

        unicafeExactum.lataaRahaaKortille(antinKortti, 100);

        onnistuiko = unicafeExactum.syoMaukkaasti(antinKortti);
        System.out.println("riittikö raha: " + onnistuiko);

        System.out.println("kortilla rahaa " + antinKortti.saldo() + " euroa");

        System.out.println(unicafeExactum);
    }
}
``` -->

```java
public class MainProgram {
    public static void main(String[] args) {
        PaymentTerminal unicafeExactum = new PaymentTerminal();
        System.out.println(unicafeExactum);

        PaymentCard annesCard = new PaymentCard(2);

        System.out.println("amount on the card " + annesCard.balance() + " euros");

        boolean wasSuccessful = unicafeExactum.eatHeartily(annesCard);
        System.out.println("there was enough money: " + wasSuccessful);

        unicafeExactum.depositToCard(annesCard, 100);

        wasSuccessful = unicafeExactum.eatHeartily(annesCard);
        System.out.println("there was enough money: " + wasSuccessful);

        System.out.println("amount on the card " + annesCard.balance() + " euros");

        System.out.println(unicafeExactum);
    }
}
```

<!-- <sample-output>

kassassa rahaa 1000.0 edullisia lounaita myyty 0 maukkaita lounaita myyty 0
kortilla rahaa 2.0 euroa
riittikö raha: false
riittikö raha: true
kortilla rahaa 97.7 euroa
kassassa rahaa 1100.0 edullisia lounaita myyty 0 maukkaita lounaita myyty 1

</sample-output> -->

<sample-output>

money: 1000.0, number of affordable meals sold: 0, number of hearty meals sold: 0
amount of money on the card is 2.0 euros
there was enough money: false
there was enough money: true
amount of money on the card is 97.7 euros
money: 1100.0, number of affordable meals sold: 0, number of hearty meals sold: 1

</sample-output>


</programming-exercise>


<!-- ## Olio oliomuuttujana -->

## Object as Instance Variables


<!-- Oliot voivat sisältää viitteitä olioihin. -->

Objects may contain references to objects.

<!-- Jatketaan henkilöiden parissa ja lisätään henkilölle syntymäpäivä. Syntymäpäivä on luonnollista esittää `Paivays`-luokan avulla: -->

We'll carry on working with persons, and add a birthday to the person class. A natural way of representing a birthday is to use a `Date` class. We could use the classname `Date`, but for the sake of avoiding confusion with the [similarly named existing Java class](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html), we will use `SimpleDate` here.

<!-- ```java
public class Paivays {
    private int paiva;
    private int kuukausi;
    private int vuosi;

    public Paivays(int paiva, int kuukausi, int vuosi) {
        this.paiva = paiva;
        this.kuukausi = kuukausi;
        this.vuosi = vuosi;
    }

    public int getPaiva() {
        return this.paiva;
    }

    public int getKuukausi() {
        return this.kuukausi;
    }

    public int getVuosi() {
        return this.vuosi;
    }

    @Override
    public String toString() {
        return this.paiva + "." + this.kuukausi + "." + this.vuosi;
    }
}
``` -->

```java
public class SimpleDate {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year) {
        this.day = day;
        this.month = month;
        this.year = year;
    }

    public int getDay() {
        return this.day;
    }

    public int getMonth() {
        return this.month;
    }

    public int getYear() {
        return this.year;
    }

    @Override
    public String toString() {
        return this.day + "." + this.month + "." + this.year;
    }
}
```

<!-- Koska tiedämme syntymäpäivän, henkilön ikää ei tarvitse säilöä erillisenä oliomuuttujana. Henkilön ikä on pääteltävissä syntymäpäivästä. Oletetaan, luokassa `Henkilo` on nyt seuraavat muuttujat. -->

Since we know the birthday, there is no need to store that age of a person as a separate instance variable. The age of the person can be inferred from their birthday. Let's assume that the class `Person` now has the following variables.

<!-- ```java
public class Henkilo {
    private String nimi;
    private Paivays syntymapaiva;
    private int paino = 0;
    private int pituus = 0;

// ...
``` -->

```java
public class Person {
    private String name;
    private SimpleDate birthday;
    private int weight = 0;
    private int length = 0;

// ...
```

<!-- Tehdään henkilölle uusi konstruktori, joka mahdollistaa syntymäpäivän asettamisen: -->

Let's create a new Person constructor that allows for setting the birthday:

<!-- ```java
public Henkilo(String nimi, Paivays paivays) {
    this.nimi = nimi;
    this.syntymapaiva = paivays;
}
``` -->

```java
public Person(String name, SimpleDate date) {
    this.name = name;
    this.birthday = date;
}
```

<!-- Edellisen konstruktorin lisäksi henkilölle voisi luoda myös konstruktorin, missä syntymäpäivä annettaisiin parametrina. -->

Along with the constructor above, we could give Person another constructor where the birthday was given as integers.

<!-- ```java
public Henkilo(String nimi, int paiva, int kuukausi, int vuosi) {
    this.nimi = nimi;
    this.syntymapaiva = new Paivays(paiva, kuukausi, vuosi);
}
``` -->

```java
public Person(String name, int day, int month, int year) {
    this.name = name;
    this.birthday = new SimpleDate(day, month, year);
}
```

<!-- Konstruktorin parametrina annetaan erikseen päiväyksen osat (päivä, kuukausi, vuosi), niistä luodaan päiväysolio, ja lopulta päiväysolion viite kopioidaan oliomuuttujan `syntymapaiva` arvoksi. -->

The constructor receives as parameters the different parts of the date (day, month, year). They are used to create a date object, and finally the reference to that date is copied as the value of the instance variable `birthday`.


<!-- Muokataan Henkilo-luokassa olevaa `toString`-metodia siten, että metodi palauttaa iän sijaan syntymäpäivän: -->

Let's modify the `toString` method of the Person class so that instead of age, the method returns the birthday:


<!-- ```java
public String toString() {
    return this.nimi + ", syntynyt " + this.syntymapaiva;
}
``` -->

```java
public String toString() {
    return this.name + ", born on " + this.birthday;
}
```

<!-- Kokeillaan miten uusittu Henkilöluokka toimii. -->

Let's see how the updated Person class works.

<!-- ```java
Paivays paivays = new Paivays(1, 1, 780);
Henkilo muhammad = new Henkilo("Muhammad ibn Musa al-Khwarizmi", paivays);
Henkilo pascal = new Henkilo("Blaise Pascal", 19, 6, 1623);

System.out.println(muhammad);
System.out.println(pascal);
``` -->

```java
SimpleDate date = new SimpleDate(1, 1, 780);
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", date);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

System.out.println(muhammad);
System.out.println(pascal);
```

<!-- <sample-output>

Muhammad ibn Musa al-Khwarizmi, syntynyt 1.1.780
Blaise Pascal, syntynyt 19.6.1623

</sample-output> -->

<sample-output>

Muhammad ibn Musa al-Khwarizmi, born on 1.1.780
Blaise Pascal, born on 19.6.1623

</sample-output>


<!-- Henkilöoliolla on nyt oliomuuttujat `nimi` ja `syntymapaiva`. Muuttuja `nimi` on merkkijono, joka sekin on siis olio, ja muuttuja `syntymapaiva` on Päiväysolio. -->

Now a person object has the instance variables `name` and `birthday`. The variable `name` is a string, which itself is an object, and the variable `birthday` is a SimpleDate object.


<!-- Molemmat muuttujat sisältävät arvon olioon. Henkilöolio sisältää siis kaksi viitettä. Alla olevassa kuvassa paino ja pituus on jätetty huomiotta. -->

Both variables contain a reference to an object. As such, a person object contains two references. In the image below, weight and height have been ignored.

<img src="../img/drawings/muhammad-ja-pascal.png"/>


<!-- Pääohjelmalla on nyt siis langan päässä kaksi Henkilö-olioa. Henkilöllä on nimi ja syntymäpäivä. Koska molemmat ovat olioita, ovat ne henkilöllä langan päässä. -->

The main program now has two person objects at the ends of the arrows. A person has a name and a birthday. Since both are objects, these attributes are at arrows' ends in the person object.


<!-- Syntymäpäivä vaikuttaa hyvältä laajennukselta Henkilö-luokkaan. Totesimme aiemmin, että oliomuuttuja `ika` voidaan laskea syntymäpäivästä, joten siitä hankkiuduttiin eroon. -->


Adding the birthday appears to be a sensible way to extend the Person class. We noted earlier on that the instance variable `age` can be calculated from birthday, which is why it was removed.

<!-- <text-box variant='hint' name='Päivämäärän käyttö Java-ohjelmissa'> -->

<text-box variant='hint' name='Date in Java Programs'>


<!-- Käytämme edellä omaa luokkaa `Paivays` päivämäärän esittämiseen, sillä sen avulla voi havainnollistaa ja harjoitella olioiden toimintaa. Mikäli omissa ohjelmissaan haluaa käsitellä päivämääriä, kannattaa tutustua Javan valmiiseen luokkaan [LocalDate](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html), joka sisältää merkittävän määrän päivämäärien käsittelyyn liittyvää toiminnallisuutta. -->

In the section above, we use our own `SimpleDate` class to represent date, as we can use it to illustrate and practice with object behavior. If you want to deal with dates in your own programs, it's worth checking out the pre-made [LocalDate](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html) class in Java that comes with a lot functionality for handling dates.

<!-- Esimerkiksi nykyinen päivä selviää Javan valmiin `LocalDate`-luokan avulla seuraavasti: -->

The current date, for example, can be used with the Java's `LocalDate` class like so:

<!-- ```java
import java.time.LocalDate;

public class Esimerkki {

    public static void main(String[] args) {

        LocalDate nyt = LocalDate.now();
        int vuosi = nyt.getYear();
        int kuukausi = nyt.getMonthValue();
        int paiva = nyt.getDayOfMonth();

        System.out.println("tänään on " + paiva + "." + kuukausi + "." + vuosi);

    }
}
``` -->

```java
import java.time.LocalDate;

public class Example {

    public static void main(String[] args) {

        LocalDate now = LocalDate.now();
        int year = now.getYear();
        int month = now.getMonthValue();
        int day = now.getDayOfMonth();

        System.out.println("today is  " + day + "." + month + "." + year);

    }
}
```

</text-box>


<!-- <programming-exercise name='Henkilö ja lemmikki' tmcname='osa05-Osa05_08.HenkiloJaLemmikki'> -->

<programming-exercise name='Biggest Pet Shop' tmcname='part05-Part05_08.BiggestPetShop'>



<!-- Tehtäväpohjassa tulee kaksi luokkaa, `Henkilo` ja `Lemmikki`. Jokaisella henkilöllä on yksi lemmikki. Täydennä luokan `Henkilo` metodia `public String toString` siten, että metodi palauttaa merkkijonon, joka kertoo henkilön nimen lisäksi lemmikin nimen ja rodun. -->

Two classes, namely `Person` and `Pet`, are included in the exercise template. Each person has one pet. Modify the `public String toString` method of the `Person` class so that the string returned by it tells the pet's name and breed, as well as the person's own name.


<!-- ```java
Lemmikki hulda = new Lemmikki("Hulda", "sekarotuinen koira");
Henkilo leevi = new Henkilo("Leevi", hulda);

System.out.println(leevi);
``` -->

```java
Pet lucy = new Pet("Lucy", "golden retriever");
Person leo = new Person("Leo", lucy);

System.out.println(leo);
```

<!-- <sample-output>

Leevi, kaverina Hulda, joka on sekarotuinen koira

</sample-output> -->

<sample-output>

Leo, has a friend called Lucy (golden retriever)

</sample-output>

</programming-exercise>


<!-- ## Samantyyppinen olio metodin parametrina -->

## Object of Same Type as Method Parameter

<!-- Jatkamme luokan `Henkilo` parissa. Kuten muistamme, henkilöt tietävät syntymäpäivänsä: -->

We'll continue with the `Person` class. We recall that persons know their birthdays:

<!-- ```java
public class Henkilo {

    private String nimi;
    private Paivays syntymapaiva;
    private int pituus;
    private int paino;

    // ...
}
``` -->

```java
public class Person {

    private String name;
    private SimpleDate birthday;
    private int height;
    private int weight;

    // ...
}
```

<!-- Haluamme vertailla kahden henkilön ikää. Vertailu voidaan hoitaa usealla tavalla. Voisimme esimerkiksi toteuttaa Henkilo-luokkaan metodin `public int ikaVuosina()`, jolloin kahden henkilön iän vertailu tapahtuisi tällöin seuraavasti: -->

We would like to compare the ages of two people. The comparison can be done in multiple ways. We could, for instance, implement a method called `public int ageAsYears()` for the Person class, in which case the comparison would happen like so:

<!-- ```java
Henkilo muhammad = new Henkilo("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Henkilo pascal = new Henkilo("Blaise Pascal", 19, 6, 1623);

if (muhammad.ikaVuosina() > pascal.ikaVuosina()) {
    System.out.println(muhammad.getNimi() + " on vanhempi kuin " + pascal.getNimi());
}
``` -->

```java
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

if (muhammad.ageAsYears() > pascal.ageAsYears()) {
    System.out.println(muhammad.getName() + " is older than " + pascal.getName());
}
```

<!-- Opettelemme tässä hieman "oliohenkisemmän" tavan kahden henkilön ikävertailun tekemiseen. -->

We're now going to learn a more "object-oriented" way to compare the ages of people.

<!-- Teemme luokalle Henkilo metodin `boolean vanhempiKuin(Henkilo verrattava)`, jonka avulla tiettyä henkilö-olioa voi verrata parametrina annettuun henkilöön iän perusteella. -->

We're going to create a new method `boolean olderThan(Person compared)` in the Person class. It can be used to perform an age-based comparison between a person object and another one provided as a parameter.

<!-- Metodia on tarkoitus käyttää seuraavaan tyyliin: -->

The method is meant to be used like this:


<!-- ```java
Henkilo muhammad = new Henkilo("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Henkilo pascal = new Henkilo("Blaise Pascal", 19, 6, 1623);

if (muhammad.vanhempiKuin(pascal)) {  //  sama kun muhammad.vanhempiKuin(pascal)==true
    System.out.println(muhammad.getNimi() + " on vanhempi kuin " + pascal.getNimi());
} else {
    System.out.println(muhammad.getNimi() + " ei ole vanhempi kuin " + pascal.getNimi());
}
``` -->

```java
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

if (muhammad.olderThan(pascal)) {  //  same as muhammad.olderThan(pascal)==true
    System.out.println(muhammad.getName() + " is older than " + pascal.getName());
} else {
    System.out.println(muhammad.getName() + " is not older than " + pascal.getName());
}
```

<!-- Yllä oleva ohjelma kysyy onko al-Khwarizmi vanhempi kuin Pascal. Metodi `vanhempiKuin` palauttaa arvon `true` jos olio jonka kohdalla metodia kutsutaan (`olio.vanhempiKuin(parametrinaAnnettavaOlio)`) on vanhempi kuin parametrina annettava olio, ja `false` muuten. -->

The program above asks if al-Khwarizmi is older than Pascal. The method `olderThan` returns `true` if the object that's calling the method (`object.olderThan(objectGivenAsParameter)`) is older than the object given as the parameter, and `false` otherwise.

<!-- Käytännössä yllä kutsutaan "Muhammad ibn Musa al-Khwarizmia" vastaavan olion, johon muuttuja `muhammad` viittaa, metodia `vanhempiKuin`. Metodille annetaan parametriksi "Blaise Pascal" vastaavan olion viite `pascal`. -->

What happens is that we call the `olderThan` method of the object for "Muhammad ibn Musa al-Khwarizmi", which is referenced to by the variable `muhammad`. The reference `pascal` that matches the object "Blaise Pascal" is given as the parameter to that method.

<!-- Ohjelma tulostaa: -->

The program prints:

<!-- <sample-output>

Muhammad ibn Musa al-Khwarizmi on vanhempi kuin Blaise Pascal

</sample-output> -->

<sample-output>

Muhammad ibn Musa al-Khwarizmi is older than Blaise Pascal

</sample-output>

<!-- Metodille `vanhempiKuin` annetaan parametrina henkilöolio. Tarkemmin sanottuna metodin parametriksi määriteltyyn muuttujaan kopioituu parametrina annettavan muuttujan sisältämä arvo, eli viite olioon. -->

The method `olderThan` receives a person object as its parameter. Put more precisely, the variable that is defined as the method parameter receives a copy of the value contained by the given variable. That value is a reference to an object in this case.

<!-- Metodin toteutus näyttää seuraavalta. Huomaa, että **metodi voi palauttaa arvon useammasta kohtaa** -- alla vertailu on pilkottu useampaan osaan vuoden, kuukauden ja päivän kohdalta: -->

The method's implementation is illustrated below. Note that the **method may return a value in more than one place** -- here the comparison has been divided into multiple parts based on the years, months, and days:


<!-- ```java
public class Henkilo {
    // ...

    public boolean vanhempiKuin(Henkilo verrattava) {
        // 1. Verrataan ensin vuosia
        int omaVuosi = this.getSyntymapaiva().getVuosi();
        int verrattavanVuosi = verrattava.getSyntymapaiva().getVuosi();

        if (omaVuosi < verrattavanVuosi) {
            return true;
        }

        if (omaVuosi > verrattavanVuosi) {
            return false;
        }

        // 2. Syntymävuosi on sama, verrataan kuukausia
        int omaKuukausi = this.getSyntymapaiva().getKuukausi();
        int verrattavanKuukausi = verrattava.getSyntymapaiva().getKuukausi();

        if (omaKuukausi < verrattavanKuukausi) {
            return true;
        }

        if (omaKuukausi > verrattavanKuukausi) {
            return false;
        }

        // 3. Syntymävuosi ja kuukausi on sama, verrataan päiviä
        int omaPaiva = this.getSyntymapaiva().getPaiva();
        int verrattavanPaiva = verrattava.getSyntymapaiva().getPaiva();

        if (omaPaiva < verrattavanPaiva) {
            return true;
        }

        return false;
    }
}
``` -->

```java
public class Person {
    // ...

    public boolean olderThan(Person compared) {
        // 1. First compare years
        int ownYear = this.getBirthday().getYear();
        int comparedYear = compared.getBirthday().getYear();

        if (ownYear < comparedYear) {
            return true;
        }

        if (ownYear > comparedYear) {
            return false;
        }

        // 2. Same birthyear, compare months
        int ownMonth = this.getBirthday().getMonth();
        int comparedMonth = compared.getBirthday().getMonth();

        if (ownMonth < comparedMonth) {
            return true;
        }

        if (ownMonth > comparedMonth) {
            return false;
        }

        // 3. Same birth year and month, compare days
        int ownDay = this.getBirthday().getDay();
        int comparedDay = compared.getBirthday().getDay();

        if (ownDay < comparedDay) {
            return true;
        }

        return false;
    }
}
```


<!-- Mietitään hieman olio-ohjelmoinnin periatteiden abstrahointia. Abstrahoinnin ajatuksena on käsitteellistää ohjelmakoodia siten, että kullakin käsitteellä on omat selkeät vastuunsa. Kun pohdimme yllä esitettyä ratkaisua, huomaamme, että päivämäärien vertailutoiminnallisuus kuuluisi mielummin luokkaan `Paivays` luokan `Henkilo`-sijaan. -->

Let's take a moment to think about abstraction, a principle of object-oriented programming. The idea behind abstraction is to conceptualize program code in such a way that each concept has its own clear responsibilities. If we consider the solution above, we notice, however, that it would be more suitable to have the comparison login within the `SimpleDate` class instead of the `Person` class.

<!-- Luodaan luokalle `Paivays` metodi `public boolean aiemmin(Paivays verrattava)`. Metodi palauttaa arvon `true`, jos metodille parametrina annettu päiväys on kyseisen olion päiväyksen jälkeen. -->

We'll create a `public boolean before(SimpleDate compared)` method for the `SimpleDate` class. The method returns `true` if the date given as the parameter comes after (or on the same day as) the date of the object whose method is called.

<!-- ```java
public class Paivays {
    private int paiva;
    private int kuukausi;
    private int vuosi;

    public Paivays(int paiva, int kuukausi, int vuosi) {
        this.paiva = paiva;
        this.kuukausi = kuukausi;
        this.vuosi = vuosi;
    }

    public String toString() {
        return this.paiva + "." + this.kuukausi + "." + this.vuosi;
    }

    // metodilla tarkistetaan onko tämä päiväysolio (`this`) ennen
    // parametrina annettavaa päiväysoliota (`verrattava`)
    public boolean aiemmin(Paivays verrattava) {
        // ensin verrataan vuosia
        if (this.vuosi < verrattava.vuosi) {
            return true;
        }

        if (this.vuosi > verrattava.vuosi) {
            return false;
        }

        // jos vuodet ovat samat, verrataan kuukausia
        if (this.kuukausi < verrattava.kuukausi) {
            return true;
        }

        if (this.kuukausi > verrattava.kuukausi) {
            return false;
        }

        // vuodet ja kuukaudet samoja, verrataan päivää
        if (this.paiva < verrattava.paiva) {
            return true;
        }

        return false;
    }
}
``` -->

```java
public class SimpleDate {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year) {
        this.day = day;
        this.month = month;
        this.year = year;
    }

    public String toString() {
        return this.day + "." + this.month + "." + this.year;
    }

    // used to check if this date object (`this`) is before
    // the date object given as the parameter (`compared`)
    public boolean before(SimpleDate compared) {
        // first compare years
        if (this.year < compared.year) {
            return true;
        }

        if (this.year > compared.year) {
            return false;
        }

        // years are same, compare months
        if (this.month < compared.month) {
            return true;
        }

        if (this.month > compared.month) {
            return false;
        }

        // years and months are same, compare days
        if (this.day < compared.day) {
            return true;
        }

        return false;
    }
}
```

<!-- Vaikka oliomuuttujat `vuosi`, `kuukausi` ja `paiva` ovat olion kapseloimia (`private`) oliomuuttujia, pystymme lukemaan niiden arvon kirjoittamalla `verrattava.*muuttujanNimi*`. Tämä johtuu siitä, että `private`-muuttujat ovat luettavissa kaikissa metodeissa, jotka kyseinen luokka sisältää. Huomaa, että syntaksi (kirjoitusasu) vastaa tässä jonkin olion metodin kutsumista. Toisin kuin metodia kutsuttaessa, viittaamme olion kenttään, jolloin metodikutsun osoittavia sulkeita ei kirjoiteta. -->

Even though the instance variables `year`, `month`, and `day` are encapsulated (`private`) instance variables, we are able to read their values by writing `compared.*variableName*`. This is because `private` variables are accessible from within all of the methods of the given class. Notice that the syntax here matches that of an object method call. Unlike a method call, however, we're refer to an object's field, so the parentheses that indicate a method call are not written.

<!-- Metodin käyttöesimerkki: -->

An example of how to use the method:

<!-- ```java
public static void main(String[] args) {
    Paivays p1 = new Paivays(14, 2, 2011);
    Paivays p2 = new Paivays(21, 2, 2011);
    Paivays p3 = new Paivays(1, 3, 2011);
    Paivays p4 = new Paivays(31, 12, 2010);

    System.out.println(p1 + " aiemmin kuin " + p2 + ": " + p1.aiemmin(p2));
    System.out.println(p2 + " aiemmin kuin " + p1 + ": " + p2.aiemmin(p1));

    System.out.println(p2 + " aiemmin kuin " + p3 + ": " + p2.aiemmin(p3));
    System.out.println(p3 + " aiemmin kuin " + p2 + ": " + p3.aiemmin(p2));

    System.out.println(p4 + " aiemmin kuin " + p1 + ": " + p4.aiemmin(p1));
    System.out.println(p1 + " aiemmin kuin " + p4 + ": " + p1.aiemmin(p4));
}
``` -->

```java
public static void main(String[] args) {
    SimpleDate d1 = new SimpleDate(14, 2, 2011);
    SimpleDate d2 = new SimpleDate(21, 2, 2011);
    SimpleDate d3 = new SimpleDate(1, 3, 2011);
    SimpleDate d4 = new SimpleDate(31, 12, 2010);

    System.out.println(p1 + " is earlier than " + p2 + ": " + p1.before(p2));
    System.out.println(p2 + " is earlier than " + p1 + ": " + p2.before(p1));

    System.out.println(p2 + " is earlier than " + p3 + ": " + p2.before(p3));
    System.out.println(p3 + " is earlier than " + p2 + ": " + p3.before(p2));

    System.out.println(p4 + " is earlier than " + p1 + ": " + p4.before(p1));
    System.out.println(p1 + " is earlier than " + p4 + ": " + p1.before(p4));
}
```

<!-- <sample-output>

14.2.2011 aiemmin kuin 21.2.2011: true
21.2.2011 aiemmin kuin 14.2.2011: false
21.2.2011 aiemmin kuin 1.3.2011: true
1.3.2011 aiemmin kuin 21.2.2011: false
31.12.2010 aiemmin kuin 14.2.2011: true
14.2.2011 aiemmin kuin 31.12.2010: false

</sample-output> -->

<sample-output>

14.2.2011 is earlier than 21.2.2011: true
21.2.2011 is earlier than 14.2.2011: false
21.2.2011 is earlier than 1.3.2011: true
1.3.2011 is earlier than 21.2.2011: false
31.12.2010 is earlier than 14.2.2011: true
14.2.2011 is earlier than 31.12.2010: false

</sample-output>

<!-- Muunnetaan vielä henkilön metodia vanhempiKuin siten, että hyödynnämme jatkossa päivämäärän tarjoamaa vertailutoiminnallisuutta. -->

Let's tweak the method olderThan of the Person class so that from here on, we make use of the comparison logic that date provides.

<!-- ```java
public class Henkilo {
    // ...

    public boolean vanhempiKuin(Henkilo verrattava) {
        if (this.syntymapaiva.aiemmin(verrattava.getSyntymapaiva())) {
            return true;
        }

        return false;

        // tai suoraan:
        // return this.syntymapaiva.aiemmin(verrattava.getSyntymapaiva());
    }
}
``` -->

```java
public class Person {
    // ...

    public boolean olderThan(Person compared) {
        if (this.birthday.before(compared.getBirthday())) {
            return true;
        }

        return false;

        // or return more directly:
        // return this.birthday.before(compared.getBirthday());
    }
}
```

<!-- Nyt päivämäärän konkreettinen vertailu on toteutettu luokassa, johon se loogisesti (luokkien nimien perusteella) kuuluukin. -->

The concrete comparison of dates is now implemented in the class that it logically (based on the class names) belongs to.


<!-- <programming-exercise name='Asuntovertailu (3 osaa)' tmcname='osa05-Osa05_11.Asuntovertailu'> -->

<programming-exercise name='Comparing Apartments (3 parts)' tmcname='part05-Part05_11.ComparingApartments'>


<!-- Asuntovälitystoimiston tietojärjestelmässä kuvataan myynnissä olevaa asuntoa seuraavasta luokasta tehdyillä olioilla: -->

In the estate agent's information system, an apartment that is on sale is represented by an object that is instantiated from the following class:

<!-- ```java
public class Asunto {
    private int huoneita;
    private int nelioita;
    private int neliohinta;

    public Asunto(int huoneita, int nelioita, int neliohinta) {
        this.huoneita = huoneita;
        this.nelioita = nelioita;
        this.neliohinta = neliohinta;
    }
}
``` -->

```java
public class Apartment {
    private int rooms;
    private int squares;
    private int pricePerSquare;

    public Apartment(int rooms, int squares, int pricePerSquare) {
        this.rooms = rooms;
        this.square = squares;
        this.pricePerSquare = pricePerSquare;
    }
}
```

<!-- Tehtävänä on toteuttaa muutama metodi, joiden avulla myynnissä olevia asuntoja voidaan vertailla. -->

Your task is to create a few methods that can be used to compare apartments being sold.


<!-- <h2>Onko asunto suurempi</h2> -->

<h2>Comparing Sizes</h2>

<!-- Tee metodi `public boolean suurempi(Asunto verrattava)` joka palauttaa true jos asunto-olio, jolle metodia kutsutaan, on pinta-alaltaan suurempi kuin verrattavana oleva asunto-olio. -->

Create a method `public boolean largerThan(Apartment compared)` that returns true if the apartment object whose method is being called has a larger total surface area than the apartment object that is being compared.

<!-- Esimerkki metodin toiminnasta: -->

An example of how the method should work:


<!-- ```java
Asunto eiraYksio = new Asunto(1, 16, 5500);
Asunto kallioKaksio = new Asunto(2, 38, 4200);
Asunto jakomakiKolmio = new Asunto(3, 78, 2500);

System.out.println(eiraYksio.suurempi(kallioKaksio));       // false
System.out.println(jakomakiKolmio.suurempi(kallioKaksio));  // true
``` -->

```java
Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);
```

<!-- <h2>Asuntojen hintaero</h2> -->

<h2>Price difference</h2>


<!-- Tee metodi `public int hintaero(Asunto verrattava)` joka palauttaa asunto-olion jolle metodia kutsuttiin ja parametrina olevan asunto-olion hintaeron. Hintaero on asuntojen hintojen erotuksen (hinta lasketaan kertomalla neliöhinta neliöillä) itseisarvo. -->

Create a method `public int priceDifference(Apartment compared)` that returns the price difference between the apartment object whose method was called and the apartment object received as the parameter. The price difference is the absolute value of the difference of the prices (price can be calculated by multiplying the price per square by the number of squares).

<!-- Esimerkki metodin toiminnasta: -->

An example of how the method should work:

<!-- ```java
Asunto eiraYksio = new Asunto(1, 16, 5500);
Asunto kallioKaksio = new Asunto(2, 38, 4200);
Asunto jakomakiKolmio = new Asunto(3, 78, 2500);

System.out.println(eiraYksio.hintaero(kallioKaksio));        // 71600
System.out.println(jakomakiKolmio.hintaero(kallioKaksio));   // 35400
``` -->

```java
Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);

System.out.println(manhattanSingleRoomApt.priceDifference(atlantaTwoRoomApt));  //71600
System.out.println(bangorThreeRoomApt.priceDifference(atlantaTwoRoomApt));   //35400
```


<!-- <h2>Onko asunto kalliimpi</h2> -->

<h2>More expensive?</h2>


<!-- Tee metodi `public boolean kalliimpi(Asunto verrattava)` joka palauttaa true jos asunto-olio, jolle metodia kutsutaan on kalliimpi kuin verrattavana oleva asunto-olio. -->

Write a method `public boolean moreExpensiveThan(Apartment compared)` that returns true if the apartment object whose method is called is more expensive than the apartment object being compared.

<!-- Esimerkki metodin toiminnasta: -->

An example of how the method should work:

<!-- ```java
Asunto eiraYksio = new Asunto(1, 16, 5500);
Asunto kallioKaksio = new Asunto(2, 38, 4200);
Asunto jakomakiKolmio = new Asunto(3, 78, 2500);

System.out.println(eiraYksio.kalliimpi(kallioKaksio));       // false
System.out.println(jakomakiKolmio.kalliimpi(kallioKaksio));   // true
``` -->

```java
Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);

System.out.println(manhattanStudioApt.moreExpensiveThan(atlantaTwoBedroomApt));  // true
System.out.println(bangorThreeBedroomApt.moreExpensiveThan(atlantaTwoBedroomApt));   // false
```

</programming-exercise>


<!-- ## Olioiden samankaltaisuuden vertailu (equals) -->

## Comparing the Equality of Objects (equals)

<!-- Opimme merkkijonojen käsittelyn yhteydessä, että merkkijonojen vertailu tulee toteuttaa `equals`-metodin avullla. Tämä tapahtuu seuraavasti. -->
We learned from working with strings that strings are compared with the `equals` method. It's done like so:

<!-- ```java
Scanner lukija = new Scanner(System.in);

System.out.println("Syötä kaksi sanaa, kumpikin omalle rivilleen.")
String eka = lukija.nextLine();
String toka = lukija.nextLine();

if (eka.equals(toka)) {
    System.out.println("Sanat olivat samat.");
} else {
    System.out.println("Sanat eivät olleet samat.");
}
``` -->

```java
Scanner scanner = new Scanner(System.in);

System.out.println("Enter two words, each on its own line.")
String first = scanner.nextLine();
String second = scanner.nextLine();

if (first.equals(second)) {
    System.out.println("The words were the same.");
} else {
    System.out.println("The words were not the same.");
}
```

<!-- Alkeistyyppisten muuttujien kuten `int` kanssa muuttujien vertailu on mahdollista kahden yhtäsuuruusmerkin avulla. Tämä johtuu siitä, että alkeistyyppisten muuttujien arvo sijaitsee "muuttujan lokerossa". Viittaustyyppisten muuttujien arvo on taas osoite viitattavaan olioon, eli viittaustyyppisten muuttujien "lokerossa" on viite muistipaikkaan. Kahden yhtäsuuruusmerkin avulla verrataan "muuttujan lokeron" sisällön yhtäsuuruutta -- viittaustyyppisillä muuttujilla vertailu tarkastelisi siis muuttujien viitteiden yhtäsuuruutta. -->

With primitive variables such as `int`, comparing two variables can be done with two equality signs. This is because the value of a primitive variable is stored directly in the "variable's container". The value of a reference variable, in contrast, is an address of the object being referenced, i.e., the "container" contains a reference to the memory location. Using two equality signs compares the equality of the values stored in the "containers of the variables" -- with reference variables the comparisons would check whether the variables of the references were equal.

<!-- Metodi `equals` on samankaltainen metodi kuin `toString` siinä, että se on käytettävissä vaikkei metodia olisi luokkaan määritelty. Metodin oletustoteutus vertaa viitteiden yhtäsuuruutta. Tarkastellaan tätä aiemmin toteuttamamme `Paivays`-luokan avulla. -->

The method `equals` is similar to the method `toString` in the sense that it's available for use even if it has not been defined in a class. The default implementation of this method compares the equality of the references. Let's observe this with the help of the already-implemented `SimpleDate` class.

<!-- ```java
Paivays eka = new Paivays(1, 1, 2000);
Paivays toka = new Paivays(1, 1, 2000);
Paivays kolmas = new Paivays(12, 12, 2012);
Paivays neljas = eka;

if (eka.equals(eka)) {
    System.out.println("Muuttujat eka ja eka ovat samat");
} else {
    System.out.println("Muuttujat eka ja eka eivät ole samat");
}

if (eka.equals(toka)) {
    System.out.println("Muuttujat eka ja toka ovat samat");
} else {
    System.out.println("Muuttujat eka ja toka eivät ole samat");
}

if (eka.equals(kolmas)) {
    System.out.println("Muuttujat eka ja kolmas ovat samat");
} else {
    System.out.println("Muuttujat eka ja kolmas eivät ole samat");
}

if (eka.equals(neljas)) {
    System.out.println("Muuttujat eka ja neljas ovat samat");
} else {
    System.out.println("Muuttujat eka ja neljas eivät ole samat");
}
``` -->

```java
SimpleDate first = new SimpleDate(1, 1, 2000);
SimpleDate second = new SimpleDate(1, 1, 2000);
SimpleDate third = new SimpleDate(12, 12, 2012);
SimpleDate fourth = first;

if (first.equals(first)) {
    System.out.println("Variables first and first are equal");
} else {
    System.out.println("Variables first and first are not equal");
}

if (first.equals(second)) {
    System.out.println("Variables first and second are equal");
} else {
    System.out.println("Variables first and second are not equal");
}

if (first.equals(third)) {
    System.out.println("Variables first and third are equal");
} else {
    System.out.println("Variables first and third are not equal");
}

if (first.equals(fourth)) {
    System.out.println("Variables first and fourth are equal");
} else {
    System.out.println("Variables first and fourth are not equal");
}
```

<!-- <sample-output>

Muuttujat eka ja eka ovat samat
Muuttujat eka ja toka eivät ole samat
Muuttujat eka ja kolmas eivät ole samat
Muuttujat eka ja neljas ovat samat

</sample-output> -->

<sample-output>

Variables first and first are equal
Variables first and second are not equal
Variables first and third are not equal
Variables first and fourth are equal

</sample-output>


<!-- Esimerkkiohjelma näyttää ongelman. Vaikka kahdella päiväyksellä (eka ja toka) on täsmälleen samat oliomuuttujan arvot, ovat ne metodin `equals` oletustoteutuksen näkökulmasta toisistaan poikkeavat. -->

There is a problem with the program above. Although the two dates (first and second) hold the exact same values in instance variables, they're different from in the eyes of the default `equals` method.


<!-- Mikäli haluamme pystyä vertailemaan kahta itse toteuttamaamme oliota equals-metodilla, tulee metodi määritellä luokkaan. Metodi equals määritellään boolean-tyyppisen arvon palauttavana metodina -- palautettu arvo kertoo ovatko oliot samat. -->

If we want to be able to compare two objects that we've ourselves implemented with the equals method, the method must be defined inside the class. The equals method should be defined as a method that returns a boolean type value -- this return value indicates whether the objects are equal.


<!-- Metodi `equals` toteutetaan siten, että sen avulla voidaan vertailla nykyistä oliota mihin tahansa muuhun olioon. Metodi saa parametrinaan Object-tyyppisen olion -- kaikki oliot ovat oman tyyppinsä lisäksi Object-tyyppisiä. Metodissa ensin vertaillaan ovatko osoitteet samat: jos kyllä, oliot ovat samat. Tämän jälkeen tarkastellaan ovatko olion tyypit samat: jos ei, oliot eivät ole samat. Tämän jälkeen parametrina saatu Object-olio muunnetaan tyyppimuunnoksella tarkasteltavan olion muotoiseksi, ja oliomuuttujien arvoja vertaillaan. Alla vertailu on toteutettu Paivays-oliolle. -->

The method `equals` is implemented in a way that makes is suitable for comparing the object in question with any other object. The method receives an object of type Object as its only parameter -- all objects are, in addition to their own type, of type Object. The equals method first compares if the addresses are equal: if so, the objects are equal. It then examines if the types of the objects are the same: if not, the objects are not equal. Then the Object object received as a parameter is converted to the type of the object being examined through type casting. The values of the instance variables can then be compared. This comparison has been implemented below for the SimpleDate class.


<!-- ```java
public class Paivays {
    private int paiva;
    private int kuukausi;
    private int vuosi;

    public Paivays(int paiva, int kuukausi, int vuosi) {
        this.paiva = paiva;
        this.kuukausi = kuukausi;
        this.vuosi = vuosi;
    }

    public int getPaiva() {
        return this.paiva;
    }

    public int getKuukausi() {
        return this.kuukausi;
    }

    public int getVuosi() {
        return this.vuosi;
    }

    public boolean equals(Object verrattava) {
        // jos muuttujat sijaitsevat samassa paikassa, ovat ne samat
        if (this == verrattava) {
            return true;
        }

        // jos verrattava olio ei ole Paivays-tyyppinen, oliot eivät ole samat
        if (!(verrattava instanceof Paivays)) {
            return false;
        }

        // muunnetaan Object-tyyppinen verrattava-olio
        // Paivays-tyyppiseksi verrattavaPaivays-olioksi
        Paivays verrattavaPaivays = (Paivays) verrattava;

        // jos olioiden oliomuuttujien arvot ovat samat, ovat oliot samat
        if (this.paiva == verrattavaPaivays.paiva &&
            this.kuukausi == verrattavaPaivays.kuukausi &&
            this.vuosi == verrattavaPaivays.vuosi) {
            return true;
        }

        // muulloin oliot eivät ole samat
        return false;
    }

    @Override
    public String toString() {
        return this.paiva + "." + this.kuukausi + "." + this.vuosi;
    }
}
``` -->

```java
public class SimpleDate {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year) {
        this.day = day;
        this.month = month;
        this.year = year;
    }

    public int getDay() {
        return this.day;
    }

    public int getMonth() {
        return this.month;
    }

    public int getYear() {
        return this.year;
    }

    public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the type of the compared object is not SimpleDate, the objects are not equal
        if (!(compared instanceof SimpleDate)) {
            return false;
        }

        // convert the Object type compared object
        // into an SimpleDate type object called comparedSimpleDate
        SimpleDate comparedSimpleDate = (SimpleDate) compared;

        // if the values of the instance variables are the same, the objects are equal
        if (this.day == comparedSimpleDate.date &&
            this.month == comparedSimpleDate.month &&
            this.year == comparedSimpleDate.year) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
    }

    @Override
    public String toString() {
        return this.day + "." + this.month + "." + this.year;
    }
}
```

<!-- Vastaavan vertailutoiminnallisuuden rakentaminen onnistuu myös Henkilö-olioille. Alla vertailu on toteutettu Henkilo-oliolle, jolla ei ole erillista Paivays-oliota. Huomaa, että henkilöiden nimet ovat merkijonoja (eli olioita), joten niiden vertailussa käytetään equals-metodia. -->

Building a similar comparison feature can be done for Person objects as well. Below, the comparison has been implemented for those Person objects that don't have a separate SimpleDate object. Notice that the names of people are strings (o.e., objects), and for this reason equals method is used for comparing them.


<!-- ```java
public class Henkilo {

    private String nimi;
    private int ika;
    private int paino;
    private int pituus;

    // konstruktorit ja metodit


    public boolean equals(Object verrattava) {
        // jos muuttujat sijaitsevat samassa paikassa, ovat ne samat
        if (this == verrattava) {
            return true;
        }

        // jos verrattava olio ei ole Henkilo-tyyppinen, oliot eivät ole samat
        if (!(verrattava instanceof Henkilo)) {
            return false;
        }

        // muunnetaan olio Henkilo-olioksi
        Henkilo verrattavaHenkilo = (Henkilo) verrattava;

        // jos olioiden oliomuuttujien arvot ovat samat, ovat oliot samat
        if (this.nimi.equals(verrattavaHenkilo.nimi) &&
            this.ika == verrattavaHenkilo.ika &&
            this.paino == verrattavaHenkilo.paino &&
            this.pituus == verrattavaHenkilo.pituus) {
            return true;
        }

        // muulloin oliot eivät ole samat
        return false;
    }

    // .. metodeja
}
``` -->

```java
public class Person {

    private String name;
    private int age;
    private int weight;
    private int height;

    // constructors and methods


    public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the compared object is not of type Person, the objects are not equal
        if (!(compared instanceof Person)) {
            return false;
        }

        // convert the object into a Person object
        Person comparedPerson = (Person) compared;

        // if the values of the instance variables are equal, the objects are equal
        if (this.name.equals(comparedPerson.name) &&
            this.age == comparedPerson.age &&
            this.weight == comparedPerson.weight &&
            this.height == comparedPerson.height) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
    }

    // .. methods
}
```


<!-- <programming-exercise name='Kappale' tmcname='osa05-Osa05_12.Kappale'> -->

<programming-exercise name='Song' tmcname='part05-Part05_12.Song'>


<!-- Tehtäväpohjassa on luokka `Kappale`, jonka perusteella voidaan luoda musiikkikappaleita esittäviä olioita. Lisää luokkaan kappale metodi `equals`, jonka avulla voidaan tarkastella musiikkikappaleiden samankaltaisuutta. -->

In the exercise base there is a class called `Song` that can be used to create new objects that represent songs. Add the `equals` method to the class so that the similarity of songs can be examined.


<!-- ```java
Kappale jackSparrow = new Kappale("The Lonely Island", "Jack Sparrow", 196);
Kappale toinenSparrow = new Kappale("The Lonely Island", "Jack Sparrow", 196);

if (jackSparrow.equals(toinenSparrow)) {
    System.out.println("Kappaleet olivat samat.");
}

if (jackSparrow.equals("Toinen olio")) {
    System.out.println("Nyt on jotain hassua.");
}
``` -->

```java
Song jackSparrow = new Song("The Lonely Island", "Jack Sparrow", 196);
Song anotherSparrow = new Song("The Lonely Island", "Jack Sparrow", 196);

if (jackSparrow.equals(anotherSparrow)) {
    System.out.println("Songs are equal.");
}

if (jackSparrow.equals("Another object")) {
    System.out.println("Strange things are afoot.");
}
```

<!-- <sample-output>

Kappaleet olivat samat.

</sample-output> -->

<sample-output>

Songs are equal

</sample-output>

</programming-exercise>


<!-- <programming-exercise name='Henkilövertailu' tmcname='osa05-Osa05_13.Henkilovertailu'> -->

<programming-exercise name='Identical Twins' tmcname='part05-Part05_13.IdenticalTwins'>


<!-- Tehtäväpohjassa on luokka `Henkilo`, johon liittyy `Paivays`-olio. Lisää luokalle Henkilo metodi `public boolean equals(Object verrattava)`, jonka avulla voidaan verrata henkilöiden samuutta. Vertailussa tulee verrata kaikkien henkilön muuttujien yhtäsuuruutta (ml. syntymäpäivä). -->

The exercise template contains the `Person`, which has a `SimpleDate` object linked to it. Add to the Person class the method `public boolean equals (Object compared)`, which can be used to compare people's similarity. This comparison should compare the equality of each and every variable (birthday included).

<!-- **Huom!** Muistathan, että et voi verrata syntymäpäivää-olioita yhtäsuuruusmerkillä! -->

**NB!** Recall that you cannot compare two birthday objects with the equals signs!

<!-- Tehtäväpohjassa ei ole ohjelman oikeellisutta tarkastavia testejä. Palauta tehtävä vasta kun vertailu toimii oikein. Alla koodia ohjelman testaamiseen. -->

There are no tests in the exercise template checking the correctness of the solution. Return your answer only once the comparison works as it should. Below is some code that you can use to test your program.


<!-- ```java
Paivays pvm = new Paivays(24, 3, 2017);
Paivays pvm2 = new Paivays(23, 7, 2017);

Henkilo leevi = new Henkilo("Leevi", pvm, 62, 9);
Henkilo lilja = new Henkilo("Lilja", pvm2, 65, 8);

if (leevi.equals(lilja)) {
    System.out.println("Meniköhän nyt ihan oikein?");
}

Henkilo leeviEriPainolla = new Henkilo("Leevi", pvm, 62, 10);

if (leevi.equals(leeviEriPainolla)) {
    System.out.println("Meniköhän nyt ihan oikein?");
}
``` -->

```java
SimpleDate date = new SimpleDate(24, 3, 2017);
SimpleDate date2 = new SimpleDate(23, 7, 2017);

Person leo = new Person("Leo", date, 62, 9);
Person lily = new Person("Lily", date2, 65, 8);

if (leo.equals(lily)) {
    System.out.println("Is this quite correct?");
}

Person leoWithDifferentWeight = new Person("Leo", date, 62, 10);

if (leo.equals(leoWithDifferentWeight)) {
    System.out.println("Is this quite correct?");
}
```

</programming-exercise>



<!-- <text-box variant='hint' name='Mikä ihmeen Object?'> -->

<text-box variant='hint' name='Object What?'>


<!-- Jokainen luomamme luokka (ja Javan valmis luokka) perii luokan Object, vaikkei sitä erikseen ohjelmakoodissa näy. Tämän takia mistä tahansa luokasta tehty ilmentymä voidaan asettaa parametriksi metodiin, joka saa parametrina Object-tyyppisen muuttujan. Object-luokan periminen näkyy myös muissa asioissa: esimerkiksi metodi `toString` on olemassa vaikkei sitä erikseen toteuteta, aivan samalla tavalla kuin metodi `equals`. -->

Every class we create (and every ready-made Java class) inherits the class Object, although it's not explicitly visible in our code. This explains why an instance of any class can be passed as a parameter to a method that receives an Object type variable as its parameter. Inheritance of Object can be seen elsewhere too. For instance, the `toString` method exists even when you've not implemented it yourself, just as the `equals` method does.

<!-- Esimerkiksi seuraava lähdekoodi kääntyy, sillä `equals`-metodi löytyy kaikkien luokkien perimästä Object-luokasta. -->

To illustrate the point, the following source code compiles since the `equals` method comes with the Object class inherited by all classes.

<!-- ```java
public class Lintu {
    private String nimi;

    public Lintu(String nimi) {
        this.nimi = nimi;
    }
}
``` -->

```java
public class Bird {
    private String name;

    public Bird(String name) {
        this.name = name;
    }
}
```


<!-- ```java
Lintu red = new Lintu("Red");
System.out.println(red);

Lintu chuck = new Lintu("Chuck");
System.out.println(chuck);

if (red.equals(chuck)) {
    System.out.println(red + " on sama kuin " + chuck);
}
``` -->

```java
Bird red = new Bird("Red");
System.out.println(red);

Bird chuck = new Bird("Chuck");
System.out.println(chuck);

if (red.equals(chuck)) {
    System.out.println(red + " equals " + chuck);
}
```

</text-box>

<!-- ## Olion samankaltaisuus ja listat -->

## Object Equality and Lists

<!-- Tarkastellaan `equals`-metodin käyttöä vielä listojen yhteydessä. Oletetaan, että käytössämme on edellä kuvattu luokka `Lintu`, jolle ei ole määritelty `equals`-metodia. -->

Let's examine how the `equals` method is used with lists. Let's assume that we have the `Bird` class that we described previously at our disposal, but the `equals` method has not been defined for it.

<!-- ```java
public class Lintu {
    private String nimi;

    public Lintu(String nimi) {
        this.nimi = nimi;
    }
}
``` -->

```java
public class Bird {
    private String name;

    public Bird(String name) {
        this.name = name;
    }
}
```

<!-- Luodaan lista, johon lisätään lintu. Tarkastellaan tämän jälkeen linnun olemassaoloa listalla. -->

Let's create a list and add a bird to it. We'll then check if for the presence of the bird on the list.

<!-- ```java
ArrayList<Lintu> linnut = new ArrayList<>()
Lintu red = new Lintu("Red");

if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}

linnut.add(red);
if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}


System.out.println("Mutta!");

red = new Lintu("Red");
if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}
``` -->

```java
ArrayList<Bird> birds = new ArrayList<>()
Bird red = new Bird("Red");

if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}

birds.add(red);
if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}


System.out.println("However!");

red = new Bird("Red");
if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}
```

<!-- <sample-output>

Red ei ole listalla.
Red on listalla.
Mutta!
Red ei ole listalla.

</sample-output> -->

<sample-output>

Red is not on the list.
Red is on the list.
However!
Red is not on the list.

</sample-output>

<!-- Yllä olevasta esimerkistä huomaamme, että voimme etsiä listalta omia olioitamme. Aluksi kun lintua ei ole lisätty listalle, sitä ei löydy -- lisäämisen jälkeen se löytyy. Kun ohjelmassa `red`-olio vaihdetaan uudeksi täysin samansisältöiseksi olioksi, ei se enää vastaa listalla olevaa oliota, ja sitä ei löydy listalta. -->

We see in the example above that we can search a list for our own objects. Initially, when the bird had not yet been added to the list, it couldn't be found -- and once it was added, it could be. When the `red` object is assigned a new object with exactly the same contents as before, it no longer represents the object on the list, and thus cannot be found on it.

<!-- Listan `contains`-metodi hyödyntää olioiden etsimiseen oliolle määriteltyä `equals`-metodia. Yllä olevassa esimerkissä luokalle `Lintu` ei tätä metodia ole määritelty, joten täysin samansisältöinen lintu, jonka viite on eri, ei listalta löydy. -->

The list's `contains` method uses the `equals` method defined for the objects in when searching for objects. In the example above, the `Bird` class has no such definition. As a result, a bird that's equal in terms of its content, but holds a different reference, cannot be found on the list.

<!-- Lisätään luokalle `Lintu` metodi `equals`. Metodi tarkastelee onko olioiden nimet samat -- mikäli nimet ovat samat, käsitellään ne samanlaisina. -->

Let's implement the `equals` method for the `Bird` class. The method examines if the names of the objects are equal -- if the names match, the birds are considered equal.


<!-- ```java
public class Lintu {
    private String nimi;

    public Lintu(String nimi) {
        this.nimi = nimi;
    }

    public boolean equals(Object verrattava) {
        // jos muuttujat sijaitsevat samassa paikassa, ovat ne samat
        if (this == verrattava) {
            return true;
        }

        // jos verrattava olio ei ole Lintu-tyyppinen, oliot eivät ole samat
        if (!(verrattava instanceof Lintu)) {
            return false;
        }

        // muunnetaan olio Lintu-olioksi
        Lintu verrattavaLintu = (Lintu) verrattava;

        // jos olioiden oliomuuttujien arvot ovat samat, ovat oliot samat
        return this.nimi.equals(verrattavaLintu.nimi);

        /*
        // yllä oleva nimen vertailu vastaa alla olevaa
        // koodia

        if (this.nimi.equals(verrattavaLintu.nimi)) {
            return true;
        }

        // muulloin oliot eivät ole samat
        return false;
        */
    }
}
``` -->

```java
public class Bird {
    private String name;

    public Bird(String name) {
        this.name = name;
    }

    public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the compared object is not of type Bird, the objects are not equal
        if (!(compared instanceof Bird)) {
            return false;
        }

        // convert the object to a Bird object
        Bird comparedBird = (Bird) compared;

        // if the values of the instance variables are equal, the objects are, too
        return this.name.equals(comparedBird.name);

        /*
        // the name comparison above is equal to
        // the following code

        if (this.name.equals(comparedBird.name)) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
        */
    }
}
```

<!-- Nyt listan contains-metodi tunnistaa samansisältöiset linnut. -->

Now the list's contains method recognizes birds with identical contents.

<!-- ```java
ArrayList<Lintu> linnut = new ArrayList<>()
Lintu red = new Lintu("Red");

if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}

linnut.add(red);
if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}


System.out.println("Mutta!");

red = new Lintu("Red");
if (linnut.contains(red)) {
    System.out.println("Red on listalla.");
} else {
    System.out.println("Red ei ole listalla.");
}
``` -->

```java
ArrayList<Bird> birds = new ArrayList<>()
Bird red = new Bird("Red");

if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}

birds.add(red);
if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}


System.out.println("However!");

red = new Bird("Red");
if (birds.contains(red)) {
    System.out.println("Red is on the list.");
} else {
    System.out.println("Red is not on the list.");
}
```

<!-- <sample-output>

Red ei ole listalla.
Red on listalla.
Mutta!
Red on listalla.

</sample-output> -->

<sample-output>

Red is not on the list.
Red is on the list.
However!
Red is on the list.

</sample-output>


<!-- <programming-exercise name='Kirjat' tmcname='osa05-Osa05_14.Kirjat'> -->

<programming-exercise name='Books' tmcname='part05-Part05_14.Books'>


<!-- Tehtäväpohjassa on ohjelma, joka lukee käyttäjältä kirjoja ja lisää niitä listalle. -->

There is a program in the exercise base that asks for books from the user and adds them to a list.

<!-- Muokkaa ohjelmaa siten, että listalle ei lisätä kirjoja, jotka ovat jo listalla. Kaksi kirjaa tulee käsittää samaksi mikäli niiden nimi ja julkaisuvuosi on sama. -->

Modify the program so that books that are already on the list are not added to it again. Two books should be considered the same if they have the same name and publication year.

<!-- Esimerkkitulostus: -->

Example print

<!-- <sample-output>

Syötä kirjan nimi, tyhjä lopettaa.
**Bossypants**
Syötä kirjan julkaisuvuosi.
**2013**
Syötä kirjan nimi, tyhjä lopettaa.
**Seriously...I'm Kidding**
Syötä kirjan julkaisuvuosi.
**2012**
Syötä kirjan nimi, tyhjä lopettaa.
**Seriously...I'm Kidding**
Syötä kirjan julkaisuvuosi.
**2012**
Kirja on jo listalla. Ei lisätä samaa kirjaa uudestaan.
Syötä kirjan nimi, tyhjä lopettaa.

Kiitos! Kirjoja lisätty: 2

</sample-output> -->

<sample-output>

Name (empty will stop):
**Bossypants**
Publication year:
**2013**
Name (empty will stop):
**Seriously...I'm Kidding**
Publication year:
**2012**
Name (empty will stop):
**Seriously...I'm Kidding**
Publication year:
**2012**
The book is already on the list. Let's not add the same book again.
Name (empty will stop):

Thank you! Books added: 2

</sample-output>

</programming-exercise>


<!-- <programming-exercise name='Keräilijän varasto (2 osaa)' tmcname='osa05-Osa05_15.KerailijanVarasto'> -->

<programming-exercise name='Archive (2 parts)' tmcname='part05-Part05_15.Archive'>


<!-- Tässä tehtävässä toteutat ohjelman, jota käytetään keräilijän varaston käsittelyyn. Varastoon voi lisätä esineitä. Kun esineiden lisääminen lopetetaan, varastossa olevat esineet tulostetaan. -->

In this exercise, you get to implement a program that can be used to handle an archive. Items can be added to the archive. When no more items are added, all the items in the archive are printed.

<!-- <h2>Esineiden lisääminen ja listaaminen</h2> -->

<h2>Adding and Listing Items</h2>


<!-- Ohjelman tulee lukea käyttäjältä esineitä. Kun kaikki käyttäjän esineet on luettu, ohjelma tulostaa esineiden tiedot. -->

The program should read items from the user. When all the items from the user have been read, the program prints the information of each item.

<!-- Kustakin esineestä tulee lukea tunnus ja nimi. Mikäli syötetty tunnus tai nimi on tyhjä, ohjelma lopettaa syötteen pyytämisen ja tulostaa esineiden tiedot. -->

For each item, its identifier and name should be read. If the identifier or name is empty, the program stops asking for input, and prints all the item information.

<!-- Esimerkkitulostus: -->

Example print:

<!-- <sample-output>

Syötä esineen tunnus, tyhjä lopettaa.
**B07H8ND8HH**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.
**B07H8ND8HH**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man**
Syötä esineen tunnus, tyhjä lopettaa.
**B07NQFMZYG**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.
**B07NQFMZYG**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.

==Esineet==
B07H8ND8HH: He-Man hahmo
B07H8ND8HH: He-Man
B07NQFMZYG: He-Man hahmo
B07NQFMZYG: He-Man hahmo

</sample-output> -->

<sample-output>

Identifier? (empty will stop)
**B07H8ND8HH**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)
**B07H8ND8HH**
Name? (empty will stop)
**He-Man**
Identifier? (empty will stop)
**B07NQFMZYG**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)
**B07NQFMZYG**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)

==Items==
B07H8ND8HH: He-Man figure
B07H8ND8HH: He-Man
B07NQFMZYG: He-Man figure
B07NQFMZYG: He-Man figure

</sample-output>

<!-- Esineiden tulostusmuodon tulee olla `tunnus: nimi`. -->

The printing format of the items should be `identifier: name`.

<!-- Huom! Älä käytä kaksoispistettä ohjelman muussa tulostuksessa. -->

NB! Don't print the colon (:) anywhere else in the output of the program.


<!-- <h2>Kukin esine tulostetaan vain kerran</h2> -->

<h2>You only print once (per item)</h2>


<!-- Muokkaa ohjelmaa siten, että esineiden syöttämisen jälkeen kukin esine tulostetaan korkeintaan kerran. Kaksi esinettä tulee käsittää samoina mikäli niiden tunnukset ovat samat (nimet voivat vaihdella esimerkiksi maittain). -->

Modify the program so that after entering the items each item is printed at most once. Two items should be considered the same if their identifiers are the same (names may vary, for instance based on country).

<!-- Mikäli käyttäjä syöttää saman esineen useaan otteeseen, tulostuksessa käytetään ensimmäisenä syötettyä esinettä. -->

If the user enters the same item multiple times, the print uses the item that was added first.

<!-- <sample-output>

Syötä esineen tunnus, tyhjä lopettaa.
**B07H8ND8HH**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.
**B07H8ND8HH**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man**
Syötä esineen tunnus, tyhjä lopettaa.
**B07NQFMZYG**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.
**B07NQFMZYG**
Syötä esineen nimi, tyhjä lopettaa.
**He-Man hahmo**
Syötä esineen tunnus, tyhjä lopettaa.

==Esineet==
B07H8ND8HH: He-Man hahmo
B07NQFMZYG: He-Man hahmo

</sample-output> -->

<sample-output>

Identifier? (empty will stop)
**B07H8ND8HH**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)
**B07H8ND8HH**
Name? (empty will stop)
**He-Man**
Identifier? (empty will stop)
**B07NQFMZYG**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)
**B07NQFMZYG**
Name? (empty will stop)
**He-Man figure**
Identifier? (empty will stop)

==Items==
B07H8ND8HH: He-Man figure
B07NQFMZYG: He-Man figure

</sample-output>


<!-- Vinkki! Tämä kannattaa toteuttaa siten, että kukin esine lisätään listalle korkeintaan kerran -- vertaile esineiden samuutta niiden tunnuksien perusteella. -->

Hint! It is probably smart to add each item to the list at most once -- compare the equality of the objects based on their identifiers.

</programming-exercise>


<!-- ## Olio metodin paluuarvona -->

## Object as a Method's Return Value

<!-- Olemme nähneet metodeja jotka palauttavat totuusarvoja, lukuja ja merkkijonoja. On helppoa arvata, että metodi voi palauttaa minkä tahansa tyyppisen olion. -->

We've seen methods that return boolean values, numbers, and strings. As you might have guessed, a method can return an object of any type.

<!-- Seuraavassa esimerkissä on yksinkertainen laskuri, jolla on metodi `kloonaa`. Metodin avulla laskurista voidaan tehdä klooni, eli uusi laskurio-olio, jolla on luomishetkellä sama arvo kuin kloonattavalla laskurilla: -->


In the next example, we present a simple counter that has a `clone` method. The method can be used to create a clone of the counter, i.e., a new counter object that has the same value at the time of its creation as the counter that is being cloned.

<!-- ```java
public Laskuri {
    private int arvo;

    // esimerkki useamman konstruktorin käytöstä:
    // konstruktorista voi kutsua toista konstruktoria this-kutsulla
    // huomaa tosin, että this-kutsun tulee olla konstruktorin ensimmäisellä rivillä.
    public Laskuri() {
        this(0);
    }

    public Laskuri(int alkuarvo) {
        this.arvo = alkuarvo;
    }

    public void kasvata() {
        this.arvo = this.arvo + 1;
    }

    public String toString() {
        return "arvo: " + arvo;
    }

    public Laskuri kloonaa() {
        // luodaan uusi laskuriolio, joka saa alkuarvokseen kloonattavan laskurin arvon
        Laskuri klooni = new Laskuri(this.arvo);

        // palautetaan klooni kutsujalle
        return klooni;
    }
}
``` -->

```java
public Counter {
    private int value;

    // example of using multiple constructors:
    // another constructor can be called from a constructor by calling this
    // notice that the this call must be on the first line of the constructor
    public Counter() {
        this(0);
    }

    public Counter(int initialValue) {
        this.value = initialValue;
    }

    public void increase() {
        this.value = this.value + 1;
    }

    public String toString() {
        return "value: " + value;
    }

    public Counter clone() {
        // create a new counter object that receives the value of the cloned counter as its initial value
        Counter clone = new Counter(this.value);

        // return the clone to the caller
        return clone;
    }
}
```

<!-- Seuraavassa käyttöesimerkki: -->

The following is an example:


<!-- ```java
Laskuri laskuri = new Laskuri();
laskuri.kasvata();
laskuri.kasvata();

System.out.println(laskuri);         // tulostuu 2

Laskuri klooni = laskuri.kloonaa();

System.out.println(laskuri);         // tulostuu 2
System.out.println(klooni);          // tulostuu 2

laskuri.kasvata();
laskuri.kasvata();
laskuri.kasvata();
laskuri.kasvata();

System.out.println(laskuri);         // tulostuu 6
System.out.println(klooni);          // tulostuu 2

klooni.kasvata();

System.out.println(laskuri);         // tulostuu 6
System.out.println(klooni);          // tulostuu 3
``` -->

```java
Counter counter = new Counter();
counter.increase();
counter.increase();

System.out.println(counter);         // prints 2

Counter clone = counter.clone();

System.out.println(counter);         // prints 2
System.out.println(clone);          // prints 2

counter.increase();
counter.increase();
counter.increase();
counter.increase();

System.out.println(counter);         // prints 6
System.out.println(clone);          // prints 2

clone.increase();

System.out.println(counter);         // prints 6
System.out.println(clone);          // prints 3
```

<!-- Kloonattavan ja kloonin sisältämä arvo on kloonauksen tapahduttua sama. Kyseessä on kuitenkin kaksi erillistä olioa, eli kun toista laskureista kasvatetaan, ei kasvatus vaikuta toisen arvoon millään tavalla. -->

Immediately after the cloning operation, the values in both the clone and the cloned object are the same. They are, however, two different objects. As such, increasing the value of one counter does in no way affect the value of the other.

<!-- Vastaavasti myös `Tehdas`-olio voisi luoda ja palauttaa uusia `Auto`-olioita. Alla on hahmoteltu tehtaan runkoa -- tehdas tietää myös luotavien autojen merkin. -->

Similarly, a `Factory` object could also be used to create and return new `Car` objects. The factory's blueprint has been laid out below -- a factory also knows the makes of the cars created.


<!-- ```java
public class Tehdas {
    private String merkki;

    public Tehdas(String merkki) {
        this.merkki = merkki;
    }

    public Auto tuotaAuto() {
        return new Auto(this.merkki);
    }
}
``` -->

```java
public class Factory {
    private String make;

    public Factory(String make) {
        this.make = make;
    }

    public Car procuceCar() {
        return new Car(this.make);
    }
}
```


<!-- <programming-exercise name='Päiväys (3 osaa)' tmcname='osa05-Osa05_16.Paivays'> -->

<programming-exercise name='Dating App (3 parts)' tmcname='part05-Part05_16.DatingApp'>


<!-- Tehtäväpohjan mukana tulee luokka `Paivays`, jossa päivämäärä talletetaan oliomuuttujien `vuosi`, `kuukausi`, ja `paiva` avulla: -->

The class `SimpleDate` is supplied in the exercise template. The date is stored with the help of the instance variables `year`, `month`, and `day`:


<!-- ```java
public class Paivays {
    private int paiva;
    private int kuukausi;
    private int vuosi;

    public Paivays(int paiva, int kuukausi, int vuosi) {
        this.paiva = paiva;
        this.kuukausi = kuukausi;
        this.vuosi = vuosi;
    }

    public String toString() {
        return this.paiva + "." + this.kuukausi + "." + this.vuosi;
    }

    public boolean aiemmin(Paivays verrattava) {
        // ensin verrataan vuosia
        if (this.vuosi < verrattava.vuosi) {
            return true;
        }

        // jos vuodet ovat samat, verrataan kuukausia
        if (this.vuosi == verrattava.vuosi && this.kuukausi < verrattava.kuukausi) {
            return true;
        }

        // vuodet ja kuukaudet samoja, verrataan päivää
        if (this.vuosi == verrattava.vuosi && this.kuukausi == verrattava.kuukausi &&
            this.paiva < verrattava.paiva) {
            return true;
        }

        return false;
    }
}
``` -->

```java
public class SimpleDate {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year) {
        this.day = day;
        this.month = month;
        this.year = year;
    }

    public String toString() {
        return this.day + "." + this.month + "." + this.year;
    }

    public boolean before(SimpleDate compared) {
        // first compare years
        if (this.year < compared.year) {
            return true;
        }

        // if the years are the same, compare months
        if (this.year == compared.year && this.month < compared.month) {
            return true;
        }

        // the years and the months are the same, compare days
        if (this.year == compared.year && this.month == compared.month &&
            this.day < compared.day) {
            return true;
        }

        return false;
    }
}
```

<!-- Tässä tehtäväsarjassa laajennetaan luokkaa. -->

We'll expand the class in this exercise series.


<!-- <h2>Seuraava päivä</h2> -->

<h2>Next day</h2>

<!-- Toteuta metodi `public void etene()`, joka siirtää päiväystä yhdellä päivällä. Tässä tehtävässä oletetaan, että jokaisessa kuukaudessa on 30 päivää. Huom! Sinun tulee *tietyissä* tilanteissa muuttaa kuukauden ja vuoden arvoa. -->

Implement the `public void advance()` method that moves the date by a day. In this exercise, we assume that each month has 30 days. NB! In *certain* situations, you will need to change the values of the month and year.


<!-- <h2>Tietty määrä päiviä eteenpäin</h2> -->

<h2>A Specific Number of Days Forward</h2>

<!-- Toteuta metodi `public void etene(int montakoPaivaa)`, joka siirtää päiväystä annetun päivien määrän verran. Käytä apuna edellisessä tehtävässä toteutettua metodia `etene()`. -->

Implement the `public void advance(int howManyDays)` method that moves the date by the number of days that is specified. Use the method `advance()` that you implemented in the previous section to help you in this.


<!-- <h2>Ajan kuluminen</h2> -->

<h2>Passing Time</h2>

<!-- Lisätään `Paivays`-olioon mahdollisuus edistää aikaa. Tee oliolle metodi `Paivays paivienPaasta(int paivia)`, joka luo **uuden** `Paivays`-olion, jonka päiväys on annetun päivien lukumäärän verran suurempi kuin oliolla, jolle sitä kutsuttiin. Voit edelleen olettaa, että jokaisessa kuukaudessa on 30 päivää. Huomaa, että vanhan päiväysolion on pysyttävä muuttumattomana! -->

Let's add the possibility to advance time to the `SimpleDate` class. Create the method `SimpleDate afterNumberOfDays(int days)` for the class. It creates a **new** `SimpleDate` object whose date is the specified number of days greater than the object that the method was called on. You may still assume that each month has 30 days. Notice that the old date object must remain unchanged!

<!-- Koska metodissa on luotava **uusi olio**, tulee rungon olla suunnilleen seuraavanlainen: -->

Since the method must create **a new object**, the structure of the code should be somewhat similar to this:


<!-- ```java
public Paivays paivienPaasta(int paivia) {
    Paivays uusiPaivays = new Paivays( ... );

    // tehdään jotain...

    return uusiPaivays;
}
``` -->

```java
public SimpleDate afterNumberOfDays(int days) {
    SimpleDate newDate = new SimpleDate( ... );

    // do something..

    return newDate;
}
```

<!-- Ohessa on esimerkki metodin toiminnasta. -->

Here is an example of how the method works.

<!-- ```java
public static void main(String[] args) {
    Paivays pvm = new Paivays(13, 2, 2015);
    System.out.println("Tarkistellun viikon perjantai on " + pvm);

    Paivays uusiPvm = pvm.paivienPaasta(7);
    int vk = 1;
    while (vk <= 7) {
        System.out.println("Perjantai " + vk + " viikon kuluttua on " + uusiPvm);
        uusiPvm = uusiPvm.paivienPaasta(7);

        vk = vk + 1;
    }


    System.out.println("Päivämäärä 790:n päivän päästä tarkistellusta perjantaista on ... kokeile itse!");
    //    System.out.println("Kokeile " + pvm.paivienPaasta(790));
}
``` -->

```java
public static void main(String[] args) {
    SimpleDate date = new SimpleDate(13, 2, 2015);
    System.out.println("Friday of the given week is " + pvm);

    SimpleDate newDate = date.afterNumberOfDays(7);
    int week = 1;
    while (week <= 7) {
        System.out.println("Friday after " + week + " weeks is " + newDate);
        newDate = newDate.afterNumberOfDays(7);

        week = week + 1;
    }


    System.out.println("The date after 790 days from the given Friday is ... try it out for yourself!");
    //    System.out.println("Try " + date.afterNumberOfDays(790));
}
```

<!-- Ohjelma tulostaa: -->

The program prints:

<!-- <sample-output>

Tarkistellun viikon perjantai on 13.2.2015
Perjantai 1 viikon kuluttua on 20.2.2015
Perjantai 2 viikon kuluttua on 27.2.2015
Perjantai 3 viikon kuluttua on 4.3.2015
Perjantai 4 viikon kuluttua on 11.3.2015
Perjantai 5 viikon kuluttua on 18.3.2015
Perjantai 6 viikon kuluttua on 25.3.2015
Perjantai 7 viikon kuluttua on 2.4.2015
Päivämäärä 790:n päivän päästä tarkistellusta perjantaista on ... kokeile itse!

</sample-output> -->

<sample-output>

Friday of the given week is 13.2.2015
Friday after 1 weeks is 20.2.2015
Friday after 2 weeks is 27.2.2015
Friday after 3 weeks is 4.3.2015
Friday after 4 weeks is 11.3.2015
Friday after 5 weeks is 18.3.2015
Friday after 6 weeks is 25.3.2015
Friday after 7 weeks is 2.4.2015
The date after 790 days from the given Friday is ... try it out for yourself!

</sample-output>


<!-- **Huom!** Sen sijaan, että muuttaisimme vanhan olion tilaa palautamme uuden olion. Kuvitellaan, että `Paivays`-luokalle on olemassa metodi `edista`, joka toimii vastaavasti kuin ohjelmoimamme metodi, mutta se muuttaa vanhan olion tilaa. Tällöin seuraava koodin pätkä tuottaisi ongelmia. -->

**NB!** Instead of modifying the state of the old object, we return a new one. Let's imagine that the `SimpleDate` class has a method `advance` that works similarly to the method we programmed, but instead modifies the state of the old object. Were this the case, the following block of code would cause issues.


<!-- ```java
Paivays nyt = new Paivays(13, 2, 2015);
Paivays viikonPaasta = nyt;
viikonPaasta.edista(7);

System.out.println("Nyt: " + nyt);
System.out.println("Viikon päästä: " + viikonPaasta);
``` -->

```java
SimpleDate now = new SimpleDate(13, 2, 2015);
SimpleDate aWeekAhead = now;
aWeekAhead.advance(7);

System.out.println("Now: " + now);
System.out.println("After a week: " + aWeekAhead);
```

<!-- Ohjelman tulostus olisi seuraavanlainen: -->

The output of the program should be:

<!-- <sample-output>

Nyt 20.2.2015
Viikon päästä 20.2.2015

</sample-output> -->

<sample-output>

Now: 20.2.2015
After a week: 20.2.2015

</sample-output>

<!-- Tämä johtuu siitä, että tavallinen sijoitus kopioi ainoastaan viitteen olioon. Siis itse asiassa ohjelman oliot `nyt` ja `viikonPaasta` viittavaat **yhteen ja samaan** `Paivays`-olioon. -->

The reason for this is that a standard assignment only copies the reference to the object. So, the objects `now` and `afterOneWeek` in the program now refer to the **one and same** `SimpleDate` object.

</programming-exercise>


<!-- <programming-exercise name='Raha (3 osaa)' tmcname='osa05-Osa05_17.Raha'> -->

<programming-exercise name='Money (3 parts)' tmcname='part05-Part05_17.Money'>


<!-- Maksukortti-tehtävässä käytimme rahamäärän tallettamiseen double-tyyppistä oliomuuttujaa. Todellisissa sovelluksissa näin ei kannata tehdä, sillä kuten jo olemme nähneet, doubleilla laskenta ei ole tarkkaa. Onkin järkevämpää toteuttaa rahamäärän käsittely oman luokkansa avulla. Seuraavassa on luokan runko: -->

In the Payment card exercise, we used a double-type instance variable to store the amount of money. This approach is not recommended in real-world applications since operations with doubles is not exact as we've already seen. A more sensible way to handle amounts of money would be by creating a separate class specifically for the task. Here's a layout for this class:


<!-- ```java
public class Raha {

    private final int euroa;
    private final int senttia;

    public Raha(int euroa, int senttia) {
        this.euroa = euroa;
        this.senttia = senttia;
    }

    public int eurot() {
        return euroa;
    }

    public int sentit() {
        return senttia;
    }

    public String toString() {
        String nolla = "";
        if (senttia <= 10) {
            nolla = "0";
        }

        return euroa + "." + nolla + senttia + "e";
    }
}
``` -->

```java
public class Money {

    private final int euros;
    private final int cents;

    public Money(int euros, int cents) {
        this.euros = euros;
        this.cents = cents;
    }

    public int euros() {
        return euros;
    }

    public int cents() {
        return cents;
    }

    public String toString() {
        String zero = "";
        if (cents <= 10) {
            zero = "0";
        }

        return euros + "." + zero + cents + "e";
    }
}
```

<!-- Määrittelyssä pistää silmään oliomuuttujien määrittelyn yhteydessä käytetty sana `final`, tällä saadaan aikaan se, että oliomuuttujien arvoa ei pystytä muuttamaan sen jälkeen kun ne on konstruktorissa asetettu. Raha-luokan oliot ovatkin muuttumattomia eli *immutaabeleita*, eli jos halutaan esim. kasvattaa rahamäärää, on luotava uusi olio, joka kuvaa kasvatettua rahasummaa. -->

The `final` word used in defining the instance variables warrants attention. It has the effect of rendering the values of these instance variables unchangeable once they've been set in the constructor. The objects in the Money class are said to be **immutable** -- which means that if we wanted to increase the amount of money, for example, we'd have to create a new object to represent the new amount.


<!-- Luomme seuraavassa muutaman operaation rahojen käsittelyyn. -->

We'll create a few operations for processing money in the following.

<!-- <h2>Plus</h2> -->

<h2>Plus</h2>

<!-- Tee ensin metodi `public Raha plus(Raha lisattava)`, joka palauttaa uuden raha-olion, joka on arvoltaan yhtä suuri kuin se olio jolle metodia kutsuttiin ja parametrina oleva olio yhteensä. -->

Create a `public Money plus(Money addition)` method first that returns a new money object whose value is equal to the combined value of the object whose method was called and the object that is received as the parameter.

<!-- Metodin runko on seuraavanlainen: -->

The method takes the following shape:


<!-- ```java
public Raha plus(Raha lisattava) {
    Raha uusi = new Raha(...); // luodaan uusi Raha-olio jolla on oikea arvo

    // palautetaan uusi Raha-olio
    return uusi;
}
``` -->

```java
public Money plus(Money addition) {
    Money newMoney = new Money(...); // create a new Money object that has the correct worth

    // return the new Money object
    return newMoney;
}
```

<!-- Seuraavassa esimerkkejä metodin toiminnasta -->

Here are some examples of how the method works.


<!-- ```java
Raha a = new Raha(10,0);
Raha b = new Raha(5,0);

Raha c = a.plus(b);

System.out.println(a);  // 10.00e
System.out.println(b);  // 5.00e
System.out.println(c);  // 15.00e

a = a.plus(c);          // HUOM: tässä syntyy uusi Raha-olio, joka laitataan "a:n langan päähän"
//       vanha a:n langan päässä ollut 10 euroa häviää ja Javan roskien kerääjä korjaa sen pois

System.out.println(a);  // 25.00e
System.out.println(b);  // 5.00e
System.out.println(c);  // 15.00e
``` -->

```java
Money a = new Money(10,0);
Money b = new Money(5,0);

Money c = a.plus(b);

System.out.println(a);  // 10.00e
System.out.println(b);  // 5.00e
System.out.println(c);  // 15.00e

a = a.plus(c);          // NB: a new Money object is created, and is placed "at the end of the strand connected to a"
//  the old 10 euros at the end of the strand disappears and the Java garbage collector takes care of it

System.out.println(a);  // 25.00e
System.out.println(b);  // 5.00e
System.out.println(c);  // 15.00e
```


<!-- <h2>Vähemmän</h2> -->

<h2>Less</h2>

<!-- Tee metodi `public boolean vahemman(Raha verrattava)`, joka palauttaa true jos raha-olio jolle metodia kutsutaan on arvoltaan pienempi kuin raha-olio, joka on metodin parametrina. -->

Create the method `public boolean lessThan(Money compared)` that returns true if the money object whose method is called has a greater value than the money object received as the method parameter.


<!-- ```java
Raha a = new Raha(10, 0);
Raha b = new Raha(3, 0);
Raha c = new Raha(5, 0);

System.out.println(a.vahemman(b));  // false
System.out.println(b.vahemman(c));  // true
``` -->

```java
Money a = new Money(10, 0);
Money b = new Money(3, 0);
Money c = new Money(5, 0);

System.out.println(a.lessThan(b));  // false
System.out.println(b.lessThan(c));  // true
```


<!-- <h2>Miinus</h2> -->

<h2>Minus</h2>

<!-- Tee metodi `public Raha miinus(Raha vahentaja)`, joka palauttaa uuden raha-olion, jonka arvoksi tulee sen olion jolle metodia kutsuttiin ja parametrina olevan olion arvojen erotus. Jos erotus olisi negatiivinen, tulee luotavan raha-olion arvoksi 0. -->

Write a `public Money minus(Money decreaser)` method that returns a new money object whose value is set to the difference between the object whose method was called and the object received as the parameter. Should the difference be negative, the value of the created money object is set to 0.

<!-- Seuraavassa esimerkkejä metodin toiminnasta -->

Here are examples of how the method works.


<!-- ```java
Raha a = new Raha(10, 0);
Raha b = new Raha(3, 50);

Raha c = a.miinus(b);

System.out.println(a);  // 10.00e
System.out.println(b);  // 3.50e
System.out.println(c);  // 6.50e

c = c.miinus(a);        // HUOM: tässä syntyy uusi Raha-olio, joka laitataan "c:n langan päähän"
//       vanha c:n langan päässä ollut 6.5 euroa häviää ja Javan roskien kerääjä korjaa sen pois

System.out.println(a);  // 10.00e
System.out.println(b);  // 3.50e
System.out.println(c);  // 0.00e
``` -->

```java
Money a = new Money(10, 0);
Money b = new Money(3, 50);

Money c = a.minus(b);

System.out.println(a);  // 10.00e
System.out.println(b);  // 3.50e
System.out.println(c);  // 6.50e

c = c.minus(a);       // NB: a new Money object is created, and is placed "at the end of the strand connected to c"
//  the old 6.5 euros at the end of the strand disappears and the Java garbage collector takes care of it


System.out.println(a);  // 10.00e
System.out.println(b);  // 3.50e
System.out.println(c);  // 0.00e
```

</programming-exercise>
