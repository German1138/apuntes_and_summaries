# React Resumen

## Basicos y Componentes

Las apps de React estan hechas a partir de componentes, estos son una pieza de interfaz de usuario (UI) que tienen su propia lógica y apariencia. Un componente puede ser un botón o una página entera.

Todos comienzan con Mayúsculas.

Ejemplo componente

```jsx
function MyButton() {
  return <button>I'm a button</button>;
}

//Ese mismo componente "envuelto" (nested) en otro.
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

El formato que se usa con React es **JSX** (TSX con TypeScript), combina JavaScript con tags de HTML, es más estricto que HTML. Se deben cerrar las tags asi `<br />` y solo se puede devolver un JSX tag (envuelto en `<div>...</div>` o `<>...</>`) que combine todo el componente creado.

### Añadir estilos

Especificamos una clase de CSS con `classname`, igual que en HMTL.

```jsx
<img className="avatar" />
```

```css
/* esto en nuestro archivo CSS*/
.avatar {
  border-radius: 50%;
}
```

### Mostrar información

Como ne en **JSX** combinamos HMTL con JavaScript podemos incorporar _llaves_ `{}` a nuestras tags para usar logica de JavaScript dentro de las mismas. Véase incorporar una variable del código.

```JSX
return <h1>{user.name}</h1>;
```

También esta bueno resaltar que en HTML un atributo es usado con comillas dobles `" "` en cambio en **JSX** podemos usar _llaves_ `{}` para leer los valores de, por ejemplo, variables y que terminen siendo los valores de los atributos. Se pueden poner valores más complejos también.

```JSX
return <img className="avatar" src={user.imageUrl} />;
```

Ejemplo más complejo

```JSX
const user = {
  name: "Hedy Lamarr",
  imageUrl: "https://react.dev/images/docs/scientists/yXOvdOSs.jpg",
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={"Photo of " + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize,
        }}
      />
    </>
  );
}
```

### Uso de Condicionales

Se pueden usar condiciones (If Statements) como normalmente lo haríamos en JavaScript, tiene el añadido de poder mezclarlo con JSX.

```jsx
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return <div>{content}</div>;

//usando un ternario

<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>

//también

<div>{isLoggedIn && <AdminPanel />}</div>;
```

### Renderizar listas y métodos de JavaScript

Se pueden usar `for` loops pero normalmente vamos a utilizar el método del array `map()`. Véase transformar un array de productos en un array de `<li>`'s.

```jsx
const products = [
  { title: "Cabbage", isFruit: false, id: 1 },
  { title: "Garlic", isFruit: false, id: 2 },
  { title: "Apple", isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map((product) => (
    <li
      key={product.id}
      style={{
        color: product.isFruit ? "magenta" : "darkgreen",
      }}
    >
      {product.title}
    </li>
  ));

  return <ul>{listItems}</ul>;
}
```

> Siempre deberiamos de darle un atributo `key` a cada item, en este caso los `<li>`'s. Ese `key` debería de venir de un ID de nuestra información, ya que React los usa para saber qué pasó si luego añadimos, quitamos o reordenamos uno de los items.

### Manejar eventos

Se declaran dentro de los componentes

```jsx
function MyButton() {
  function handleClick() {
    alert("You clicked me!");
  }

  return <button onClick={handleClick}>Click me</button>;
}
```

Nótese que `handleClick` no esta siendo ejecutado dentro del botón, solo es necesario pasarlo, React va a ejecutarlo (callearlo) cuando el usuario haga clic.

### Actualizar la pantalla, useState

Cuando queremos que la información persista o sea recordada y luego mostrarla tenemos que añadir un `state` a nuestro componente, un ejemplo es mostrar cuantas veces se hizo clic en un botón. Usamos el `useState`.

El `useState` te devuelve primero el state actual y luego la function con la cual actualizas ese state, cuando lo delcaramos en sus arguments podemos pasarle su state inical. La naming convention es usar `something` y `setSomething` para cada una respectivamente.

Cada componente va a tener su state propio.

```jsx
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return <button onClick={handleClick}>Clicked {count} times</button>;
}
```

Las functions o métodos que arrancan con `use` se llaman `hooks`, hay muchos mas además de `useState` y también podes crear propios. Solo se pueden llamar por encima de tus componentes. No se pueden llamar dentro de condicionales (if's), loops, u otras nested functions.

Puede pasar que queramos que el state se actualice de forma unificada, no que cada boton tenga el suyo propio separado, eso lo logramos subiendo el `useState` un componente hacia arrib (formalmente llamado “lifting state up”) y pasandole nuestro state y handler como _props_ a los componentes que querramos.

```jsx
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

