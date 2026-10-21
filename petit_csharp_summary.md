# PEtit C# Resumen

> You might be tempted to select Copy and Run and skip all the keystrokes. However, there are benefits to typing code yourself. Entering the code yourself reinforces memory and understanding that will help you gain insights that you wouldn't get otherwise.

## What is a programming language?

Programming languages like C# let you write instructions that you want the computer to carry out. Each programming language has its own syntax, but after learning your first programming language and attempting to learn another one, you'll quickly realize that they all share many similar concepts. A programming language's job is to allow a human to express their intent in a human-readable and understandable way. The instructions you write in a programming language are called "source code" or just "code". Software developers write code.

At this point, a developer can update and change the code, but the computer can't understand the code. The code first must be compiled into a format that the computer can understand.

## What is compilation?

A special program called a compiler converts your source code into a different format that the computer's central processing unit (CPU) can execute. When you used the `dotnet run [archivo]` Run button in the previous unit, the code you wrote was first compiled, then executed.

Why does code need to be compiled? Although most programming languages seem cryptic at first, they can be more easily understood by humans than the computer's preferred language. The CPU understands instructions that are expressed by turning thousands or millions of tiny switches either on or off. Compilers bridge these two worlds by translating your human-readable instructions into a computer-understandable set of instructions.

When the phrase is surrounded by double-quotation marks in your C# code, it's called a **literal string**. In other words, you literally wanted the characters `H, e, l, l, o,` and so on, sent to the output.

_Classes_ "own" methods; or you could say that methods live inside of a class. To visit the method, you must know which class it's in. For now, think of a class as a way to represent an object. In this case, all of the methods that operate on your output console are defined inside of the _Console class_.

The period is the **member access operator**. In other words, the dot is how you "navigate" from the class to one of its methods. The `WriteLine()` part is called a method. You can always spot a method because it has a set of parentheses after it. Some methods need input parameters, while others don't. But if you want to invoke a method, you must always use the parentheses after the method's name.

The semicolon is the end of **statement** operator. A statement is a complete instruction in C#.

Notice that the letter `b` is surrounded with single quotation marks 'b'. **Single quotes create a character literal**. Recall that using double quotation marks creates a **string** data type.

## Why emphasize data types?

The emphasis on data types is one of the key distinguishing features of C# compared to other languages like JavaScript. The designers of C# believed they can help developers avoid common software bugs by enforcing data types.

The main takeaway is that there are many data types, but you'll focus on just a few for now:

- `string` for words, phrases, or any alphanumeric data for presentation, not calculation
- `char` for a single alphanumeric character
- `int` for a whole number
- `decimal` for a number with a fractional component
- `bool` for a true/false value

## What is a variable?

A variable is a container for storing a type of value. Variables are important because their values can change, or vary, throughout the execution of a program. Variables can be assigned, read, and changed. You use variables to store values that you intend to use in your code.

To create a new variable, you must first declare the data type of the variable, and then give it a name.

```c#
string firstName;
```

Here's a few important considerations about variable names:

- Variable names can contain alphanumeric characters and the underscore character. Special characters like the hash symbol `#` (also known as the number symbol or pound symbol) or dollar symbol `$` are not allowed.
- Variable names must begin with an alphabetical letter or an underscore, not a number.
- Variable names are case-sensitive, meaning that `string Value`; and `string value`; are two different variables.
- Variable names must **not** be a C# keyword. For example, you cannot use the following variable declarations: `decimal decimal;` or `string string;`.
- Variable names should use **camel case**, which is a style of writing that uses a lower-case letter at the beginning of the first word and an upper-case letter at the beginning of each subsequent word. For example, `string thisIsCamelCase;`.
- Variable names should begin with an alphabetical letter. Developers use the underscore for a special purpose, so try to not use that for now.
- Variable names should be descriptive and meaningful in your app. Choose a name for your variable that represents the kind of data it will hold.
- Variable names should be one or more entire words appended together. Don't use contractions or abbreviations because the name of the variable (and therefore, its purpose) may be unclear to others who are reading your code.
- Variable names shouldn't include the data type of the variable. You might see some advice to use a style like `string strValue;`. That advice is no longer current.

