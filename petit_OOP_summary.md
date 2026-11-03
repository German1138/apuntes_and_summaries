The term programming paradigm refers to a style of programming. It does not refer to a specific language, but rather it refers to the way you program. There's two ways, imperative and declarative paradigms.

So the main differences are that **imperative** tells you how to do something and **declarative** tells you what to do.

Under **imperative** programming paradigms we have Object-Oriented Pogramming (OOP)

The key characteristics of object-oriented programming include Class, Abstraction, Encapsulation, Inheritance and Polymorphism.

A class is a template or blueprint from which objects are created.

Objects are instances of classes. Objects have attributes/states and methods/behaviors. Attributes are data associated with the object while methods are actions/functions that the object can perform.

**Abstraction** separates the interface from implementation. **Encapsulation** is the process of hiding the internal implementation of an object.

**Inheritance** enables hierarchical relationships to be represented and refined. **Polymorphism** allows objects of different types to receive the same message and respond in different ways.

Object-oriented programming is best used when:

- You have multiple programmers who don’t need to understand each component
- There is a lot of code that could be shared and reused
- The project is anticipated to change often and be added to over time.

Why should you consider learning the object-oriented programming paradigm?

- Reuse of code through Inheritance.
- Flexibility through Polymorphism.
- High security with the use of data hiding (Encapsulation) and Abstraction mechanisms.
- Improved software development productivity: An object-oriented programmer can stitch new software objects to make completely new programs.
- Faster development: Reuse enables faster development.
- Lower cost of development: The reuse of software also lowers the cost of development. Typically, more effort is put into the object-oriented analysis and design (OOAD), which lowers the overall cost of development.
- Lower cost of development: The reuse of software also lowers the cost of development. Typically, more effort is put into the object-oriented analysis and design, which lowers the overall cost of development.
- Higher-quality software: Faster development of software and lower cost of development allows more time and resources to be used in the verification of the software. Although quality is dependent upon the experience of the teams, objectoriented programming tends to result in higher-quality software.

> Things are bundled as objects, containing both properties (attributes) and functionalities (methods). For example, you can have a Player object that has attributes like strength, agility, and so on, as well as methods like attack, defend, etc. It is a different way of organizing data and actions, when it makes sense to do so. There are several benefits to OOP, including inheritance (taking on the attributes and methods of a related class), composition (building an object out of two or more existing ones), and the fact that you can pass a single object back and forth without having to worry about passing many variable values (because they are bundled in the object), but there is also some overhead in setting things up that may be overkill for very simple programs.

Some of the disadvantages of object-oriented programming include:

1. Steep learning curve: The thought process involved in object-oriented programming may not be natural for some people, and it can take time to get used to it. It is complex to create programs based on interaction of objects. Some of the key programming techniques, such as inheritance and polymorphism, can be challenging to comprehend initially.
2. Larger program size: Object-oriented programs typically involve more lines of code than procedural programs.
3. Slower programs: Object-oriented programs are typically slower than procedurebased programs, as they typically require more instructions to be executed.
4. Not suitable for all types of problems: There are problems that lend themselves well to functional-programming style, logic-programming style, or procedure-based programming style, and applying object-oriented programming in those situations will not result in efficient programs.

# SOLID

Son cinco principios básicos de la programación orientada a objetos (OOP) y el diseño. Cuando estos principios se aplican en conjunto es más probable que un desarrollador cree un sistema que sea fácil de mantener y ampliar con el tiempo.

| Inicial | Acrónimo | Concepto                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S       | SRP      | [Principio de responsabilidad única](https://es.wikipedia.org/wiki/Principio_de_responsabilidad_%C3%BAnica "Principio de responsabilidad única") (_Single responsibility principle_)<br><br>la noción de que un [objeto](<https://es.wikipedia.org/wiki/Objeto_(programaci%C3%B3n)> "Objeto (programación)") solo debería tener una única razón para cambiar.                                                                                                                     |
| O       | OCP      | [Principio de abierto/cerrado](https://es.wikipedia.org/wiki/Principio_de_abierto/cerrado "Principio de abierto/cerrado") (_Open/closed principle_)<br><br>la noción de que las “entidades de software … deben estar abiertas para su extensión, pero cerradas para su modificación”.                                                                                                                                                                                             |
| L       | LSP      | [Principio de sustitución de Liskov](https://es.wikipedia.org/wiki/Principio_de_sustituci%C3%B3n_de_Liskov "Principio de sustitución de Liskov") (_Liskov substitution principle_)<br><br>la noción de que los “objetos de un programa deberían ser reemplazables por instancias de sus subtipos sin alterar el correcto funcionamiento del programa”. Véase también [diseño por contrato](https://es.wikipedia.org/wiki/Dise%C3%B1o_por_contrato "Diseño por contrato").         |
| I       | ISP      | [Principio de segregación de la interfaz](https://es.wikipedia.org/wiki/Principio_de_segregaci%C3%B3n_de_la_interfaz "Principio de segregación de la interfaz") (_Interface segregation principle_)<br><br>la noción de que “muchas interfaces cliente específicas son mejores que una interfaz de propósito general”.                                                                                                                                                            |
| D       | DIP      | [Principio de inversión de la dependencia](https://es.wikipedia.org/wiki/Principio_de_inversi%C3%B3n_de_la_dependencia "Principio de inversión de la dependencia") (_Dependency inversion principle_)<br><br>la noción de que se debe “depender de abstracciones, no depender de implementaciones”. <br>La [Inyección de Dependencias](https://es.wikipedia.org/wiki/Inyecci%C3%B3n_de_Dependencias "Inyección de Dependencias") es uno de los métodos que siguen este principio. |
