──────────────────────────────────────

             TypeScript

        Resumen Personal

        Basado en el Handbook
      + notas y ejemplos propios

           Versión 1.0
              2026

──────────────────────────────────────

# TypeScript Resumen

La idea es que sea un handbook para los conceptos básicos de TypeScript escrito en idioma coloquial y con ejemplos.

## Types

### Basicos

Primitivos:

- string ej 'hello world'
- number ej: 42
- boolean ej: true/false

Arrays:

Se puede escribir de dos formas `Type[]` o `Array<Type>` donde `Type` es string, boolean, number, etc.

- number[]
- Array<number>

#### Any

Es un Type especial que se usa cuando no queres que un valor particular cause typechecking errors. **No es buena práctica** usarlo porque justamente te salteas el typado, es como usar JavaScript nomas.

```typescript
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

Cuando no definis un Type y TypeScript no puede inferirlo por contexto lo que ocurre es que defaultea a `Any`, esto no es bueno.

### Como Typar

**Siempre** el Type va a estar a la derecha de lo "typado".

#### Variables

```typescript
let myName: string = "Alice";
```

#### Funciones

Podemos Typar el argumento poniendo `:` luego del mismo (dentro del parentesis) y también lo que retornará la function, luego del parentesis. Desde ya que si pasamos cualquier arguemnto que no sea _string_ va a resultar en un error.

```typescript
function greet(name: string): string {
  console.log("Hello, " + name.toUpperCase() + "!!");
  return name.toUpperCase();
}
```

Cabe aclarar que ponerle el Type al return es medio por preferencia de cada dev, técnicamente TypeScript ya lo infiere.

Si queremos decir que una function retorna una Promise hacemos lo siguiente:

```typescript
async function getFavoriteNumber(): Promise<number> {
  return 26;
}
```

Con las funciones flecha (o anonimas) ocurre algo parecido en cuanto a que TypeScript ya infiere el Type que estamos usando para el argument.

#### Object Types

Types pero para objetos. Más adelante hay un capitulo entero más en detalle.

Funciona bastante como un objeto aparte que solo usa los keys y en vez del value tenes el Type.

```typescript
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

También se puede escribir así pero es adelantarse al tema de las Interfaces, cosa que se ve más adelante. Sirve más que nada para que sea más legible el código.

```typescript
interface Coords {
  x: number;
  y: number;
}

function printCoord(pt: Coords) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

En los Object Types también se puede poner que las properties o keys son opcionales, usando el `?`.

```typescript
function printName(obj: { first: string; last?: string }) {
  // ...
}

// esto da bien en ambos casos porque el segundo argument esta puesto como optional

printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

Sea como fuere, cuando ponemos una property como _optional_ **tenemos** que chequear que no sea `undefined` antes de usarla.

Acá un ejemplo de como se veria ese error, como solucionarlo con un condicional y también usando syntaxis moderna de JavaScript.

```typescript
function printName(obj: { first: string; last?: string }) {
  // Error - El siguiente codigo quizá crashea si 'last' es undefined.
  console.log(obj.last.toUpperCase());

  if (obj.last !== undefined) {
    // OK, porque chequeamos que no es undefined.
    console.log(obj.last.toUpperCase());
  }

  // alternativa usando modern JavaScript syntax:
  console.log(obj.last?.toUpperCase());
}
```

### Union Types

Los Union Types son Types que son formados por dos o más Types, reprensentan valores que pueden ser cualquiera de esos Types. Nos podemos referir a ellos como union’s members. Por ejemplo acá una function que puede esperar un ID como number o string:

```typescript
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
// OK
printId(101);
// OK
printId("202");
// Error
printId({ myID: 22342 });
```

Tener en cuenta que si usamos _Union Types_ los métodos que utilicemos dentro de la function tienen que ser válidos para ambos, es decir, en la function anterior no podríamos haber utilizado `.toUpperCase()` porque solo se puede usar con valores de type _strings_. La solución sería que el código detecte cuando estamos manejando un tipo de dato u otro:

```typescript
//Acá genera error porque no manejamos el error.

function printId(id: number | string) {
  console.log(id.toUpperCase());
  //Property 'toUpperCase' does not exist on type 'string | number'.
  //Property 'toUpperCase' does not exist on type 'number'.
}

//Acá anda bien porque TypeScript detecta que manejamos el error.

function printId(id: number | string) {
  if (typeof id === "string") {
    // In this branch, id is of type 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, id is of type 'number'
    console.log(id);
  }
}

//si uno de los Types fuese un array

function welcomePeople(x: string[] | string) {
  if (Array.isArray(x)) {
    // Here: 'x' is 'string[]'
    console.log("Hello, " + x.join(" and "));
  } else {
    // Here: 'x' is 'string'
    console.log("Welcome lone traveler " + x);
  }
}
```

### Narrowing

Cuando un valor puede ser de varios Types (por ejemplo un Union Type), TypeScript necesita que comprobemos cuál estamos usando antes de acceder a propiedades o métodos específicos.

A este proceso se lo llama **Narrowing**, porque TypeScript "reduce" un Type general a uno más específico.

```typescript
function printId(id: number | string) {
  if (typeof id === "string") {
    // id es string
    console.log(id.toUpperCase());
  } else {
    // id es number
    console.log(id);
  }
}
```

Las formas más comunes de hacer Narrowing son:

