# Clean Code
Notes on the book: ["Clean Code - A Handbook of Agile Software Craftsmanship"](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) written by Robert C. Martin

# Contents

[Foreword](#foreword)

[Introduction](#introduction)

1. [Clean Code](#chapter-1---clean-code)
2. [Meaningful Names](#chapter-2---meaningful-names)
3. [Functions](#chapter-3---functions)
4. [Comments](#chapter-4---comments)
5. [Formatting](#chapter-5---formatting) 
6. [Objects and Data Structures](#chapter-6---objects-and-data-structures) 
7. [Error Handling](#chapter-7---error-handling) 
8. [Boundaries](#chapter-8---boundaries) 
9. [Unit Tests](#chapter-9---unit-tests)
10. [Classes](#chapter-10---classes) 
11. [Systems](#chapter-11---systems) 
12. [Emergence](#chapter-12---emergence)
13. [Concurrency](#chapter-13---concurrency)
14. [Successive Refinement](#chapter-14---successive-refinement) 
15. [JUnit Internals](#chapter-15---junit-internals) 
16. [Refactoring SerialDate](#chapter-16---refactoring-serialdate) 
17. [Smells and Heuristics](#chapter-17---smells-and-heuristics)

## Foreword
In software, **80%** or more of what we do is quaintly called *"maintenance"*: the act of repair.

Good software practice requires such discipline: *focus, presence of mind, and thinking*

The 5S philosophy comprises these concepts (Total Productive Maintenance - 1951):
* **S**eiri (organization): Knowing where things are—using approaches such as suitable naming—is crucial
* **S**eiton (tidiness): There is an old American saying: *A place for everything, and everything in its place*. A piece of code should be where you expect to find it - and, if not, you should re-factor to get it there.
* **S**eiso (cleaning): Remove unused things (comments, etc). Get rid of them
* **S**eiketsu (standardization): The group agrees about how to keep the workplace clean.
* **S**hutsuke (self-discipline): Follow the practices, frequently reflect on one's work and be willing to change

You should name a variable using the same care with which you name a first-born child.

"The code is the design" and "Simple code" are their mantras.

We are honest about the state of our code because code is never perfect.

## Introduction

Learning to write clean code is *hard work*. It requires more than just the knowledge of principles and patterns. 

You must practice it yourself, and watch yourself fail.

This book will make you work, *and work hard*. What kind of work will you be doing?

You'll be reading code, lots of code. And you will be challenged to think about what's right about that code and what's wrong with it.

This book into three parts:
* First part: The first several chapters describe the principles, patterns, and practices of writing clean code.
* Second part: It consists of several case studies of ever-increasing complexity. Each case study is an exercise in cleaning up some code of transforming code that has some problems into code that has fewer problems.
* Third part: It is a single chapter containing a list of heuristics and smells gathered while creating the case studies.

## Chapter 1 - Clean Code

### There will be code

*Programmers simply won't be needed because business people will generate programs from specifications*

=> Nonsense! We will never be rid of code, because code represents the details of the requirements

### Bad code

*It was the bad code that brought the company down.*

*Have you ever been significantly impeded by bad code?* If you are a programmer of any experience then you've felt this impediment many times.
 
So then, *why did you write it?* Were you trying to go fast? Were you in a rush?

We've all said we'd go back and clean it up later. **Later equals never.**

### Attitude

Why does this happen to code? Why does good code rot so quickly into bad code? What about the requirements? What about the schedule? What about the stupid managers and the useless marketing types? Don't they bear some of the blame? **No!**

Most managers want good code, even when they are obsessing about the schedule. They may defend the schedule and requirements with passion, but that's their job.

It's your job to defend the code with equal passion.

### The Primal Conundrum

The only way to make the deadline - * the only way to go fast * - is to keep the code as clean as possible at all times.

### The Art of Clean Code?

Let's say you believe that messy code is a significant impediment. 

*"How do I write clean code?"* It's no good trying to write clean code if you don't know what it means for code to be clean!

Clean code is a lot like painting a picture.

Able to recognize clean code from dirty code does **not mean that we know how to write clean code!**

Writing clean code requires using myriad little techniques. *This "code-sense" is the key.*

In short, a programmer who writes clean code is an artist.

### What is Clean Code?

Clean code is pleasing to read.

Clean code exhibits close attention to detail.

Clean code makes it easy for other people to enhance it.

There is a difference between *code that is easy to read* and *code that is easy to change*.

Code, without tests, is not clean.

Smaller is better. Keep it simple and orderly.

The beautiful code *makes the language look like it was made for the problem!*

### We are Authors

The ratio of time spent reading vs. writing is well over 10:1.

To write new code we need to read the old one.

## Chapter 2 - Meaningful Names

Names are everywhere in software. We name our variables, our functions, our arguments, classes, and packages...

So, we need to follow some simple rules for creating good names.

### Use Intention-Revealing Names

The name should answer all the big questions. It should tell you:
* Why it exists?
* What it does?
* How it is used?

The name *d* reveals nothing:
```java
int d; // elapsed time in days
```

We should choose a name that specifies what is being measured and the unit of that measurement:
```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

Choosing names that reveal intent can make it much easier to understand and change code.

What is the purpose of this code?
```java
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>();
	for (int[] x : theList)
		if (x[0] == 4)
			list1.add(x);
	return list1;
}
```

The code implicitly requires that we know the answers to questions such as:
* What kinds of things are in *theList*?
* What is the significance of the zeroth subscript of an item in *theList*?
* What is the significance of the value *4*?
* How would I use the list being returned?

We can improve the code:
```java
public List<int[]> getFlaggedCells() {
	List<int[]> flaggedCells = new ArrayList<int[]>();
	for (int[] cell : gameBoard)
		if (cell[STATUS_VALUE] == FLAGGED)
			flaggedCells.add(cell);
	return flaggedCells;
}
```

or

```java
// Class Cell instead of ints
// Function isFlagged to hide the magic numbers
public List<Cell> getFlaggedCells() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
	for (Cell cell : gameBoard)
		if (cell.isFlagged())
			flaggedCells.add(cell);
	return flaggedCells;
}
```

With these simple name changes, it's not difficult to understand what's going on. This is the power of choosing good names.

### Avoid Disinformation

Do not refer to a *grouping of accounts* as an *accountList* unless it’s actually a *List*.

*accountGroup* or *bunchOfAccounts* or just plain accounts would be better.

Avoid use of lower-case **L** or uppercase **O** as variable names, especially in combination. They look almost entirely like the constants one and zero.
```java
int a = l;
if ( O == l )
	a = O1;
else
	l = 01;
```

### Make Meaningful Distinctions

Consider:
```java
public static void copyChars(char a1[], char a2[]) {
	for (int i = 0; i < a1.length; i++) {
		a2[i] = a1[i];
	}
}
```

This function reads much better when *source* and *destination* are used for the argumentnames.

Noise words are redundant, avoid use words: *info, data, a, an, the*

How are the programmers supposed to know which of these functions to call?
```java
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

Distinguish names in such a way that the reader knows what the differences offer.
* *moneyAmount* is indistinguishable from *money*
* *customerInfo( is indistinguishable from *customer*
* *accountData* is indistinguishable from *account*
* *theMessage* is indistinguishable from *message*

### Use Pronounceable Names

If you can't pronounce it, you can't discuss it.

Compare
```java
class DtaRcrd102 {
	private Date genymdhms;
	private Date modymdhms;
	private final String pszqint = "102";
	/* ... */
};
```

to
```java
class Customer {
	private Date generationTimestamp;
	private Date modificationTimestamp;;
	private final String recordId = "102";
	/* ... */
};
```

### Use Searchable Names

If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name.

Compare
```java
for (int j=0; j<34; j++) {
	s += (t[j]*4)/5;
}
```

to
```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
	sum += realTaskWeeks;
}
```

### Avoid Encodings

Encoded names are seldom pronounceable and are easy to mis-type.

#### Member Prefixes

You also don't need to prefix member variables with *m_* anymore.

Compare
```java
public class Part {
	private String m_dsc; // The textual description
	void setName(String name) {
		m_dsc = name;
	}
}
```

to
```java
public class Part {
	private String description;
	void setDescription(String description) {
		this.description = description;
	}
}
```

#### Interfaces and Implementations

The interface *IShapeFactory* and implement *ShapeFactory* is bad code.
The interface *ShapeFactory* and implement *ShapeFactoryImp* is good code.

### Avoid Mental Mapping

One difference between a smart programmer and a professional programmer is that the professional understands that **clarity is king**. Professionals use their powers for good and write code that others can understand.

### Class Names

Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words like Manager, Processor, Data, or Info in the name of a class.

**A class name should not be a verb**

### Method Names

Methods should have verb or verb phrase names like postPayment, deletePage, or save.

Accessors, mutators, and predicates should be named for their value and prefixed with get, set, and is according to the javabean standard.

### Don't Be Cute

Don't use the name *whack()* to mean *kill()*

Say what you mean. Mean what you say.

### Pick One Word per Concept

A consistent lexicon is a great boon to the programmers who must use your code.

### Don't Pun
Avoid using the same word for two purposes. 

### Use Solution Domain Names

Remember that the people who read your code will be programmers. So go ahead and use computer science terms, algorithm names, pattern names, math terms, and so forth.

Choosing technical names for those things is usually the most appropriate course.

### Add Meaningful Context

You have variables named *firstName, lastName, street, houseNumber, city, state, and zipcode*. Taken together it's pretty clear that they form an address.

You can add context by using prefixes: *addrFirstName, addrLastName, addrState, and so on*. At least readers will understand that these variables are part of a larger structure. Of course, a better solution is to create a class named **Address**.

The algorithm to be made much cleaner by breaking it into many smaller functions.

### Don't Add Gratuitous Context

it is a bad idea to prefix every class: GSDAccountAddress (GSD = Gas Station Deluxe)

Shorter names are generally better than longer ones

### Final Words

Follow some of these rules and see whether you don’t improve the readability of your code.

If you are maintaining someone else’s code, use refactoring tools to help resolve these problems.

## Chapter 3 - Functions

### Small!

The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

Tunctions should be very small.

Functions should hardly ever be 20 lines long.

Each was transparently obvious. Each told a story. Each led you to the next in a compelling order. *That’s how short your functions should be!*

#### Blocks and Indenting

This implies that the blocks within *if* statements, *else* statements, *while* statements, and so on should be one line long. Probably that line should be a function call.

The indent level of a function *should not be greater than one or two*. This, of course, makes the functions easier to read and understand.

### Do One Thing

**FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY**

we write functions is to decompose a larger concept (in other words, the name of the function) into a set of steps at the next level of abstraction.

#### Sections within Functions

Functions do one thing cannot be reasonably divided into sections.

### One Level of Abstraction per Function

In order to make sure functions are doing *"one thing"*, you need to make sure that the statements within our function are all at the same level of abstraction.

Mixing levels of abstraction within a function is always confusing.

#### Reading Code from Top to Bottom: The Stepdown Rule

Making the code read like a top-down set of TO paragraphs is an effective technique for keeping the abstraction level consistent.

### Switch Statements

The example shows just one of the operations that might depend on the type of employee:
```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
	switch (e.type) {
		case COMMISSIONED:
			return calculateCommissionedPay(e);
		case HOURLY:
			return calculateHourlyPay(e);
		case SALARIED:
			return calculateSalariedPay(e);
		default:
			throw new InvalidEmployeeType(e.type);
	}
}
```

There are several problems with this function. 
* It's large, and when new employee types are added, it will grow. 
* It very clearly does more than one thing.
* It violates the Single Responsibility Principle (SRP) because there is more than one reason for it to change.
* It violates the Open Closed Principle8 (OCP) because it must change whenever new types are added.

Use *Abstract Factory Pattern* to solve above problem.

### Use Descriptive Names

The smaller and more focused a function is, the easier it is to choose a descriptive name.

Don't be afraid to make a name long. A long descriptive name is better than a short enigmatic name.

Don't be afraid to spend time choosing a name.

Be consistent with your names. For example, the names *includeSetupAndTeardownPages*, *includeSetupPages*, *includeSuiteSetupPage*, and *includeSetupPage*. The similar phraseology in those names allows the sequence to tell a story.

### Function Arguments

* The ideal number of arguments for a function is zero (niladic)
* Argument is one (monadic)
* Arguments are two (dyadic)
* Arguments are Three arguments (triadic) should be avoided where possible
* More than three (polyadic)

The difficulty of writing all the test cases to ensure that all the various combinations of arguments work properly. If there are no arguments, this is trivial.

Output arguments are harder to understand than input arguments.

#### Common Monadic Forms

There are two very common reasons to pass a single argument into a function:
* Asking a question about that argument, as in boolean fileExists(“MyFile”)
* Transforming it into something. For example, InputStream fileOpen(“MyFile”) transforms a file name String into an InputStream return value

A somewhat less common, but still very useful form for a single argument function, is an event.

#### Flag Arguments

Flag arguments are ugly. It does one thing if the flag is true and another if the flag is false!

Ex:
```java
render(boolean isSuite)
```

helps a little, but not that much. We should have split the function into two: 

```java
renderForSuite()
...
renderForSingleTest()
```

#### Dyadic Functions

A function with two arguments is harder to understand than a monadic function.

For example: *outputStream,writeField(name)* is easier to understand than *writeField(outputStream, name)*

Sometimes, two arguments are appropriate. For example: *Point p = new Point(0,0);* is perfectly reasonable.

#### Triads

Functions that take three arguments are significantly harder to understand than dyads.

**Very carefully before creating a triad.**

#### Argument Objects

Reducing the number of arguments by creating objects, example:

```java
Circle makeCircle(double x, double y, double radius); // Bad code
Circle makeCircle(Point center, double radius); // Good code
```

#### Argument Lists

The declaration of String.format as shown below is clearly dyadic.

```java
public String format(String format, Object... args)
```

So all the same rules apply.

But it would be a mistake to give them more arguments than
that:
```java
void monad(Integer... args);
void dyad(String name, Integer... args);
void triad(String name, int count, Integer... args);
```
#### Verbs and Keywords

Choosing good names for a function can go a long way toward explaining the intent of the function and the order and intent of the arguments. In the case of a monad, the function and argument should form a very nice verb/noun pair.

For example: *write(name)* is very evocative. Whatever this "name" thing is, it is being "written".

An even better name might be *writeField(name)*, which tells us that the "name" thing is a "field"

#### Output Arguments

```java
appendFooter(s); // s is notot clear: Is s an input or an output?

public void appendFooter(StringBuffer report) // This clarifies the issue, but only at the expense of checking the declaration of the function.

// it would be better for appendFooter to be invoked as
report.appendFooter();
```

### Command Query Separation

Functions should either do something or answer something, but not both.

```java
public boolean set(String attribute, String value); // Bad code when call: if (set("username", "unclebob"))...

// The solution is to separate the command from the query so that the ambiguity cannot occur.
public boolean attributeExists(String attribute);
public boolean setAttribute(String attribute, String value);

```

### Prefer Exceptions to Returning Error Codes

Returning error codes from command functions is a subtle violation of command query separation. 
```java
if (deletePage(page) == E_OK) {
	if (registry.deleteReference(page.name) == E_OK) {
		if (configKeys.deleteKey(page.name.makeKey()) == E_OK){
			logger.log("page deleted");
		} else {
			logger.log("configKey not deleted");
		}
	} else {
		logger.log("deleteReference from registry failed");
	}
} else {
	logger.log("delete failed");
	return E_ERROR;
}
```

use exceptions instead of returned error codes:
```java
try {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
	logger.log(e.getMessage());
}
```

#### Extract Try/Catch Blocks
```java
public void delete(Page page) {
	try {
		deletePageAndAllReferences(page);
	}
	catch (Exception e) {
		logError(e);
	}
}

private void deletePageAndAllReferences(Page page) throws Exception {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
	logger.log(e.getMessage());
}
```

#### Error Handling Is One Thing

Functions should do one thing. Error handing is one thing.

The keyword **try** exists in a function should be the very first word in the function and that there should be nothing after the **catch/finally** blocks.

#### The Error.java Dependency Magnet

When you use exceptions instead of error codes, then new exceptions are derivatives of the exception class. They can be added without forcing any recompilation or redeployment

### Don’t Repeat Yourself

Duplication may be the root of all evil in software. Many principles and practices have been created for the purpose of controlling or eliminating it

### Structured Programming

Some programmers follow Edsger Dijkstra's rules: *every function, and every block within a function, should have one entry and one
exit*.

Following these rules means that there should only be one *return* statement in a function, no *break* or *continue* statements in a loop, and never, ever, any *goto* statements.

### How Do You Write Functions Like This?

* You can write a function with: long, complicated, lots of indenting, nested loops, long argument lists, duplicated code...
* After that, you can refine that code, splitting out functions, changing names, eliminating duplication, shrink the methods and reorder them..., all the while keeping the tests passing.

### Conclusion

Functions are the verbs of that language, and classes are the nouns.

If you follow the rules herein, your functions will be short, well named, and nicely organized.

## Chapter 4 - Comments

To be updated...

## Chapter 5 - Formatting

### The Purpose of Formatting

Code formatting is *important*

Code formatting is about communication, and communication is the professional developer's first order of business.

### Vertical Formatting

Let's start with vertical size.

How big should a source file be? In Java, file size is closely related to class size.

Small files are usually easier to understand than large files are.

#### The Newspaper Metaphor

We would like a source file to be like a newspaper article.
* The name should be simple but explanatory.
* The topmost parts of the source file should provide the high-level concepts and algorithms.
* Detail should increase as we move downward
* We find the lowest level functions and details in the source file at the end.

#### Vertical Openness Between Concepts

Use blank lines to separate the package declaration, the import(s), and each of the functions:
```java
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
		Pattern.MULTILINE + Pattern.DOTALL
	);

	public String render() throws Exception {
		StringBuffer html = new StringBuffer("<b>");
		html.append(childHtml()).append("</b>");
		return html.toString();
	}
}
```

#### Vertical Density

Avoid useless comments

#### Vertical Distance

**Variable Declarations**: Variables should be declared as close to their usage as possible and should appear a the top of each function

Control variables for loops should usually be declared within the loop statement.
```java
public int countTestCases() {
	int count= 0;
	for (Test each : tests)
		count += each.countTestCases();
	return count;
}
```

In rare cases, a variable might be declared at the top of a block or just before a loop:
```java
...
.
for (XmlTest test : m_suite.getTests()) {
	TestRunner tr = m_runnerFactory.newTestRunner(this, test);
	tr.addListener(m_textReporter);
	m_testRunners.add(tr);

	invoker = tr.getInvoker();

	for (ITestNGMethod m : tr.getBeforeSuiteMethods()) {
		beforeSuiteMethods.put(m.getMethod(), m);
	}

	for (ITestNGMethod m : tr.getAfterSuiteMethods()) {
		afterSuiteMethods.put(m.getMethod(), m);
	}
}
...
```

**Instance variables**: should be declared at the top of the class.

**Dependent Functions**: If one function calls another, they should be vertically close, and the caller should be above the callee, if at all possible.

**Conceptual Affinity**

A group of functions perform a similar operation:
```javav
public class Assert {
	static public void assertTrue(String message, boolean condition) {
		if (!condition)
			fail(message);
	}
	static public void assertTrue(boolean condition) {
		assertTrue(null, condition);
	}
	static public void assertFalse(String message, boolean condition) {
		assertTrue(message, !condition);
	}
	static public void assertFalse(boolean condition) {
		assertFalse(null, condition);
	}
...
```

#### Vertical Ordering

That is, a function that is called should be below a function that does the calling

### Horizontal Formatting

Avoid to scroll to the right to see your source.

#### Horizontal Openness and Density

Use white space (left side and the right side) with assignment operators, Assignment statements.

Do not put spaces between the function names and the opening parenthesis.

Use comma to separate arguments within the function.

Ex:
```java
private void measureLine(String line) {
	lineCount++;
	int lineSize = line.length();
	totalChars += lineSize;
	lineWidthHistogram.addLine(lineSize, lineCount);
	recordWidestLine(lineSize);
}
```

#### Horizontal Alignment

Do not need to line up all the variable names in a set of declarations, or all the rvalues in a set of assignment statements.

#### Indentation

Without indentation, programs would be virtually unreadable by humans

### Team Rules

A team of developers should agree upon a single formatting style, and then every member of that team should use that style.

A good software system is composed of a set of documents that read nicely

## Chapter 6 - Objects and Data Structures

Keep variables private. Don't expose the with public getters an setters function.

### Data Abstraction

Hiding implementation is about abstractions!

Allow users use abstract interfaces to manipulate the essence of the data, without having to know its implementation.

Consider the example below:
```java
//Concrete Vehicle
public interface Vehicle {
	double getFuelTankCapacityInGallons();
	double getGallonsOfGasoline();
}

//Abstract Vehicle
public interface Vehicle {
	double getPercentFuelRemaining();
}
```

The first uses concrete terms to communicate the fuel level of a vehicle, whereas the second does so with the abstraction of percentage.

In both of the above cases, the second option is preferable. Do not expose the details of data.

### Data/Object Anti-Symmetry
Example with Procedural Shape
```java
public class Square {
	public Point topLeft;
	public double side;
}

public class Rectangle {
	public Point topLeft;
	public double height;
	public double width;
}

public class Circle {
	public Point center;
	public double radius;
}

public class Geometry {
	public final double PI = 3.141592653589793;
	public double area(Object shape) throws NoSuchShapeException
	{
		if (shape instanceof Square) {
			Square s = (Square)shape;
			return s.side * s.side;
		}
		else if (shape instanceof Rectangle) {
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		}
		else if (shape instanceof Circle) {
			Circle c = (Circle)shape;
			return PI * c.radius * c.radius;
		}
		throw new NoSuchShapeException();
	}
}
```

What would happen if a *perimeter()* function were added to *Geometry*. The shape classes would be unaffected! Any other classes that depended upon the shapes would also be unaffected!

However, if we add a **new shape**, we must change all the functions in Geometry to deal with it. 

This is the object-oriented solution:
```java
//Polymorphic Shapes
public interface Shape {
	public double area();
}

public class Square implements Shape {
	private Point topLeft;
	private double side;
	public double area() {
		return side*side;
	}
}

public class Rectangle implements Shape {
	private Point topLeft;
	private double height;
	private double width;
	public double area() {
		return height * width;
	}
}

public class Circle implements Shape {
	private Point center;
	private double radius;
	public final double PI = 3.141592653589793;
	public double area() {
		return PI * radius * radius;
	}
}
```

Here the *area()* method is polymorphic. No *Geometry* class is necessary. So if we add a new shape, none of the existing functions are affected!

> Procedural code (code using data structures) makes it easy to *add new functions without changing the existing data structures*. OO code, on the other hand, makes it easy to *add new classes without changing existing functions*.

The complement is also true:

> Procedural code makes it hard to *add new data structures because all the functions must change*. OO code makes it hard to *add new functions because all the classes must change*.

### The Law of Demeter

The *Law of Demeter* says a module should not know about the innards of the objects it manipulates. 

This means that an object should not expose its internal structure through accessors.

More precisely, the Law of Demeter says that a method f of class C should only call the methods of these:
* C
* An object created by f
* An object passed as an argument to f
* An object held in an instance variable of C

The following code appears to violate the Law of Demeter (among other things) because it calls the getScratchDir() function on the return value of getOptions() and then calls getAbsolutePath() on the return value of getScratchDir().
```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
#### Train Wrecks

Continue with example above, it is usually best to split them up as follows:
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

* If *ctxt, options and scratchDir* are objects is a clear violation of the Law.
* If *ctxt, options and scratchDir* are data structures with no behavior Demeter's Law does not apply.

The code had been written as follows if they are data structures:
```java
final String outputDir = ctxt.options.scratchDir.absolutePath;
```

#### Hybrids

Avoid creating hybrid structures that are half object and half data structure.

#### Hiding Structure

If *ctxt* is an object, consider this code from the same module:
```java

// Function of ctxt object
public BufferedOutputStream createScratchFileStream(className) {
	String outFile = outputDir + "/" + className.replace('.', '/') + ".class";
	FileOutputStream fout = new FileOutputStream(outFile);
	BufferedOutputStream bos = new BufferedOutputStream(fout);
}

BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

This allows *ctxt* to hide its internals and prevents the current function from having to violate the Law of Demeter.

### Data Transfer Objects

The data structure is a class with public variables and no functions (called a data transfer object, or DTO). DTOs are very useful structures, especially when communicating with databases or parsing messages from sockets, and so on.

### Conclusion

* Using objects if you sometimes want the flexibility to add new data types.
* Using data types and procedures if you sometimes want the flexibility to add new behaviors.

## Chapter 7 - Error Handling

> Error handling is important, but if it obscures logic, it's wrong.

### Use Exceptions Rather Than Return Codes

You either set an error flag or returned an error code.
```java
public class DeviceController {
	...
	public void sendShutDown() {
		DeviceHandle handle = getHandle(DEV1);
		// Check the state of the device
		if (handle != DeviceHandle.INVALID) {
			// Save the device status to the record field
			retrieveDeviceRecord(handle);
			// If not suspended, shut down
			if (record.getStatus() != DEVICE_SUSPENDED) {
				pauseDevice(handle);
				clearDeviceWorkQueue(handle);
				closeDevice(handle);
			} else {
				logger.log("Device suspended. Unable to shut down");
			}
		} else {
			logger.log("Invalid handle for: " + DEV1.toString());
		}
	}
...
}
```

Unfortunately, it's easy to forget. For this reason, it is better to throw an exception when you encounter an error. The calling code is cleaner. Its logic is not obscured by error handling.
```java
public class DeviceController {
	...
	public void sendShutDown() {
		try {
			tryToShutDown();
		} catch (DeviceShutDownError e) {
			logger.log(e);
		}
	}
	
	private void tryToShutDown() throws DeviceShutDownError {
		DeviceHandle handle = getHandle(DEV1);
		DeviceRecord record = retrieveDeviceRecord(handle);
		
		pauseDevice(handle);
		clearDeviceWorkQueue(handle);
		closeDevice(handle);
	}
	
	private DeviceHandle getHandle(DeviceID id) {
		...
		throw new DeviceShutDownError("Invalid handle for: " + id.toString());
		...
	}
...
}
```

The code is better because two concerns that were tangled, the algorithm for device shutdown and error handling, are now separated. You can look at each of those concerns and understand them independently.

### Write Your Try-Catch-Finally Statement First

Try blocks are like transactions. Your catch has to leave your program in a consistent state, no matter what happens in the try

Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests.

### Use Unchecked Exceptions

### Provide Context with Exceptions

Each exception that you throw should provide enough context to determine the source and location of an error. Mention the operation that failed and the type of failure.

### Define Exception Classes in Terms of a Caller's Needs

There are many ways to classify errors. However, when we define exception classes in an application, our most important concern should be how they are caught.

Example for all of the exceptions that the calls can throw:
```java
ACMEPort port = new ACMEPort(12);
try {
	port.open();
} catch (DeviceResponseException e) {
	reportPortError(e);
	logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
	reportPortError(e);
	logger.log("Unlock exception", e);
} catch (GMXError e) {
	reportPortError(e);
	logger.log("Device response exception");
} finally {
…
}
```

We can simplify our code considerably by wrapping the API that we are calling and making sure that it returns a common exception type:
```java
public class LocalPort {
	private ACMEPort innerPort;
	public LocalPort(int portNumber) {
		innerPort = new ACMEPort(portNumber);
	}
	public void open() {
		try {
			innerPort.open();
		} catch (DeviceResponseException e) {
			throw new PortDeviceFailure(e);
		} catch (ATM1212UnlockedException e) {
			throw new PortDeviceFailure(e);
		} catch (GMXError e) {
			throw new PortDeviceFailure(e);
		}
	}
	…
}

LocalPort port = new LocalPort(12);
try {
	port.open();
} catch (PortDeviceFailure e) {
	reportError(e);
	logger.log(e.getMessage(), e);
} finally {
…
}
```

The advantage of wrapping is: 
* Minimize your dependencies upon it
* Can choose to move to a different library in the future without much penalty
* Makes it easier to calls when you are testing your own code.

### Define the Normal Flow

Look at an example
```java
try {
	MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
	m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
	m_total += getMealPerDiem();
}
```

In this business, if meals are expensed, they become part of the total. If they aren't, the employee gets a meal per diem amount for that day. The exception clutters the logic.

Our code would look much simpler, if:
```java
public class PerDiemMealExpenses implements MealExpenses {
	public int getTotal() {
		// return the per diem default
	}
}
...
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

This is called the **SPECIAL CASE PATTERN**. You create a class or configure an object so that it handles a special case for you.

### Don't return null

In many cases, special case objects are an easy remedy. Imagine that you have code like this:
```java
List<Employee> employees = getEmployees();
if (employees != null) {
	for(Employee e : employees) {
		totalPay += e.getPay();
	}
}
```

The *getEmployees* function can return null, but does it have to? If we change getEmployee so that it returns an empty list, we can clean up the code:
```java
public List<Employee> getEmployees() {
	if( .. there are no employees .. )
		return Collections.emptyList();
}
...
List<Employee> employees = getEmployees();
for(Employee e : employees) {
	totalPay += e.getPay();
}
```

If you code this way, you will minimize the chance of NullPointerExceptions and your code will be cleaner.

### Don't Pass Null

Returning null from methods is bad, but passing null into methods is worse.
Let's look at an example:
```java
public class MetricsCalculator
{
	public double xProjection(Point p1, Point p2) {
		return (p2.x – p1.x) * 1.5;
	}
	…
}
...
// What happens when someone passes null as an argument?
calculator.xProjection(null, new Point(12, 13));
```

How can we fix it? We could create a new exception type and throw it:
```java
public class MetricsCalculator
{
	public double xProjection(Point p1, Point p2) {
		if (p1 == null || p2 == null) {
			throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
		}
		return (p2.x – p1.x) * 1.5;
	}
}
```

### Conclusion

Clean code is readable, but it must also be robust. 

We can write robust clean code if we see error handling as a separate concern, something that is viewable independently of our main logic.

## Chapter 8 - Boundaries

## Chapter 9 - Unit Tests

### The Three Laws of TDD

By now everyone knows that TDD asks us to write unit tests first, before we write production code:
* First Law: You may not write production code until you have written a failing unit test.
* Second Law: You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
* Third Law: You may not write more production code than is sufficient to pass the currently failing test. 

The tests and the production code are written together, with the tests just a few seconds ahead of the production code.

If we work this way, those tests will cover virtually all of our production code.

### Keeping Tests Clean

"Quick and dirty" was the watchword.

The moral of the story is simple: Test code is just as important as production code. It is not a second-class citizen. It requires thought, design, and care. It must be kept as clean as production code.

#### Tests Enable the -ilities

If you don't keep your tests clean, you will lose them. 

It is unit tests that keep our code flexible, maintainable, and reusable. The reason is simple. 

So having an automated suite of unit tests that cover the production code is the key to keeping your design and architecture as clean as possible. Tests enable all the *-ilities*, because tests enable change.

The dirtier your tests, the dirtier your code becomes.

### Clean Tests

What makes a clean test? Three things: *Readability, readability, and readability*.

Readability is perhaps even more important in unit tests than it is in production code.

### Single Concept per Test

Perhaps a better rule is that we want to test a single concept in each test function.

What makes tests readable? The same thing that makes all code readable: *clarity, simplicity, and density of expression*.

The **BUILD-OPERATE-CHECK** pattern is made obvious by the structure of these tests.

Each of the tests is clearly split into three parts.
* The first part builds up the test data
* The second part operates on that test data
* The third part checks that the operation yielded the expected results.

Anyone who reads these tests should be able to work out what they do very quickly, without being misled or overwhelmed by details.

### A Dual Standard

Testing runs in a test environment, not a production environment, and those two environment have very different needs.

That is the nature of the dual standard. There are things that you might never do in a production environment that are perfectly fine in a test environment. Usually, they involve issues of memory or CPU efficiency. But they never involve issues of cleanliness. 

### One Assert per Test

There is a school of thought that says that every test function in a JUnit test should have one and only one assert statement. A single conclusion is quick and easy to understand. 

We can eliminate the duplication by using the **TEMPLATE METHOD** pattern and putting the given/when parts in the base class, and the then parts in different derivatives.

The number of asserts in a test ought to be minimized.

### Single Concept per Test

Perhaps a better rule is that we want to test a single concept in each test function

### F.I.R.S.T

Clean tests follow five other rules:
* **Fast**: Tests should be fast. They should run quickly. When tests run slow, you won't want
to run them frequently.

* **Independent**: Tests should not depend on each other. One test should not set up the conditions for the next test.

* **Repeatable**: Tests should be repeatable in any environment.

* **Self-Validating**: The tests should have a boolean output.

* **Timely**: The tests need to be written in a timely fashion. Unit tests should be written just
before the production code that makes them pass.

### Conclusion

Tests are as important to the health of a project as the production code is.

Tests preserve and enhance the flexibility, maintainability, and reusability of the production code.

## Chapter 10 - Classes

### Class Organization

A class should:
* Begin with a list of variables. Public static constants, if any, should come first.
* Then private static variables, followed by private instance variables.
* *There is seldom a good reason to have a public variable*
* Public functions should follow the list of variables.
* Put the private utilities called by a public function right after the public function itself.

#### Encapsulation

We like to keep our variables and utility functions private.

Sometimes we need to make a variable or utility function protected so that it can be accessed by a test.

### Classes Should Be Small!

The first rule of classes is that they should be small. The second rule of classes is that they should be smaller than that. (same text from the *Functions* chapter)

With functions we measured size by counting physical lines. With classes we use a different measure. We count **responsibilities**

Example:
```java
public class SuperDashboard extends JFrame implements MetaDataUser
	public Component getLastFocusedComponent()
	public void setLastFocused(Component lastFocused)
	public int getMajorVersionNumber()
	public int getMinorVersionNumber()
	public int getBuildNumber()
}
```

We should also be able to write a brief description of the class in about 25 words, without using the words "if," "and," "or," or "but." -> That class has one responsibility.

#### The Single Responsibility Principle

The **Single Responsibility Principle** states that a class or module should have one, and only one reason to change. 

Classes should have one responsibility—one reason to change.

#### Cohesion

Classes should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables.

In general, the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.

In general it is neither advisable nor possible to create such maximally cohesive classes.

You should try to separate the variables and methods into two or more classes such that the new classes are more cohesive.

#### Maintaining Cohesion Results in Many Small Classes

Just the act of breaking large functions into smaller functions causes a proliferation of classes. 

If we promoted those variables to instance variables of the large class, then we could extract the code without passing any variables at all. It would be easy to break the function up into small pieces.

### Organizing for Change

The **Open-Closed Principle**: Classes should be open for extension but closed for modification. Our restructured *Sql* class is open to allow new functionality via subclassing, but we can make this change while keeping every other class closed. 

We simply drop our *UpdateSql* class in place.

We want to structure our systems so that we muck with as little as possible when we update them with new or changed features.

In an ideal system, we incorporate new features by extending the system, not by making modifications to existing code. 

## Chapter 11 - Systems
## Chapter 12 - Emergence
## Chapter 13 - Concurrency
## Chapter 14 - Successive Refinement
## Chapter 15 - JUnit Internals
## Chapter 16 - Refactoring SerialDate
## Chapter 17 - Smells and Heuristics]