//Actualizamos el componente MyButton

function MyButton({ count, handler }) {
  return <button onClick={handler}>Clicked {count} times</button>;
}
```

### Cómo se piensa React

Cuando creamos una interfaz de usuario en React primero la dividimos en partes llamadas componentes, después vas a describir el state de cada uno y finalmente los conectamos para que la información fluya entre los mismos.

Cuando construimos componentes a los cuales les vamos a ir pasando props normalmente en proyectos chicos conviene arrancar desde arriba ó el componente más grande, en proyectos grandes al revés. Por ejemplo hacer primero el `CardContainer` y luego las `Cards`.

Think of state as the minimal set of changing data that your app needs to remember. The most important principle for structuring state is to keep it DRY (Don’t Repeat Yourself).
Pensemos al state como el conjunto/set mínimo de información cambiante/mutable que tu app necesita recordar. El principio más importante para estructurar el state es mantenerlo bajo el principio DRY (Don’t Repeat Yourself).

- Se mantiene sin cambios a lo largo del tiempo? Si es el caso, no es un state.
- Esta siendo pasado por un _parent_ via _props_? Si es el caso, no es un state.
- Podes computarlo/calcularlo basandote en un estado existente o props en tu componente? Si es el caso, no es un state.

### Diferencia entre PROPS y STATE

Las _props_ son argumentos que pasas a una function, desde un parent para que un child las pueda usar. Por ejemplo un `form` puede pasar un color como _props_ a un componente `Button`.

El state es como la memoria de un componente, le deja al mismo tener seguimiento de alguna información y cambiarla en respuesta a interacciones del usuario. Por ejemplo un botón puede tener el seguimiento de un isHovered state.

For each piece of state in your application:

Identify every component that renders something based on that state.
Find their closest common parent component—a component above them all in the hierarchy.

Dónde debería de vivir un state:

- Normalmente podes poner el state directamente en el parent común de uno o varios componentes.
- También podes poner el state en un componente que este por encima de su parent común.
- Si no podes encontrar un componente el cual tenga sentido que aloje el state crea un nuevo componente solamente para alojarlo y añadilo por en algun lado por encima de la jerarquia del componente parent que haya en comun.

## Añadir Interactividad

Local variables don’t persist between renders. When React renders this component a second time, it renders it from scratch—it doesn’t consider any changes to the local variables.
Changes to local variables won’t trigger renders. React doesn’t realize it needs to render the component again with the new data.

To update a component with new data, two things need to happen:
Para actualizar un componente con nueva información tienen que pasar dos cosas:

Mantener la información entre renders.
Provocar que React renderice el componente con nueva información (re-rendering).

The useState Hook provides those two things:
El useState hook provee estas dos cosas:
Una variable state que retiene la información entre renders.
Una setter function para actualizar la variable/state y provocar que React renderice de nuevo el componente.

Hay que darse cuenta cuando usar el state en conjunto o no, por ejemplo, si tenemos un `form` no vamos a tener varios `useState`'s para cada uno de los `inputs` sino que tendríamos uno solo que maneje todos.

Principios para estructurar/organizar el state:

1. Group related state. Si siempre actualizas dos o más state variables al mismo tiempo considera fusionarlas en una única state variable.
2. Avoid contradictions in state. Cuando un state esta estructurado/organizado de una forma que varias partes del mismo pueden contradecirse o no coincidir entre ellas dejas espacio a que aparezcan errores. Tratá de evitar esto.
3. Avoid redundant state. Si podes calcular alguna información que necesites desde las _props_ o un state ya existente en el componente, no deberías de poner esa información en un nuevo state.
4. Avoid duplication in state. Cuando la misma información esta duplicada en varias state variables o dentro nested objects es difícil mantenerla sincronizada en todas partes a la vez. Reducí la duplicación siempre que puedas.
5. Avoid deeply nested state. Evita un state que este muy jerarquizado, esto que es haya variables muy dentro de un objeto por ejemplo (tener que usar muchas dot notations para acceder), es difícil actualizarlos. Si es posible preferí estructurarlos en una forma llana, o sea usar el flat.

A diferencia de las props, el state es totalmente privado para el componente que lo declara.

Hay dos razones por las que un componente se renderiza.

1. Es el renderizado inicial del mismo.
2. El state del componente (o alguno de sus acestros/parents) se actualizó. Podes provocar renderiados actualizando el state con la set function, cuando lo actualizas automáticamente el render entra a la cola.
