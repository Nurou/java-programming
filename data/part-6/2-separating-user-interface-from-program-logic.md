---
path: "/part-6/2-separating-user-interface-from-program-logic"
title: "Separating the user interface from program logic"
hidden: false
---

<text-box variant='learningObjectives' name='Learning objectives'>

<!-- - Tutustut sovelluksen luomiseen siten, että käyttöliittymä ja sovelluslogiikka ovat erillään. -->
<!-- - Osaat luoda tekstikäyttöliittymän, joka saa parametrinaan sovelluskohtaisen sovelluslogiikan sekä syötteen lukemiseen käytettävän Scanner-olion. -->

 - You become familiar with creating programs where the user interface and the application logic are separated
 - You know how to create a text-based user interface that takes as its parameters program-specific application logic and a Scanner object for reading inputs


</text-box>

<!-- Tarkastellaan erään ohjelman rakennusprosessia sekä tutustutaan sovelluksen vastuualueiden erottamiseen toisistaan. Ohjelma kysyy käyttäjältä sanoja kunnes käyttäjä syöttää saman sanan uudestaan. Ohjelma käyttää listaa sanojen tallentamiseen. -->

Let's take a look at the process of implementing a program, and how to separate areas of responsibility. This program asks the user to input words until they input the same word twice.

<sample-output>

Write a word: **carrot**
Write a word: **turnip**
Write a word: **potato**
Write a word: **celery**
Write a word: **potato**
You wrote the same word twice!

</sample-output>

<!-- Rakennetaan ohjelma osissa. Eräs haasteista on se, että on vaikea päättää miten lähestyä tehtävää, eli miten ongelma tulisi jäsentää osaongelmiksi, ja mistä osaongelmasta kannattaisi aloittaa. Yhtä oikeaa vastausta ei ole -- joskus on hyvä lähteä pohtimaan ongelmaan liittyviä käsitteitä ja niiden yhteyksiä, joskus taas ohjelman tarjoamaa käyttöliittymää. -->

Let's build this program piece by piece. One challenge is that it can be difficult to decide on how to approach a problem, or how to split it into smaller subproblems, and which subproblem to start off with. There isn't a clear answer -- sometimes it's a good idea to start from the problem domain, its concepts and their connections, and other times it may be better to start with the user interface.

<!-- Käyttöliittymän hahmottelu voisi lähteä liikenteeseen luokasta Kayttoliittyma. Käyttöliittymä käyttää syötteen lukemiseen Scanner-oliota, joka annetaan sille käyttöliittymän luonnin yhteydessä. Tämän lisäksi käyttöliittymällä on käynnistämiseen tarkoitettu metodi. -->

We could begin implementing the user interface by creating a UserInterface class. This UserInterface class uses a Scanner object to read user input. The class also has a method for the purpose of launching the interface.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {
        // tehdään jotain
    }
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {
        // we do something
    }
}
```

<!-- Käyttöliittymän luominen ja käynnistäminen onnistuu seuraavasti. -->

 Creating and launching a user interface can be done like so.

<!-- ```java
public static void main(String[] args) {
    Scanner lukija = new Scanner(System.in);
    Kayttoliittyma kayttoliittyma = new Kayttoliittyma(lukija);
    kayttoliittyma.kaynnista();
}
``` -->
```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    UserInterface userInterface = new UserInterface(scanner);
    userInterface.start();
}
```

<!-- ## Toisto ja lopetus -->

## Looping and Quitting


<!-- Ohjelmassa on (ainakin) kaksi "aliongelmaa". Ensimmäinen on sanojen toistuva lukeminen käyttäjältä kunnes tietty ehto toteutuu. Tämä voitaisiin hahmotella seuraavaan tapaan. -->

This program has (at least) two "subproblems". The first problem is continuously reading words from the user until a certain condition is reached. We can outline this as follows.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (*pitää lopettaa*) {
                break;
            }

        }

        System.out.println("Annoit saman sanan uudestaan!");
    }
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (*stop condition*) {
                break;
            }

        }

        System.out.println("You entered the same word twice!");
    }
}
```

<!-- Sanojen kysely jatkuu kunnes käyttäjä syöttää jo aiemmin syötetyn sanan. Täydennetään ohjelmaa siten, että se tarkastaa onko sana jo syötetty. Vielä ei tiedetä miten toiminnallisuus kannattaisi tehdä, joten tehdään siitä vasta runko. -->

The program continues to ask for words until the user enters a word that has already been entered. Let's modify the program so that it checks if the word has already been entered. We don't yet know how to implement the functionality for this, so we'll begin by constructing an outline for it.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        // tänne jotain

        return false;
    }
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

        }

        System.out.println("You entered the same word twice!");
    }

    public boolean alreadyEntered(String word) {
        // do something here

        return false;
    }
}
```

<!-- Ohjelmaa on hyvä testata koko ajan, joten tehdään metodista kokeiluversio: -->

It'd be a good idea to test the program continuously. Let's make a test version of the method:

<!-- ```java
public boolean onJoSyotetty(String sana) {
    if (sana.equals("loppu")) {
        return true;
    }

    return false;
}
``` -->

```java
public boolean alreadyEntered(String word) {
    if (word.equals("end")) {
        return true;
    }

    return false;
}
```

<!-- Nyt toisto jatkuu niin kauan kunnes syötteenä on sana loppu: -->

The loop now continues until the word "end" is entered:

<!-- <sample-output>

Anna sana: **porkkana**
Anna sana: **selleri**
Anna sana: **nauris**
Anna sana: **lanttu**
Anna sana: **loppu**
Annoit saman sanan uudestaan!

</sample-output> -->

<sample-output>

Enter a word: **carrot**
Enter a word: **celery**
Enter a word: **turnip**
Enter a word: **end**
You entered the same word twice!

</sample-output>

<!-- Ohjelma ei toimi vielä kokonaisuudessaan, mutta ensimmäinen osaongelma eli ohjelman pysäyttäminen kunnes tietty ehto toteutuu on saatu toimimaan.-->

The program doesn't work completely yet, but the first subproblem, i.e., stopping the loop when a certain condition has been met, has been implemented.

<!-- ## Oleellisten tietojen tallentaminen -->

## Storing Relevant Information

<!-- Toinen osaongelma on aiemmin syötettyjen sanojen muistaminen. Lista sopii mainiosti tähän tarkoitukseen. -->

Another subproblem is remembering the words that have already been entered. A list is a good fit for this purpose.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    //...
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    //...
}
```

<!-- Kun uusi sana syötetään, on se lisättävä syötettyjen sanojen joukkoon. Tämä tapahtuu lisäämällä while-silmukkaan listan sisältöä päivittävä rivi:-->

When a new word is entered, it has to be added to the list of words that have been input. This is done by adding a line to the while-loop that updates our list:

<!--
```java
while (true) {
    System.out.print("Anna sana: ");
    String sana = lukija.nextLine();

    if (onJoSyotetty(sana)) {
        break;
    }

    // lisätään uusi sana aiempien sanojen listaan
    this.sanat.add(sana);
}
``` -->

```java
while (true) {
    System.out.print("Enter a word: ");
    String word = scanner.nextLine();

    if (alreadyEntered(word)) {
        break;
    }

    // adding the word to the list of previous words
    this.words.add(word);
}
```

<!-- Kayttoliittyma näyttää kokonaisuudessaan seuraavalta.
 -->
The user interface in its entirety looks like so:

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

            // lisätään uusi sana aiempien sanojen listaan
            this.sanat.add(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        if (sana.equals("loppu")) {
            return true;
        }

        return false;
    }
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

            // adding the word to the list of previous words
            this.words.add(word);

        }

        System.out.println("You entered the same word twice!");
    }

    public boolean alreadyEntered(String word) {
       if (word.equals("end")) {
            return true;
        }

        return false;
    }
}
```

<!-- Jälleen kannattaa testata, että ohjelma toimii edelleen. Voi olla hyödyksi esimerkiksi lisätä kaynnista-metodin loppuun testitulostus, joka varmistaa että syötetyt sanat todella menivät listaan.  -->

Once again, it's a good idea to test that the program still works. It might be useful, for example, to add a test print to the end of the start-method to make sure that the words entered as input have in fact been added to the list.

<!-- ```java
// testitulostus joka varmistaa että kaikki toimii edelleen
for(String sana: this.sanat) {
    System.out.println(sana);
}
``` -->

```java
// test print to check that everything still works
for (String word: this.words) {
    System.out.println(word);
}
```

<!-- ## Osaongelmien ratkaisujen yhdistäminen -->

## Combining Solutions to Subproblems

<!-- Muokataan vielä äsken tekemämme metodi `onJoSyotetty` tutkimaan onko kysytty sana jo syötettyjen joukossa, eli listassa. -->

Let's change the method 'alreadyEntered' so that it checks whether a given word already exists among the set of inputs, i.e., in our list of words that have been input.

<!-- ```java
public boolean onJoSyotetty(String sana) {
    return this.sanat.contains(sana);
}
```-->

```java
public boolean alreadyEntered(String word) {
    return this.words.contains(word);
}
```

<!-- Nyt sovellus toimii kutakuinkin halutusti.-->
The application now works as intended.

<!-- ## Oliot luonnollisena osana ongelmanratkaisua -->

## Objects as a Natural Part of Problem Solving


<!-- Rakensimme äsken ratkaisun ongelmaan, missä luetaan käyttäjältä sanoja, kunnes käyttäjä antaa saman sanan uudestaan. Syöte ohjelmalle oli esimerkiksi seuraavanlainen. -->

We've just built a solution to a problem where the program reads words from a user until the user enters a word that has already been entered before. The program input was similar to the following:


<!-- <sample-output>

Anna sana: **porkkana**
Anna sana: **selleri**
Anna sana: **nauris**
Anna sana: **lanttu**
Anna sana: **selleri**
Annoit saman sanan uudestaan!

</sample-output> -->

<sample-output>

Enter a word: **carrot**
Enter a word: **celery**
Enter a word: **turnip**
Enter a word: **potato**
Enter a word: **celery**
You entered the same word twice!

</sample-output>

<!-- Päädyimme ratkaisuun -->

We came up with the following solution:


<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

            // lisätään uusi sana aiempien sanojen listaan
            sanat.add(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        return this.sanat.contains(sana);
    }
}
``` -->

