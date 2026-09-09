# 💻 Ejemplos Prácticos de Spring Boot y Microservicios
### Todos los conceptos "en acción", explicados línea por línea (con el concepto literal de programación en cada ejemplo)

> 📘 Este documento es la **continuación** de `01-conceptos-spring-boot.md`.
> Aquí veremos, con **código real y comentado**, cada uno de los conceptos que aprendiste, y en cada sección se incluye primero el **concepto literal en programación** antes de la explicación sencilla.

---

## 🏗️ El proyecto que vamos a construir

Vamos a crear un **microservicio de una tienda de productos** 🛒.
Nuestro "mesero digital" atenderá estas peticiones:

| Método HTTP | Ruta | ¿Qué hace? |
|---|---|---|
| `GET` | `/productos` | Devuelve todos los productos |
| `GET` | `/productos/{id}` | Devuelve un producto por su ID |
| `POST` | `/productos` | Crea un producto nuevo |
| `DELETE` | `/productos/{id}` | Borra un producto |

---

## 📂 Estructura del proyecto (cómo se organizan los archivos)

```
tiendra-microservicio/
│
├── pom.xml                          ← Lista de "ingredientes" (dependencias)
│
└── src/main/java/com/tienda/
    │
    ├── TiendaApplication.java       ← El "botón de encendido" 🔌
    │
    ├── controller/
    │   └── ProductoController.java  ← El mesero 🧑‍💼 (recibe peticiones)
    │
    ├── service/
    │   ├── ProductoService.java     ← Interfaz (el contrato / enchufe) 🔌
    │   └── ProductoServiceImpl.java ← El cocinero 👨‍🍳 (implementa el contrato)
    │
    ├── repository/
    │   └── ProductoRepository.java  ← El bodeguero 📦 (habla con la BD)
    │
    ├── model/
    │   └── Producto.java            ← El "molde" de un producto
    │
    └── exception/
        ├── ProductoNoEncontradoException.java   ← Alarma personalizada 🚨
        ├── SaldoInsuficienteException.java      ← Otra alarma 🚨
        └── ManejadorGlobalDeErrores.java        ← El "911" central 📞
```

---

## 1️⃣ El "botón de encendido": `TiendaApplication.java`

**Concepto literal en programación:** La clase anotada con `@SpringBootApplication` es el punto de entrada de la aplicación (entry point); combina `@Configuration`, `@EnableAutoConfiguration` y `@ComponentScan` para iniciar el contenedor de Spring Boot.

**Explicación sencilla:** Es como el botón de encendido de un televisor. Sin él, nada prende.

```java name=TiendaApplication.java
package com.tienda;

// Importamos las herramientas de Spring Boot que vamos a usar
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// 🏷️ Esta ETIQUETA (anotación) le dice a Spring: "Esta es la clase principal, empieza aquí"
@SpringBootApplication
public class TiendaApplication {

    // Este es el método que se ejecuta cuando prendemos la aplicación
    public static void main(String[] args) {
        // Le decimos a Spring: "Enciende la app usando esta clase como base"
        SpringApplication.run(TiendaApplication.class, args);
        // 🎉 En este momento arranca el servidor web (por defecto en el puerto 8080)
    }
}
```

---

## 2️⃣ El "molde" del producto: `Producto.java`

**Concepto literal en programación:** Una clase modelo representa la estructura de un objeto de dominio con atributos y, si hace falta, métodos; en este caso funciona como un modelo simple de datos.

**Explicación sencilla:** Es como el molde para hacer galletas 🍪. El molde define la forma; cada galleta (objeto) tendrá esa forma pero con sabores distintos.

```java name=Producto.java
package com.tienda.model;

// Esta clase es el "molde" de un producto
public class Producto {

    // Los atributos son las "características" del producto
    private Long id;           // Número único que identifica al producto
    private String nombre;     // Nombre del producto (ej: "Café")
    private Double precio;     // Precio del producto (ej: 3500.0)
    private Integer stock;     // Cuántos hay disponibles

    // Constructor vacío (útil para frameworks y serialización)
    public Producto() {}

    // Constructor con todos los datos (para crear productos rápido)
    public Producto(Long id, String nombre, Double precio, Integer stock) {
        this.id = id;
        this.nombre = nombre;
        this.precio = precio;
        this.stock = stock;
    }

    // 🔹 Getters y Setters: son "puertas" para leer y cambiar los datos (encapsulamiento)
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public Double getPrecio() { return precio; }
    public void setPrecio(Double precio) { this.precio = precio; }

    public Integer getStock() { return stock; }
    public void setStock(Integer stock) { this.stock = stock; }
}
```

