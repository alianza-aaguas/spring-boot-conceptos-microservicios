# 🎯 5. Conceptos de Java: Programación Funcional, Errores y Map
### Explicado para personas que **no saben programar**

---

## 🧭 Introducción: ¿Qué es "funcional" en programación?

Imagina que tienes una **fábrica de pasteles**:
- **Forma tradicional (POO):** Haces una clase `FábricadePasteles` con todas las máquinas, recetas y reglas adentro. Es un bloque grande.
- **Forma funcional:** Tienes recetas pequeñas e independientes que puedes combinar: una para mezclar, otra para hornear, otra para decorar.

👉 **La programación funcional es pensar en transformaciones pequeñas y reutilizables en lugar de objetos gigantes.**

**Idea clave:** en lugar de "qué es" (objeto), piensas en "qué hace" (transformación).

---

## 📑 Índice

1. [Programación funcional](#51-qué-es-la-programación-funcional)
2. [Lambda](#52-lambda)
3. [Interfaces funcionales](#53-interfaces-funcionales)
4. [Predicate](#54-predicate)
5. [Function](#55-function)
6. [Consumer](#56-consumer)
7. [Supplier](#57-supplier)
8. [Streams](#58-streams)
9. [Optional](#59-optional)
10. [Map en profundidad](#510-map-en-más-profundidad)
11. [Errores y buenas prácticas](#511-errores-y-buenas-prácticas)
12. [Conclusiones](#512-conclusiones)

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
- Recoges tu bolsa de basura.
- El camión se la lleva.
- No devuelve nada.

```
Basura → [Consumer: llevar al basurero] → [Acción completada, sin retorno]
```

**Ejemplo de uso:**
```java
Consumer<String> imprimir = mensaje -> System.out.println(mensaje);
imprimir.accept("Hola mundo"); // Imprime el mensaje, no devuelve nada
```

**Casos de uso real:**
- Registrar logs.
- Enviar correos.
- Guardar algo en la base de datos.
- Notificar al usuario.

---

## 5.7 📦 Supplier: "Proveedor de datos"

**Concepto literal en programación:** `Supplier<T>` no recibe argumentos y devuelve un valor de tipo `T`.

**Explicación sencilla:** Es un "proveedor". Llamas → te entrega algo. Sin pedir nada a cambio.

**Analogía:**
Imagina un **dispensador automático de agua**:
- No le pides con palabras (no da argumentos).
- Solo presionas un botón.
- Te entrega agua (devuelve).

```
[Acción: presionar] → [Supplier: obtener agua] → Agua
```

**Ejemplo de uso:**
```java
Supplier<Double> precioActual = () -> Math.random() * 100;
Double precio = precioActual.get(); // Obtiene un precio aleatorio
```

**Casos de uso real:**
- Generar IDs únicos.
- Obtener la hora actual.
- Leer configuración del sistema.
- Crear objetos por defecto.

---

## 5.8 🚰 Streams: "Tubería de procesamiento"

**Concepto literal en programación:** Un stream es una secuencia de elementos sobre la que aplicas operaciones intermedias (que devuelven otro stream) y operaciones terminales (que dan el resultado final).

**Explicación sencilla:** Es una "tubería" por donde pasan datos, cada tubería puede filtrar, transformar o contar.

**Analogía:**
Imagina una **cadena de producción de agua embotellada**:
1. Agua bruta entra. ← **Fuente**
2. **Filtro 1:** quita sedimentos. ← **Operación intermedia**
3. **Filtro 2:** quita químicos. ← **Operación intermedia**
4. **Empacador:** empacan en botellas. ← **Operación terminal**
5. Botellas listas para vender. ← **Resultado**

```
Lista original 
  → filtrar (mayores a 18)
  → transformar (agregar descuento)
  → contar
  → Resultado
```

**Operaciones intermedias:**
- `filter()`: solo deja pasar los que cumplen la condición.
- `map()`: transforma cada elemento.
- `sorted()`: ordena.
- `distinct()`: quita duplicados.

**Operaciones terminales:**
- `collect()`: recopila en una colección.
- `count()`: cuenta.
- `findFirst()`: obtiene el primero.
- `forEach()`: hace algo con cada uno.

**Ejemplo visual:**
```java
List<Producto> productos = lista;
List<Producto> resultado = productos.stream()     // Abre tubería
    .filter(p -> p.getPrecio() > 100)              // Filtra
    .map(p -> p.conDescuento())                    // Transforma
    .sorted(Comparator.comparing(Producto::getNombre))  // Ordena
    .collect(Collectors.toList());                 // Recopila (terminal)
```

**¿Por qué es útil?**
- Código más limpio y legible.
- Puedes encadenar operaciones.
- Menos código boilerplate.

---

## 5.9 🎁 Optional: "Quizás hay algo, quizás no"

**Concepto literal en programación:** `Optional<T>` es un contenedor que encapsula un valor que puede existir o no, evitando trabajar directamente con `null`.

**Explicación sencilla:** Es una "caja de sorpresa" que puede estar vacía o tener algo adentro. Evitas el miedo a abrirla y encontrar `null`.

**Analogía:**
Imagina que **esperas un paquete por correo**:
- Puede llegar (tiene valor).
- Puede no llegar (está vacío).

En lugar de asumir que llegó y abrir nada, pregunta primero: "¿llegó?"

```
¿Hay un usuario con ese ID?
  → Sí: aquí está
  → No: está vacío

Sin Optional: 
  Usuario usuario = buscar(id);  // ¿Qué pasa si es null? 💥

Con Optional:
  Optional<Usuario> usuario = buscar(id);
  if (usuario.isPresent()) {
    // Usa usuario.get()
  }
```

**Métodos útiles:**
- `isPresent()`: ¿hay algo?
- `get()`: dame lo que hay (cuidado si está vacío).
- `orElse(valor)`: dame lo que hay, o esto por defecto.
- `ifPresent(acción)`: si hay algo, haz esto con ello.

**Ejemplo:**
```java
Optional<Producto> producto = buscarPorId(5);
producto.ifPresent(p -> System.out.println(p.getNombre()));
// Si existe, imprime. Si no, no hace nada.
```

**¿Por qué es útil?**
- Evita `NullPointerException`.
- Obliga a pensar en qué pasa si "no hay".
- El código es más seguro y legible.

---

## 5.10 🗂️ Map en Más Profundidad

**Concepto literal en programación:** `Map` es una estructura que almacena pares clave-valor. Cada clave es única y se usa para acceder rápidamente a su valor asociado.

**Explicación sencilla:** Es como un **diccionario telefónico** o un **archivo clasificado**.

**Analogía:**
Imagina una **biblioteca**:
- **Clave:** el título del libro (único).
- **Valor:** el contenido del libro.
- Accedes por título → encuentras el libro.

```
Map (Diccionario):
"manzana" → 5 (cantidad)
"plátano" → 3
"naranja" → 8
```

### Métodos Importantes de Map

#### Insertar y Acceder
- `put(clave, valor)`: agregar un par clave-valor.
- `get(clave)`: obtener el valor de una clave.
- `getOrDefault(clave, default)`: obtener o devolver un valor por defecto.

#### Verificar
- `containsKey(clave)`: ¿existe esta clave?
- `containsValue(valor)`: ¿existe este valor?

#### Modificar
- `remove(clave)`: eliminar un par.
- `replace(clave, nuevoValor)`: reemplazar un valor.
- `putIfAbsent(clave, valor)`: agregar solo si la clave no existe.
- `computeIfAbsent(clave, función)`: si no existe, crea el valor usando una función.

#### Recorrer
- `keySet()`: obtener todas las claves.
- `values()`: obtener todos los valores.
- `entrySet()`: obtener pares clave-valor.
- `forEach(acción)`: hacer algo con cada par.

#### Consultar
- `size()`: cantidad de pares.
- `isEmpty()`: ¿está vacío?

#### Limpiar
- `clear()`: eliminar todo.

**Ejemplo de uso real:**
```java
Map<String, Integer> carrito = new HashMap<>();

// Insertar
carrito.put("Manzana", 5);
carrito.put("Plátano", 3);

// Acceder
Integer cantidad = carrito.get("Manzana"); // 5

// Verificar
if (carrito.containsKey("Naranja")) {
    System.out.println("Tenemos naranjas");
} else {
    System.out.println("No tenemos");
}

// Modificar
carrito.replace("Plátano", 5);

// Recorrer
carrito.forEach((fruta, cant) -> System.out.println(fruta + ": " + cant));

// Limpiar
carrito.clear();
```

**¿Por qué es útil?**
- Acceso rápido por clave.
- Perfecto para asociaciones lógicas.
- Usado en cachés, configuraciones y muchas más aplicaciones.

---

## 5.11 🚨 Errores y Buenas Prácticas

**Concepto literal en programación:** Un error (excepción) es un evento anómalo que interrumpe el flujo normal. Las buenas prácticas garantizan que se maneje de forma estructurada, con mensajes claros y limpieza cuando sea necesario.

**Explicación sencilla:** Si algo falla, el programa debe reaccionar de forma **ordenada y predecible**, no quedarse congelado.

**Analogía:**
Imagina que **conducir un auto**:
- **Sin manejo de errores:** el motor se apaga → auto se queda en la carretera → caos.
- **Con manejo de errores:** el motor se apaga → activas plan de emergencia → enciendes las luces → llamas grúa → cosas en orden.

### Tipos de Errores Comunes

| Error | Analogía | Ejemplo | Manejo |
|---|---|---|---|
| **División entre cero** | Dividir 1 pastel entre 0 personas (sin sentido) | `10 / 0` | Validar antes |
| **Archivo no existe** | Buscar un libro en la biblioteca, no está | `leer_archivo("inexistente.txt")` | Capturar y notificar |
| **Conversión inválida** | Intentar envasar jugo en una botella de vidrio frágil | `Integer.parseInt("abc")` | Validar entrada |
| **Base de datos caída** | Ir al banco y encontrarlo cerrado | Consulta a BD desconectada | Reintentar o fallar graceful |
| **Null Pointer** | Abrir una caja vacía esperando un regalo | `objeto.metodo()` si `objeto` es null | Usar Optional o validar |

### Buenas Prácticas

1. **Validar entrada:** verifica que los datos sean correctos antes de usarlos.
   ```java
   if (precio < 0) {
       throw new IllegalArgumentException("Precio no puede ser negativo");
   }
   ```

2. **Capturar específicamente:** no atrapes todas las excepciones genéricamente.
   ```java
   try {
       int resultado = 10 / numero;
   } catch (ArithmeticException e) {
       System.out.println("No puedes dividir entre cero");
   }
   ```

3. **Limpiar recursos:** si abres un archivo, ciérralo (incluso si falla).
   ```java
   try (BufferedReader br = new BufferedReader(new FileReader("archivo.txt"))) {
       // Usar archivo
   } catch (IOException e) {
       System.out.println("Error al leer archivo");
   }
   // El archivo se cierra automáticamente
   ```

4. **Mensajes claros:** explica qué salió mal.
   ```java
   throw new ProductoNoEncontradoException(
       "El producto con ID " + id + " no existe en la base de datos"
   );
   ```

5. **No ocultes errores:** no hagas nada silenciosamente cuando falla algo.
   ```java
   // ❌ Malo
   try {
       operacion();
   } catch (Exception e) {
       // Nada
   }

   // ✅ Bueno
   try {
       operacion();
   } catch (IOException e) {
       logger.error("Error al procesar archivo", e);
       throw new ProcessingException("No se pudo procesar", e);
   }
   ```

---

## 5.12 ✅ Conclusiones

**La programación funcional y POO no son enemigas; se complementan perfectamente.**

| Cuando usar... | Razón |
|---|---|
| **POO (Objetos)** | Para estructurar datos complejos (Usuario, Producto, Pedido) |
| **Programación funcional** | Para transformar y procesar colecciones (filtrar, mapear, reducir) |
| **Maps** | Para asociaciones clave-valor rápidas y lógicas |
| **Streams** | Para operaciones en cadena sobre colecciones |
| **Optional** | Para evitar `null` y ser explícito con "quizás" |
| **Lambda** | Para funciones pequeñas y únicas que usas una sola vez |
| **Manejo de errores** | Para que el programa reaccione de forma ordenada ante lo inesperado |

### Idea Final

En Java moderno, combines:
- **POO:** estructuras.
- **Programación funcional:** transformaciones.
- **Errores controlados:** resiliencia.

Esto te permite escribir código que es:
- ✅ Claro y mantenible.
- ✅ Seguro ante fallos.
- ✅ Reutilizable.
- ✅ Escalable.

---

**👉 Próximo documento:** [Ejemplos prácticos de programación funcional, errores y Map con código real](./06-ejemplos-java-funcional-errores-maps.md)