```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

            // adding the word to the list of previous words
            this.words.add(word);

        }

        System.out.println("You entered the same word twice!");
    }

    public boolean alreadyEntered(String word) {
       if (word.equals("end")) {
            return true;
        }

        return false;
    }
}
```

<!-- Ohjelman käyttämä apumuuttuja lista `sanat` on yksityiskohta käyttöliittymän kannalta. Käyttöliittymän kannaltahan on oleellista, että muistetaan niiden *sanojen joukko* jotka on nähty jo aiemmin. Sanojen joukko on selkeä erillinen "käsite", tai abstraktio. Tälläiset selkeät käsitteet ovat potentiaalisia olioita; kun koodissa huomataan "käsite" voi sen eristämistä erilliseksi luokaksi harkita. -->

The 'words' helper variable used by the program is just a detail from the user interface perspective. What's relevant is that the user interface remembers the *set of words* that have been entered prior. The set of words is a clear and distinct "concept", or abstraction. These kinds of clear concepts are all potential objects: whenever we come across them in out code, we may consider separating the concept into its own class.

<!-- ### Sanajoukko -->

### Word Set

<!-- Tehdään luokka `Sanajoukko`, jonka käyttöönoton jälkeen käyttöliittymän metodi `kaynnista` on seuraavanlainen: -->

 Let's make a class called 'WordSet'. After implementing the class, the user interface's start method looks like so:

<!-- ```java
while (true) {
    String sana = lukija.nextLine();

    if (sanat.sisaltaa(sana)) {
        break;
    }

    sanajoukko.lisaa(sana);
}

System.out.println("Annoit saman sanan uudestaan!");
``` -->

```java
while (true) {
    String word = scanner.nextLine();

    if (words.contains(word)) {
        break;
    }

    wordSet.add(word);
}

System.out.println("You entered the same word twice!");
```

<!-- Käyttöliittymän kannalta Sanajoukolla kannattaisi siis olla metodit `boolean sisaltaa(String sana)` jolla tarkastetaan sisältyykö annettu sana jo sanajoukkoon ja `void lisaa(String sana)` jolla annettu sana lisätään joukkoon.

Huomaamme, että näin kirjoitettuna käyttöliittymän luettavuus on huomattavasti parempi.

Luokan `Sanajoukko` runko näyttää seuraavanlaiselta:-->

From the user interface's perspective, the class WordSet should contain the method 'boolean contains(String word)', that checks whether the given word is contained in our set of words, and the method 'void add(String word)', that adds the given word into the set.

We see that the readability of the user interface is much better when written this way.

The outline for the class 'WordSet' looks like this:

<!-- ```java
public class Sanajoukko {
    // oliomuuttuja(t)

    public Sanajoukko() {
        // konstruktori
    }

    public boolean sisaltaa(String sana) {
        // sisältää-metodin toteutus
        return false;
    }

    public void lisaa(String sana) {
        // lisaa-metodin toteutus
    }
}
``` -->

```java
public class WordSet {
    // instance variable(s)

    public WordSet() {
        // constructor
    }

    public boolean contains(String word) {
        // implementation of the contains method
        return false;
    }

    public void add(String word) {
        // implementation of the add method
    }
}
```

<!-- ### Toteutus aiemmasta ratkaisusta -->

### Implementing the Previous Solution


<!-- Voimme toteuttaa sanajoukon siirtämällä aiemman ratkaisumme listan sanajoukon oliomuuttujaksi: -->

We can implement the set of words by turning our previous solution, the list, into an instance variable of WordSet:


<!-- ```java
import java.util.ArrayList;

public class Sanajoukko {
    private ArrayList<String> sanat;

    public Sanajoukko() {
        this.sanat = new ArrayList<>();
    }

    public void lisaa(String sana) {
        this.sanat.add(sana);
    }

    public boolean sisaltaa(String sana) {
        return this.sanat.contains(sana);
    }
}
``` -->

```java
import java.util.ArrayList;

public class WordSet {
    private ArrayList<String> words

    public WordSet() {
        this.words = new ArrayList<>();
    }

    public void add(String word) {
        this.words.add(word);
    }

    public boolean contains(String word) {
        return this.words.contains(word);
    }
}
```

<!-- Ratkaisu on nyt melko elegantti. Erillinen käsite on saatu erotettua ja käyttöliittymä näyttää siistiltä. Kaikki "likaiset yksityiskohdat" on saatu siivottua eli kapseloitua olion sisälle.

Muokataan käyttöliittymää niin, että se käyttää Sanajoukkoa. Sanajoukko annetaan käyttöliittymälle samalla tavalla parametrina kuin Scanner. -->

Our solution is now quite elegant. We have separated a distinct concept into a class of its own, and our user interface looks clean. All the "dirty details" have been encapsulated neatly inside an object.

Let's now edit the user interface so that it uses the class WordSet. The class is given to the user interface as a parameter, in the same way that Scanner is.

<!-- ```java
public class Kayttoliittyma {
    private Sanajoukko sanajoukko;
    private Scanner lukija;

    public Kayttoliittyma(Sanajoukko sanajoukko, Scanner lukija) {
        this.sanajoukko = sanajoukko;
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (this.sanajoukko.sisaltaa(sana)) {
                break;
            }

            this.sanajoukko.lisaa(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }
}
``` -->

```java
public class UserInterface {
    private WordSet wordSet;
    private Scanner scanner;

    public userInterface(WordSet wordSet, Scanner scanner) {
        this.wordSet = wordSet;
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (this.wordSet.contains(word)) {
                break;
            }

            this.wordSet.add(word);
        }

        System.out.println("You entered the same word twice!");
    }
}
```

<!-- Ohjelman käynnistäminen tapahtuu nyt seuraavasti: -->

Starting the program is now done as follows:


<!-- ```java
public static void main(String[] args) {
    Scanner lukija = new Scanner(System.in);
    Sanajoukko joukko = new Sanajoukko();

    Kayttoliittyma kayttoliittyma = new Kayttoliittyma(joukko, lukija);
    kayttoliittyma.kaynnista();
}
``` -->

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    WordSet set = new WordSet();

    UserInterface userInterface = new UserInterface(set, scanner);
    userInterface.start();
}
```

<!-- ## Luokan sisäisen toteutuksen muuttaminen -->

## Changing the Internal Implementation of a Class


<!-- Olemme päätyneet tilanteeseen missä `Sanajoukko` ainoastaan "kapseloi" ArrayList:in. Onko tässä järkeä? Kenties. Voimme nimittäin halutessamme tehdä Sanajoukolle muitakin muutoksia. Ennen pitkään saatamme esim. huomata, että sanajoukko pitää tallentaa tiedostoon. Jos tekisimme nämä muutokset Sanajoukkoon muuttamatta käyttöliittymän käyttävien metodien nimiä, ei käyttöliittymää tarvitsisi muuttaa mitenkään. -->

We've arrived at a situation where the only thing the class 'WordSet' does is "encapsulate" an ArrayList. Is this sensible? Perhaps. We're in a position where we're able to make changes to the class if there is a need. We may before long run into a situation where the word set has to be saved into a file, for example. If we make all these changes inside the class WordSet without changing the names of the methods that the user interface uses, the user interface wouldn't need to be modified at all.

<!-- Oleellista on tässä se, että Sanajoukko-luokkaan tehdyt sisäiset muutokset eivät vaikuta luokkaan Käyttöliittymä. Tämä johtuu siitä, että käyttöliittymä käyttää sanajoukkoa sen tarjoamien metodien -- eli julkisten rajapintojen -- kautta. -->

The main point here is that changes made inside the class WordSet don't affect the UserInterface class. This is because the user interface uses WordSet through the methods that it provides, i.e., through its public interfaces.

<!-- ## Uusien toiminnallisuuksien toteuttaminen: palindromit -->

## Implementing New Functionality: Palindromes


<!-- Voi olla, että jatkossa ohjelmaa halutaan laajentaa siten, että `Sanajoukko`-luokan olisi osattava uusia asiota. Jos ohjelmassa haluttaisiin esimerkiksi tietää kuinka moni syötetyistä sanoista oli palindromi, voidaan sanajoukkoa laajentaa metodilla `palindromeja`. -->

We may want to augment the program in the future so that the class 'WordSet' offers some new functionalities. If, for example, we wanted to know how many of the words entered were palindromes, we could extend the word set by adding a 'palindromes' method.


<!-- ```java
public void kaynnista() {

    while (true) {
        System.out.print("Anna sana: ");
        String sana = lukija.nextLine();

        if (this.sanajoukko.sisaltaa(sana)) {
            break;
        }

        this.sanajoukko.lisaa(sana);
    }

    System.out.println("Annoit saman sanan uudestaan!");
    System.out.println("Sanoistasi " + this.sanajoukko.palindromeja() + " oli palindromeja");
}
``` -->

```java
public void start() {

    while (true) {
        System.out.print("Enter a word: ");
        String word = scanner.nextLine();

        if (this.wordSet.contains(word)) {
            break;
        }

        this.wordSet.add(word);
    }

    System.out.println("You entered the same word twice!");
    System.out.println(this.wordSet.palindromes() + " of the words were palindromes.");
}
```

<!-- Käyttöliittymä säilyy siistinä ja palindromien laskeminen jää `Sanajoukko`-olion huoleksi. Metodin toteutus voisi olla esimerkiksi seuraavanlainen. -->

The user interface remains tidy and the counting of palindromes is confined to within the 'WordSet' object. The method could be implemented in the following way.

<!-- ```java
import java.util.ArrayList;