📌 **Analogía:** Piensa en un formulario en blanco 📄 con campos nombre, precio y stock. Cada producto es un formulario llenado.

---

## 3️⃣ El "bodeguero": `ProductoRepository.java`

**Concepto literal en programación:** El repositorio es una capa de abstracción sobre el acceso a datos (patrón Repository), aislando la lógica de negocio de los detalles de persistencia.

**Explicación sencilla:** Es el encargado de la bodega 📦. Solo él sabe dónde están guardados los productos y cómo sacarlos.

Para simplificar, guardaremos los productos en **memoria** (una lista/mapa), no en base de datos real.

```java name=ProductoRepository.java
package com.tienda.repository;

import com.tienda.model.Producto;
import org.springframework.stereotype.Repository;
import java.util.*;

// 🏷️ ETIQUETA (anotación) que le dice a Spring: "Esta clase es el bodeguero (capa de datos)"
@Repository
public class ProductoRepository {

    // Nuestra "bodega" es un mapa: id → producto
    private final Map<Long, Producto> bodega = new HashMap<>();
    private Long siguienteId = 1L; // Contador para asignar IDs únicos

    // Constructor: llenamos la bodega con algunos productos de ejemplo
    public ProductoRepository() {
        guardar(new Producto(null, "Café", 3500.0, 10));
        guardar(new Producto(null, "Pan", 1500.0, 20));
    }

    // 📋 Traer TODOS los productos
    public List<Producto> buscarTodos() {
        return new ArrayList<>(bodega.values());
    }

    // 🔍 Buscar UN producto por ID (puede que no exista → Optional evita el null)
    public Optional<Producto> buscarPorId(Long id) {
        return Optional.ofNullable(bodega.get(id));
    }

    // 💾 Guardar un producto nuevo (o actualizar uno existente)
    public Producto guardar(Producto producto) {
        if (producto.getId() == null) {
            producto.setId(siguienteId++); // Le asignamos un ID único
        }
        bodega.put(producto.getId(), producto);
        return producto;
    }

    // ❌ Borrar un producto por ID
    public boolean borrar(Long id) {
        return bodega.remove(id) != null;
    }
}
```

---

## 4️⃣ El "contrato" (interfaz): `ProductoService.java`

**Concepto literal en programación:** Una interfaz define un conjunto de métodos abstractos sin cuerpo que actúan como contrato; cualquier clase que la implemente debe ofrecer esas operaciones.

**Explicación sencilla:** Es el enchufe de la pared ⚡. Dice: si quieres ser un servicio de productos, debes saber hacer estas cosas.

```java name=ProductoService.java
package com.tienda.service;

import com.tienda.model.Producto;
import java.util.List;

// 🔌 Esta es la INTERFAZ = el contrato = el enchufe
public interface ProductoService {

    // Cualquier clase que "se enchufe" aquí debe saber hacer estas 4 cosas:
    List<Producto> obtenerTodos();
    Producto obtenerPorId(Long id);
    Producto crear(Producto producto);
    void eliminar(Long id);
}
```

📌 **¿Por qué usar una interfaz?**
Porque mañana podrías tener dos tipos de servicios: uno con base de datos MySQL y otro con MongoDB. Ambos cumplen el mismo contrato, y el controller no necesita saber cuál está usando.

---

## 5️⃣ El "cocinero" (implementación): `ProductoServiceImpl.java`

**Concepto literal en programación:** La clase de implementación provee la lógica concreta de los métodos declarados en la interfaz y suele ser gestionada por el contenedor de Spring.

**Explicación sencilla:** Es el cocinero de verdad 👨‍🍳 que sabe cómo preparar los platos. Aquí también veremos inyección de dependencias y cómo lanzar excepciones.