- `typeof`
- `instanceof`
- `in`
- `Array.isArray()`
- Chequeos de `null` o `undefined`
- Truthy/Falsy (`if (value)`)

### Type ALIASES e Interface's

Son muy parecidos y prácticamente se pueden usar de forma libre entre los dos, pero lo que las diferencia es que no podemos añadir nuevas properties a un ALIAS pero a una INTERFACE si. Además claro dejar esto: los Type Alias PUEDEN renombrar types primitivos (string, boolean, number), las Interfaces son para OBJETOS.

La buena práctica es usar las interfaces hasta que necesites usar los types aliases.

Esto es un TYPE ALIAS

```typescript
//esto es poner un alias nuevo al ejemplo anterior

type ID = number | string;

// técnicamente esto también es legal

type UserInputSanitizedString = string;

// Se puede hacer la misma con los objetos

type Point = {
  x: number;
  y: number;
};
```

y las interfaces **son para objetos**

```typescript
interface Point {
  x: number;
  y: number;
}
```

Con las interfaces se puede hacer esto y con los aliases no:

```typescript
// INTERFACE

interface Window {
  title: string;
}

interface Window {
  ts: TypeScriptAPI;
}

const src = "Hello World";
window.ts.transpileModule(src, {});

// ALIAS
type Window = {
  title: string;
};

type Window = {
  ts: TypeScriptAPI;
};

// Error: Duplicate identifier 'Window'.
```

Ambas se pueden extender a su manera, es buena practica extender solo interfaces porque es más performático.

```typescript
// INTERFACE

interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;

// ALIAS

type Animal = {
  name: string;
};

type Bear = Animal & {
  honey: boolean;
};

const bear = getBear();
bear.name;
bear.honey;
```

### Type Assertions

> Una type assertion es decirle a TypeScript: "Confiá, sé que este valor es de este tipo".

Se usan cuando **vos sabés más que TypeScript** sobre el tipo de un valor. Es decir, le indicás explícitamente qué tipo debería tener.

Un caso muy común es cuando usamos `document.getElementById`. TypeScript solo sabe que devuelve un `HTMLElement | null`, pero nosotros sabemos que ese elemento es un, por ejemplo, `<canvas>`.

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

Las _type assertions_ son eliminadas por el compilador, por lo que **no modifican el comportamiento del código en tiempo de ejecución**.

TypeScript solo permite convertir un tipo a otro que sea más específico o más general. Esto evita conversiones imposibles como:

```typescript
const x = "hello" as number;
```

### Literal Types

> Un literal type no representa un tipo de dato (string), sino un valor específico ("hola").

Son tipos que representan **un valor exacto**, en lugar de cualquier valor de ese tipo.

Por ejemplo, podemos decir que una function solo acepta `2` o `7`, y que solo puede devolver `"hello world"` o `"fratacho"`.

Se usan mucho junto con los Union Types y también pueden combinarse con Object Types y tipos primitivos.

```typescript
function ejemplo(digito: 2 | 7): "hello world" | "fratacho" {
  if (digito === 7) {
    return "hello world";
  }

  return "fratacho";
}

ejemplo(2); // ✅
ejemplo(15); // ❌
```

#### Type Widening

Ocurre que TypeScript asume que cuando declaramos un objeto y le asignamos un valor lo más probable es que cambie, por esta razón es que deberíamos de sacarle la ambiguedad a los objetos que sabemos que no van a cambiar.

En lugar de inferir el valor literal `"GET"`, TypeScript infiere el tipo más general `string`.

Vease

```typescript
declare function handleRequest(url: string, method: "GET" | "POST"): void;

const req = {
  url: "https://example.com",
  method: "GET",
};

handleRequest(req.url, req.method);
// ❌ Argument of type 'string' is not assignable to '"GET" | "POST"'
```

En el caso de arriba ocurre exactamente esto, como el objeto `req` puede ser modificado el key `method` es de tipo `string` nada más y nuestra function handleRequest valida que solo pueda ser `"GET"` o `"POST"` los argumentos que puedan pasar.

Podemos corregirlo sacandole la ambiguedad al objeto y diciendo lo siguiente:

```typescript
const REQ = { url: "https://example.com", method: "GET" as "GET" };

// incluso podemos usar el as const, que viene a decir che estas properties son literalmente así, no strings o numbers, sino el valor que tienen puesto.

const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```

`as const` le dice a TypeScript que **todos los valores del objeto son literales y además son `readonly`**, por lo que `"GET"` deja de ser un `string` y pasa a ser el literal `"GET"`.

### strictNullChecks

Cuando está activado, `null` y `undefined` pasan a ser tipos distintos. Viene activado por defecto.

Esto obliga a comprobar que un valor existe antes de usarlo.

```ts
function print(name: string | null) {
  if (name) {
    console.log(name.toUpperCase());
  }
}
```

#### Non-null Assertion (!)

`!` le dice a TypeScript "confiá en mí, esto no es null ni undefined".

```ts
const input = document.getElementById("input")!;

//o también

ref.current!.focus();

//otro ejemplo

function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

Documentación

## Interfaces u Object Types en detalle

Usadas para describir la forma de un **objeto**.

En pocas palabras es para escribir el type que tiene un _argument_ cuando es un objeto, esto sirve tanto para la entrada (arguments) como para lo que retorna. Aca en el ejemplo podemos ver **LabeledValue** hace de interfaz para el argument de la function.
También hacemos notar que no hace falta nombrar a todos los _keys_ del objeto porque quizá en nuestra function solo necesitamos usar uno de los mismos.

```typescript
interface LabeledValue {
  label: string;
}

