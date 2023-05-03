# Java Constructors and Overloading

## Learning Objectives
* Use class constructors to initialise state in a class
* Use overloading to add optional parameters to class methods
* Use overloading to perform similar logic on different types

## Class Constructors

When we've created new classes up until now we've either assigned default values to the member variables/attributes or we've changed them as we go. There is a much better way of doing this, which is to use a special Method called a **Constructor** to assign values to the variables as we create (or Construct) them.

You've actually already seen Constructors in action when we've been creating **new** instances of some of the built in objects.

For instance when we want to get user input we've been doing:

```java
Scanner input = new Scanner(System.in);
```

The part to the right of `new` is us calling the **Constructor** for the `Scanner` class and passing it a single argument, in this case `System.in`. 

Let's make a new class called Book that has the following attributes defined, but without values assigned to them:

- title - A String
- author - A String
- price - A double
- year - An int
- genre - A String
- paperback - A boolean

```java
package com.booleanuk;

public class Book {
    String title;
    String author;
    double price;
    int year;
    String genre;
    boolean paperback;
}
```

If we created an instance of the Book class now then it would have no data stored in its attributes as they are all set to null or the equivalent.

To populate our Book we need can add a `Constructor` to the `class` definition. Mainly this will be used to assign values to the member variables, either using values that are passed to it as arguments, or by running some code to assign a value. Constructors are methods on the class, which don't have a return value specified and take the name of the class as the name of the method.

So for our `Book` class the constructor will be:

```java
public Book() {
    
        }
```

Then we want to add parameters to the method call. To begin with let's add arguments for each of the member variables, the standard way to do this is to call them by the same names as the member variables (but the can actually have any names you want).

```java
public Book(String title, String author, double price, int year, String genre, boolean paperback) {
    
        }
```

Next we need to make it so that these values are assigned to the correct attributes. In order to allow us to use the same names for the arguments and the attributes we need to refer to the attributes using the `this` keyword. In practice, it is always a good idea to use `this` when referring to an attribute as it ensures we aren't inadvertently creating or updating a local variable. So in this case we would end up with the whole `Book` class looking like this:

```java
package com.booleanuk;

public class Book {
    String title;
    String author;
    double price;
    int year;
    String genre;
    boolean paperback;

    public Book(String title, String author, double price, int year, String genre, boolean paperback) {
        this.title = title;
        this.author = author;
        this.price = price;
        this.year = year;
        this.genre = genre;
        this.paperback = paperback;
    }
}
```

If I have a Main class and create an instance of book in there as follows, then outputting it to the console will sadly not show the information inside the instance of the object, but just a descriptor for the object.

```java
package com.booleanuk;

public class Main {
    public static void main(String[] args) {
        Book book = new Book("The Lord of the Rings", "JRR Tolkien", 11.99, 1954, "Fantasy", false);

        System.out.println(book);

    }
}
```

As this is such a common problem, we can make our objects output relevant details whenever we output them to the console by adding a special `toString()` method to our `Book` class which will return a String that will be used whenever we try to output a book object. If you add something like this to the class then running the original code will give it a much nicer output.

```java
    public String toString() {
        String book = "";
        book += "Title: " + this.title + "\n";
        book += "Author: " + this.author + "\n";
        book += "Price: £" + this.price + "\n";
        book += "Year: " +  this.year + "\n";
        book += "Genre: " + this.genre + "\n";
        if (paperback) {
            book += "Paperback";
        } else {
            book += "Hardback";
        }
        return book;
    }
```

## Activity

Create 2 more instances of the Book class using suitable examples and output all three books in the Main.main() method.

## Overloading

We can supply multiple Constructors for our class by giving each a unique pattern of arguments, so we might add a second constructor that just takes the title and author fields and sets everything else to a default value like this:

```java
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
        // It is up to us as the programmer how we add values to the rest of the attributes
        this.price = 0.0;
        this.year = 0;
        this.genre = "Unknown";
        this.paperback = true;
    }
```

What we couldn't do is have another constructor that only accepted the title and the genre, as this would have arguments which were two Strings and which therefore could be mistaken for the one shown above. So if we try to add this:

```java
    public Book(String title, String genre) {
        this.title = title;
        this.author = "Unknown";
        // It is up to us as the programmer how we add values to the rest of the attributes
        this.price = 0.0;
        this.year = 0;
        this.genre = genre;
        this.paperback = true;
    }
```

You will see the error `error: constructor Book(String,String) is already defined in class Book` as it only looks at the types of the arguments being passed in.

## Activity 

Create an Album class for CDs/Records that has 

- title
- artist
- year
- genre
- record company
- price
- format

As suitable attributes and multiple, overloaded constructors for different combinations of information.

## Overloading other Methods

Other methods can also be overloaded provided they have unique argument signatures. This is how Java deals with supplying default values to methods by creating unique method signatures. Lets create a new class called Account that looks like this:

```java
package com.booleanuk;

public class Account {
    int balance;

    public Account() {
        this.setBalance(0);
    }

    public Account(int amount) {
        this.setBalance(amount);
    }

    public Account(double amount) {
        this.setBalance(amount);
    }

    public void setBalance(int amount) {
        this.balance = amount;
    }

    public void setBalance(double amount) {
        this.balance = (int) (amount * 100);
    }

    public String toString() {
        return "Amount in account is: £" + (double) this.balance / 100;
    }
}
```

If we then create and access the objects as follows we get some insight into what is happening.

```java
package com.booleanuk;

public class Main {
public static void main(String[] args) {
Account blank = new Account();
Account withInt = new Account(257);
Account withDouble = new Account(15.65);

        System.out.println("Balance: " + blank.balance + " Print the object - " + blank);
        System.out.println("Balance: " + withInt.balance + " Print the object - " + withInt);
        System.out.println("Balance: " + withDouble.balance  + " Print the object - " + withDouble);
    }
}
```

We can't overload methods or constructors purely by return value as this doesn't work. Although it would work if the signatures were different as there are lots of methods which return values of a given type that match what the arguments were. You can often see these in the Maths libraries where if an argument is a float it returns a float, if it's a double it returns a double etc.

## Activtity

* Create a book library.
* The library will contain an Array of books (pick the number of books at the start and populate them as the library is created).
* Instantiate books in various ways, make sure to include overloaded constructors for each combination.
* Add methods to search for books by author, title, year, genre etc which will output each of the books that match.
* Add a method to calculate the average (mean) of all of the books in the library that have a price above 0.
* Add in any other methods that you think might be useful.
* Are there any overloaded methods you might add to the Library class?