To retrieve a value from a variable, you just use the name of the variable.Retrieving a value from a variable is also referred to as "getting the variable", or simply, a "get" operation.

## What are implicitly typed local variables?

An implicitly typed local variable is created by using the `var` keyword followed by a variable initialization.

The `var` keyword tells the C# compiler that the data type is implied by the assigned value. After the type is implied, the variable acts the same as if the actual data type had been used to declare it. The `var` keyword is used to save on keystrokes when types are lengthy or when the type is obvious from the context.

Implicitly-typed variables **must** be initialized. (the ones using `var`)

```c#
var message = "Hello World!";
message = 10.703m;

// If you run this code, you'll see the following error message.
//CS0029: Cannot implicitly convert type 'decimal' to 'string'
```

> Other programming languages use the `var` keyword differently. In C#, variables are assigned a type by the compiler regardless of whether you use the actual data type name or allow the compiler to imply the data type. In other words, the type is locked in at the time of declaration and therefore will never be able to hold values of a different data type.

### Why use the var keyword?

It's likely that if you look at a code example in a book or online, you'll see the `var` keyword used instead of the actual data type name.

The `var` keyword has an important use in C#. Many times, the type of a variable is obvious from its initialization. In those cases, it's simpler to use the `var` keyword. The `var` keyword can also be useful when planning the code for an application. When you begin developing code for a task, you may not immediately know what data type to use. Using `var` can help you develop your solution more dynamically.

## String interpolation

```cs
string projectName = "First-Project";
Console.WriteLine($@"C:\Output\{projectName}\Data");
```

In this example, the `$` symbol allows you to reference the projectName variable inside the braces, while the `@` symbol allows you to use the unescaped `\` character.

## Add code to cast results of integer division

What if you are not working with literal values? In other words, what if you need to divide two variables of type int but do not want the result truncated? In that case, you must perform a data type cast from `int` to `decimal`. Casting is one type of data conversion that instructs the compiler to temporarily treat a value as if it were a different data type.

To cast `int` to `decimal`, you add the cast operator before the value. You use the name of the data type surrounded by parentheses in front of the value to cast it. In this case, you would add `(decimal)` before the variables `first` and `second`.

```cs
int first = 7;
int second = 5;
decimal quotient = (decimal)first / (decimal)second;
Console.WriteLine(quotient);

//1.4
```

> You've seen three uses for the parenthesis operator: method invocation, order of operations and casting.

When the modulus `%` is 0, that means the dividend is divisible by the divisor.

## Order of operations

In math, **PEMDAS** is an acronym that helps students remember the order of operations. The order is:

1. **P**arentheses (whatever is inside the parenthesis is performed first)
2. **E**xponents
3. **M**ultiplication and **D**ivision (from left to right)
4. **A**ddition and **S**ubtraction (from left to right)

> Operators like `+=`, `-=`, `*=`, `++`, and `--` are known as _compound assignment_ operators because they compound some operation in addition to assigning the result to the variable. The `+=` operator is specifically termed the _addition assignment_ operator.

Increment and decrement operators perform differently depending on whether the operator is before or after the operand.

```cs
int value = 1;
value++;
Console.WriteLine("First: " + value);
Console.WriteLine($"Second: {value++}");
Console.WriteLine("Third: " + value);
Console.WriteLine("Fourth: " + (++value));

/* First: 2
Second: 2
Third: 3
Fourth: 4 */
```

Since you'll write the code once but read it many times, you should prioritize readability.

Breaking down a problem into smaller pieces is an important skill for developers. Building your code incrementally and checking your work frequently allows you to quickly develop reliable applications.