let myObj = { size: 10, label: "Size 10 Object" };

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label);
}

printLabel(myObj);
```

### Optional

Propiedades que puedan no estar en el objeto.

Podemos poner una interface la cual se use cuando alguno de los arguments son opcionales (not required). En este caso la interface **SquareConfig** es en la práctica opcional.

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

let mySquare = createSquare({ color: "black" });

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: "white", area: 100 };
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}
```

### Readonly property

Es para que no se puedan mutar las properties (key:values) de un objeto.

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
//Cannot assign to 'x' because it is a read-only property.
```

También se puede usar en arrays

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;

ro[0] = 12; // error!
//Index signature in type 'readonly number[]' only permits reading.

ro.push(5); // error!
//Property 'push' does not exist on type 'readonly number[]'.

ro.length = 100; // error!
//Cannot assign to 'length' because it is a read-only property.

a = ro; // error!
//The type 'readonly number[]' is 'readonly' and cannot be assigned to the mutable type 'number[]'.
Try;
```

The easiest way to remember whether to use readonly or const is to ask whether you’re using it on a variable or a property. Variables use **const** whereas properties use **readonly**.

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Germán",
};

user.name = "Juan"; // ✅

user.id = 2; // ❌ Error

user = otroUsuario; // ❌
```

### Index signature

Una index signature le dice a TypeScript: "No sé qué nombres tendrán las propiedades, pero sí sé qué tipo de valor habrá dentro de cada una".

```typescript
interface Countries {
  [key: string]: string;
}
```

"Este objeto puede tener cualquier clave de tipo string, y el valor asociado debe ser un string.":

```typescript
const countries: Countries = {
  AR: "Argentina",
  BR: "Brasil",
  CL: "Chile",
};

//esto da bien

const countries: Countries = {
  AR: "Argentina",
  BR: 123,
};

//error

//porque dijimos que todos los valores deben ser strings:
[key: string]: string
```

También podemos decir que las keys van a ser de dos types distintos:

```typescript
interface NumberOrStringDictionary {
  [index: string]: number | string;
  length: number; // ok, length is a number
  name: string; // ok, name is a string
}
```

#### Excess Property Checks

"Si me dijiste que el objeto tiene estas propiedades, no te voy a dejar agregar otras por error."

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

const square: SquareConfig = {
  color: "red",
  width: 30,
  city: "Buenos Aires", // ❌ Error
};
```

Podemos añadir una string index signature si estamos seguros que el objeto puede tener properties extras. If SquareConfig can have color and width properties with the above types, but could also have any number of other properties, then we could define it like so:

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
  [propName: string]: unknown;
}
```

Acá arriba we’re saying that SquareConfig can have any number of properties, and as long as they aren’t color or width, their types don’t matter

### Extending Types e Intersection Types

#### Extending Types

Interfaces que extienden a otras interfaces. Versiones más específicas por así decir. Así no estas repitiendo código.

En vez de tener dos interfaces separadas:

```typescript
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

//y

