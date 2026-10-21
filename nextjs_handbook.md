# Nextjs Resumen

## Server-Side y Client-Side Components

La principal diferencia es que uno esta hecho para manejar las request con el servidor y el otro para las interacciones del usuario.

### Server-Side

Es el default cuando creamos un componente. Los Server Components renderizan en el servidor. Pueden mostrar información estática o dinámica y son ideales para obtener datos, acceder a bases de datos y reducir el JavaScript enviado al navegador.

En ciertos cosos puede ser _dinámico_, esto cuando se usan cookies o params para acceder a información personalizada del usuario. Esto no es algo que se hace completamente solo pero Nextjs puede detectar ciertas API's que son dinámicas; lo que nosotros podemos elegir es cuando hacer cache, relavidar data específica, o forzar que sea dinámico.

Lo siguiente es dinámico.

```tsx
const user = await getUser();
```

Un ejemplo de component _server-side_ puede ser un logo.

Un _Client Component_ no puede importar un _Server Component_. Si queremos usarlo en el _client-side_ component hay que pasarlo como _prop_ o children.

Si hay que pedir información al servidor se lo hace en el _Server Component_ y luego se pasa como _prop_ al _Client Component_. Si los datos cambian desde el cliente (polling, cache, refetch, etc.) suele usarse React Query o SWR.

### Client-Side

Hay que específicar al principio del file `"use client"`.

La idea es que sean los componentes que vayan a tener algo de interactividad con el usuario, véase un _SearchBar_, donde usemos `localStorage` o _hooks_ como `useState`.

Conviene que los Client Components estén lo más abajo posible del árbol para minimizar el JavaScript enviado al navegador, idealmente son una hoja. todo lo que esté dentro de un _Client Component_, véase otros componentes hijos, no pueden ser _server-side_. Esto es para que el _layout_ se quede en el servidor.

### Ejemplo de como se veria el layout.

```tsx
return (
  <>
    <nav>
      <logo /> //server-side
      <SearchBar /> //client-side
    </nav>
    <main>{children}</main>
  </>
);
```

## Server Actions

Async functions que se ejecutan en el servidor. Normalmente manejan form submissions y data mutations, también se pueden usar en _event handlers_, `useEffect` y otros form-like elements tipo buttons.

Se pueden usar en _server-side_ como _client-side_ components. Para utilizarlas en un _client component_ hay que importarla, esto se puede hacer ya que son **functions** y no componentes.

Para todo el file.

```tsx
"use server"

export async function(){}
export async function(){}
```

Para una function específica

```tsx
async function Create() {
  "use server";
  //mutate data
}
```

Reemplazan muchos usos de APIs REST internas cuando únicamente necesitamos mutar datos desde la propia aplicación Next.

## Routing

Primero dejemos en claro la estructura de carpetas y archivos.

Dentro de la carpeta "_src/app_" podemos crear la sub carpeta _api_ que es nuestro _backend_ (si es que necesitamos Route Handlers) y las demás carpetas que creemos dentro van a ser las diferentes rutas del _frontend_. También van a estar presentes los archivos `page.tsx` que es lo que se va a renderizar en `"/"` y `layout.tsx` que se usa para mostrar la UI que se ve a traves de multiples rutas del sitio y estan siempre presentes, por ejemplo el `<Footer>` y `<Nav>`.

Para que haya una ruta **tiene que estar** el `route.ts` (backend) o el `page.tsx` (frontend) dentro de la carpeta con un nombre representativo. Por esta razón la carpeta components no crea una ruta.

Si queremos usar un ID para una ruta, o sea que cuando vayamos a _"sitio.com/2231"_ y nos aparezca algo, tendríamos que hacer una subcarpeta y nombrarla así: `[id]` y dentro de su `page.tsx` obtener ese ID con `useParams` (en un Client Components). Un ejemplo de esto es cuando tenemos el detalle de un producto y queremos visualizarlo, el orden de las carpetas dentro de app quedaría así: _src/app/detail/[id]/`page.tsx`_.

En los _Server Component_ los params los podemos obtener directamente desde los arguments.

```tsx
interface iPage {
  params: { id: string };
}

export default function Page({ params }: iPage) {}
```

### Redirigir

Para los _server-side_ components se usa el `redirect`.

Para los _client-side_ components se usa el `<Link>` o el `useRouter`.

## Accesibilidad 101

Se basa en aplicar los siguientes conceptos:

- HMTL semántico
- Ponerle el alt a las imagenes
- Que haya buen contraste entre los colores que usemos, podemos usar las dev tools para chequear.
- Navegación con teclado usando `aria-label`'s.
- Labels asociados a inputs.
- Foco visible (focus).
- Usar botones en vez de divs clickeables.

## Mejorar performance

- Base de datos no relacional.
- Microservicios, usando kubernetes.
- Optimización usando useMemo y useCallback.
- Lazyloading.
- Websockets para que no se repitan requests.
- Interserction observer.
- Server Components.
- Streaming.
- Suspense.
- Lazy loading (dynamic()).
- Caché.
- Revalidación (revalidate).
- Optimización de imágenes (Image).
- Optimización de fuentes (next/font).
- Partial Prerendering (si aplica a la versión).
- Code Splitting.
