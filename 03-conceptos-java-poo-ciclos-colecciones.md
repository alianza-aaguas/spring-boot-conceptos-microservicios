# 3. Conceptos de Java: POO, ciclos y colecciones

## Índice

1. [Qué es Java orientado a objetos](#31-qué-es-java-orientado-a-objetos)
2. [Clase](#32-clase)
3. [Objeto](#33-objeto)
4. [Atributos](#34-atributos)
5. [Métodos](#35-métodos)
6. [Encapsulamiento](#36-encapsulamiento)
7. [Herencia](#37-herencia)
8. [Polimorfismo](#38-polimorfismo)
9. [Abstracción](#39-abstracción)
10. [Ciclos](#310-ciclos)
11. [Colecciones](#311-colecciones)
12. [List](#312-list)
13. [Set](#313-set)
14. [Map](#314-map)
15. [Try, catch y finally](#315-try-catch-y-finally)
16. [Conclusiones y buenas prácticas](#316-conclusiones-y-buenas-prácticas)

---

## 3.1 Qué es Java orientado a objetos

Java organiza el software en objetos que combinan estado y comportamiento. Eso permite representar entidades reales del negocio de una forma más natural.

### Idea clave

```text
Clase = plano
Objeto = instancia
Atributos = estado
Métodos = comportamiento
```

---

## 3.2 Clase

### Definición técnica

Una clase es una plantilla que define atributos y métodos comunes para crear objetos.

### Explicación sencilla

Es como el plano de una casa: describe cómo será, pero todavía no existe físicamente.

### Caso de uso real

Una clase `Producto` puede representar nombre, precio y stock.

### Ventajas

- Reutilización de código.
- Organización clara.
- Facilita mantenimiento.

### Desventajas o consideraciones

- Si una clase hace demasiado, se vuelve difícil de entender.

### Buenas prácticas

- Una clase debe tener una responsabilidad principal.
- Usa nombres claros y del dominio.

---

## 3.3 Objeto

### Definición técnica

Un objeto es una instancia concreta de una clase.

### Explicación sencilla

Si la clase es el plano, el objeto es la casa ya construida.

### Caso de uso real

Cada producto registrado en un sistema es un objeto diferente.

---

## 3.4 Atributos

### Definición técnica

Son las variables que representan el estado de un objeto.

### Explicación sencilla

Son las características del objeto, como nombre, edad o saldo.

### Caso de uso real

Un `Cliente` puede tener nombre, correo y teléfono.

### Buenas prácticas

- Preferir atributos privados.
- Validar valores cuando sea necesario.

---

## 3.5 Métodos

### Definición técnica

Son bloques de código dentro de una clase que representan comportamientos.

### Explicación sencilla

Son las acciones que el objeto puede hacer.

### Caso de uso real

`depositar`, `retirar`, `calcularTotal`.

---

## 3.6 Encapsulamiento

### Definición técnica

Oculta el estado interno de un objeto y controla el acceso mediante métodos.

### Explicación sencilla

Es como una caja fuerte: no todos pueden tocar el contenido directamente.

### Caso de uso real

Evitar que un saldo se modifique sin validar una operación.

### Buenas prácticas

- Atributos privados.
- Métodos públicos solo cuando aporten valor.

---

## 3.7 Herencia

### Definición técnica

Permite que una clase hija reutilice atributos y métodos de una clase padre.

### Explicación sencilla

Es como heredar rasgos familiares.

### Caso de uso real

`Empleado` y `Cliente` pueden compartir datos de una persona.

### Consideración importante

Conviene usarla solo cuando existe una relación real de tipo “es un”.

---

## 3.8 Polimorfismo

### Definición técnica

Una misma referencia puede representar diferentes comportamientos según el tipo concreto del objeto.

### Explicación sencilla

Es como un control remoto que funciona con distintos aparatos, pero cada uno responde a su manera.

### Caso de uso real

Distintos métodos de pago implementan una operación común como `procesarPago()`.

---

## 3.9 Abstracción

### Definición técnica

Consiste en representar lo esencial y ocultar los detalles innecesarios.

### Explicación sencilla

Es enfocarse en lo importante y dejar fuera lo que complica de más.

### Caso de uso real

Una interfaz de pago muestra qué se puede hacer, no cómo se conecta con cada proveedor.

---

## 3.10 Ciclos

### Definición técnica

Los ciclos repiten instrucciones mientras se cumpla una condición o por cada elemento de una colección.

### Explicación sencilla

Es repetir una tarea varias veces sin escribir el mismo código una y otra vez.

### Caso de uso real

Recorrer una lista de productos o intentar una operación hasta que funcione.

### Tipos comunes

- `for`
- `while`
- `do-while`
- `for-each`

---

## 3.11 Colecciones

### Definición técnica

Las colecciones son estructuras de datos que almacenan y organizan múltiples elementos.

### Explicación sencilla

Son como cajas o listas para guardar varios valores.

### Caso de uso real

Guardar nombres de clientes, productos o pedidos.

### Tipos comunes

- `List`
- `Set`
- `Map`

---

## 3.12 List

### Definición técnica

Una `List` es una colección ordenada que permite elementos repetidos y acceso por índice.

### Explicación sencilla

Es una fila ordenada de elementos.

### Caso de uso real

Lista de tareas, lista de productos o historial de compras.

### Consideraciones

- Mantiene el orden.
- Puede tener duplicados.

---

## 3.13 Set

### Definición técnica

Un `Set` es una colección que no permite elementos duplicados.

### Explicación sencilla

Es como una colección de entradas únicas.

### Caso de uso real

Correos únicos, etiquetas sin repetir o códigos distintos.

### Consideraciones

- Útil cuando no quieres repetidos.
- No siempre mantiene orden visible.

---

## 3.14 Map

### Definición técnica

Un `Map` guarda pares clave-valor.

### Explicación sencilla

Es como una agenda: buscas una clave y obtienes su valor.

### Caso de uso real

Buscar el nombre de un producto usando su ID.

### Consideraciones

- La clave debe ser única.
- Es ideal para búsquedas rápidas.

### Métodos comunes de `Map`

- `put`
- `get`
- `getOrDefault`
- `containsKey`
- `remove`
- `keySet`
- `values`
- `entrySet`
- `size`
- `isEmpty`
- `clear`

---

## 3.15 Try, catch y finally

### Definición técnica

Permiten manejar errores de forma controlada durante la ejecución.

### Explicación sencilla

Es tener un plan B cuando algo sale mal.

### Caso de uso real

Intentar convertir texto a número, leer archivos o dividir valores.

### Rol de cada parte

- `try`: intenta ejecutar el bloque.
- `catch`: captura el error.
- `finally`: se ejecuta pase lo que pase.

---

## 3.16 Conclusiones y buenas prácticas

POO, ciclos, colecciones y manejo de errores forman la base de cualquier programa Java bien organizado.

### Buenas prácticas generales

- Usa POO para modelar el negocio.
- Usa colecciones según la necesidad real.
- Usa `Map` cuando necesites clave-valor.
- Usa ciclos claros y cortos.
- Maneja errores con mensajes entendibles.

[Siguiente entrega: ejemplos de Java POO, ciclos y colecciones](./04-ejemplos-java-poo-ciclos-colecciones.md)