interface AddressWithUnit {
  name?: string;
  unit: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

//podemos hacer

interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```

También podemos unir varias interfaces

```typescript
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

interface ColorfulCircle extends Colorful, Circle {}

const cc: ColorfulCircle = {
  color: "red",
  radius: 42,
};
```

#### Intersection Types

Un poco lo mismo, sirve para combinar interfaces con el `&`.

```typescript
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

// también lo podemos mandar de forma directa en el argument:

function draw(circle: Colorful & Circle) {
  console.log(`Color was ${circle.color}`);
  console.log(`Radius was ${circle.radius}`);
}

// okay
draw({ color: "blue", radius: 42 });

// oops
draw({ color: "red", raidus: 42 });
```

#### Diferencias entre las mismas

Es sutil pero la gran diferencia es que si queremos extender una interface (Extending Types) y en la otra ya hay una property con el mismo nombre pero diferente type va a dar error. En caso de las Intersection Types va a ocurrir que ese va a compilar el código pero va a ser una **"Never"** type porque la property tiene que ser dos types al mismo tiempo.

```typescript
// extender esto da error

interface Person {
  name: string;
}
interface Person {
  name: number;
}

// la intersección da esto

interface Person1 {
  name: string;
}

interface Person2 {
  name: number;
}

type Staff = Person1 & Person2;

const staffer: Staff;
staffer.name;

//(property) name: never
```

### Generic Object Types

Sirve para no repetir codigo todo el rato, es una interface para nosotros elegir el tipo de `Type` que va a tener su contenido en casos específicos que no sepamos cual puede llegar a ser ese `Type`.

```typescript
// en vez de tener que hacer esto, que tenemos el drama de repetir interfaces.

interface NumberBox {
  contents: number;
}

interface StringBox {
  contents: string;
}

interface BooleanBox {
  contents: boolean;
}

function setContents(box: StringBox, newContents: string): void;
function setContents(box: NumberBox, newContents: number): void;
function setContents(box: BooleanBox, newContents: boolean): void;
function setContents(box: { contents: any }, newContents: any) {
  box.contents = newContents;
}

// Hacemos esto y ta. Hay que pensar a Type como un placeholder que nosotros luego remplazamos con un Type específico.

interface Box<Type> {
  contents: Type;
}

let boxA: Box<string> = { contents: "hello" };
boxA.contents;

//(property) Box<string>.contents: string
```

Incluso ese type Type puede ser una interface

```typescript
interface Box<Type> {
  contents: Type;
}

interface Apple {
  // ....
}

// Same as '{ contents: Apple }'.
type AppleBox = Box<Apple>;
```

#### The Array Type

Los Arrays también usan **Generics**.

Estas dos formas son exactamente equivalentes:

```typescript
string[]

Array<string>
```

`Array<Type>` es una interfaz genérica donde `Type` representa el tipo de dato que contendrá el array.

Por ejemplo:

```typescript
Array<number>;

Array<string>;

Array<boolean>;
```

Cada una garantiza que todos los elementos del array sean de ese Type.

Hay que pensar `Type` o los _generics_ como un placeholder que luego reemplazamos por un Type real.

```typescript
Array<Type>

// ↓

Array<string>

// ↓

string[]
```

##### The ReadonlyArray Type

Arrays que no se pueden mutar

```typescript
function doStuff(values: ReadonlyArray<string>) {
  // We can read from 'values'...
  const copy = values.slice();
  console.log(`The first value is ${values[0]}`);

  // ...but we can't mutate 'values'.
  values.push("hello!");
  //Property 'push' does not exist on type 'readonly string[]'.
}

// otro ejemplo
const roArray: ReadonlyArray<string> = ["red", "green", "blue"];
```

Tenemos la forma larga de escribirlo y la corta `ReadonlyArray<Type>` y `readonly Type[]`

#### Tuple Types

Es un **array** de longitud dos que nosotros sabemos cuál es el tipo de dato (type) en cada posición.

```typescript
//ej
type StringNumberPair = [string, number];

//ej
function doSomething(pair: [string, number]) {
  const a = pair[0];
  //const a: string;

  const b = pair[1];
  //const b: number;
}

doSomething(["hello", 42]);
```

Si tratamos de acceder a una posición [2] por ejemplo nos dará error TypeScript ya que nosotros aclaramos que era una tupla en el Type.

Se puede hacer el destructuring de JavaScript:

```typescript
function doSomething(stringHash: [string, number]) {
  const [inputString, hash] = stringHash;

  console.log(inputString);

  //const inputString: string;

  console.log(hash);

  //const hash: number;
}
```

Ocurre que las tuplas también pueden tener elementos opcionales usando `?` pero en este caso tenemos que dejarlo para el final cuando declaramos el Type. Esto cambia el lenght.

```typescript
type Either2dOr3d = [number, number, number?];

function setCoordinate(coord: Either2dOr3d) {
  const [x, y, z] = coord;

  //const z: number | undefined

  console.log(`Provided coordinates had ${coord.length} dimensions`);

  //(property) length: 2 | 3
}
```

También se les puede añadir el `readonly`, suele ser una buena practica ya que normalmente no se cambian.

```typescript
function doSomething(pair: readonly [string, number]) {
  pair[0] = "hello!";
  //Cannot assign to '0' because it is a read-only property.
}
```

## Generics

Ya veniamos viendo un poco pero esta sección va a profundizar.

La idea de los _generics_ es crear componentes reutilizables, que puedan usarse con diferentes tipos de `Type` en vez de con solo uno. Para esto por convención usamos la variable `Type` o `T`, que almacenaria el tipo en vez de valores.

```ts
function identity<Type>(arg: Type): Type {
  return arg;
}

//Cuando usamos la function hacemos lo siguiente para definir el tipo de Type.

let output = identity<string>("myString");

// Aunque técnicamente typescript ya infiere el tipo de Type por el valor del argumento que estamos pasandole a la function, o sea que esto es igual de valido que el de arriba

let output = identity("myString");
```

Tener en cuenta que si TypeScript no puede inferir el tipo de Type por contexto va a ponerlo como `undefined` y tendremos que marcarselo.

Este ejemplo hace que se entienda más todavía. La misma function funciona con distintos Types sin perder información sobre cuál es el tipo que se está usando.

```ts
function identity<T>(arg: T): T {
  return arg;
}

const a = identity(10);
// number

const b = identity("hola");
// string

const c = identity(true);
// boolean
```

> Si usaramos `Any` lo que ocurriría es que perdemos la información del tipo de type de entrada y salida, incluso en el caso anterior si usaramos `any` en vez de `Type` no sabríamos que retorna.

Es muy importante tener en cuenta que al ser generic y carecer de un `Type` específico no vamos a poder usar métodos exclusivos de ciertos tipos, véase `.length`, porque cómo hacemos para medir la longitud si el tipo que pasamos es `number`?. Más adelante veremos cómo solucionar esto usando Generic Constraints (extends).

En el caso de querer hacer una _generic function_ que tome como argument un array de cualquier tipo de `Type` y devuelva otro del mismo tipo lo haríamos así:

```ts
function loggingIdentity<Type>(arg: Type[]): Type[] {
  console.log(arg.length);
  return arg;
}

// también podemos escribirlo así

function loggingIdentity<Type>(arg: Array<Type>): Array<Type> {
  console.log(arg.length); // Array has a .length, so no more error
  return arg;
}
```

En este caso anterior podemos usar `.length` sin problemas y sin _narrowing_ porque todos los arrays tienen ese método incorporado, más allá del tipo de `Type` que sean.

### Generic Types

> Es hacer una interface generic.

Una Generic Type es simplemente una interfaz o un type alias que recibe uno o más parámetros de tipo. Esto permite reutilizar la misma definición con distintos Types, sin tener que crear una interfaz para cada uno.

```ts
interface Box<T> {
  value: T;
}

const numberBox: Box<number> = {
  value: 10,
};

const stringBox: Box<string> = {
  value: "hola",
};
```

Si hacemos una arrow function viene bien porque en la interface ya le estamos diciendo que es una generic function y queda bastante resumido.

```ts
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}

function identity<Type>(arg: Type): Type {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;

// cuando nosotros ponemos arriba que es type number en el generic interface lo que ocurre es que la function va a ser de ese type también, más que nada porque ya la interface esta chequeando que sea una function con esa forma, entrada y salida tipo number.
```

Tener en cuenta cuándo el generic es para la interface o para la function

Si el <T> está **después del nombre de la interfaz**, el generic pertenece a la interfaz.

```ts
interface Box<T> { ... }
```

Si el <T> está **justo antes de los parámetros de la function**, el generic pertenece a la function.

```ts
interface Identity {
  <T>(arg: T): T;
}
```

Este último puede ser útil si queremos tener templates de functions, porque no va a importar el type que usemos, en el otro caso siempre vamos a tener que definirlo cuando señalemos la interface.

### Generic Classes

Las clases también pueden ser genéricas.

```ts
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

El Generic (`T`) solo puede usarse en la **instancia** de la clase (las properties y métodos de cada objeto).

No puede usarse en miembros `static`, porque estos pertenecen a la clase en sí y no a cada instancia.

```ts
class Box<T> {
  value: T; // ✅

  static defaultValue: T; // ❌ Error
}
```

Si pudiéramos hacer esto, ¿qué tipo tendría `defaultValue` si existen `Box<number>` y `Box<string>` al mismo tiempo? Por eso TypeScript lo prohíbe.

> Los generics pertenecen al objeto que estoy creando, no a la clase. Es decir a `new Box<number>()` no solo a la clase `Box`.

### Generic Constraints

Es cuando sabemos qué capacidades van a tener los types que se van a usar en ese generic, por ejemplo el método `.length` no funciona en `number` pero si en `string`, pero si sabemos que nuestro generic va a ser de `arrays` sabemos a ciencia cierta que va a tener ese método habilitado.

Esto es más sencillo cuando nuestro Type es de arrays pero en casos de types primitivos podemos usar constraints (restricciones) hechas a partir de interfaces.

```ts
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length); // Now we know it has a .length property, so no more error
  return arg;
}
```

Ahora la function acepta **cualquier Type que tenga una propiedad `length`**.

No importa cuál sea el Type, siempre que cumpla con la restricción definida por la interfaz.

> `extends` en un generic no significa herencia de clases. Significa "debe cumplir con esta estructura".

### Type Parameters in Generic Constraints

> Un Generic Constraint no tiene por qué usar una interfaz; también puede depender de otro Generic.

Hasta ahora restringíamos un Generic usando una interfaz:

```ts
<T extends HasLength>
```

También podemos restringir un Generic usando otro Generic.

```ts
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}
```

En este caso:

- `Type` representa el tipo del objeto.
- `Key` representa una de sus propiedades.

La restricción `Key extends keyof Type` le dice a TypeScript que `Key` solo puede ser una propiedad existente de `Type`, evitando acceder a propiedades inexistentes.

```ts
const user = {
  name: "German",
  age: 27,
};

getProperty(user, "name"); // ✅

getProperty(user, "banana"); // ❌ Error

//otro ejemplo

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // ✅
getProperty(x, "m"); // ❌ Error
```

Segun la docu

> You can declare a type parameter that is constrained by another type parameter. For example, here we’d like to get a property from an object given its name. We’d like to ensure that we’re not accidentally grabbing a property that does not exist on the obj, so we’ll place a constraint between the two types:

### Generic Parameter Defaults

> "Un Generic Default es el equivalente a un argumento con un valor por defecto, pero aplicado a un tipo."

Al igual que las funciones pueden tener argumentos por defecto, los Generics también.

```ts
interface Box<T = string> {
  value: T;
}
```

Si no especificamos el `Type`, TypeScript usa el valor por defecto.

```ts
const box: Box = {
  value: "Hola",
};

// Equivale a:

const box: Box<string>;
```

Podemos sobrescribir el valor por defecto cuando lo necesitemos.

```ts
const box: Box<number> = {
  value: 10,
};
```

A generic parameter default follows the following rules:

- A type parameter is deemed optional if it has a default.
- Required type parameters must not follow optional type parameters.
- Default types for a type parameter must satisfy the constraint for the type parameter, if it exists.

Tener en cuenta:

- Un Generic con valor por defecto pasa a ser opcional.

```ts
interface Box<T = string> {
  value: T;
}

const box: Box = {
  value: "hola",
};
```

- Los Generics obligatorios siempre deben ir antes que los opcionales.

```ts
interface Example<T = string, U> {} // error

interface Example<T, U = string> {} // ✅
```

- Si el Generic tiene una restricción (`extends`), el valor por defecto también debe cumplirla.

```ts
interface HasLength {
  length: number;
}

interface Box<T extends HasLength = string> {} // ✅

interface Box<T extends HasLength = number> {} // error
```

## Types from Types

Typescript permite expresar types en terminos de otros types, la forma más simple de esta idea es los _generics_ que vimos antes.

Adicionalmente tenemos una variedad de type operators disponibles.

Combinando varios operadores podemos expresar operaciones complejas y valores en una forma mantenible. En esta sección vamos a cubrir formas de expresar un type en términos de un type o valor existente.

- **Generics** - Types which take parameters.
- **Keyof Type Operator** - Using the `keyof` operator to create new types
- **Typeof Type Operator** - Using the `typeof` operator to create new types
- **Indexed Access Types** - Using `Type['a']` syntax to access a subset of a type
- **Conditional Types** - Types which act like an `if statements` in the type system
- **Mapped Types** - Creating types by mapping each property in an existing type
- **Template Literal Types** - Mapped types which change properties via template literal strings

### Keyof Type Operator

`keyof` obtiene las keys de un tipo y las convierte en un Union Type. O sea, lee todas las propiedades del objeto y crea un Union Type con sus nombres.

```ts
type User = {
  name: string;
  age: number;
  active: boolean;
};

type UserKeys = keyof User;

type UserKeys = "name" | "age" | "active";
```

Se usa principalmente para evitar acceder a propiedades inexistentes y suele combinarse con Generics.

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

// T es el objeto
// keyof T son todas las keys del objeto
// K extends keyof T --> `K` solo puede ser una propiedad existente de `T`.

const user = {
  name: "German",
  age: 27,
};

getProperty(user, "name"); // OK

getProperty(user, "age"); // OK

getProperty(user, "banana"); // Error
```

Esto también funciona con _interface_'s.

> `keyof` pregunta "¿qué propiedades tiene este tipo?" y responde con un Union Type.

### Typeof Type Operator

- **JavaScript:** obtiene el tipo de un valor en tiempo de ejecución (`"string"`, `"number"`, `"object"`, etc.).
- **TypeScript:** obtiene el **Type** de una variable o function para reutilizarlo sin tener que volver a escribirlo.

`typeof` obtiene el Type de una variable o function ya existente.

```ts
const user = {
  name: "German",
  age: 27,
};

type User = typeof user;

// o sea que el type de User es

{
  name: string;
  age: number;
}
```

También puede usarse con funciones.

```ts
function greet(name: string) {
  return `Hola ${name}`;
}

type Greet = typeof greet;
// (name: string) => string
```

#### ReturnType<T>

`ReturnType<T>` obtiene el Type que **retorna** una function. Suele combinarse con `typeof`.

```ts
function getUser() {
  return {
    name: "German",
    age: 27,
  };
}

type User = ReturnType<typeof getUser>;

//En este caso `User` será:

{
  name: string;
  age: number;
}
```

Generalmente se combina con `typeof`, ya que `ReturnType` espera recibir el **Type de una function**, no la function en sí.

La relación es la siguiente:

function || getUser

↓

typeof || typeof getUser

↓

Tipo de la function || () => User

↓

ReturnType

↓

Tipo del retorno || User

### Indexed Access Types

Es acceder al type de un key específico en un type object.

```ts
interface Person {
  age: number;
  name: string;
  alive: boolean;
}

type Age = Person["age"];

// el type de Age es 'number'
```

Solo podemos indexar usando types, no variables o constantes.

Podemos hacer las siguientes cosas también

```ts
type I1 = Person["age" | "name"];

//type I1 = string | number

type I2 = Person[keyof Person];

//type I2 = string | number | boolean

//Lo siguiente es usando un type alias que es un union type.

type AliveOrName = "alive" | "name";
type I3 = Person[AliveOrName];

// type I3 = string | boolean
```

Otra cosa extraña que podemos hacer es obtener el type de los elementos de un array usando "number" para indexar, todo combinado con typeof.

> `number` en este caso es "dame el tipo de cualquier elemento de este array".

```ts
const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
  { name: "Eve", age: 38 },
];