public class Sanajoukko {
    private ArrayList<String> sanat;

    public Sanajoukko() {
        this.sanat = new ArrayList<>();
    }

    public boolean sisaltaa(String sana) {
        return this.sanat.contains(sana);
    }

    public void lisaa(String sana) {
        this.sanat.add(sana);
    }

    public int palindromeja() {
        int lukumaara = 0;

        for (String sana: this.sanat) {
            if (onPalindromi(sana)) {
                lukumaara++;
            }
        }

        return lukumaara;
    }

    public boolean onPalindromi(String sana) {
        int loppu = sana.length() - 1;

        int i = 0;
        while (i < sana.length() / 2) {
            // metodi charAt palauttaa annetussa indeksissä olevan merkin
            // alkeistyyppisenä char-muuttujana
            if(sana.charAt(i) != sana.charAt(loppu - i)) {
                return false;
            }

            i++;
        }

        return true;
    }
}
``` -->

```java
import java.util.ArrayList;

public class WordSet {
    private ArrayList<String> words;

    public WordSet() {
        this.words = new ArrayList<>();
    }

    public boolean contains(String word) {
        return this.words.contains(word);
    }

    public void add(String word) {
        this.words.add(word);
    }

    public int palindromes() {
        int count = 0;

        for (String word: this.words) {
            if (isPalindrome(word)) {
                count++;
            }
        }

        return count;
    }

    public boolean isPalindrome(String word) {
        int end = word.length() - 1;

        int i = 0;
        while (i < word.length() / 2) {
            // method charAt returns the character at given index
            // as a simple variable
            if(word.charAt(i) != word.charAt(end - i)) {
                return false;
            }

            i++;
        }

        return true;
    }
}
```

<!-- Metodissa `palindromeja` käytetään apumetodia `onPalindromi`, joka tarkastaa onko sille parametrina annettu sana palindromi. -->

The method 'palindromes' uses the helper method 'isPalindrome' to check whether the word that's given to it as a parameter is, in fact, a palindrome.

<!-- <text-box variant='hint' name='Uusiokäyttö'>

Kun ohjelmakoodin käsitteet on eriytetty omiksi luokikseen, voi niitä uusiokäyttää helposti muissa projekteissa. Esimerkiksi luokkaa `Sanajoukko` voisi käyttää yhtä hyvin graafisesta käyttöliittymästä, ja se voisi myös olla osa kännykässä olevaa sovellusta. Tämän lisäksi ohjelman toiminnan testaaminen on huomattavasti helpompaa silloin kun ohjelma on jaettu erillisiin käsitteisiin, joita kutakin voi käyttää myös omana itsenäisenä yksikkönään.

</text-box> -->

<text-box variant='hint' name='Recycling'>

When concepts appearing in code have been separated into separate classes, they can easily be reused in other projects. As an example, the class 'WordSet' could equally well be used in a graphical user interface, or as part of a mobile phone application. In addition, testing a program is considerably easier if it has been divided into several concepts, each of which can function as its own unit.

</text-box>

<!-- ## Neuvoja ohjelmointiin -->

## Tips for Programming


<!-- Yllä kuvatussa laajemmassa esimerkissä noudatettiin seuraavia neuvoja. -->

We followed the advice given here in the broader example above.

<!-- - Etene pieni askel kerrallaan

  - Yritä pilkkoa ongelma osaongelmiin ja **ratkaise vain yksi osaongelma kerrallaan**
  - Testaa aina että ohjelma on etenemässä oikeaan suuntaan eli että osaongelman ratkaisu meni oikein
  - Tunnista ehdot, minkä tapauksessa ohjelman tulee toimia eri tavalla. Esimerkiksi yllä tarkistus, jolla katsotaan onko sana jo syötetty, johtaa erilaiseen toiminnallisuuden.

- Kirjoita mahdollisimman "siistiä" koodia

  - sisennä koodi
  - käytä kuvaavia muuttujien ja metodien nimiä
  - älä tee liian pitkiä metodeja, edes mainia
  - tee yhdessä metodissa vaan yksi asia
  - **poista koodistasi kaikki copy-paste**
  - korvaa koodisi "huonot" ja epäsiistit osat siistillä koodilla

- Astu tarvittaessa askel taaksepäin ja mieti kokonaisuutta. Jos ohjelma ei toimi, voi olla hyvä idea palata aiemmin toimineeseen tilaan. Käänteisesti voidaan sanoa, että rikkinäinen ohjelma korjaantuu harvemmin lisäämällä siihen lisää koodia. -->

-  Proceed in small steps
    -  Try to separate the program into several subproblems and **work on only one subproblem at a time**
    -  Always test that the program code is advancing in the right direction, in other words: test that the solution to the subproblem is correct
    -  Recognize the conditions that require the program to work differently. In the example above, we needed a different functionality to test whether a word had been already entered before.

-  Write as "clean" code as possible
    -  Indent your code
    -  Use descriptive method and variable names
    -  Don't make your methods too long, not even the main method
    -  Do only one thing inside one method
    -  **Remove all copy-paste code**
    -  Replace the "bad" and unclean parts of your code with clean code

-  If needed, take a step back to assess the program as a whole. If it doesn't work, it might be a good idea to return it into a previous, working state. A program that's broken is rarely fixed by adding more code to it.

<!-- Ohjelmoijat noudattavat näitä käytänteitä sen takia että ohjelmointi olisi helpompaa. Käytänteiden noudattaminen tekee myös ohjelmien lukemisesta, ylläpitämisestä ja muokkaamisesta helpompaa muille. -->

Programmers follow these conventions to make programming easier. Following them also makes programs easier to be read, maintained, and modified by others.


<programming-exercise name='Simple Dictionary (4 parts)' tmcname='part06-Part06_09.SimpleDictionary'>

<!-- Tehtäväpohjassa on valmiiksi annettuna luokka `SimpleDictionary`, joka tarjoaa toiminnallisuuden sanojen ja niiden käännösten tallentamiseen. Vaikka luokan sisäisessä totetuksessa on asioita, joita kurssilla ei ole käsitelty, on sen käyttö suoraviivaista: -->

The exercise base contains a class `SimpleDictionary` that allows for storing words and their translations. Although the internal implementation of the class contains techniques that haven't been covered on the course yet, it's use is straightforward:

<!-- ```java
SimpleDictionary book = new SimpleDictionary();
kirja.lisaa("yksi", "one");
kirja.lisaa("kaksi", "two");

System.out.println(kirja.kaanna("yksi"));
System.out.println(kirja.kaanna("kaksi"));
System.out.println(kirja.kaanna("kolme"));
``` -->

```java
SimpleDictionary book = new SimpleDictionary();
book.add("one", "yksi");
book.add("two", "kaksi");

System.out.println(book.translate("one"));
System.out.println(book.translate("two"));
System.out.println(book.translate("three"));