```java name=ProductoServiceImpl.java
package com.tienda.service;

import com.tienda.model.Producto;
import com.tienda.repository.ProductoRepository;
import com.tienda.exception.ProductoNoEncontradoException;
import org.springframework.stereotype.Service;
import java.util.List;

// 🏷️ ETIQUETA (anotación): "Esta clase es un servicio de negocio (cocinero)"
@Service
public class ProductoServiceImpl implements ProductoService {

    // 💉 INYECCIÓN DE DEPENDENCIAS (Dependency Injection):
    // Le decimos a Spring: "Yo necesito un bodeguero, por favor tráemelo listo"
    private final ProductoRepository repositorio;

    // Constructor: aquí Spring nos inyecta el bodeguero automáticamente
    public ProductoServiceImpl(ProductoRepository repositorio) {
        this.repositorio = repositorio; // Ya tenemos al bodeguero disponible ✅
    }

    // 📋 Traer todos los productos
    @Override
    public List<Producto> obtenerTodos() {
        return repositorio.buscarTodos();
    }

    // 🔍 Traer un producto por ID
    @Override
    public Producto obtenerPorId(Long id) {
        // Le preguntamos al bodeguero. Si no existe, lanzamos la alarma personalizada 🚨
        return repositorio.buscarPorId(id)
            .orElseThrow(() -> new ProductoNoEncontradoException(
                "No se encontró el producto con ID: " + id
            ));
    }

    // ➕ Crear un producto nuevo
    @Override
    public Producto crear(Producto producto) {
        return repositorio.guardar(producto);
    }

    // ❌ Eliminar un producto
    @Override
    public void eliminar(Long id) {
        // Si no existe, lanzamos la excepción antes de intentar borrarlo
        if (!repositorio.borrar(id)) {
            throw new ProductoNoEncontradoException(
                "No se puede eliminar. Producto con ID " + id + " no existe."
            );
        }
    }
}
```

---

## 6️⃣ Las "alarmas" personalizadas (excepciones)

### 🚨 `ProductoNoEncontradoException.java`

**Concepto literal en programación:** Una excepción personalizada extiende `RuntimeException` y puede anotarse con `@ResponseStatus` para que Spring devuelva un código HTTP específico.

```java name=ProductoNoEncontradoException.java
package com.tienda.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

// 🏷️ ETIQUETA: "Cuando lancen esta alarma, responde con 404 NOT FOUND"
@ResponseStatus(HttpStatus.NOT_FOUND) // 404
public class ProductoNoEncontradoException extends RuntimeException {

    // Constructor que recibe el mensaje de la alarma
    public ProductoNoEncontradoException(String mensaje) {
        super(mensaje); // Le pasamos el mensaje a la clase padre (herencia)
    }
}
```

### 🚨 `SaldoInsuficienteException.java`

**Concepto literal en programación:** Esta es otra excepción personalizada que hereda de `RuntimeException` y utiliza `@ResponseStatus` para mapear un error de negocio a una respuesta HTTP.

```java name=SaldoInsuficienteException.java
package com.tienda.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

// 🏷️ Esta alarma responde con 402 PAYMENT REQUIRED (pago requerido)
@ResponseStatus(HttpStatus.PAYMENT_REQUIRED) // 402
public class SaldoInsuficienteException extends RuntimeException {

    public SaldoInsuficienteException(String mensaje) {
        super(mensaje);
    }
}
```

---

## 7️⃣ El "911 central": `ManejadorGlobalDeErrores.java`

**Concepto literal en programación:** Un `@ControllerAdvice` con métodos anotados `@ExceptionHandler` centraliza el manejo de excepciones a nivel global.

**Explicación sencilla:** Es un centro de emergencias 911 📞 que responde a todas las alarmas y decide qué mensaje enviar al cliente.

```java name=ManejadorGlobalDeErrores.java
package com.tienda.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;

// 🏷️ ETIQUETA (anotación): "Yo soy el 911 global de toda la aplicación"
@ControllerAdvice
public class ManejadorGlobalDeErrores {

    // 📞 Responde específicamente a la alarma "Producto no encontrado"
    @ExceptionHandler(ProductoNoEncontradoException.class)
    public ResponseEntity<Map<String, Object>> manejarProductoNoEncontrado(
            ProductoNoEncontradoException ex) {

        // Construimos una respuesta amigable
        Map<String, Object> respuesta = new HashMap<>();
        respuesta.put("fecha", LocalDateTime.now());
        respuesta.put("codigo", 404);
        respuesta.put("error", "Producto no encontrado");
        respuesta.put("mensaje", ex.getMessage());

        return new ResponseEntity<>(respuesta, HttpStatus.NOT_FOUND);
    }

    // 📞 Responde a la alarma "Saldo insuficiente"
    @ExceptionHandler(SaldoInsuficienteException.class)
    public ResponseEntity<Map<String, Object>> manejarSaldoInsuficiente(
            SaldoInsuficienteException ex) {

        Map<String, Object> respuesta = new HashMap<>();
        respuesta.put("fecha", LocalDateTime.now());
        respuesta.put("codigo", 402);
        respuesta.put("error", "Saldo insuficiente");
        respuesta.put("mensaje", ex.getMessage());

        return new ResponseEntity<>(respuesta, HttpStatus.PAYMENT_REQUIRED);
    }

    // 📞 Respuesta genérica: si pasa cualquier otro error inesperado
    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String, Object>> manejarErrorGeneral(Exception ex) {
        Map<String, Object> respuesta = new HashMap<>();
        respuesta.put("fecha", LocalDateTime.now());
        respuesta.put("codigo", 500);
        respuesta.put("error", "Error interno del servidor");
        respuesta.put("mensaje", ex.getMessage());

        return new ResponseEntity<>(respuesta, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

## 8️⃣ El "mesero": `ProductoController.java`

**Concepto literal en programación:** Un controlador REST anotado con `@RestController` expone endpoints HTTP mediante anotaciones de mapeo (`@GetMapping`, `@PostMapping`, `@DeleteMapping`) y devuelve datos al cliente.

**Explicación sencilla:** Es el mesero 🧑‍💼. Recibe pedidos del cliente, se los pasa al cocinero y trae la respuesta.

```java name=ProductoController.java
package com.tienda.controller;

import com.tienda.model.Producto;
import com.tienda.service.ProductoService;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import java.util.List;

// 🏷️ ETIQUETA (anotación): "Yo soy un mesero web que devuelve JSON"
@RestController
// 🏷️ Todas las peticiones que empiecen con /productos vienen a mí
@RequestMapping("/productos")
public class ProductoController {

    // 💉 Inyección de dependencias: le pedimos a Spring el "cocinero"
    private final ProductoService servicio;

    public ProductoController(ProductoService servicio) {
        this.servicio = servicio;
    }

    // 📋 GET /productos → devuelve todos los productos (200 OK)
    @GetMapping
    public List<Producto> obtenerTodos() {
        return servicio.obtenerTodos();
    }

    // 🔍 GET /productos/{id} → devuelve un producto (200 OK o 404 si no existe)
    @GetMapping("/{id}")
    public Producto obtenerPorId(@PathVariable Long id) {
        // Si no existe, el servicio lanza ProductoNoEncontradoException
        // → el 911 lo atrapa y responde 404 automáticamente ✨
        return servicio.obtenerPorId(id);
    }

    // ➕ POST /productos → crea un producto (201 CREATED)
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED) // 201 = creado
    public Producto crear(@RequestBody Producto producto) {
        // @RequestBody = "toma el JSON que envió el cliente y conviértelo en Producto"
        return servicio.crear(producto);
    }

    // ❌ DELETE /productos/{id} → borra un producto (204 NO CONTENT)
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT) // 204 = hecho, sin nada que devolver
    public void eliminar(@PathVariable Long id) {
        servicio.eliminar(id);
    }
}
```

---

## 9️⃣ Archivo de "ingredientes": `pom.xml`

**Concepto literal en programación:** El `pom.xml` es el archivo de configuración de Maven que declara dependencias, plugins y metadata del proyecto Java, gestionando la construcción y empaquetado de la aplicación.

**Explicación sencilla:** Es la lista de mercado 🛒. Le dice al programa qué ingredientes (librerías) necesita descargar.

```xml name=pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>

    <!-- Heredamos configuración base de Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>

    <groupId>com.tienda</groupId>
    <artifactId>tienda-microservicio</artifactId>
    <version>1.0.0</version>

    <properties>
        <java.version>21</java.version>
    </properties>

    <dependencies>
        <!-- Ingrediente principal: todo lo necesario para hacer un servicio web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Plugin que empaqueta la aplicación en un .jar ejecutable -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## 🧪 Probando el microservicio

**Concepto literal en programación:** Probar manualmente los endpoints consiste en enviar peticiones HTTP reales al servidor embebido y verificar que el código de estado, el cuerpo y el comportamiento sean correctos.

Una vez lo enciendes con `mvn spring-boot:run`, puedes usar el navegador o herramientas como **Postman** o **curl**.

### ✅ Caso 1: Traer todos los productos (200 OK)

```bash
GET http://localhost:8080/productos
```

**Respuesta:**
```json
[
  { "id": 1, "nombre": "Café", "precio": 3500.0, "stock": 10 },
  { "id": 2, "nombre": "Pan", "precio": 1500.0, "stock": 20 }
]
```

### ✅ Caso 2: Traer un producto que sí existe (200 OK)

```bash
GET http://localhost:8080/productos/1
```

**Respuesta:**
```json
{ "id": 1, "nombre": "Café", "precio": 3500.0, "stock": 10 }
```

### ❌ Caso 3: Traer un producto que no existe (404 NOT FOUND)

```bash
GET http://localhost:8080/productos/999
```

**Respuesta:**
```json
{
  "fecha": "2026-09-09T12:34:56",
  "codigo": 404,
  "error": "Producto no encontrado",
  "mensaje": "No se encontró el producto con ID: 999"
}
```

🎉 ¡Fíjate cómo la excepción personalizada + el manejador global producen una respuesta clara y profesional!

