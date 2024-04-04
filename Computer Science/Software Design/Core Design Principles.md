---
Tags: 
Created: 2024-03-05 01:36:02
---
(Links:: [[Software Design]])

- Strive towards having **deep classes**
## Information hiding and operation usability
- Minimize the *information needs* of each class
	- Prioritize information hiding
	- `private` is your default
	- `getter()` and `setter()` methods only when needed
- Focus on the *usability* of the operations of each class
	- Exposed APIs should be easily and intuitively understandable
	- When adding an operation/parameter, think if it is really needed by the rest of the system
	- Push complexity as low as possible in the class diagram hierarchy
-> **deep class** hides less relevant information/complexity and provides valuable and easily usable operations.
# Single Responsibility Principle
Design classes in such a way that there is **only a single reason to change a class**
- Leads to smaller and more cohesive classes
- Leads to less complex classes
-> Classes that are easier to understand and change

Therefore:
- Group entities that change for the same reasons
- Separate entities that change for different reasons
-> **Functional cohesion**
# Encapsulation & Immutability
## Encapsulation
> The act of keeping both the data and the computation together to limit the number of contact points between different parts of your system

- Closely related to information hiding
- Advantages:
	- Understanding a piece of code in isolation is easier
	- Using a piece of code become less error-prone
	- Changing a piece of code less likely breaks something else

Encapsulation is often violated through references that escape.

> [!example]- Escaping references
> ```mermaid
> classDiagram
> 	directions LR
> 	main --> Deck
> 	main --> ListCard
> 	Deck --> ListCard
> 	class main {
> 		deck = 
> 		cards =
> 	}
> 	class Deck {
> 		aCards =
> 	}
> ```
> ```java
> public class Card {
> 	private Rank aRank;
> 	private Suit aSuit;
> 	
> 	public Card(Ran pRank, Suit pSuit) {
> 		aRank = pRank;
> 		aSuit = pSuit;
> 	}
> 	
> 	public Rank getRank() {
> 		return aRank;
> 	}
> 	
> 	public Suit getSuit() {
> 		return aSuit;
> 	}
> }
> ```
> ```java
> public class Deck {
> 	private List\<Card> aCards = new Arraylist<>();
> 	
> 	public Deck() {
> 		// add and shuffle cards
> 	}
> 	
> 	public Card draw () {
> 		return this.aCard.remove(0);
> 	}
> 	
> 	public List\<Card> getCard() {
> 		return this.aCards:
> 	}
> }
> ```
> ```java
> Deck deck = new Deck();
> List\<Card> cards = deck.getCards();
> cars.add(new Card(Rank.ACE, Suit.HEARTS));
> ```

### How references escape
There are 3 ways in which references can escape:
- Returning a reference to an external object
- Storing an external reference internally
- Leaking a reference through a shared structure
	- Similar to the previous ones but indirect e.g. lists of lists

> [!example]
> ```java
> public class Deck {
> private List\<Card> aCards = new Arraylist<>();
> 
> 	public Deck() {
> 		// add and shuffle cards
> 	}
> 
> 	public void setCards(List\<Card> cards) {
> 		this.aCards = cards;
> 	}
> }
> 
> List\<Card> cards = new Arraylist<>();
> Deck dekc = new Deck();
> deck.setCards(cards);
> cards.add(new Card(Rank.ACE, Suit.HEARTS));
> ```
## Immutability
Objects are *immutable* if their class provides no way to change the internal state of the object after instantiation.
- Immutable class = a class that yields immutable objects

| Advantages                                                | Disadvantages                                              |
| --------------------------------------------------------- | ---------------------------------------------------------- |
| You can share information without breaking encapsulation  | You tend to create more objects                            |
| You avoid temporal method dependencies (invocation order) | Decreased performance efficiency (more garbage collection) |
| It leads to thread safety                                 |                                                            |
| It allows the caching of objects                          |                                                            |

In java primitive types and enumerations are immutable by default
- Some other cases in Java libraries (see documentation)

Ensure that all the fields of your class are:
- Either `private` and not changed by any instance method
- Or immutable by default
	- Primitive types or enumerations
	- `final`

Expose internal data consciously:
	- [[#Extended interfaces]]
	- [[#Return copies]]
	- Via dedicated design patterns

## Extended interfaces
You extend the interface of the class, i.e. its set of `public` methods, with methods returning only references to immutable objects.

> [!example]
> ```java
> public class Deck {
> 	private List\<Card> aCards = new Arraylist<>();
> 
> 	public int size() {
> 		return this.aCards.size();
> 	}
> 
> 	public Card getCard(int index) {
> 		//assuming Card is immutable 
> 		return this.aCards.get(index);
> 	}
> }
> ```

## Return copies
You internally **clone** the stored object and return the newly created copy instead of the original

```java
public class Deck {
	private List<Card> aCards = new Arraylist<>();

	public Card getCards() {
		//assuming Card is immutable
		return new Arraylist<>(this.aCards);
	}
}
```


We are trusting the implementation of the constructor of `ArrayList`. 
- Always check the documentation of the methods you are calling!
- This could have led to the same result

> [!danger]- How deep should we copy objects?
> -> Until we reach immutable referenced objects

# Avoid complexity (or design for simplicity)
> [!definition] Complexity
> Anything related to the structure of a software system that makes it **hard to understand and modify** the system.

- Encapsulate **inherent** complexity
	- Unavoidable domain complexity
- Reduce **accidental** complexity through good, simple design
	- Avoidable technical complexity introduced through suboptimal design

- **Structural complexity**: The number and strength of relationship between the structures in your program (packages, classes, methods)
- **Reading complexity**: How hard it is to read and understand the program
- **Data complexity**: The data representations and relationships between the data elements in your program
- **Decision complexity**: The complexity of the decision flows in your program
## Reducing complexity
- Structural complexity
	- Methods should do one thing and one thing only
	- Every class should have a single responsibility
	- Methods should not have side-effects
	- Minimize the depth of inheritance hierarchies
	- Avoid multiple inheritance
	- Avoid threads (parallelism) unless absolutely necessary
- Data complexity
	- Define understandable interfaces for important abstractions
	- Define abstract data types if it substantially reduces duplication
	- Avoid using floating-point numbers if possible
- Decision complexity
	- Avoid deeply nested conditional statements
	- Avoid complex conditional expressions, e.g., extract parts to functions with clearly understandable names

---
References: