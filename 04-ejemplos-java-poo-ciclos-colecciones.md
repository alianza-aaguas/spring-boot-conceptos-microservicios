# 💻 4. Ejemplos de Java: POO, Ciclos y Colecciones
### Código práctico línea por línea

> 📘 Este documento es la **continuación práctica** de `03-conceptos-java-poo-ciclos-colecciones.md`.
> Aquí veremos código real con explicaciones detalladas de cada concepto.

---

## 📑 Índice

1. [Clase y objeto](#41-clase-y-objeto)
2. [Atributos y métodos](#42-atributos-y-métodos)
3. [Encapsulamiento](#43-encapsulamiento)
4. [Ciclos](#44-ciclos)
5. [List: Colección ordenada](#45-list-colección-ordenada)
6. [Set: Sin repetidos](#46-set-sin-repetidos)
7. [Map: Pares clave-valor](#47-map-pares-clave-valor)
8. [Try, catch y finally: Manejo de errores](#48-try-catch-y-finally-manejo-de-errores)
9. [Resumen y buenas prácticas](#49-resumen-y-buenas-prácticas)
10. [Navegación](#🔗-navegación-de-la-serie)

---

## 4.1 Clase y objeto

### Concepto literal en programación

Una clase define una plantilla y un objeto es una instancia concreta creada a partir de esa plantilla.

### Explicación sencilla

Si la clase es el molde, el objeto es la pieza ya hecha.

```java
public class Producto {
    String nombre;

    public Producto(String nombre) {
        this.nombre = nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Producto producto = new Producto("Café");
        System.out.println(producto.nombre);
    }
}
```

### Explicación línea por línea

- `class Producto`: define la clase.
- `String nombre`: atributo.
- `Producto(String nombre)`: constructor.
- `this.nombre = nombre`: asigna el valor recibido.
- `new Producto("Café")`: crea un objeto.

---

## 4.2 Atributos y métodos

### Concepto literal en programación

Los atributos guardan el estado y los métodos representan el comportamiento.

```java
public class CuentaBancaria {
    private double saldo;

    public void depositar(double monto) {
        saldo += monto;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

### Explicación línea por línea

- `private double saldo`: atributo encapsulado.
- `depositar`: método que modifica el estado.
- `getSaldo`: método que devuelve el saldo.

---

## 4.3 Encapsulamiento

### Concepto literal en programación

Encapsular es ocultar el estado interno y exponer acceso controlado.

```java
public class Usuario {
    private String nombre;

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        if (nombre != null && !nombre.isBlank()) {
            this.nombre = nombre;
        }
    }
}
```

### Explicación línea por línea

- `private String nombre`: no se modifica directamente.
- `getNombre`: permite leer.
- `setNombre`: permite cambiar con validación.
- `!nombre.isBlank()`: evita valores vacíos.

---

## 4.4 Ciclos

### Concepto literal en programación

Un ciclo repite instrucciones mientras se cumpla una condición o para cada elemento de una colección.

```java
public class CiclosEjemplo {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            System.out.println("For: " + i);
        }

        int contador = 1;
        while (contador <= 3) {
            System.out.println("While: " + contador);
            contador++;
        }

        int numero = 1;
        do {
            System.out.println("Do-while: " + numero);
            numero++;
        } while (numero <= 3);
    }
}
```

### Explicación línea por línea

- `for`: repite con contador.
- `while`: repite mientras la condición sea verdadera.
- `do`: ejecuta al menos una vez.

---

## 4.5 List: Colección ordenada

### Concepto literal en programación

`List` es una colección ordenada que permite repetidos.

```java
import java.util.ArrayList;
import java.util.List;

public class ListEjemplo {
    public static void main(String[] args) {
        List<String> productos = new ArrayList<>();
        productos.add("Café");
        productos.add("Pan");
        productos.add("Café");

        System.out.println(productos);
        System.out.println(productos.get(1));
        System.out.println(productos.size());
    }
}
```

### Explicación línea por línea

- `new ArrayList<>()`: crea la lista.
- `add`: agrega elementos.
- `get(1)`: obtiene por índice.
- `size()`: cuenta elementos.

---

## 4.6 Set: Sin repetidos

### Concepto literal en programación

`Set` no permite elementos duplicados.

```java
import java.util.HashSet;
import java.util.Set;

public class SetEjemplo {
    public static void main(String[] args) {
        Set<String> correos = new HashSet<>();
        correos.add("ana@mail.com");
        correos.add("luis@mail.com");
        correos.add("ana@mail.com");

        System.out.println(correos);
        System.out.println(correos.contains("luis@mail.com"));
    }
}
```

### Explicación línea por línea

- `HashSet`: implementación de `Set`.
- `add`: intenta agregar.
- El duplicado no se repite.
- `contains`: verifica existencia.

---

## 4.7 Map: Pares clave-valor

### Concepto literal en programación

`Map` guarda pares clave-valor.

```java
import java.util.HashMap;
import java.util.Map;

public class MapEjemplo {
    public static void main(String[] args) {
        Map<Long, String> productos = new HashMap<>();
        productos.put(1L, "Café");
        productos.put(2L, "Pan");

        System.out.println(productos.get(1L));
        System.out.println(productos.getOrDefault(3L, "No existe"));
        System.out.println(productos.containsKey(2L));
        System.out.println(productos.size());
    }
}
```

### Explicación línea por línea

- `put`: guarda clave y valor.
- `get`: busca por clave.
- `getOrDefault`: devuelve un valor alternativo si no existe.
- `containsKey`: pregunta si la clave existe.
- `size`: cuenta entradas.

### Más métodos útiles

```java
productos.remove(2L);
System.out.println(productos.keySet());
System.out.println(productos.values());
System.out.println(productos.entrySet());
System.out.println(productos.isEmpty());
productos.clear();
```

---

## 4.8 Try, catch y finally: Manejo de errores

### Concepto literal en programación

`try` intenta, `catch` captura el error y `finally` siempre se ejecuta.

```java
public class TryCatchFinallyEjemplo {
    public static void main(String[] args) {
        try {
            int resultado = 10 / 0;
            System.out.println(resultado);
        } catch (ArithmeticException e) {
            System.out.println("No se puede dividir entre cero: " + e.getMessage());
        } finally {
            System.out.println("Este bloque siempre se ejecuta");
        }
    }
}
```

### Explicación línea por línea

- `try`: bloque de intento.
- `10 / 0`: provoca error.
- `catch`: captura la excepción.
- `finally`: se ejecuta siempre.

---

## 4.9 Resumen y buenas prácticas

### Concepto literal en programación

Las clases modelan el dominio, las colecciones organizan datos y `try/catch/finally` controlan errores.

### Buenas prácticas

- Usa clases pequeñas y claras.
- Usa `List` para orden y repetidos.
- Usa `Set` para evitar duplicados.
- Usa `Map` para búsquedas por clave.
- Usa `try/catch/finally` solo cuando realmente haya riesgo de error.

---

## 🔗 Navegación de la serie

| Archivo | Contenido |
|---------|----------|
| [01-conceptos-spring-boot.md](./01-conceptos-spring-boot.md) | 📚 Conceptos de Spring Boot y microservicios |
| [02-ejemplos-spring-boot.md](./02-ejemplos-spring-boot.md) | 💻 Ejemplos prácticos de Spring Boot |
| [03-conceptos-java-poo-ciclos-colecciones.md](./03-conceptos-java-poo-ciclos-colecciones.md) | 🧠 Conceptos de Java: POO, ciclos y colecciones |
| **04-ejemplos-java-poo-ciclos-colecciones.md** | 💻 Ejemplos de Java: POO, ciclos y colecciones |
| [05-conceptos-java-funcional-errores-maps.md](./05-conceptos-java-funcional-errores-maps.md) | 🎯 Conceptos de Java: programación funcional y Map |
| [06-ejemplos-java-funcional-errores-maps.md](./06-ejemplos-java-funcional-errores-maps.md) | 💻 Ejemplos de Java: programación funcional y Map |

---

**👉 Próximo documento:** [Conceptos de programación funcional y Map](./05-conceptos-java-funcional-errores-maps.md)