```

<sample-output>

yksi
kaksi
null

</sample-output>

<!-- Tässä tehtävässä toteutat luokkaa `SimpleDictionary` hyödyntävän tekstikäyttöliittymän. -->

In this exercise, you will implement a text-based user interface that makes use of the `SimpleDictionary` class. You may pick up a few Finnish words while doing it!


<!-- <h2>Tekstikäyttöliittymän käynnistys ja lopetus</h2> -->

<h2>Starting and Stopping the UI</h2>

<!-- Toteuta luokka `Tekstikayttoliittyma`, joka saa konstruktorin parametrina `Scanner`-olion sekä `Sanakirja`-olion. Lisää tämän jälkeen luokalle metodi `public void kaynnista()`. Metodin tulee toimia seuraavalla tavalla: -->

Implement the class `TextUI` that receives `Scanner` and `SimpleDictionary` objects as its constructor's parameters. After this, equip the class with a  `public void start()` method. The method should work as follows:

<!-- 1. Metodi kysyy käyttäjältä komentoa. -->

1. The method asks the user for a command

<!-- 2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy. -->

2. If the command is `end`, the UI prints the string "Bye bye!", and the execution of the `start` method ends.

<!-- 3. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

3. Otherwise, the text UI prints the message `Unknown command` and the method continues by asking for a new command, i.e., from stage 1.


<!-- ```java
Scanner lukija = new Scanner(System.in);
Sanakirja kirja = new Sanakirja();

Tekstikayttoliittyma kayttoliittyma = new Tekstikayttoliittyma(lukija, kirja);
kayttoliittyma.kaynnista();
``` -->

```java
Scanner scanner = new Scanner(System.in);
SimpleDictionary dictionary = new SimpleDictionary();

TextUI ui = new TextUI(scanner, dictionary);
ui.start();
```

<!-- <sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Tuntematon komento
Komento: **lopeta**
Hei hei!

</sample-output> -->

<sample-output>

Command: **something**
Unknown command
Command: **add**
Unknown command
Command: **end**
Bye bye!

</sample-output>


<!-- <h2>Käännösten lisääminen</h2> -->

<h2>Adding a Translation</h2>

<!-- Muokkaa metodia `public void kaynnista()` siten, että se toimii seuraavalla tavalla: -->

Modify the method `public void start()` so that it works in the following way:


<!-- 1. Metodi kysyy käyttäjältä komentoa. -->

1. The method asks the user for a command.

<!-- 2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy. -->

2. If the command is `end`, the UI prints the string "Bye bye!", and the execution of the `start` method ends.

<!-- 3. Mikäli komento on `lisaa`, tekstikäyttöliittymä kysyy käyttäjältä sanaa ja käännöstä, kumpaakin omalla rivillään. Tämän jälkeen sanat lisätään sanakirjaan ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

3. If the command is `add`, the text UI asks the user for a word and a translation, each on its own line. After this, the words are added to the dictionary, and the method continues by asking for a new command (returns back to stage 1).

<!-- 4. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

4. Otherwise, the text UI prints the message `Unknown command` and the method continues by asking for a new command, i.e., from stage 1.

<!-- <sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **lopeta**
Hei hei!

</sample-output> -->

<sample-output>

Command: **something**
Unknown command
Command: **add**
Word: **pike**
Translation: **hauki**
Command: **change**
Unknown command
Command: **end**
Bye bye!

</sample-output>

<!-- Yllä kuvatussa esimerkissä sanakirja-olioon lisätään sana "hauki" sekä sen käännös "pike". Sanakirjaa voisi tällöin käyttöliittymästä poistumisen jälkeen käyttää seuraavasti: -->

In the example above, we added the word "pike" and its translation "hauki" to the SimpleDictionary object. After exiting the text user interface, the dictionary could be used like so:

<!-- ```java
Scanner lukija = new Scanner(System.in);
Sanakirja kirja = new Sanakirja();

Tekstikayttoliittyma kayttoliittyma = new Tekstikayttoliittyma(lukija, kirja);
kayttoliittyma.kaynnista();
System.out.println(kirja.kaanna("hauki")); // tulostaa merkkijonon "pike"
``` -->

```java
Scanner scanner = new Scanner(System.in);
SimpleDictionary dictionary = new SimpleDictionary();

TextUI textUI = new TextUI(scanner, dictionary);
textUI.start();
System.out.println(dictionary.translate("pike")); // prints the string "hauki"
```

<!-- <h2>Sanan kääntäminen</h2> -->

<h2>Translating a Word</h2>

<!-- Muokkaa metodia `public void kaynnista()` siten, että se toimii seuraavalla tavalla: -->

Modify the method `public void start()` so that it works the following way:

<!-- 1. Metodi kysyy käyttäjältä komentoa. -->

1. The method asks the user for a command.

<!-- 2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy. -->

2. If the command is `end`, the UI prints the string "Bye bye!", and the execution of the `start` method ends.

<!-- 3. Mikäli komento on `lisaa`, tekstikäyttöliittymä kysyy käyttäjältä sanaa ja käännöstä, kumpaakin omalla rivillään. Tämän jälkeen sanat lisätään sanakirjaan ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

3. If the command is `add`, the text UI asks the user for a word and a translation, each on its own line. After this, the words are stored in the dictionary and the method continues by asking for a new command (loops back to stage 1).

<!-- 4. Mikäli komento on `hae`, tekstikäyttöliittymä kysyy käyttäjältä käännettävää sanaa. Tämän jälkeen tekstikäyttöliittymä tulostaa sanan käännöksen ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

4. If the command is `search`, the text UI asks the user for the word that is to be translated. After that, it prints the translation of the given word, and the method continues by asking for a new command, i.e., from stage 1.

<!-- 5. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1. -->

5. Otherwise, the text UI prints the message `Unknown command` and the method continues by asking for a new command, i.e., from stage 1.

<!-- <sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **hae**
Haettava: **hauki**
Käännös: pike
Komento: **hae**
Haettava: **nauris**
Käännös: null
Komento: **lopeta**
Hei hei!

</sample-output> -->

<sample-output>

Command: **something**
Unknown command
Command: **add**
Word: **pike**
Translation: **hauki**
Command: **change**
Unknown command
Command: **search**
To be translated: **pike**
Translation: hauki
Command: **search**
To be translated: **carrot**
Translation: null
Command: **end**
Bye bye!

</sample-output>


<!-- <h2>Käännöksen siistiminen</h2> -->

<h2>Tidying the Translation</h2>

<!-- Muokkaa tekstikäyttöliittymän hakutoiminnallisuutta siten, että mikäli sanaa ei löydy (eli sanakirja palauttaa `null`-viitteen), tekstikäyttöliittymä tulostaa viestin "Sanaa (haettava) ei löydy". -->

Modify the searching functionality of the UI so that if a word isn't found (i.e., the dictionary returns `null`), the UI prints the message "Word (searched word) was not found".

<!-- <sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **hae**
Haettava: **hauki**
Käännös: pike
Komento: **hae**
Haettava: **nauris**
Sanaa nauris ei löydy
Komento: **lopeta**
Hei hei!

</sample-output> -->

<sample-output>

Command: **something**
Unknown command
Command: **add**
Word: **pike**
Translation: **hauki**
Command: **change**
Unkown command
Command: **search**
To be translated: **pike**
Translation: hauki
Command: **search**
To be translated: **carrot**
Word carrot was not found
Command: **end**
Bye bye!

</sample-output>

</programming-exercise>


<programming-exercise name='To do list (2 parts)' tmcname='part06-Part06_10.TodoList'>

<!-- Tässä tehtävässä tehdään sovellus tehtävälistan luomiseen ja käsittelyyn. Lopullinen sovellus tulee toimimaan seuraavalla tavalla. -->

In this exercise, we're going to create a program that can be used to create and modify a to-do list. The final product will work in the following manner.

<!-- <sample-output>

Komento: **lisaa**
Tehtävä: **käy kaupassa**
Komento: **lisaa**
Tehtävä: **imuroi**
Komento: **listaa**
1: käy kaupassa
2: imuroi
Komento: **valmis**
Mikä valmistui? **2**
Tehtävä käy kaupassa tehty
Komento: **listaa**
1: käy kaupassa
Komento: **lisaa**
Tehtävä: **ohjelmoi**
Komento: **listaa**
1: käy kaupassa
2: ohjelmoi
Komento: **lopeta**

</sample-output> -->
<sample-output>

Command: **add**
Task: **go to the store**
Command: **add**
Task: **vacuum clean**
Command: **list**
1: go to the store
2: vacuum clean
Command: **completed**
Which task was completed? **2**
Task go to the store tehty
Command: **list**
1: go to the store
Command: **add**
Task: **program**
Command: **list**
1: go to the store
2: program
Command: **stop**

</sample-output>

<!-- Tehdään sovellus osissa. -->

We'll build the program in parts.

<!-- <h2>Tehtävälista</h2> -->

<h2>ToDoList</h2>

<!-- Luo luokka `Tehtavalista`. Luokalla tulee olla parametriton konstruktori sekä seuraavat metodit:

- `public void lisaa(String tehtava)` - lisää tehtävälistalle parametrina annetun tehtävän.
- `public void tulosta()` - tulostaa tehtävät. Tulostuksessa jokaiselle tehtävällä on myös numero -- käytä tässä tehtävän indeksiä (+1).
- `public void poista(int numero)` - poistaa annettua numeroa vastaavan tehtävän; numero liittyy tulostuksessa nähtyyn tehtävän numeroon. -->

Create a class called `ToDoList`. It should have a constructor without parameters and the following methods:

- `public void add(String task)` - add the task passed as a parameter to the todo list.
- `public void print()` - prints the exercises. Each task has a number associated with it in the print statement -- use the task's index here (+1).
- `public void remove(int number)` - removes the task associated with the given number; the number is the one seen associated with the task in the print output.

<!-- ```java
Tehtavalista lista = new Tehtavalista();
lista.lisaa("lue kurssimateriaalia");
lista.lisaa("katso uusin fool us");
lista.lisaa("ota rennosti");