type Person = (typeof MyArray)[number];

/* {
  name: string;
  age: number;
}; */

type Age = (typeof MyArray)[number]["age"];

//type Age = number;

type Age2 = Person["age"];

//type Age2 = number;
```

### Conditional Types

> Un Conditional Type es un `if` para Types.

Es un operador ternario usando los extends de los types y viendo si es truthy o falsy.

```ts
SomeType extends OtherType ? TrueType : FalseType

//o sea

interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}

type Example1 = Dog extends Animal ? number : string;

// en este caso como Dog extiende a Animal el type Example1 es tipo number.

// otro ejemplo

type IsString<T> =
    T extends string
        ? true
        : false;

type A = IsString<string>; //TRUE
type B = IsString<number>; //FALSE
```

Se usan mucho junto con Generics para que un Type cambie automáticamente dependiendo del Type recibido.

```ts
type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel;

type A = NameOrId<number>; // daria IdLabel
type B = NameOrId<string>; // daria NameLabel
```

#### infer

`infer` permite que TypeScript descubra automáticamente un Type dentro de otro Type.

Por ejemplo, podemos obtener el Type de retorno de una function.

```ts
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

Si hacemos:

```ts
function greet() {
  return "Hola";
}

type Result = GetReturnType<typeof greet>;
```

TypeScript infiere que `R` es `string`, por lo que:

```ts
type Result = string;
```

`ReturnType<T>` está implementado usando este mismo concepto.

infer es más una herramienta para crear tus propios Utility Types