### ✅ Caso 4: Crear un producto (201 CREATED)

```bash
POST http://localhost:8080/productos
Content-Type: application/json

{ "nombre": "Leche", "precio": 4200.0, "stock": 15 }
```

**Respuesta:**
```json
{ "id": 3, "nombre": "Leche", "precio": 4200.0, "stock": 15 }
```

---

## 🔗 Cómo se conecta todo (flujo completo)

**Concepto literal en programación:** Este diagrama representa el flujo de una petición a través de las capas de una arquitectura en capas: controller, service, repository y manejo global de errores.

```text
      Cliente (Postman/Navegador)
              │
              │  GET /productos/999
              ▼
   ┌──────────────────────┐
   │ ProductoController   │   🧑‍💼 El mesero recibe la petición
   │  (@RestController)   │
   └──────────┬───────────┘
              │  llama a servicio.obtenerPorId(999)
              ▼
   ┌──────────────────────┐
   │ ProductoServiceImpl  │   👨‍🍳 El cocinero busca el producto
   │     (@Service)       │
   └──────────┬───────────┘
              │  llama a repositorio.buscarPorId(999)
              ▼
   ┌──────────────────────┐
   │ ProductoRepository   │   📦 El bodeguero busca en la bodega
   │   (@Repository)      │      → No lo encuentra
   └──────────┬───────────┘
              │
              ▼
     🚨 ProductoNoEncontradoException
              │
              ▼
   ┌──────────────────────┐
   │ ManejadorGlobal      │   📞 El 911 atrapa la alarma
   │ (@ControllerAdvice)  │      y responde 404 al cliente
   └──────────────────────┘
              │
              ▼
        Cliente recibe 404 con JSON amigable ✅
```

---

## 🎯 Resumen de qué se usó, dónde y su concepto literal

| Concepto | Concepto literal en programación | ¿Dónde lo vimos? |
|---|---|---|
| **Spring Boot arranque** | Punto de entrada que inicia el contenedor de Spring Boot | `@SpringBootApplication` en `TiendaApplication` |
| **Microservicio REST** | Arquitectura de servicios independientes comunicados por HTTP | Todo el proyecto expone endpoints HTTP |
| **Anotaciones** | Metadata declarativa procesada por el framework en tiempo de ejecución | `@RestController`, `@Service`, `@Repository`, `@RequestMapping`, `@GetMapping`, etc. |
| **Interfaz (contrato)** | Conjunto de métodos abstractos que definen un contrato | `ProductoService` |
| **Implementación** | Clase concreta que cumple el contrato de una interfaz | `ProductoServiceImpl implements ProductoService` |
| **Inyección de dependencias** | Patrón de diseño donde el contenedor provee las dependencias | Constructor en Service y Controller |
| **Excepciones personalizadas** | Clases que extienden `RuntimeException`/`Exception` para errores de dominio | `ProductoNoEncontradoException`, `SaldoInsuficienteException` |
| **Códigos HTTP personalizados** | Mapeo de excepciones/respuestas a códigos de estado HTTP específicos | `@ResponseStatus(HttpStatus.NOT_FOUND)`, `@ResponseStatus(HttpStatus.CREATED)`, etc. |
| **Manejo global de errores** | Interceptación centralizada de excepciones | `@ControllerAdvice` + `@ExceptionHandler` |

---

## 🚀 ¿Qué aprendiste?

Con estos ejemplos ya viste en código real cómo:

1. ✅ Se enciende un microservicio con Spring Boot.
2. ✅ Se organizan las capas (Controller → Service → Repository).
3. ✅ Se usan anotaciones como etiquetas para que Spring entienda cada clase.
4. ✅ Una interfaz define un contrato, y la implementación cumple ese contrato.
5. ✅ Spring inyecta automáticamente las dependencias.
6. ✅ Se crean excepciones personalizadas para representar errores del negocio.
7. ✅ Se lanzan códigos HTTP personalizados de forma elegante.
8. ✅ Un manejador global centraliza y estandariza las respuestas de error.

---

## 🌟 Siguientes pasos (si quieres seguir aprendiendo)

- Conectar a una base de datos real (MySQL, PostgreSQL) con Spring Data JPA.
- Agregar validaciones con `@Valid` y `@NotNull`.
- Proteger el microservicio con Spring Security (autenticación y autorización).
- Documentar la API con Swagger / OpenAPI.
- Comunicar varios microservicios entre sí con Feign Client o RestTemplate.
- Desplegar el microservicio en Docker y Kubernetes.

---

**¡Felicidades! 🎉 Ya entiendes cómo se construye un microservicio profesional con Spring Boot.**