lista.tulosta();
lista.poista(2);

System.out.println();
lista.tulosta();
``` -->

```java
TodoList list = new TodoList();
list.add("read the course material");
list.add("watch the latest fool us");
list.add("take it easy");

list.print();
list.remove(2);

System.out.println();
list.print();
```

<!-- <sample-output>

1: lue kurssimateriaalia
2: katso uusin fool us
3: ota rennosti

1: lue kurssimateriaalia
2: ota rennosti

</sample-output> -->
<sample-output>

1: read the course material
2: watch the latest fool us
3: take it easy

1: read the course material
2: take it easy

</sample-output>

<!-- **Huom!** Voit olettaa, että metodille `poista` syötetään oikea tehtävää vastaava numero. Metodin tarvitsee toimia oikein vain kerran kunkin tulostuskutsun jälkeen. -->

**NB!** You may assume that the `remove` method is given a number that corresponds to a real task. The method has to work correctly only once after each print call.

<!-- Toinen esimerkki: -->

Another example:

<!-- ```java
Tehtavalista lista = new Tehtavalista();
lista.lisaa("lue kurssimateriaalia");
lista.lisaa("katso uusin fool us");
lista.lisaa("ota rennosti");
lista.tulosta();
lista.poista(2);
lista.tulosta();
lista.lisaa("osta rusinoita");
lista.tulosta();
lista.poista(1);
lista.poista(1);
lista.tulosta();
``` -->

```java
TodoList list = new TodoList();
list.add("read the course material");
list.add("watch the latest fool us");
list.add("take it easy");
list.print();
list.remove(2);
list.print();
list.add("buy rasins");
list.print();
list.remove(1);
list.remove(1);
list.print();
```

<!-- <sample-output>

1: lue kurssimateriaalia
2: katso uusin fool us
3: ota rennosti
1: lue kurssimateriaalia
2: ota rennosti
1: lue kurssimateriaalia
2: ota rennosti
3: osta rusinoita
1: osta rusinoita

</sample-output> -->
<sample-output>

1: read the course material
2: watch the latest fool us
3: take it easy
1: read the course material
2: take it easy
1: read the course material
2: take it easy
3: buy rasins
1: buy rasins

</sample-output>

<!-- <h2>Käyttöliittymä</h2> -->

<h2>User Interface</h2>

<!-- Toteuta seuraavaksi luokka `Kayttoliittyma`. Luokalla `Kayttoliittyma` tulee olla kaksiparametrinen konstruktori. Ensimmäisenä parametrina annetaan luokan `Tehtavalista` ilmentymä ja toisena parametrina luokan `Scanner` ilmentymä. Konstruktorin lisäksi luokalla tulee olla metodi `public void kaynnista()`, joka käynnistää tekstikäyttöliittymän. Tekstikäyttöliittymä toteutetaan ikuisen toistolauseen (`while-true`) avulla, ja sen tulee tarjota seuraavat komennot:

- Komento `lopeta` lopettaa toistolauseen suorituksen, jonka jälkeen ohjelman suoritus palaa `kaynnista`-metodista.
- Komento `lisaa` kysyy käyttäjältä lisättävää tehtävää. Kun käyttäjä syöttää lisättävän tehtävän, tulee se lisätä tehtävälistalle.
- Komento `listaa` tulostaa kaikki tehtävälistalla olevat tehtävät.
- Komento `poista` kysyy käyttäjältä poistettavan tehtävän tunnusta ja poistaa käyttäjän syöttämää tunnusta vastaavan tehtävän tehtävälistalta.

Alla on esimerkki sovelluksen toiminnasta. -->

Next, implement a class called `UserInterface`. It should have a constructor with two parameters. The first parameter is an instance of the class `ToDoList`, and the second is an instance of the class `Scanner`. In addition to the constructor, the class should have the method `public void start()` used to start the text user interface (UI). The text UI is implemented using an infite loop (`while-true`), and it must offer the following commands:

- The command `stop` stops the execution of the loop, after which the execution of the program advances out of the `start` method.

- The command `add` asks the user for the next task to be added. Once the user enters this task, it should be added to the to-do list.

- The commmand `list` prints all the tasks on the to-do list.

- The command `remove` asks the user to enter the id of the task to be removed. When this has been entered, the specified task should be removed from the list of tasks.

Below is an example of how the program should work.

<!-- <sample-output>

Komento: **lisaa**
Lisättävä: **kirjoita essee**
Komento: **lisaa**
Lisättävä: **lue kirja**
Komento: **listaa**
1: kirjoita essee
2: lue kirja
Komento: **poista**
Mikä poistetaan? **1**
Komento: **listaa**
1: lue kirja
Komento: **poista**
Mikä poistetaan? **1**
Komento: **listaa**
Komento: **lisaa**
Lisättävä: **lopeta**
Komento: **listaa**
1: lopeta
Komento: **lopeta**

</sample-output> -->

<sample-output>

Command: **add**
To add: **write an essay**
Command: **add**
To add: **read a book**
Command: **list**
1: write an essay
2: read a book
Command: **remove**
Which one is removed? **1**
Command: **list**
1: read a book
Command: **remove**
Which one is removed? **1**
Command: **list**
Command: **add**
To add: **stop**
Command: **list**
1: stop
Command: **stop**

</sample-output>

<!-- Huom! Käyttöliittymän tulee käyttää sille parametrina annettua tehtävälistaa ja Scanneria. -->

NB! The user interface should use the list and scanner objects that are passed as parameters to its constructor.


</programming-exercise>

<!-- ## Sovelluksesta osakokonaisuuksiin -->

## From a Whole Application to Its Constituent Parts

<!-- Tarkastellaan ohjelmaa, joka kysyy käyttäjältä koepisteitä, muuntaa ne arvosanoiksi, ja lopulta tulostaa kurssin arvosanajakauman tähtinä. Ohjelma lopettaa lukemisen kun käyttäjä syöttää tyhjän merkkijonon. Ohjelman käyttö näyttää seuraavalta: -->

Let's examine a program that asks the user to enter exam points and turns them into grades. Finally, the program prints the distribution of the grades as stars. The program stops reading inputs when the user inputs an empty string. An example program looks as follows:

<!-- <sample-output>

Syötä koepisteet: **91**
Syötä koepisteet: **98**
Syötä koepisteet: **103**
Epäkelpo luku.
Syötä koepisteet: **90**
Syötä koepisteet: **89**
Syötä koepisteet: **89**
Syötä koepisteet: **88**
Syötä koepisteet: **72**
Syötä koepisteet: **54**
Syötä koepisteet: **55**
Syötä koepisteet: **51**
Syötä koepisteet: **49**
Syötä koepisteet: **48**
Syötä koepisteet:

5: \*\*\*
4: \*\*\*
3: \*
2:
1: \*\*\*
0: \*\*

</sample-output> -->

<sample-output>

Points: **91**
Points: **98**
Points: **103**
Invalid number.
Points: **90**
Points: **89**
Points: **89**
Points: **88**
Points: **72**
Points: **54**
Points: **55**
Points: **51**
Points: **49**
Points: **48**
Points:

5: \*\*\*
4: \*\*\*
3: \*
2:
1: \*\*\*
0: \*\*

</sample-output>

<!-- Kuten lähes kaikki ohjelmat, ohjelman voi kirjoittaa yhtenä kokonaisuutena mainiin. Eräs mahdollinen toteutus on seuraavanlainen. -->

As almost all programs, this program can be written into main as one entity. Here is one possibility.

<!-- ```java
import java.util.ArrayList;
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        ArrayList<Integer> arvosanat = new ArrayList<>();

        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            int arvosana = 0;
            if (pisteet < 50) {
                arvosana = 0;
            } else if (pisteet < 60) {
                arvosana = 1;
            } else if (pisteet < 70) {
                arvosana = 2;
            } else if (pisteet < 80) {
                arvosana = 3;
            } else if (pisteet < 90) {
                arvosana = 4;
            } else {
                arvosana = 5;
            }

            arvosanat.add(arvosana);
        }

        System.out.println("");
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = 0;
            for (int saatu: arvosanat) {
                if (saatu == arvosana) {
                    tahtia++;
                }
            }

            System.out.print(arvosana + ": ");
            while (tahtia > 0) {
                System.out.print("*");
                tahtia--;
            }
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }
}
``` -->

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Program {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        ArrayList<Integer> grades = new ArrayList<>();

        while (true) {
            System.out.print("Points: ");
            String input = scanner.nextLine();
            if (input.equals("")) {
                break;
            }

            int points = Integer.valueOf(input);

            if (points < 0 || points > 100) {
                System.out.println("Invalid number.");
                continue;
            }

            int grade = 0;
            if (points < 50) {
                grade = 0;
            } else if (points < 60) {
                grade = 1;
            } else if (points < 70) {
                grade = 2;
            } else if (points < 80) {
                grade = 3;
            } else if (points < 90) {
                grade = 4;
            } else {
                grade = 5;
            }

            grades.add(grade);
        }

        System.out.println("");
        int grade = 5;
        while (grade >= 0) {
            int stars = 0;
            for (int received: grades) {
                if (received == grade) {
                    stars++;
                }
            }

            System.out.print(grade + ": ");
            while (stars > 0) {
                System.out.print("*");
                stars--;
            }
            System.out.println("");

            grade = grade - 1;
        }
    }
}
```

