# Petit Python Resumen

Esta hecho a partir de unas mini notas que hice en enero de 2025, en algún momento será ampliado con la documnetación para mayor entendimiento del lenguaje pero esto ya funciona como introducción al mismo y cumple su objetivo de poder leerlo sin tantas complicaciones.

## Machete

|       Python       |           JavaScript            |
| :----------------: | :-----------------------------: |
|        list        |              Array              |
|        dict        | Object (aprox., no exactamente) |
|        None        |              null               |
|    True / False    |          true / false           |
|       lambda       |     arrow function (aprox.)     |
| list comprehension |      Array methods / loops      |

## Basics

Es un lenguaje de programación de alto nivel, es multiparadigma pero tiene enfasis en el paradigma de la progrmación orientada a objetos (OOP) y es dinamicamente tipado.

La naming convention es `snake_case`, esto es asi para variables, functions, métodos, y packages. Para constantes y variables de entorno se usa `SCREAMING_SNAKE_CASE`.

```python
names = ["juan", "dana", "lucas", "camila"]  # > list o array
names2 = ["jose", "caro", "seba"]

names[1]  # > dana
names[1:3]  # > ["dana", "lucas"]
names[:3]  # > ["juan","dana", "lucas"]

names_combined = names + names2  # > Concatenación

print()  # > Relativo a console.log()

```

También podemos hacer lo siguiente

```py
names_combined = names[1:] + names2[:2]

names[0:-1]  # > Droppea el úlitmo número. ["juan","dana", "lucas"]
```

## Condicionales

La identación (espacios horizontales) reemplazan a las llaves `{ }` de JavaScript o Csharp. Véase en el siguiente ejemplo de un If statement.

```py
age = 23

if age > 21:
    print("podes tomar alcohol")
```

Usar else if, y else.

```py
if age > 50:
    print("ejemplo")
elif age > 21:
    print("sedse")
else:
    print("sedse")
```

Usar operadores lógicos `and` y `or`.

```py
age = 55

if age > 50 and age < 70:
    print("sedse")

if age > 50 or age < 70:
    print("xdd")
```

## Loops

```py
for i in range(0,5):
    print(i)

# 0,1,2,3,4
```

```py
names = ["juan", "dana", "lucas", "camila"]

for name in names:
    print(name)

# "juan","dana", "lucas","camila"
```

## Dictionaries

Un `dic` es una estructura de datos, no el equivalente general a los objetos de JavaScript.

```py
person = {"name": "Juan", "age": 27, 66:'ruta'}

person["name"]  # > Juan
person["age"]  # > 27
```

Si hicieramos `person[age]` sin las comillas dobles significa que es una variable.

El segundo argumento de la function es el default si la _key_ específica no existe.

```py
person.get("phone","not found")
```

Obetener las _keys_ de un dic.

```py
person.keys() # dict_keys(['name', 'age'])
```

Obetener los _values_ de un dic.

```py
person.values() # dict_values(['Juan', 27])
```

Obetener todo un dic como array.

```py
person.items() # dict_items([('name', 'Juan'), ('age', 27)])
```

Si queremos transformar las _views_ que nos devuelven estos métodos tendríamos que pasarlos por el método `list()`.

### Tuplas

Los arrays o listas en Python son dinámicos, esto quiere decir que se pueden modificar añadiendoles nuevos valores, las _tuplas_ por otro lado no dejan que se modifique su contenido es decir son inmutables.

```py
numbers = (32, 27, 5, 10)
```

### for in loop pero en Python

```py
person = {"name": "Juan", "age": 27}

for key, value in person.items():
    print(key, value)

# name Juan
# age 27
```

## Unpacking

Es lo relativo al destructuring en JavaScript.

```py
water = ["clear", "high ph"]

a, b = water

# Donde "a" es "clear" y "b" es "high ph"
```

Esto se puede llevar más allá con diferentes modificadores.

```py
names = ["juan", "dana", "lucas", "camila"]

a, *b = names
# juan ['dana', 'lucas', 'camila']

a, *b, c = names
# juan ['dana', 'lucas'] camila
```

También tenemos la siguiente manera de modificar listas.

```py
names = ["juan", "dana", "lucas", "camila"]

names[2] = "cristian" # ['juan', 'dana', 'cristian', 'camila']

names[1:3] = ["rita", "maria"] # ['juan', 'rita', 'maria', 'camila']

names[1:2] = ["rita", "maria"] # ['juan', 'rita', 'maria', 'lucas', 'camila']
```

A su vez podemos poner condicionales y loops, a esto se lo llama _list comprehension_.

```py
new_list = [name + "xd" for name in names] # ['juanxd', 'danaxd', 'lucasxd', 'camilaxd']

new_list = [name + "xd" for name in names if name[-1] == "a"] # ['danaxd', 'camilaxd']
```

## Métodos de arrays/listas

Python tiene funciones como `map()` y `filter()` que cumplen funciones similares a `.map()` y `.filter()` de JavaScript. En los arguements primero el callback con lo que queremos que haga y luego la lista o array. `lambda` permite crear functions anónimas pequeñas, sería lo relativo a una arrow function de JavaScript.

```py
names = ["juan", "dana", "lucas", "camila"]

obj = map(lambda n: n + "xd", names)

name_and_xd = list(obj)

print(name_and_xd) # ['juanxd', 'danaxd', 'lucasxd', 'camilaxd']
```

## Functions

```py
def greet(name):
    return f"Hello {name}"
```

# WIP Agregar

- [ ] parámetros
- [ ] default parameters
- [ ] \*args \
- [ ] \*\*kwargs
- [ ] return type
- [ ] hints
- [ ] scope

## Classes

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        return f"Hello, I'm {self.name}"
```

Ahí tendríamos que dedicarle un poco más de atención a self, **init**, instance attributes, methods, inheritance, etc.