> `infer` solo puede usarse dentro del extends de un Conditional Type.

#### Distributive Conditional Types

Cuando un Conditional Type recibe un Union Type, TypeScript evalúa cada miembro del Union por separado y luego une los resultados.

```ts
type ToArray<T> = T extends any ? T[] : never;

type Result = ToArray<string | number>;

// string[] | number[]
```

Este comportamiento es el que permite implementar Utility Types como `Exclude`, `Extract` o `NonNullable`.

### Mapped Types

Son Types que se construyen a partir de otros Types recorriendo sus properties.

Internamente usan un Generic junto con `keyof` para recorrer todas las properties de un Type y generar uno nuevo.

```ts
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

//en este ejemplo `OptionsFlags` recorre todas las properties de `Features` y reemplaza el Type de cada una por `boolean`.

type Features = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<Features>;

//o sea queda asi

/* type FeatureOptions = {
    darkMode: boolean;
    newUserProfile: boolean;
}
 */
```

#### Mapping Modifiers

Se pueden aplicar dos modificadores durante el _mapping_, `readonly` y `?`, que afectan la mutabilidad y opcionalidad respectivamente.

Durante el mapping podemos agregar (`+`) o remover (`-`) los modificadores `readonly` y `?`.

```ts
// Remueve el atributo 'readonly' de la property del type.
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property];
};

type LockedAccount = {
  readonly id: string;
  readonly name: string;
};

type UnlockedAccount = CreateMutable<LockedAccount>;

//quedaria asi
/* type UnlockedAccount = {
    id: string;
    name: string;
} */

//Lo mismo con optional

// Remueve el atributo 'optional' de la property del type.
type Concrete<Type> = {
  [Property in keyof Type]-?: Type[Property];
};

type MaybeUser = {
  id: string;
  name?: string;
  age?: number;
};

type User = Concrete<MaybeUser>;

/* type User = {
    id: string;
    name: string;
    age: number;
} */
```

#### Key Remapping con _as_

Permite cambiar el nombre de las properties mientras se realiza el mapping utilizando la palabra clave `as`.

```ts
type MappedTypeWithNewProperties<Type> = {
  [Properties in keyof Type as NewKeyType]: Type[Properties];
};

//En este ejemplo se combinan Mapped Types, Template Literal Types y `Capitalize` (siguiente tema).

type Getters<Type> = {
  [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property];
};

interface Person {
  name: string;
  age: number;
  location: string;
}

type LazyPerson = Getters<Person>;

//asi quedaria
/* type LazyPerson = {
    getName: () => string;
    getAge: () => number;
    getLocation: () => string;
} */
```

También podemos filtrar properties. Si durante el `as` una property se convierte en `never`, esa property desaparece del nuevo Type.

```ts
// Remove the 'kind' property
type RemoveKindField<Type> = {
  [Property in keyof Type as Exclude<Property, "kind">]: Type[Property];
};

interface Circle {
  kind: "circle";
  radius: number;
}

type KindlessCircle = RemoveKindField<Circle>;

/* type KindlessCircle = {
    radius: number;
} */
```

Como nota también se pueden unir varios conceptos junto con los _mapped types_, por ejemplo los _conditional types_.

En el siguiente ejemplo para cada property se evalúa si su Type contiene `pii: true`; si es así el nuevo Type será `true`, de lo contrario será `false`.

```ts
type ExtractPII<Type> = {
  [Property in keyof Type]: Type[Property] extends { pii: true } ? true : false;
};

type DBFields = {
  id: { format: "incrementing" };
  name: { type: string; pii: true };
};

type ObjectsNeedingGDPRDeletion = ExtractPII<DBFields>;

/* type ObjectsNeedingGDPRDeletion = {
    id: false;
    name: true;
} */
```

> La idea principal de los **Mapped Types** es recorrer las properties de un Type y transformarlas. Durante ese proceso podemos:
>
> - Cambiar el Type de las properties.
> - Agregar o remover modificadores (`readonly`, `?`).
> - Renombrar properties (`as`).
> - Filtrarlas.
> - Combinar todo esto con Conditional Types, Utility Types, etc.

### Template Literal Types

> Los Template Literal Types toman uno o varios String Literal Types y generan automáticamente nuevos String Literal Types combinándolos.

Funcionan igual que los _template literals_ de JavaScript, pero en lugar de generar strings generan nuevos **String Literal Types**. Su principal utilidad es construir automáticamente nuevos Types a partir de otros, especialmente cuando se combinan con Union Types.

```ts
type World = "world";

type Greeting = `hello ${World}`;

// o sea
/* type Greeting = "hello world" */
```

Si dentro del Template Literal hay un Union Type, TypeScript genera automáticamente todas las combinaciones posibles.

```ts
type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";

type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;

/* type AllLocaleIDs = "welcome_email_id" | "email_heading_id" | "footer_title_id" | "footer_sendoff_id" */
```

Cada Union se combina con las demás, por lo que la cantidad de posibles Types crece rápidamente.

```ts
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
type Lang = "en" | "ja" | "pt";

type LocaleMessageIDs = `${Lang}_${AllLocaleIDs}`;

//Acá se multiplicarian por los lenguajes añadidos

/* 
type LocaleMessageIDs =
  | "en_welcome_email_id"
  | "en_email_heading_id"
  | "en_footer_title_id"
  | "en_footer_sendoff_id"
  | "ja_welcome_email_id"
  | "ja_email_heading_id"
  | "ja_footer_title_id"
  | "ja_footer_sendoff_id"
  | "pt_welcome_email_id"
  | "pt_email_heading_id"
  | "pt_footer_title_id"
  | "pt_footer_sendoff_id"; */
```

