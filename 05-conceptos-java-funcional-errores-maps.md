# 🎯 5. Conceptos de Java: Programación Funcional, Errores y Map
### Explicado para personas que **no saben programar**

> 📘 Este documento es la continuación de `04-ejemplos-java-poo-ciclos-colecciones.md`.
> Aquí nos adentramos en un paradigma diferente: **la programación funcional**.

---

## 🧭 Introducción: ¿Qué es "funcional" en programación?

Imagina que tienes una **fábrica de pasteles**:
- **Forma tradicional (POO):** Haces una clase `FábricadePasteles` con todas las máquinas, recetas y reglas adentro. Es un bloque grande.
- **Forma funcional:** Tienes recetas pequeñas e independientes que puedes combinar: una para mezclar, otra para hornear, otra para decorar.

👉 **La programación funcional es pensar en transformaciones pequeñas y reutilizables en lugar de objetos gigantes.**

**Idea clave:** en lugar de "qué es" (objeto), piensas en "qué hace" (transformación).

---

## 📑 Índice

1. [Programación funcional](#51-¿qué-es-la-programación-funcional)
2. [Lambda](#52-⚡-lambda)
3. [Interfaces funcionales](#53-📋-interfaces-funcionales)
4. [Predicate: ¿Sí o No?](#54-✅-predicate-¿sí-o-no)
5. [Function: Convierte una cosa en otra](#55-🔄-function-convierte-una-cosa-en-otra)
6. [Consumer: Consume y no devuelve](#56-📤-consumer-consume-y-no-devuelve)
7. [Supplier: Proveedor de datos](#57-📥-supplier-proveedor-de-datos)
8. [Streams: Tubería de datos](#58-💧-streams-tubería-de-datos)
9. [Optional: Valor que puede no existir](#59-📦-optional-valor-que-puede-no-existir)
10. [Map en profundidad](#510-map-en-más-profundidad)
11. [Errores y buenas prácticas](#511-errores-y-buenas-prácticas)
12. [Conclusiones](#512-conclusiones)
13. [Navegación](#🔗-navegación-de-la-serie)

---

## 5.1 ¿Qué es la Programación Funcional?

**Concepto literal en programación:** La programación funcional es un paradigma que favorece el uso de funciones como valores de primera clase, la inmutabilidad de datos y la composición de operaciones.

**Explicación sencilla:** En lugar de pensar en objetos que cambian de estado, piensas en **transformaciones de datos** que puedes encadenar.

**Analogía:** 
Imagina una **línea de ensamble en una fábrica**:
- Operario 1 recibe materia prima → la procesa → la pasa.
- Operario 2 recibe lo que pasó el operario 1 → lo transforma → la pasa.
- Operario 3 hace lo suyo → resultado final.

Cada operario tiene un trabajo específico y predecible. Si cambias el orden o añades operarios, sabes qué pasa.

**Caso de uso real:**
- Filtrar productos de un catálogo.
- Transformar textos (mayúsculas, minúsculas, truncar).
- Calcular totales sobre listas de compras.
- Buscar y modificar datos en lotes.

**¿Por qué es útil?**
- El código es más legible.
- Es fácil reutilizar funciones pequeñas.
- Menos errores porque los datos no cambian inesperadamente.

---

## 5.2 ⚡ Lambda

**Concepto literal en programación:** Una lambda es una función anónima (sin nombre) que se define con sintaxis compacta y puede pasarse como argumento a otros métodos.

**Explicación sencilla:** Es un "atajo mental" para escribir una función sin crear una clase completa.

**Analogía:**
Normalmente, si quieres hacer algo, tienes que:
1. Crear un documento formal.
2. Imprimirlo.
3. Llevarlo.

Con lambda es como escribir una nota rápida en un papel: eficiente y directa.

**Ejemplo visual (pseudo-código):**
```
Sin lambda:
-----------
class Filtro implements Predicate<Producto> {
    public boolean test(Producto p) {
        return p.getPrecio() > 100;
    }
}
Filtro filtro = new Filtro();

Con lambda:
-----------
p -> p.getPrecio() > 100
```

**¿Por qué es útil?**
- Menos código.
- Más directo y legible.
- Ideal para operaciones pequeñas y únicas.

---

## 5.3 📋 Interfaces Funcionales

**Concepto literal en programación:** Una interfaz funcional es una interfaz con **exactamente un único método abstracto**. Puede anotarse con `@FunctionalInterface`.

**Explicación sencilla:** Es un contrato súper pequeño que define "una sola acción principal".

**Analogía:**
Piensa en un **botón:**
- Solo hace una cosa.
- Presionas → sucede algo.
- No tiene distracciones ni opciones secundarias.

Una interfaz funcional es como ese botón: un contrato limpio para "una acción".

**Ejemplo de interfaz funcional:**
```java
@FunctionalInterface
public interface Verificador {
    boolean chequear(String valor);
    // Solo un método abstracto ✓
}
```

**¿Por qué es útil?**
- El contrato es claro: "esto hace una sola cosa".
- Se pueden usar lambdas para implementarla.
- Facilita la composición de pequeñas acciones.

---

## 5.4 ✅ Predicate: "¿Sí o No?"

**Concepto literal en programación:** `Predicate<T>` es una interfaz funcional que recibe un valor de tipo `T` y devuelve un `boolean`.

**Explicación sencilla:** Es un "juez" que responde solo **sí o no**.

**Analogía:**
Eres un portero en una discoteca:
- Te llega una persona.
- Tienes una regla: "mayor de 18 años".
- Respondes: ¿cumple? → Sí (true) o No (false).

```
Persona → [Predicate: ¿es mayor de 18?] → Sí/No
```

**Ejemplo de uso (pseudo-código):**
```java
Predicate<Integer> esMayorQueNueve = numero -> numero > 9;
boolean resultado = esMayorQueNueve.test(15); // true
```

**Casos de uso real:**
- Validar si un email es válido.
- Verificar si un usuario tiene permisos.
- Filtrar productos por precio.
- Revisar si una contraseña es fuerte.

---

## 5.5 🔄 Function: "Convierte una cosa en otra"

**Concepto literal en programación:** `Function<T, R>` recibe un valor de tipo `T` y devuelve un valor de tipo `R`.

**Explicación sencilla:** Es una "máquina de transformación". Entra un ingrediente, sale otro.

**Analogía:**
Imagina un **procesador de alimentos**:
- Entra: tomate rojo.
- Sale: jugo de tomate.

La máquina transforma entrada en salida.

```
Tomate → [Function] → Jugo
Número → [Function] → Número * 2
Texto → [Function] → Texto mayúsculas
```

**Ejemplo de uso:**
```java
Function<Integer, Integer> duplicar = numero -> numero * 2;
Integer resultado = duplicar.apply(5); // 10
```

**Casos de uso real:**
- Convertir precios de una moneda a otra.
- Transformar JSON a objeto Java.
- Extraer información de un producto.
- Formatear textos.

---

## 5.6 📤 Consumer: "Consume y no devuelve"

**Concepto literal en programación:** `Consumer<T>` recibe un valor de tipo `T` y no devuelve nada (`void`).

**Explicación sencilla:** Es un "comedor". Recibe algo y lo consume para hacer una acción, pero no devuelve resultado.

**Analogía:**
Imagina un **camión de basura**:
- Llega el camión.
- Recibe tu basura.
- Se la lleva.
- No devuelve nada.

```
Datos → [Consumer] → Se consume (sin retorno)
```

**Ejemplo de uso:**
```java
Consumer<String> imprimir = mensaje -> System.out.println(mensaje);
imprimir.accept("Hola mundo"); // imprime sin devolver
```

**Casos de uso real:**
- Imprimir datos en la consola.
- Guardar datos en una base de datos.
- Enviar un email.
- Registrar un evento (logging).

---

## 5.7 📥 Supplier: "Proveedor de datos"

**Concepto literal en programación:** `Supplier<T>` es una interfaz funcional que no recibe entrada y devuelve un valor de tipo `T`.

**Explicación sencilla:** Es un "proveedor". No pide nada, pero te devuelve algo.

**Analogía:**
Imagina un **dispensador de café**:
- No le pides ingredientes.
- Solo presionas un botón.
- Te devuelve café.

```
(sin entrada) → [Supplier] → Valor de salida
```

**Ejemplo de uso:**
```java
Supplier<String> saludo = () -> "¡Hola!";
String resultado = saludo.get(); // "¡Hola!"
```

**Casos de uso real:**
- Generar números aleatorios.
- Obtener la fecha/hora actual.
- Crear nuevas instancias.
- Valores por defecto.

---

## 5.8 💧 Streams: "Tubería de datos"

**Concepto literal en programación:** Un stream representa una secuencia de elementos sobre la que se aplican operaciones intermedias y terminales de forma declarativa.

**Explicación sencilla:** Es una "tubería" por donde pasan los datos, transformándose en cada paso.

**Analogía:**
Imagina una **línea de lavado de carros**:
1. El carro entra sucio.
2. Pasa por agua (filter).
3. Pasa por jabón (map).
4. Pasa por aire (forEach).
5. Sale limpio.

Cada paso transforma el carro sin que intermedios deban preocuparse del resultado anterior.

```
Datos → [filter] → [map] → [forEach] → Resultado
```

**Ejemplo de uso:**
```java
List<Integer> numeros = List.of(1, 2, 3, 4, 5);
numeros.stream()
       .filter(n -> n > 2)          // Solo mayores a 2
       .map(n -> n * 2)              // Duplica cada uno
       .forEach(System.out::println); // Imprime
```

**Casos de uso real:**
- Filtrar listas grandes.
- Transformar datos.
- Calcular totales o promedios.
- Buscar elementos específicos.

---

## 5.9 📦 Optional: "Valor que puede no existir"

**Concepto literal en programación:** `Optional<T>` es un contenedor que encapsula un valor que puede estar presente o ausente, evitando trabajar con `null` directamente.

**Explicación sencilla:** Es una caja que **puede estar vacía o llenar**. No es directamente `null`.

**Analogía:**
Imagina una **caja de regalo**:
- Puede estar llena (tiene valor).
- Puede estar vacía (no tiene valor).
- Siempre sabes que existe la caja (no es null).

```
Valor    → Optional.of(valor)
null     → Optional.empty()
incierto → Optional.ofNullable(valor)
```

**Ejemplo de uso:**
```java
Optional<String> nombre = Optional.of("María");
System.out.println(nombre.orElse("Desconocido")); // María

Optional<String> vacio = Optional.empty();
System.out.println(vacio.orElse("Desconocido")); // Desconocido
```

**Casos de uso real:**
- Búsquedas en base de datos que pueden no encontrar nada.
- Parámetros opcionales.
- Datos que vienen de APIs externas.
- Evitar NullPointerException.

---

## 5.10 Map en más profundidad

**Concepto literal en programación:** `Map` es una estructura de datos que organiza información en pares clave-valor y ofrece múltiples métodos para consultar, recorrer, eliminar y limpiar contenido.

### Métodos importantes

- `put`: agrega o reemplaza una entrada.
- `get`: obtiene el valor por clave.
- `getOrDefault`: obtiene con valor alternativo.
- `containsKey`: pregunta si existe la clave.
- `containsValue`: pregunta si existe el valor.
- `remove`: elimina una entrada.
- `keySet`: obtiene todas las claves.
- `values`: obtiene todos los valores.
- `entrySet`: obtiene todos los pares clave-valor.
- `size`: cuenta las entradas.
- `isEmpty`: pregunta si está vacío.
- `clear`: elimina todo.
- `putIfAbsent`: agrega solo si no existe.
- `replace`: reemplaza solo si existe.
- `computeIfAbsent`: calcula valor si falta la clave.
- `forEach`: itera sobre pares clave-valor.

---

## 5.11 Errores y buenas prácticas

**Concepto literal en programación:** Los errores controlados deben manejarse con estructura, mensajes claros y bloques de limpieza cuando aplique.

**Explicación sencilla:** Si algo falla, el programa debe reaccionar de forma ordenada.

**Caso de uso real:**
- División entre cero.
- Archivos inexistentes.
- Conversiones inválidas.
- Acceso a índices fuera de rango.

### Buenas prácticas

1. **Captura específica:** Atrapa el tipo de error exacto, no genérico.
2. **Mensajes claros:** Explica qué pasó y por qué.
3. **Finally para limpieza:** Cierra recursos (archivos, conexiones) aunque falle.
4. **No silencies errores:** Logging o re-lanzamiento es mejor que ignorar.
5. **Usa Optional:** Para ausencia de valor en lugar de null.

---

## 5.12 Conclusiones

**La programación funcional complementa muy bien a la POO en Java.** 

Con lambdas, streams y Optional, tu código será:
- ✅ Más legible.
- ✅ Más conciso.
- ✅ Menos propenso a errores.
- ✅ Más fácil de mantener.

La clave es saber **cuándo usar cada paradigma**: POO para modelar entidades complejas, funcional para transformaciones de datos.

---

## 🔗 Navegación de la serie

| Archivo | Contenido |
|---------|----------|
| [01-conceptos-spring-boot.md](./01-conceptos-spring-boot.md) | 📚 Conceptos de Spring Boot y microservicios |
| [02-ejemplos-spring-boot.md](./02-ejemplos-spring-boot.md) | ��� Ejemplos prácticos de Spring Boot |
| [03-conceptos-java-poo-ciclos-colecciones.md](./03-conceptos-java-poo-ciclos-colecciones.md) | 🧠 Conceptos de Java: POO, ciclos y colecciones |
| [04-ejemplos-java-poo-ciclos-colecciones.md](./04-ejemplos-java-poo-ciclos-colecciones.md) | 💻 Ejemplos de Java: POO, ciclos y colecciones |
| **05-conceptos-java-funcional-errores-maps.md** | 🎯 Conceptos de Java: programación funcional y Map |
| [06-ejemplos-java-funcional-errores-maps.md](./06-ejemplos-java-funcional-errores-maps.md) | 💻 Ejemplos de Java: programación funcional y Map |

---

**👉 Próximo documento:** [Ejemplos prácticos de programación funcional con código real](./06-ejemplos-java-funcional-errores-maps.md)