<!-- Pilkotaan ohjelma pienempiin osiin. Ohjelman pilkkominen tapahtuu tunnistamalla ohjelmasta vastuualueita. Arvosanojen kirjanpito, mukaanlukien pisteiden muunnos arvosanoiksi, voisi olla erillisen luokan vastuulla. Tämän lisäksi käyttöliittymälle voidaan luoda oma luokkansa. -->

Let's separate the program into smaller parts. This is done by identifying areas of responsibility within the program. Keeping track of grades, which includes the conversin of points into grades, could be the responsibility of a separate class. In addition, the user interface could have a class of its own.

<!-- ### Sovelluslogikkka -->

### Application Logic


<!-- Sovelluslogiikka sisältää ohjelman toiminnan kannalta oleellisen osat kuten tiedon säilöntätoiminnallisuuden. Edellisestä esimerkistä voidaan tunnistaa arvosanojen säilömiseen tarvittava toiminnallisuus. Eriytetään luokka `Arvosanarekisteri`, jonka vastuulle tulee kirjanpito arvosanojen lukumääristä. Arvosanarekisteriin voidaan lisätä arvosana pisteiden perusteella, jonka lisäksi arvosanarekisteristä voi kysyä kuinka moni on saanut tietyn arvosanan. -->

Application logic includes parts that are crucial for the execution of the program, such as methods of storing data. From the previous example, we're able to identify the functionality for storing the grades. Let's form a 'GradeRegister' class that's responsible for keeping count of the number of different grades that the students have received. Grades can be added to this based on points. We can also use the register to find out how many people have received a certain grade.

<!-- Luokka voi näyttää esimerkiksi seuraavalta. -->

The class could look like the following example.

<!--
```java
import java.util.ArrayList;

public class Arvosanarekisteri {

    private ArrayList<Integer> arvosanat;

    public Arvosanarekisteri() {
        this.arvosanat = new ArrayList<>();
    }

    public void lisaaArvosanaPisteidenPerusteella(int pisteet) {
        this.arvosanat.add(pisteetArvosanaksi(pisteet));
    }

    public int montakoSaanutArvosanan(int arvosana) {
        int lkm = 0;
        for (int saatu: this.arvosanat) {
            if (saatu == arvosana) {
                lkm++;
            }
        }

        return lkm;
    }

    public static int pisteetArvosanaksi(int pisteet) {

        int arvosana = 0;
        if (pisteet < 50) {
            arvosana = 0;
        } else if (pisteet < 60) {
            arvosana = 1;
        } else if (pisteet < 70) {
            arvosana = 2;
        } else if (pisteet < 80) {
            arvosana = 3;
        } else if (pisteet < 90) {
            arvosana = 4;
        } else {
            arvosana = 5;
        }

        return arvosana;
    }
}
``` -->

```java
import java.util.ArrayList;

public class GradeRegister {

    private ArrayList<Integer> grades;

    public GradeRegister() {
        this.grades = new ArrayList<>();
    }

    public void addGradeBasedOnPoints(int points) {
        this.grades.add(pointsToGrades(points));
    }

    public int numberOfGrades(int grade) {
        int count = 0;
        for (int received: this.grades) {
            if (received == grade) {
                count++;
            }
        }

        return count;
    }

    public static int pointsToGrades(int points) {

        int grade = 0;
        if (points < 50) {
            grade = 0;
        } else if (points < 60) {
            grade = 1;
        } else if (points < 70) {
            grade = 2;
        } else if (points < 80) {
            grade = 3;
        } else if (points < 90) {
            grade = 4;
        } else {
            grade = 5;
        }

        return grade;
    }
}
```

<!-- Kun arvosanarekisteri on eriytetty omaksi luokakseen, voidaan siihen liittyvä toiminnallisuus poistaa pääohjelmastamme. Pääohjelman muoto on nyt seuraavanlainen. -->

Once the grade register has been separated into a class, the functionality associated with it can be removed from our main program. The main program looks like this now.

<!-- ```java
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        Arvosanarekisteri rekisteri = new Arvosanarekisteri();

        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            rekisteri.lisaaArvosanaPisteidenPerusteella(pisteet);
        }

        System.out.println("");
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = rekisteri.montakoSaanutArvosanan(arvosana);
            System.out.print(arvosana + ": ");
            while (tahtia > 0) {
                System.out.print("*");
                tahtia--;
            }
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }
}
``` -->

```java
import java.util.Scanner;

public class Program {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        GradeRegister register = new GradeRegister();

        while (true) {
            System.out.print("Points: ");
            String input = scanner.nextLine();
            if (input.equals("")) {
                break;
            }

            int points = Integer.valueOf(input);

            if (points < 0 || points > 100) {
                System.out.println("Invalid number.");
                continue;
            }

            register.addGradeBasedOnPoints(points);
        }

        System.out.println("");
        int grade = 5;
        while (grade >= 0) {
            int stars = register.numberOfGrades(grade);
            System.out.print(grade + ": ");
            while (stars > 0) {
                System.out.print("*");
                stars--;
            }
            System.out.println("");

            grade = grade - 1;
        }
    }
}
```

<!-- Sovelluslogiikan eriyttämisestä tulee merkittävä hyöty ohjelman ylläpidettävyyden kannalta. Koska sovelluslogiikka -- tässä Arvosanarekisteri -- on erillinen luokka, voidaan sitä myös testata ohjelmasta erillisenä osana. Luokan `Arvosanarekisteri` voisi halutessaan kopioida myös muihin ohjelmiinsa. Alla on esimerkki yksinkertaisesta luokan `Arvosanarekisteri` manuaalisesta testaamisesta -- tämä kokeilu huomioi vain pienen osan rekisterin toiminnallisuudesta. -->

Separating program logic provides a significant advantage with regard to the program's maintainability. Since the program logic -- the GradeRegister in this case -- lives in its own class, it can also be tested separately from the other parts of the program. If you wanted to, you could copy the class `GradeRegister` and use it in your other programs. The example below is a simple demonstration of the GradeRegister class being manually tested -- it only concerns a small part of the register's functionality.

<!-- ```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(51);
rekisteri.lisaaArvosanaPisteidenPerusteella(50);
rekisteri.lisaaArvosanaPisteidenPerusteella(49);

System.out.println("Arvosanan 0 saaneita (pitäisi olla 1): " + rekisteri.montakoSaanutArvosanan(0));
System.out.println("Arvosanan 1 saaneita (pitäisi olla 2): " + rekisteri.montakoSaanutArvosanan(2));
``` -->

```java
GradeRegister register = new GradeRegister();
register.addGradeBasedOnPoints(51);
register.addGradeBasedOnPoints(50);
register.addGradeBasedOnPoints(49);

System.out.println("Number of students with grade 0 (should be 1): " + register.numberOfGrades(0);
System.out.println("Number of students with grade 0 (should be 2): " + register.numberOfGrades(2);
```

<!-- ### Käyttöliittymä -->

### User Interface

<!-- Käyttöliittymä on tyypillisesti sovelluskohtainen. Luodaan luokka `Kayttoliittyma` ja eriytetään se pääohjelmasta. Käyttöliittymälle annetaan parametrina arvosanarekisteri, jota käytetään arvosanojen säilömiseen, ja Scanner-olio, jota käytetään syötteen lukemiseen. -->

A user interface is typically application specific. Let's create a `UserInterface` class and separate it from the main program. The user interface's constructor receives two parameters : a grade register for storing the grades, and a Scanner object for reading input.

<!-- Kun käytössämme on käyttöliittymä, muodostuu ohjelman käynnistävästä pääohjelmasta hyvin selkeä. -->

Once we have a separate user interface, the main program which is used to initialize the whole program becomes crystal clear.

<!-- ```java
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        Arvosanarekisteri rekisteri = new Arvosanarekisteri();

        Kayttoliittyma kayttoliittyma = new Kayttoliittyma(rekisteri, lukija);
        kayttoliittyma.kaynnista();
    }
}
``` -->

```java
import java.util.Scanner;

public class Program {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        GradeRegister register = new GradeRegister();

        UserInterface userInterface = new UserInterface(register, scanner);
        userInterface.start();
    }
}
```

