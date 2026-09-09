# 🌱 1. Conceptos Profundos de Spring Boot y Microservicios
### Explicado para personas que **no saben programar**

---

## 📑 Índice

1. [Introducción](#🧭-introducción-qué-es-programar-un-servicio-web)
2. [¿Qué es Java y qué es Spring Boot?](#1-☕-¿qué-es-java-y-qué-es-spring-boot)
3. [¿Qué es un Microservicio?](#2-🧩-¿qué-es-un-microservicio)
4. [¿Qué es HTTP y los códigos que se lanzan?](#3-📮-¿qué-es-http-y-los-códigos-que-se-lanzan)
5. [¿Qué es una Anotación?](#4-🏷️-¿qué-es-una-anotación-annotation)
6. [¿Qué es una Interfaz?](#5-🔌-¿qué-es-una-interfaz-interface)
7. [¿Qué es la Inyección de Dependencias?](#6-💉-¿qué-es-la-inyección-de-dependencias-di)
8. [Manejo de Excepciones](#7-🚨-manejo-de-excepciones)
9. [Excepciones Personalizadas](#8-🎨-excepciones-personalizadas)
10. [Lanzar Códigos HTTP Personalizados](#9-🌐-lanzar-códigos-http-personalizados)
11. [Resumen mental](#10-🧠-resumen-mental-para-no-perderte)
12. [Conclusión](#✅-con-esto-ya-entiendes)

---

## 🧭 Introducción: ¿Qué es programar un "servicio web"?

Imagina un **restaurante**:
- Tú (el **cliente**) llegas y le pides algo al mesero.
- El mesero lleva tu pedido a la cocina.
- La cocina prepara tu plato y te lo devuelve.

En internet pasa lo mismo:
- Tu celular o navegador es el **cliente**.
- El **servidor** es la cocina.
- El **servicio web** es el mesero: recibe pedidos (peticiones) y devuelve respuestas.

Programar un servicio web significa escribir las instrucciones para que esa "cocina digital" sepa qué hacer cuando alguien le pide algo.

**Idea clave:** el cliente pide, el servidor procesa y el servicio responde.

---

## 1. ☕ ¿Qué es Java y qué es Spring Boot?

### Java
**Concepto literal en programación:** Java es un lenguaje de programación orientado a objetos, compilado a bytecode que se ejecuta sobre la Máquina Virtual de Java (JVM).

**Explicación sencilla:** Java es un idioma que usamos para hablarle a la computadora. Así como el español sirve para que dos personas se entiendan, Java sirve para que un programador y una máquina se entiendan.

**Idea importante:** Java no solo es un lenguaje; también tiene una plataforma de ejecución muy usada en empresas.

### Spring Boot
**Concepto literal en programación:** Spring Boot es un framework de Java basado en Spring que permite crear aplicaciones autónomas y listas para producción con configuración mínima, usando convenciones y embebiendo un servidor web.

**Explicación sencilla:** Imagina que quieres construir una casa. Podrías fabricar cada ladrillo y cada puerta desde cero, o podrías usar una base ya preparada que te ahorra muchísimo trabajo. Spring Boot es eso: una base preparada.

👉 **Spring Boot es una base preparada para construir aplicaciones web en Java.**

**¿Qué aporta realmente?**
- Menos configuración manual.
- Arranque más rápido del proyecto.
- Integración sencilla con web, seguridad, base de datos y pruebas.
- Convenciones que evitan repetir trabajo.

---

## 2. 🧩 ¿Qué es un Microservicio?

**Concepto literal en programación:** Un microservicio es un estilo arquitectónico donde una aplicación se estructura como un conjunto de servicios pequeños, independientes, desplegables por separado y altamente desacoplados.

**Explicación sencilla:** Piensa en un **centro comercial**:
- Hay una tienda de ropa.
- Hay una tienda de comida.
- Hay un cine.

Cada tienda funciona por su cuenta, tiene su propio personal y sus propias reglas. Si el cine cierra, la tienda de ropa sigue funcionando.

👉 Un microservicio es como una de esas tiendas: hace una sola cosa muy bien y puede vivir sin que todo lo demás dependa de él.

### Comparación:
| Aplicación tradicional (monolito) | Microservicios |
|---|---|
| Una sola aplicación grande | Muchas aplicaciones pequeñas |
| Todo se despliega junto | Cada servicio se despliega por separado |
| Si falla una parte, puede caer todo | Si falla uno, los demás pueden seguir |
| Más simple de iniciar | Más compleja de operar |

**Importante:** microservicios no significa "más fácil siempre"; significa "más flexible, pero también más complejo de gestionar".

---

## 3. 📮 ¿Qué es HTTP y los "códigos" que se lanzan?

**Concepto literal en programación:** HTTP (HyperText Transfer Protocol) es el protocolo de comunicación cliente-servidor sobre el cual se intercambian mensajes en la web, usando métodos como GET, POST, PUT, PATCH y DELETE.

**Explicación sencilla:** HTTP es el lenguaje de intercambio de mensajes en internet. Cuando pides algo, envías una petición, y cuando el servidor responde, te devuelve un mensaje con información y un número.

### Ciclo básico request/response
```text
Cliente -> Petición HTTP -> Servidor
Cliente <- Respuesta HTTP <- Servidor
```

### Métodos HTTP más comunes
- `GET`: consultar información.
- `POST`: crear algo nuevo.
- `PUT`: reemplazar o actualizar completamente.
- `PATCH`: actualizar parcialmente.
- `DELETE`: eliminar algo.

### Códigos HTTP comunes
| Código | Significado sencillo | Analogía |
|---|---|---|
| **200 OK** | Todo salió bien | "Aquí tienes tu pedido" |
| **201 Created** | Se creó algo nuevo | "Ya quedó registrado" |
| **204 No Content** | Se procesó bien, pero no hay contenido | "Todo bien, no hay nada extra que mostrar" |
| **400 Bad Request** | Pediste algo mal | "No entiendo tu pedido" |
| **401 Unauthorized** | No te identificaste | "¿Y tú quién eres?" |
| **403 Forbidden** | No tienes permiso | "No puedes entrar aquí" |
| **404 Not Found** | No existe lo que buscas | "Aquí no vive esa persona" |
| **409 Conflict** | Hay un conflicto con el estado actual | "Ya existe algo así" |
| **422 Unprocessable Entity** | Entendí el pedido, pero no lo puedo procesar | "La información está mal formada" |
| **500 Internal Server Error** | Falló la aplicación | "Se quemó la cocina" |
| **503 Service Unavailable** | El servicio no está disponible | "La cocina está cerrada" |

---

## 4. 🏷️ ¿Qué es una Anotación (Annotation)?

**Concepto literal en programación:** Una anotación es una forma de metadata que se añade al código fuente para proporcionar información al compilador o al framework en tiempo de ejecución, sin afectar directamente la lógica del código.

**Explicación sencilla:** Imagina que organizas cajas y les pegas etiquetas de colores:
- Roja = frágil
- Verde = comida
- Azul = ropa

Las etiquetas no cambian el contenido de la caja, pero le dicen a los demás cómo tratarla.

👉 En Spring Boot, una anotación es como una etiqueta que le dice al framework: "trata este código de una manera especial".

### Ejemplos comunes de anotaciones en Spring Boot
- `@RestController` → indica que la clase responde a peticiones web.
- `@Service` → indica que la clase contiene lógica de negocio.
- `@Repository` → indica que la clase accede a datos.
- `@Autowired` → indica que Spring debe inyectar una dependencia automáticamente.
- `@RequestMapping` / `@GetMapping` / `@PostMapping` → indican rutas HTTP.

---

## 5. 🔌 ¿Qué es una Interfaz (Interface)?

**Concepto literal en programación:** Una interfaz es un contrato que define un conjunto de métodos que una clase debe implementar, sin especificar cómo se implementan.

**Explicación sencilla:** Piensa en un enchufe de la pared.
- No le importa si conectarás un televisor, una licuadora o un cargador.
- Solo define una forma estándar de conectarse.

👉 Una interfaz es un contrato: dice qué se debe hacer, pero no cómo hacerlo.

**¿Para qué sirve?**
Para poder cambiar la implementación sin cambiar el código que la usa.

**Ejemplo de idea:** hoy puedes usar un proveedor de pagos, mañana otro, y el resto del sistema sigue funcionando igual.

---

## 6. 💉 ¿Qué es la Inyección de Dependencias (DI)?

**Concepto literal en programación:** La Inyección de Dependencias es un patrón de diseño en el que un objeto recibe las dependencias que necesita desde el exterior, normalmente a través de un constructor, setter o anotación.

**Explicación sencilla:** Imagina que eres un chef y necesitas cuchillos para cocinar.

**Sin inyección de dependencias:**
Tú mismo tendrías que salir a fabricar tus cuchillos.

**Con inyección de dependencias:**
Un asistente te entrega los cuchillos listos para usar.

👉 En Spring Boot, ese asistente es el contenedor de Spring. Él crea los objetos, los guarda y los entrega donde se necesitan.

**Términos útiles:**
- **Bean**: objeto administrado por Spring.
- **ApplicationContext**: el contenedor donde Spring gestiona esos objetos.

**¿Por qué es útil?**
- Reduce acoplamiento.
- Facilita pruebas.
- Centraliza la creación y configuración de objetos.

---

## 7. 🚨 Manejo de Excepciones

**Concepto literal en programación:** Una excepción es un evento anómalo que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de instrucciones. El manejo de excepciones es el mecanismo para capturar, procesar y recuperarse de esos errores.

**Explicación sencilla:** Imagina que vas en carro y de repente se pincha una llanta o se acaba la gasolina. Son imprevistos. Si no tienes plan, te quedas varado. Si tienes un plan, puedes reaccionar.

👉 Manejar excepciones es tener un plan B cuando algo sale mal.

### Tipos de imprevistos en un servicio web
- El usuario pide un producto que no existe → responder `404`.
- El usuario envía datos incompletos → responder `400` o `422`.
- La base de datos está caída → responder `500` o `503`.

### Idea importante
No todas las excepciones significan lo mismo. Algunas son errores de validación, otras son errores del sistema, y otras son problemas de infraestructura.

---

## 8. 🎨 Excepciones Personalizadas

**Concepto literal en programación:** Una excepción personalizada es una clase creada por el desarrollador que extiende de `Exception` o `RuntimeException`, permitiendo representar errores específicos del dominio de negocio.

**Explicación sencilla:** Java trae alarmas generales como "algo salió mal", pero eso es muy vago. Es mejor tener alarmas específicas para saber exactamente qué pasó.

👉 Las excepciones personalizadas sirven para representar errores de negocio con nombres claros:
- `ProductoNoEncontradoException`
- `SaldoInsuficienteException`
- `UsuarioBloqueadoException`

**¿Por qué son útiles?**
- Hacen el error más entendible.
- Permiten responder con un HTTP correcto.
- Mejoran el mantenimiento del sistema.

---

## 9. 🌐 Lanzar Códigos HTTP Personalizados

**Concepto literal en programación:** En Spring Boot, se pueden asociar excepciones con códigos de estado HTTP específicos mediante `@ResponseStatus` o mediante un manejador global como `@ControllerAdvice`.

**Explicación sencilla:** Cuando algo falla en tu servicio, tú decides qué número le devuelves al cliente.

Ejemplo:
- Si buscan un producto que no existe → `404 Not Found`.
- Si los datos no son válidos → `400 Bad Request` o `422 Unprocessable Entity`.
- Si no hay stock → `409 Conflict` o un código de negocio definido por el equipo.

👉 Es como decirle al mesero: "si no hay el plato, no te calles; responde con educación y claridad".

**Importante:** en arquitectura real, elegir el código correcto ayuda a que el cliente entienda qué pasó y pueda reaccionar mejor.

---

## 10. 🧠 Resumen mental (para no perderte)

| Concepto | Analogía simple |
|---|---|
| **Spring Boot** | Base preparada para construir apps Java |
| **Microservicio** | Tienda pequeña especializada |
| **HTTP** | Sistema de mensajes entre cliente y servidor |
| **Códigos HTTP** | Números que explican el resultado |
| **Anotación** | Etiqueta que guía al framework |
| **Interfaz** | Contrato estándar |
| **Inyección de dependencias** | Asistente que entrega objetos listos |
| **Excepción** | Imprevisto que rompe el flujo normal |
| **Excepción personalizada** | Alarma específica del negocio |
| **Lanzar código HTTP** | Decidir la respuesta correcta para el cliente |

---

## ✅ Con esto ya entiendes:
- Qué es Spring Boot y para qué sirve.
- Qué es un microservicio y qué lo hace distinto de un monolito.
- Cómo funciona HTTP en un servicio web.
- Qué papel cumplen las anotaciones, interfaces e inyección de dependencias.
- Cómo manejar errores de forma más ordenada.
- Cómo traducir errores del negocio a respuestas HTTP claras.

---

## 🔗 Navegación de la serie

| Archivo | Contenido |
|---------|----------|
| **01-conceptos-spring-boot.md** | 📚 Conceptos de Spring Boot y microservicios |
| [02-ejemplos-spring-boot.md](./02-ejemplos-spring-boot.md) | 💻 Ejemplos prácticos de Spring Boot |
| [03-conceptos-java-poo-ciclos-colecciones.md](./03-conceptos-java-poo-ciclos-colecciones.md) | 🧠 Conceptos de Java: POO, ciclos y colecciones |
| [04-ejemplos-java-poo-ciclos-colecciones.md](./04-ejemplos-java-poo-ciclos-colecciones.md) | 💻 Ejemplos de Java: POO, ciclos y colecciones |
| [05-conceptos-java-funcional-errores-maps.md](./05-conceptos-java-funcional-errores-maps.md) | 🎯 Conceptos de Java: programación funcional y Map |
| [06-ejemplos-java-funcional-errores-maps.md](./06-ejemplos-java-funcional-errores-maps.md) | 💻 Ejemplos de Java: programación funcional y Map |

---

**👉 Próximo documento:** [ejemplos de código reales, comentados línea por línea](./02-ejemplos-spring-boot.md)
