# Apex Code Standards

8Squad's Coding Conventions for the Apex language

## Background

Apex is a strongly-typed, object-oriented, proprietary programming language for the Salesforce platform. It lets you execute flow and transaction control statements in conjunction with calls to the Salesforce API. Apex borrows its syntax from Java, and functions like a database stored procedure.

Unfortunately, there is no official coding convention defined by Salesforce which the developers can follow to create Salesforce applications. This has led to developers using random coding style. If the developer has previously worked with another language they carry over the coding convention of that language to Apex. It's even more chaotic if they haven't used a programming language before. They just create their own coding style. This leads to inconsistency and becomes frustrating when you are reading an open source code or a code snippet in a blog post.

The aim of this document is to list a set of coding standards that can be used as a reference when developing applications on the Salesforce platform. These standards are heavily influenced by the official [Java Coding Convention](http://www.oracle.com/technetwork/java/codeconv-138413.html). Some of these rules have been copied verbatim from that document.

## Introduction

This document is written to have multiple levels of specificity of rules. More specific rules override less specific rules. For example, the rule for Classes specifies that class names should not have underscores or other special characters, but the rule for Test Classes specifies an underscore.

You will see references to 4-space and 8-space indentation, or single or double indentation levels. Salesforce's built-in Developer Tools uses 4 spaces as a single indentation level. 8 spaces is, thereforce, two indentation levels.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Naming Convention

### Classes

As per Object Oriented Programming language rules, class names SHOULD be nouns. They SHOULD define or describe a thing or entity. The class name SHOULD be simple and descriptive. The first letter of each word in the class name MUST be capitalised (Pascal Case). Acronyms and abbreviations SHOULD be used sparingly and economically to avoid confusion. Underscores or other special characters MUST NOT be used in a class name.  
e.g. `Account`, `RevenueDepartment`, `MinecraftVehicle`

There are a few RECOMMENDED variations for different types of classes, detailed below.

#### Batch Apex Class

The class name SHOULD be suffixed by 'Batch'.  
e.g. `AccountRefundBatch`, `ValidateOpportunityBatch`

#### Schedulable Apex Class

The class name SHOULD be suffixed by 'Schedulable'.  
e.g. `AccountRefundSchedulable`, `ValidateOpportunitySchedulable`

#### Visualforce Controller

The class name SHOULD be suffixed by `Controller`.  
e.g. `SolarEnergyController`, `NewAccountController`

#### Trigger

The main SObject trigger name SHOULD be suffixed by `Trigger`, and the handler class SHOULD be suffixed by `TriggerHandler`.  
e.g. `AccountTrigger`, `AccountTriggerHandler`

#### Test Classes

A test class SHOULD have the name of the class it is testing, suffixed by `_Test`.
e.g. `DebitAccount_Test`, `CreditAccount_Test`

#### Interfaces

Interface names follow the same rules as classes.  

Where the interface is describing the ability to be used in a certain way, the interface name SHOULD be an adjective and end with the suffix `ible` or `able`.  
e.g. `Batchable`, `Schedulable`, `Iterable`

### Variables

Variable names MUST be mixed case with a lowercase first letter, with each consecutive word starting with a capital letter (camelCase). This rule applies to instance and class variables. They MUST not start with an underscore (`_`), or a dollar sign (`$`), though both are permitted in Apex. You SHOULD avoid using special characters in variable names. Variable names SHOULD be short yet meaningful - the name of a variable SHOULD indicate the intent of its use. Single letter variable names (like `i`, `j`, `k`) MUST be avoided, unless they are used as a throwaway variable in a short loop.  
e.g. `paymentType`,  `donationContact`, `selectedAmount`

#### Maps, Sets, Lists

Variables of the type `Map`, `Set`, and `List` MUST have the suffix `Map`, `Set`, or `List` (as appropriate).
e.g. `contactIdSet`, `accountMap`, `donationList`

##### Conflicts with reserved keywords

Where a variable name conflicts with a reserved keyword, and an alternative variable name cannot be found, the variable name MAY be prefixed with `x_`. The `x` MUST be capitalised for constants.

#### Constants

Constants (i.e. variables defined as `final`) are a special type of variable and a constant name MUST be all uppercase with words separated by underscores (`_`). Any other special characters in a constant name SHOULD be avoided.  
e.g `MIN_WIDTH`, `UK_CURRENCY`

### Methods

Method names SHOULD be a verb; an action. Like a variable name, it MUST be mixed case with a lowercase first letter, and the first letter of each subsequent word capitalised.  
e.g. `getRecordTypeId`, `setPersonName`, `processDocuments`

## Code Layout and Formatting

Well-formatted code increases readability, understanding and, ultimately, maintainability of the code base. A developer should strive to use a consistent layout and format. It will make the lives of other developers (who want to read, review, or maintain your code) a lot easier. This document lists a few guidelines as to how a Salesforce developer should format code.

### Tabs VS Spaces

Each org in Salesforce has a limit of custom code (previously 3 million characters, now 6 million characters as of Summer '18). This limit *_includes_* whitespace (but excludes test classes). While it's difficult to reach this limit for smaller organisations, efforts should be made to produce code in such a way as to avoid reaching this limit in the future.

Salesforce's own web-base Developer Console formats code with 4-space indentation. In order to maintain compatibility with this, you MUST indent your code with tabs, and set your editor to a tab width of 4-spaces.

### Wrapping Lines

Traditionally in coding, a maximum line width of 80 characters has been used. With the advent of higher resolution monitors, widescreen monitors, and object-oriented programming, this may be seen as unnecessarily strict, however it helps to promote clean, readable code. You SHOULD keep to a maximum of 80 characters whever possible; lines greater than 80 characters are permitted, but lines MUST be no longer than 120 characters.

When an expression will not fit on a single line, break it according to these general principles:
* Break after a comma
* Break before an operator
* Prefer higher-level breaks to lower-level breaks
* Align the new line with the beginning of the expression at the same level on the previous line.
* If the above rules lead to code that's squished up against the right margin, then break after the opening parenthesis and before the closing parenthesis, and break the inner lines appropriately
* If the above rule leads to confusing code, just indent 8 spaces instead

#### Method Calls

Here are some examples of breaking method calls:

```java
someMethod(longExpression1, longExpression2, longExpression3,
           longExpression4, longExpression5);
```

```java
var = someMethod1(
    longExpression1,
    someMethod2(
        longExpression2,
        longExpression3
    )
);
```

```java
var = someMethod1(longExpression1,
                  someMethod2(longExpression2,
                  longExpression3));
);
```

#### Arithmetic Expression

Following are two examples of breaking an arithmetic expression. The first is RECOMMENDED, since the break occurs outside the parenthesised expression, which is at a higher level.

```java
longName1 = longName2 * (longName3 + longName4 - longName5)
            + 4 * longname6; // PREFER
```

```java
longName1 = longName2 * (longName3 + longName4
                         - longName5) + 4 * longname6; // AVOID
```

#### `if` Statements

`if` statements SHOULD be written in such a way as to avoid lengthy expressions.

Line wrapping for `if` statements SHOULD use the 8-space rule, since conventional (4 space) indentation makes it difficult to differentiate the body. For example:

```java
// DON'T USE THIS INDENTATION
if ((condition1 && condition2)
   || (condition3 && condition4)
   ||!(condition5 && condition6)) { // BAD WRAPS
   doSomethingAboutIt();            // MAKES THIS LINE EASY TO MISS
}
```

```java
// USE THIS INDENTATION INSTEAD
if (
    (condition1 && condition2)
    ||(condition3 && condition4)
    ||!(condition5 && condition6)
) {
    doSomethingAboutIt();
}
```

```java
// OR THIS INDENTATION
if ((condition1 && condition2)
        || (condition3 && condition4)
        ||!(condition5 && condition6)) {
    doSomethingAboutIt();
}
```

```java
// OR USE THIS
if ((condition1 && condition2) || (condition3 && condition4)
        ||!(condition5 && condition6)) {
    doSomethingAboutIt();
}
```

#### Ternary Expressions

Here are three acceptable ways to format ternary expressions:

```java
alpha = (aLongBooleanExpression) ? beta : gamma;
```

```java
alpha = (aLongBooleanExpression) ? beta
                                 : gamma;
```

```java
alpha = (aLongBooleanExpression)
        ? beta
        : gamma;
```

#### SOQL Queries

When they cannot fit on a single line, SOQL queries SHOULD be broken at control statements (`FROM`, `WHERE`, `LIMIT`, etc). When multiple control statements need to be grouped together (as in WHERE conditions), they SHOULD be indented such that the body of the statements match. When selecting so many fields that the SELECT statement cannot fit on a single line, the fields SHOULD be placed each on their own line, and indented one level; the SELECT statement SHOULD NOT have any fields on its line in this case. If they cannot fit on a single line, sub-queries (whether as part of the SELECT clause or in the WHERE clause) SHOULD follow the same breaking practices as top-level queries and be indented one level. Closing parentheses (`)`) and opening parentheses (`(`) of consecutive sub-queries SHOULD be on separate lines.

```java
List<SObject> = [SELECT Id FROM SObject];
```

```java
List<SObject> = [
    SELECT
        Id,
        ...
    FROM SObject
    WHERE condition
    AND condition
    AND (
            condition
        OR condition
    )
];
```

```java
  List<SObject> = [
      SELECT
          Id,
          (
              SELECT Id
              FROM Children
              WHERE condition
              OR (
                      condition
                  AND condition
              )
          ),
          (
              // another long sub-query
          )
      FROM SObject
      WHERE condition
      AND condition
      AND (
              condition
          OR condition
      )
      AND Field NOT IN (
          SELECT Field FROM OtherObject
      )
  ];
```

### Variable getters and setters

When a custom variable getter and/or setter is used, the entire getter/setter block SHOULD be broken so that it matches the formatting of any other function. Inline getters and setters, even very short ones (other than the defaults), are NOT RECOMMENDED.

```java
// default inline getter/setter is OK
public Integer myInt { get; set; }
```

```java
// AVOID THIS
public Integer myInt { get { return 1; }; set; }
```

```java
// DO THIS INSTEAD
public Integer myInt {
    get {
        return 1;
    };
    set;
}
```

### Placement

You SHOULD put variable declarations only at the beginning of blocks. A block is any code surrounded by curly braces `{` and `}`. Don't wait to declare variables until its first use; it can confuse the unwary programmer and hamper code portability.

```java
private void myMethod() {
    Integer int1 = 0;         // beginning of method block

    if (condition) {
        Integer int2 = 0;     // beginning of "if" block
        ...
    }
}
```

The one exception to the rule is indices of `for` loops, which MAY be declared in the `for` statement:

```java
for (Integer i = 0; i < maxLoops; i++) {
    ...
}
```

Avoid local declarations that hide declarations at higher levels (termed 'shadowing'). For example, do not declare the same variable name in an inner block:

```java
Integer count;
...
myMethod() {
    if (condition) {
        Integer count = 0; // AVOID!
        ...
    }
    ...
}
```

### Class and Interface Declarations:

When coding Apex classes and interfaces, the following formatting rules SHOULD be followed:
* No space between a method name and the parenthesis `(` starting its parameter list
* One space between the closing parenthesis `)` of the parameter list and the opening brace `{` of the block statement
* Open brace `{` appears at the end of the same line as the declaration statement
* Closing brace `}` starts a line by itself indented to match its corresponding opening statement - except when it is a null statement; the `}` SHOULD appear immediately after the `{`

```java
class Sample extends Object {
    Integer ivar1;
    Integer ivar2;

    Sample(Integer i, Integer j) {
        ivar1 = i;
        ivar2 = j;
    }

    Integer emptyMethod() {}
 }
```

### `if`, `if-else`, `if else-if else` Statements:

The if-else class of statements SHOULD have the following form:

```java
if (condition) {
    statements;
}
```

```java
if (condition) {
    statements;
} else {
    statements;
}
```

```java
if (condition) {
    statements;
} else if (condition) {
    statements;
} else {
    statements;
}
```

Note: `if` statements MUST always use braces `{}`. Avoid the following error-prone form:

```java
if (condition) // AVOID! THIS OMITS THE BRACES {}!
    statements;
```

### `for` Loops

A for loop SHOULD have the following format:

```java
// traditional for loop
for (initStatement; exitCondition; incrementStatement) {
    // code block
}
```

```java
// set iteration for loop
for (variable : listOrSet) {
    // code block
}
```

```java
// SOQL for loop
for (variable : [soql query]) {
    // code block
}
```

You SHOULD break a SOQL query into multiple lines if it is a long query.

```java
// SOQL for loop
for (variable : [
    SELECT Fields
    FROM SObject
    WHERE criteria
]) {
    // code block
}
```

### `try-catch` Statements:

A `try-catch` statement SHOULD have the following format:

```java
try {
    statements;
} catch (ExceptionClass e) {
    statements;
}
```

A try-catch statement MAY also be followed by finally, which executes regardless of whether or not the try block has completed successfully.

```java
try {
    statements;
} catch (ExceptionClass e) {
    statements;
} finally {
    statements;
}
```

### Salesforce Keywords

Salesforce keywords, such as DML operators and SOQL statements SHOULD be capitalised.

```java
List<Contact> conList = [SELECT Id FROM Contact WHERE condition];
```

```java
UPDATE conList;
```

```java
trigger ContactTrigger on Contact (before INSERT, before UPDATE) {
```

### Blank Lines:

Blank lines improve readability by setting off sections of code that are logically related.

Two blank lines SHOULD be used in the following circumstances:

* Between sections of a source file
* Between class and interface definitions

One blank line SHOULD be used in the following circumstances:

* Between methods
* Between the local variables in a method and its first statement
* Before a block or single-line comment
* Between logical sections inside a method to improve readability

## Common Mistakes / Anti-Patterns

Don't initialise the results of a SOQL query into a list. A SOQL query always returns a list.

```java
List<SObject> sobjList = new List<SObject>([SELECT Id FROM SObject]);
```

Don't check if a list is empty before performing DML. Performing DML on an empty list does not count towards DML limits.

```java
if (!conList.isEmpty()) {
    INSERT conList;
}
```

When querying for a record, don't run the query and check that the resulting list is not empty, and use the first result. Always iterate over the result list, or properly handle multiple returned records.


```java
// BAD
List<Contact> conList = [SELECT Id FROM Contact WHERE Conditions];
if (!conList.isEmpty()) {
    con = conList[0];
}
```

```java
// GOOD
for (Contact c : [SELECT Id FROM Contact WHERE Conditions]) {
    con = c;
}
```

Always query for the Id of an SObject in SOQL, even if you're not going to explicitly use it later.

```java
List<Contact> conList = [SELECT Id, Fields... FROM Contact];
```

Avoid instantiating an SObject and setting its properties later.

```java
// BAD
Contact con = new Contact();
con.LastName = 'Jackman';
```

```java
// GOOD
Contact con = new Contact(
  LastName = 'Jackman'
);
```

Do not prefix instance variables with `m_`.

```java
class SomeClass {
    public String m_someString;
}
```