<!-- Tarkastellaan käyttöliittymän toteutusta. Käyttöliittymässä on oleellisesti kaksi osaa: pisteiden lukeminen sekä arvosanajakauman tulostaminen. -->

Let's have a look at how the user interface is implemented. There are two essential parts to the UI: reading the points, and printing the grade distribution.

<!-- ```java
import java.util.Scanner;

public class Kayttoliittyma {

    private Arvosanarekisteri rekisteri;
    private Scanner lukija;

    public Kayttoliittyma(Arvosanarekisteri rekisteri, Scanner lukija) {
        this.rekisteri = rekisteri;
        this.lukija = lukija;
    }

    public void kaynnista() {
        lueKoepisteet();
        System.out.println("");
        tulostaArvosanajakauma();
    }

    public void lueKoepisteet() {
    }

    public void tulostaArvosanajakauma() {
    }
}
``` -->

```java
import java.util.Scanner;

public class UserInterface {

    private GradeRegister register;
    private Scanner scanner;

    public UserInterface(GradeRegister register, Scanner scanner) {
        this.register = register;
        this.scanner = scanner;
    }

    public void start() {
        readPoints();
        System.out.println("");
        printGradeDistribution();
    }

    public void readPoints() {
    }

    public void printGradeDistribution() {
    }
}
```

<!-- Voimme kopioida koepisteiden lukemisen sekä arvosanajakauman tulostamisen lähes suoraan aiemmasta pääohjelmastamme. Alla olevassa ohjelmassa osat on kopioitu aiemmasta pääohjelmasta, jonka lisäksi tähtien tulostukseen on luotu erillinen metodi -- tämä selkiyttää arvosanojen tulostamiseen käytettävää metodia. -->

We are able to copy the code used for reading exam points and printing the grade distribution almost directly from our previous main program. In the program below, the sections have been copied from our earlier main program, and a new method for printing the stars has also been added -- this clarifies the method that is used for printing the grade distribution.

<!-- ```java
import java.util.Scanner;

public class Kayttoliittyma {

    private Arvosanarekisteri rekisteri;
    private Scanner lukija;

    public Kayttoliittyma(Arvosanarekisteri rekisteri, Scanner lukija) {
        this.rekisteri = rekisteri;
        this.lukija = lukija;
    }

    public void kaynnista() {
        lueKoepisteet();
        System.out.println("");
        tulostaArvosanajakauma();
    }

    public void lueKoepisteet() {
        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            this.rekisteri.lisaaArvosanaPisteidenPerusteella(pisteet);
        }
    }

    public void tulostaArvosanajakauma() {
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = rekisteri.montakoSaanutArvosanan(arvosana);
            System.out.print(arvosana + ": ");
            tulostaTahtia(tahtia);
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }

    public static void tulostaTahtia(int tahtia) {
        while (tahtia > 0) {
            System.out.print("*");
            tahtia--;
        }
    }
}
``` -->

```java
import java.util.Scanner;

public class UserInterface {

    private GradeRegister register;
    private Scanner scanner;

    public UserInterface(GradeRegister register, Scanner scanner) {
        this.register = register;
        this.scanner = scanner;
    }

    public void start() {
        readPoints();
        System.out.println("");
        printGradeDistribution();
    }

    public void readPoints() {
        while (true) {
            System.out.print("Points: ");
            String input = scanner.nextLine();
            if (input.equals("")) {
                break;
            }

            int points = Integer.valueOf(input);

            if (points < 0 || points > 100) {
                System.out.println("Invalid number.");
                continue;
            }

            this.register.addGradeBasedOnPoints(pisteet);
        }
    }

    public void printGradeDistribution() {
        int grade = 5;
        while (grade >= 0) {
            int stars = register.numberOfGrades(grade);
            System.out.print(grade + ": ");
            printStars(stars);
            System.out.println("");

            grade = grade - 1;
        }
    }

    public static void printStars(int stars) {
        while (stars > 0) {
            System.out.print("*");
            stars--;
        }
    }
}
```


<programming-exercise name='Averages (3 parts)' tmcname='part06-Part06_11.Averages'>

<!-- Tehtäväpohjassa on edellisessä esimerkissä rakennettu arvosanojen tallentamiseen tarkoitettu ohjelma. Tässä tehtävässä täydennät luokkaa `Arvosanarekisteri` siten, että se tarjoaa toiminnallisuuden arvosanojen ja koepisteiden keskiarvon laskemiseen. -->

The exercise base includes the program constructed previously that keeps track of grades. In this exercise, you'll extend the `GradeRegister` class so that it provides the opportunity to calculate the average of grades and exam results.


<!-- <h2>Arvosanojen keskiarvo</h2> -->

<h2>Average Grade</h2>

<!-- Lisää luokalle `Arvosanarekisteri` metodi `public double arvosanojenKeskiarvo()`, joka palauttaa arvosanojen keskiarvon. Mikäli arvosanarekisterissä ei ole yhtäkään arvosanaa, tulee metodin palauttaa luku `-1`.  Laske arvosanojen keskiarvo `arvosanat`-listaa hyödyntäen. -->

Create a method `public double averageOfGrades()` for the `GradeRegister` class. It should return the average of the grades. If the register contains no grades, the method should return `-1`. Calculate the average using the `grades` list.

<!-- Käyttöesimerkki: -->

Example:

<!-- ```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(93);
rekisteri.lisaaArvosanaPisteidenPerusteella(91);
rekisteri.lisaaArvosanaPisteidenPerusteella(92);
rekisteri.lisaaArvosanaPisteidenPerusteella(88);

System.out.println(rekisteri.arvosanojenKeskiarvo());
``` -->

```java
GradeRegister register = new GradeRegister();
register.addGradeBasedOnPoints(93);
register.addGradeBasedOnPoints(91);
register.addGradeBasedOnPoints(92);
register.addGradeBasedOnPoints(88);

System.out.println(register.averageOfGrades());
```

<sample-output>

4.75

</sample-output>



<!-- <h2>Koepisteiden keskiarvo</h2> -->

<h2>Average Points</h2>

<!-- Lisää luokalle `Arvosanarekisteri` uusi oliomuuttuja lista, johon lisäät koepisteitä aina kun luokkaa käyttävä ohjelma kutsuu metodia `lisaaArvosanaPisteidenPerusteella`. Lisää tämän jälkeen luokalle metodi `public double koepisteidenKeskiarvo()`, joka laskee ja palauttaa koepisteiden keskiarvon. Mikäli arvosanarekisteriin ei ole lisätty yhtäkään koepistettä, tulee metodin palauttaa luku `-1`. -->

Add a new list as an instance variable to the `GradeRegister` class that you add exam points to every time the method `addGradeBasedOnPoints` is called. After that, create a method `public double averageOfPoints()` that calculates and returns the average of the exam points. If there are no points added to the register, the method should return the number `-1`.

Example:

<!-- ```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(93);
rekisteri.lisaaArvosanaPisteidenPerusteella(91);
rekisteri.lisaaArvosanaPisteidenPerusteella(92);

System.out.println(rekisteri.koepisteidenKeskiarvo());
``` -->

```java
GradeRegister register = new GradeRegister();
register.addGradeBasedOnPoints(93);
register.addGradeBasedOnPoints(91);
register.addGradeBasedOnPoints(92);

System.out.println(register.averageOfPoints());
```

<sample-output>

92.0

</sample-output>


<!-- <h2>Tulostukset käyttöliittymään</h2> -->

<h2>Printing to the User Interface</h2>

<!-- Lisää lopulta edellä toteutetut metodit osaksi käyttöliittymää. Kun sovellus tulostaa arvosanajakauman, tulee sovelluksen tulostaa myös pisteiden ja arvosanojen keskiarvo. -->

Finally, add the methods implemented above to the user interface. When the program prints the grade distribution, it should also print the averages of both the points and the grades.

<!-- <sample-output>

Syötä koepisteet: **82**
Syötä koepisteet: **83**
Syötä koepisteet: **96**
Syötä koepisteet: **51**
Syötä koepisteet: **48**
Syötä koepisteet: **56**
Syötä koepisteet: **61**
Syötä koepisteet:

5: \*
4: \*\*
3:
2: \*
1: \*\*
0: \*
Koepisteiden keskiarvo: 68.14285714285714
Arvosanojen keskiarvo: 2.4285714285714284

</sample-output> -->

<sample-output>

Points: **82**
Points: **83**
Points: **96**
Points: **51**
Points: **48**
Points: **56**
Points: **61**
Points:

5: \*
4: \*\*
3:
2: \*
1: \*\*
0: \*
The average of points: 68.14285714285714
The average of grades: 2.4285714285714284

</sample-output>

</programming-exercise>

<!-- <programming-exercise name='Vitsipankki (2 osaa)' tmcname='osa06-Osa06_12.Vitsipankki'> -->
<programming-exercise name='Joke Manager (2 parts)' tmcname='part06-Part06_12.JokeManager'>

<!-- Tehtäväpohjassa on valmiina seuraava "mainiin" kirjoitettu sovellus. -->