> Si las combinaciones posibles son muy grandes, suele ser preferible generar esos Types automáticamente antes de la compilación en lugar de depender de Template Literal Types.

```ts
type Event = "click" | "change";

type DOMEvent = `on${Capitalize<Event>}`;

// "onClick" | "onChange"
```

Su mayor utilidad es combinarse con `keyof` para generar automáticamente nuevos Types a partir de las properties de un objeto.

```ts
interface Person {
  firstName: string;
  lastName: string;
  age: number;
}

type PersonKeys = keyof Person;

type PersonEvents = `${PersonKeys}Changed`;

// "firstNameChanged"
// | "lastNameChanged"
// | "ageChanged"
```

Los Template Literal Types suelen combinarse con Mapped Types para generar automáticamente nombres de properties o métodos.

```ts
type Getters<Type> = {
  [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property];
};
```

En este ejemplo se combinan varios conceptos vistos anteriormente.

1. `keyof Type` obtiene todas las properties del Type.
2. El Mapped Type recorre cada una de ellas.
3. `as` junto con el Template Literal cambia el nombre de cada property (`name` → `getName`).
4. `Type[Property]` obtiene el Type original de esa property.
5. Finalmente se crea una function que retorna ese mismo Type.

> Comentario aparte, se pone `<string & Property>` y no solo `<Property>` porque `Capitalize` solo acepta strings y `keyof` puede devolver `string | number | symbol`.

```ts
//Si nosotros ponemos esto
interface Person {
  name: string;
  age: number;
}

//Obtenemos esto
type Getters<Person> = {
  getName: () => string;
  getAge: () => number;
};
```

#### Intrinsic String Manipulation Types

TypeScript incorpora algunos Utility Types para modificar strings dentro de Template Literal Types. El Type (`<T>`) de todos estos es un `<StringType>`.

```ts
type Loud = Uppercase<"hello">;

// "HELLO"

type Quiet = Lowercase<"HELLO">;

// "hello"

type Name = Capitalize<"german">;

// "German"

type Id = Uncapitalize<"UserId">;

// "userId"
```

## Utility Types

TypeScript provee muchas Utility Types para facilitar transformaciones de type comunes, estan disponibles de forma global.

A continuación estan las fundamentales y luego algunas útiles.

### Partial<T>

`Partial<T>` Hace todas las properties opcionales. Es muy usado para updates.

```ts
interface User {
  name: string;
  age: number;
}

type PartialUser = Partial<User>;

// {
//   name?: string;
//   age?: number;
// }
```

### Pick<T, K>

Selecciona únicamente las properties indicadas.

```ts
type UserPreview = Pick<User, "name">;

// {
// name: string;
// }
```

### Omit<T, K>

Hace exactamente lo contrario de `Pick`. Se usa mucho en React.

```ts
type UserWithoutAge = Omit<User, "age">;

// {
//   name: string;
// }
```

### Record<K, T>

Crea un objeto cuyas keys son `K` y cuyos valores son `T`.

```ts
type Roles = "admin" | "user";

type Permissions = Record<Roles, boolean>;

// {
//   admin: boolean;
//   user: boolean;
// }
```

### Importantes

#### Required<T>

`Required<T>` hace obligatorias todas las properties. Es básicamente el inverso de Partial.

#### Readonly<T>

`Readonly<T>` Hace todas las properties readonly.

#### Exclude<T, U>

Elimina miembros de un Union.

```ts
type A = Exclude<"a" | "b" | "c", "b">;

// "a" | "c"
```

#### Extract<T, U>

Hace lo contrario.

```ts
type A = Extract<"a" | "b" | "c", "b" | "c">;

// "b" | "c"
```

#### NonNullable<T>

`NonNullable<T>` elimina null y undefined.

```ts
type A = NonNullable<string | null | undefined>;

// string
```

#### Los que ya mancionamos.

`ReturnType<T>` obtiene los Types del retorno y `Parameters<T>` obtiene los Types de los argumentos.

```ts
function greet(name: string, age: number) {
  console.log(`${name} is ${age}`);
}

type GreetParams = Parameters<typeof greet>;

// [string, number]
```

Uppercase, Lowercase, Capitalize, Uncapitalize ya los explicamos en _Template Literal Types_

## ¿Qué hace cada herramienta?

**Literal Types**
→ Restringen valores.

**Union Types**
→ Permiten varios Types.

**Intersection Types**
→ Combinan Types.

**Generics**
→ Hacen Types reutilizables.

**keyof**
→ Obtiene las keys.

**typeof**
→ Obtiene el Type de un valor.

**Indexed Access**
→ Obtiene el Type de una property.

**Conditional Types**
→ Type según una condición.

**Mapped Types**
→ Transforman un Type.

**Template Literal Types**
→ Generan nuevos String Literal Types.

**Utility Types**
→ Helpers construidos con todo lo anterior.

## Documentación

Comentarios, ejemplos, descripciones más técnicas y específicas en:

1. [Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#arrays)
2. [Interfaces u Object Types en detalle](https://www.typescriptlang.org/docs/handbook/2/objects.html)
3. [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)
4. [Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)
5. [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
