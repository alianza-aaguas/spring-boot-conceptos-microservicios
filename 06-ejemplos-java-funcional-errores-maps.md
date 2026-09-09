# 💻 6. Ejemplos de Java: Programación Funcional, Errores y Map
### Código práctico línea por línea

> 📘 Este documento es la **continuación práctica** de `05-conceptos-java-funcional-errores-maps.md`.
> Aquí veremos código real con explicaciones detalladas de cada concepto funcional.

---

## 📑 Índice

1. [Lambdas e interfaces funcionales](#61-lambdas-e-interfaces-funcionales)
2. [Predicate, Function, Consumer y Supplier](#62-predicate-function-consumer-y-supplier)
3. [Streams](#63-streams)
4. [Optional](#64-optional)
5. [Map avanzado](#65-map-avanzado)
6. [Try, catch y finally en casos reales](#66-try-catch-y-finally-en-casos-reales)
7. [Resumen final](#67-resumen-final)
8. [Navegación](#🔗-navegación-de-la-serie)

---

## 6.1 Lambdas e interfaces funcionales

### Concepto literal en programación

Una lambda es una función anónima y una interfaz funcional tiene un solo método abstracto.

```java
@FunctionalInterface
interface Operacion {
    int aplicar(int a, int b);
}

public class LambdaEjemplo {
    public static void main(String[] args) {
        Operacion suma = (a, b) -> a + b;
        System.out.println(suma.aplicar(3, 5));
    }
}
```

### Explicación línea por línea

- `@FunctionalInterface`: valida que exista un solo método abstracto.
- `Operacion`: contrato funcional.
- `(a, b) -> a + b`: lambda que implementa el contrato.
- `aplicar(3, 5)`: ejecuta la operación.

---

## 6.2 Predicate, Function, Consumer y Supplier

### Concepto literal en programación

Estas interfaces funcionales representan una condición, una transformación, una acción y un proveedor.

```java
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;

public class FuncionesBasicasEjemplo {
    public static void main(String[] args) {
        Predicate<Integer> esMayor = n -> n >= 18;
        Function<String, Integer> longitud = String::length;
        Consumer<String> imprimir = System.out::println;
        Supplier<String> saludo = () -> "Hola mundo";

        System.out.println(esMayor.test(20));
        System.out.println(longitud.apply("Java"));
        imprimir.accept("Mensaje desde Consumer");
        System.out.println(saludo.get());
    }
}
```

### Explicación línea por línea

- `Predicate`: responde verdadero o falso.
- `Function`: transforma texto en número.
- `Consumer`: imprime un mensaje.
- `Supplier`: devuelve un texto sin recibir datos.
- `test`, `apply`, `accept`, `get`: métodos principales.

---

## 6.3 Streams

### Concepto literal en programación

Un stream permite procesar una secuencia de datos de forma declarativa.

```java
import java.util.List;

public class StreamsEjemplo {
    public static void main(String[] args) {
        List<String> nombres = List.of("ana", "andres", "luis", "alberto");

        nombres.stream()
               .filter(nombre -> nombre.startsWith("a"))
               .map(String::toUpperCase)
               .forEach(System.out::println);
    }
}
```

### Explicación línea por línea

- `stream()`: convierte la lista en flujo.
- `filter`: filtra nombres que empiezan con `a`.
- `map`: transforma a mayúsculas.
- `forEach`: consume cada resultado.

### Otro ejemplo con reducción

```java
import java.util.List;

public class ReduceEjemplo {
    public static void main(String[] args) {
        List<Integer> valores = List.of(1, 2, 3, 4);
        int suma = valores.stream().reduce(0, Integer::sum);
        System.out.println(suma);
    }
}
```

---

## 6.4 Optional

### Concepto literal en programación

`Optional` representa un valor que puede estar o no estar presente.

```java
import java.util.Optional;

public class OptionalEjemplo {
    public static void main(String[] args) {
        Optional<String> nombre = Optional.ofNullable(null);

        System.out.println(nombre.orElse("Desconocido"));
        nombre.ifPresent(System.out::println);
    }
}
```

### Explicación línea por línea

- `ofNullable(null)`: crea un Optional vacío.
- `orElse`: da un valor alternativo.
- `ifPresent`: ejecuta algo solo si hay valor.

---

## 6.5 Map avanzado

### Concepto literal en programación

`Map` permite almacenar pares clave-valor y recorrerlos de distintas formas.

```java
import java.util.HashMap;
import java.util.Map;

public class MapAvanzadoEjemplo {
    public static void main(String[] args) {
        Map<Long, String> productos = new HashMap<>();
        productos.put(1L, "Café");
        productos.put(2L, "Pan");
        productos.putIfAbsent(2L, "Leche");
        productos.replace(1L, "Café premium");

        System.out.println(productos.getOrDefault(3L, "No existe"));
        System.out.println(productos.containsKey(2L));
        System.out.println(productos.containsValue("Pan"));
        System.out.println(productos.keySet());
        System.out.println(productos.values());
        System.out.println(productos.entrySet());

        productos.forEach((id, nombre) -> System.out.println(id + " -> " + nombre));

        productos.computeIfAbsent(3L, id -> "Leche");
        System.out.println(productos);
        productos.remove(2L);
        System.out.println(productos.size());
        System.out.println(productos.isEmpty());
        productos.clear();
        System.out.println(productos.isEmpty());
    }
}
```

### Explicación línea por línea

- `put`: agrega o reemplaza.
- `putIfAbsent`: agrega solo si no existe.
- `replace`: reemplaza el valor de una clave.
- `getOrDefault`: devuelve un fallback.
- `containsKey` y `containsValue`: validan existencia.
- `keySet`, `values`, `entrySet`: vistas del mapa.
- `forEach`: recorre clave y valor.
- `computeIfAbsent`: crea valor si falta.
- `remove`, `size`, `isEmpty`, `clear`: manipulan el contenido.

---

## 6.6 Try, catch y finally en casos reales

### Concepto literal en programación

`try`, `catch` y `finally` permiten ejecutar código riesgoso, capturar errores y limpiar recursos.

```java
public class ManejoErroresEjemplo {
    public static void main(String[] args) {
        try {
            int numero = Integer.parseInt("abc");
            System.out.println(numero);
        } catch (NumberFormatException e) {
            System.out.println("No se pudo convertir el texto a número: " + e.getMessage());
        } finally {
            System.out.println("Bloque finally ejecutado siempre");
        }
    }
}
```

### Otro ejemplo

```java
public class DivisionEjemplo {
    public static void main(String[] args) {
        try {
            int resultado = 10 / 0;
            System.out.println(resultado);
        } catch (ArithmeticException e) {
            System.out.println("Error matemático: " + e.getMessage());
        } finally {
            System.out.println("Cierre seguro de la operación");
        }
    }
}
```

### Explicación línea por línea

- `try`: bloque principal.
- `catch`: captura el tipo de error.
- `finally`: se ejecuta siempre.
- `getMessage()`: muestra el mensaje del error.

---

## 6.7 Resumen final

### Concepto literal en programación

La programación funcional, `Map` avanzado y el manejo de errores completan una base sólida para escribir código más expresivo y seguro.

### Buenas prácticas

- Usa lambdas cuando simplifiquen el código.
- Usa streams para transformaciones de colecciones.
- Usa `Optional` para representar ausencia de valor.
- Usa `Map` cuando necesites búsquedas por clave.
- Usa `try/catch/finally` para operaciones con riesgo real.

---

## 🔗 Navegación de la serie

| Archivo | Contenido |
|---------|----------|
| [01-conceptos-spring-boot.md](./01-conceptos-spring-boot.md) | 📚 Conceptos de Spring Boot y microservicios |
| [02-ejemplos-spring-boot.md](./02-ejemplos-spring-boot.md) | 💻 Ejemplos prácticos de Spring Boot |
| [03-conceptos-java-poo-ciclos-colecciones.md](./03-conceptos-java-poo-ciclos-colecciones.md) | 🧠 Conceptos de Java: POO, ciclos y colecciones |
| [04-ejemplos-java-poo-ciclos-colecciones.md](./04-ejemplos-java-poo-ciclos-colecciones.md) | 💻 Ejemplos de Java: POO, ciclos y colecciones |
| [05-conceptos-java-funcional-errores-maps.md](./05-conceptos-java-funcional-errores-maps.md) | 🎯 Conceptos de Java: programación funcional y Map |
| **06-ejemplos-java-funcional-errores-maps.md** | 💻 Ejemplos de Java: programación funcional y Map |

---

## 🎓 ¿Qué sigue ahora?

¡Felicidades! 🎉 Ya tienes una base sólida de Java y Spring Boot. 

Desde aquí, puedes:
- ✅ Crear aplicaciones Spring Boot reales.
- ✅ Conectar a bases de datos con JPA/Hibernate.
- ✅ Agregar seguridad con Spring Security.
- ✅ Documentar APIs con Swagger.
- ✅ Desplegar en Docker y Kubernetes.
- ✅ Explorar testing con JUnit y Mockito.

**¡Sigue aprendiendo y construyendo cosas increíbles!** 🚀
