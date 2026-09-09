# 5. Conceptos de Java: programación funcional, errores y Map

## Índice

1. [Qué es la programación funcional](#51-qué-es-la-programación-funcional)
2. [Lambda](#52-lambda)
3. [Interfaces funcionales](#53-interfaces-funcionales)
4. [Predicate](#54-predicate)
5. [Function](#55-function)
6. [Consumer](#56-consumer)
7. [Supplier](#57-supplier)
8. [Streams](#58-streams)
9. [Optional](#59-optional)
10. [Map en más profundidad](#510-map-en-más-profundidad)
11. [Errores y buenas prácticas](#511-errores-y-buenas-prácticas)
12. [Conclusiones](#512-conclusiones)

---

## 5.1 Qué es la programación funcional

La programación funcional favorece el uso de funciones como valores, la inmutabilidad y la composición de operaciones.

### Explicación sencilla

En lugar de pensar solo en objetos, también piensas en transformaciones de datos.

### Caso de uso real

Filtrar productos, transformar textos o calcular resultados sobre listas.

---

## 5.2 Lambda

### Definición técnica

Una lambda es una función anónima que puede pasarse como argumento.

### Explicación sencilla

Es una forma corta de escribir una función sin crear una clase completa.

---

## 5.3 Interfaces funcionales

### Definición técnica

Son interfaces con un único método abstracto.

### Explicación sencilla

Son contratos pequeños que representan una sola acción principal.

---

## 5.4 Predicate

### Definición técnica

`Predicate<T>` evalúa una condición y devuelve `boolean`.

### Explicación sencilla

Sirve para responder sí o no.

---

## 5.5 Function

### Definición técnica

`Function<T, R>` recibe un valor y devuelve otro.

### Explicación sencilla

Convierte una cosa en otra.

---

## 5.6 Consumer

### Definición técnica

`Consumer<T>` recibe un valor y no devuelve nada.

### Explicación sencilla

Consume un dato para hacer una acción.

---

## 5.7 Supplier

### Definición técnica

`Supplier<T>` devuelve un valor sin recibir entrada.

### Explicación sencilla

Es un proveedor de datos.

---

## 5.8 Streams

### Definición técnica

Un stream representa una secuencia de elementos sobre la que se aplican operaciones intermedias y terminales.

### Explicación sencilla

Es una tubería para procesar datos paso a paso.

---

## 5.9 Optional

### Definición técnica

`Optional<T>` encapsula un valor que puede existir o no.

### Explicación sencilla

Ayuda a evitar trabajar con `null` directamente.

---

## 5.10 Map en más profundidad

### Definición técnica

`Map` organiza datos en pares clave-valor y ofrece métodos para consultar, recorrer, eliminar y limpiar contenido.

### Métodos importantes

- `put`
- `get`
- `getOrDefault`
- `containsKey`
- `containsValue`
- `remove`
- `keySet`
- `values`
- `entrySet`
- `size`
- `isEmpty`
- `clear`
- `putIfAbsent`
- `replace`
- `computeIfAbsent`
- `forEach`

---

## 5.11 Errores y buenas prácticas

### Definición técnica

Los errores controlados deben manejarse con estructura, mensajes claros y bloques de limpieza cuando aplique.

### Explicación sencilla

Si algo falla, el programa debe reaccionar de forma ordenada.

### Caso de uso real

División entre cero, archivos inexistentes o conversiones inválidas.

---

## 5.12 Conclusiones

La programación funcional complementa muy bien a la POO en Java y facilita el trabajo con colecciones y transformaciones de datos.

[Siguiente entrega: ejemplos de programación funcional, errores y Map](./06-ejemplos-java-funcional-errores-maps.md)