The exercise template contains the following program that has been written in the program's "main".


<!-- ```java
Scanner lukija = new Scanner(System.in);
ArrayList<String> vitsit = new ArrayList<>();
System.out.println("Voihan vitsi!");

while (true) {
    System.out.println("Komennot:");
    System.out.println(" 1 - lisää vitsi");
    System.out.println(" 2 - arvo vitsi");
    System.out.println(" 3 - listaa vitsit");
    System.out.println(" X - lopeta");

    String komento = lukija.nextLine();

    if (komento.equals("X")) {
        break;
    }

    if (komento.equals("1")) {
        System.out.println("Kirjoita lisättävä vitsi:");
        String vitsi = lukija.nextLine();
        vitsit.add(vitsi);
    } else if (komento.equals("2")) {
        System.out.println("Arvotaan vitsi.");

        if (vitsit.isEmpty()) {
            System.out.println("Vitsit vähissä.");
        } else {
            Random arpa = new Random();
            int indeksi = arpa.nextInt(vitsit.size());
            System.out.println(vitsit.get(indeksi));
        }

    } else if (komento.equals("3")) {
        System.out.println("Tulostetaan vitsit.");
        for (String vitsi : vitsit) {
            System.out.println(vitsi);
        }
    }
}
``` -->

```java
Scanner scanner = new Scanner(System.in);
ArrayList<String> jokes = new ArrayList<>();
System.out.println("What a joke!");

while (true) {
    System.out.println("Commands:");
    System.out.println(" 1 - add a joke");
    System.out.println(" 2 - draw a joke");
    System.out.println(" 3 - list jokes");
    System.out.println(" X - stop");

    String command = scanner.nextLine();

    if (command.equals("X")) {
        break;
    }

    if (command.equals("1")) {
        System.out.println("Write the joke to be added:");
        String joke = scanner.nextLine();
        jokes.add(joke);
    } else if (command.equals("2")) {
        System.out.println("Drawing a joke.");

        if (jokes.isEmpty()) {
            System.out.println("Jokes are in short supply.");
        } else {
            Random draw = new Random();
            int index = draw.nextInt(jokes.size());
            System.out.println(jokes.get(index));
        }

    } else if (command.equals("3")) {
        System.out.println("Printing the jokes.");
        for (String joke : jokes) {
            System.out.println(joke);
        }
    }
}
```

<!-- Sovellus on käytännössä vitsipankki. Vitsipankkiin voi lisätä vitsejä, vitsipankista voi arpoa vitsejä, ja vitsipankissa olevat vitsit voidaan tulostaa. Tässä tehtävässä sovellus pilkotaan osiin ohjatusti. -->

The application is a joke bank. Jokes can be added the bank, random ones drawn from it, and the jokes may also be printed. In this exercise, the program is divided into parts in an instructed way.

<!-- <h2>Vitsipankki</h2> -->

<h2>Joke Manager</h2>

<!-- Luo luokka `Vitsipankki` ja siirrä sinne vitsien hallinnointiin liittyvä toiminnallisuus. Luokalla tulee olla parametriton konstruktori sekä seuraavat metodit:

- `public void lisaaVitsi(String vitsi)` - lisää vitsin vitsipankkiin.
- `public String arvoVitsi()` - arpoo vitsipankin vitseistä yhden vitsin ja palauttaa sen. Mikäli vitsipankissa ei ole vitsejä, palauttaa merkkijonon "Vitsit vähissä.".
- `public void tulostaVitsit()` - tulostaa vitsipankissa olevat vitsit.

Esimerkki luokan käytöstä: -->

Create a class called `JokeManager` and move the functionality to manage jokes in it. The class must have a parameterless constructor and the following methods:

- `public void addJoke(String joke)` - adds a joke to the manager.
- `public String drawJoke()` - chooses one joke at random and returns it. It there are no jokes stored in the joke manager, the method should return the string "Jokes are in short supply.".
- `public void printJokes()` - prints all the jokes stored in the joke manager.

An example of how to use the class:

<!-- ```java
Vitsipankki pankki = new Vitsipankki();
pankki.lisaaVitsi("Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.");
pankki.lisaaVitsi("Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.");

System.out.println("Arvotaan vitsejä:");
for (int i = 0; i < 5; i++) {
    System.out.println(pankki.arvoVitsi());
}

System.out.println("");
System.out.println("Tulostetaan vitsit:");
pankki.tulostaVitsit();
``` -->

```java
JokeManager manager = new JokeManager();
manager.addJoke("What is red and smells of blue paint? - Red paint.");
manager.addJoke("What is blue and smells of red paint? - Blue paint.");

System.out.println("Drawing jokes:");
for (int i = 0; i < 5; i++) {
    System.out.println(manager.drawJokes());
}

System.out.println("");
System.out.println("Drawing jokes:");
manager.printJokes();
```

<!-- Alla ohjelman mahdollinen tulostus. Huomaa, että arvotut vitsit eivät todennäköisesti tulostu alla kuvatun esimerkin mukaisesti. -->

Below is one possible output produced by the program. Notice that the jokes will probably not be drawn as in this example.

<!-- <sample-output>

Arvotaan vitsejä:
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.

Tulostetaan vitsit:
Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.

</sample-output> -->
<sample-output>

Drawing jokes:
What is blue and smells of red paint? - Blue paint.
What is red and smells of blue paint? - Red paint.
What is blue and smells of red paint? - Blue paint.
What is blue and smells of red paint? - Blue paint.
What is blue and smells of red paint? - Blue paint.

Printing jokes:
What is red and smells of blue paint? - Red paint.
What is blue and smells of red paint? - Blue paint.

</sample-output>

<!-- <h2>Käyttöliittymä</h2> -->

<h2>User Interface</h2>

<!-- Luo luokka `Kayttoliittyma` ja siirrä sinne sovelluksen käyttöliittymätoiminnallisuus. Luokalla tulee olla kaksiparametrinen konstruktori. Ensimmäisenä parametrina annetaan Vitsipankki-luokan ilmentymä, ja toisena parametrina Scanner-luokan ilmentymä. Tämän lisäksi luokalla tulee olla metodi `public void kaynnista()`, joka käynnistää käyttöliittymän.

Käyttöliittymän tulee tarjota seuraavat komennot:

- `X` - lopettaminen: poistuu metodista `kaynnista`.
- `1` - lisääminen: kysyy käyttäjältä vitsipankkiin lisättävää vitsiä ja lisää käyttäjän syöttämän vitsin vitsipankkiin.
- `2` - arpominen: arpoo vitsipankista vitsin ja tulostaa sen. Mikäli vitsipankissa ei ole vitsejä, tulostaa merkkijonon "Vitsit vähissä.".
- `3` - tulostaminen: tulostaa kaikki vitsipankissa olevat vitsit.

Esimerkki käyttöliittymän käytöstä: -->

Create a class called `UserInterface` and move the UI functionality of the program into it. The class must have a constructor with two parameters. The first parameter is an instance of the JokeManager class, and the second parameter is an instance of the Scanner class. In addition, the class should have the method `public void start()` that can be used to start the user interface.

The user interface should provide the user with the following commands:

- `X` - ending: exits the `start` method.
- `1` - adding: asks the user for the joke to be added to the joke manager, and then adds it.
- `2` - drawing: chooses a random joke from the joke manager and prints it. If there are no jokes in the manager, the string "Jokes are in short supply." will be printed.
- `3` - printing: prints all the jokes stored in the joke manager.

An example of using UI:

<!-- ```java
Vitsipankki pankki = new Vitsipankki();
Scanner lukija = new Scanner(System.in);

Kayttoliittyma liittyma = new Kayttoliittyma(pankki, lukija);
liittyma.kaynnista();
``` -->

```java
JokeManager manager = new JokeManager();
Scanner scanner = new Scanner(System.in);

UserInterface interface = new UserInterface(manager, scanner);
interface.start();
```

<!-- <sample-output>

Komennot:
1 - lisää vitsi
2 - arvo vitsi
3 - listaa vitsit
X - lopeta
**1**
Kirjoita lisättävä vitsi:
**Did you hear about the claustrophobic astronaut? -- He just needed a little space.**
Komennot:
1 - lisää vitsi
2 - arvo vitsi
3 - listaa vitsit
X - lopeta
**3**
Tulostetaan vitsit.
Did you hear about the claustrophobic astronaut? -- He just needed a little space.
Komennot:
1 - lisää vitsi
2 - arvo vitsi
3 - listaa vitsit
X - lopeta
**X**

</sample-output> -->

<sample-output>

Commands:
1 - add a joke
2 - draw a joke
3 - list jokes
X - stop
**1**
Write the joke to be added:
**Did you hear about the claustrophobic astronaut? -- He just needed a little space.**
Commands:
1 - add a joke
2 - draw a joke
3 - list jokes
X - stop
**3**
Printing the jokes.
Did you hear about the claustrophobic astronaut? -- He just needed a little space.
Commands:
1 - add a joke
2 - draw a joke
3 - list jokes
X - stop
**X**

</sample-output>

</programming-exercise>
