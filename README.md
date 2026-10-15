# Entrega del laboratorio HTTP

## Identificación

- Alumno/a: Sara Romero Peralta
- Fecha: 15/10/2026
- Java: Eclipse Temurin 25.0.4.1 LTS
- Maven Wrapper: Apache Maven 3.9.11
- Herramienta HTTP utilizada: cURL (curl.exe) en PowerShell

## 1. Representaciones

| Endpoint | Estado | Content-Type | Método Java | Explicación |
|---|---:|---|---|---|
| `/` | 200 | `text/html;charset=UTF-8` | `home()` | Devuelve una cadena que contiene el documento HTML. La anotación `produces = MediaType.TEXT_HTML_VALUE` indica que la representación es HTML. |
| `/api/hello` | 200 | `application/json` | `hello()` | Devuelve un `Map<String, String>` con el mensaje. Spring convierte el mapa a JSON mediante el conversor HTTP correspondiente. |
| `/health` | 200 | `application/json` | `health()` | Devuelve un mapa con el estado `UP`, convertido a JSON por Spring. |

## 2. Trazas y errores

### Traza de `GET /api/hello`

```http
> GET /api/hello HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/8.21.0
> Accept: */*

< HTTP/1.1 200
< Content-Type: application/json
< Content-Length: 36

{"message":"Hola desde el servidor"}
```

**1. Método, destino, campos y contenido:**

La petición utiliza el método `GET` y tiene como destino `/api/hello` en el servidor local `localhost:8080`. Incluye las cabeceras `Host`, `User-Agent` y `Accept`. No contiene cuerpo, ya que se solicita una representación mediante `GET` sin enviar datos en la petición.

**2. Estados observados:**

- `GET /api/hello`: `200`, la petición se ha procesado correctamente y devuelve un objeto JSON.
- `HEAD /api/hello`: `200`, devuelve las cabeceras de la respuesta sin incluir el cuerpo. Se observan `Content-Type: application/json` y `Content-Length: 36`.
- `OPTIONS /api/hello`: `200`, devuelve la cabecera `Allow: GET,HEAD,OPTIONS`, que anuncia los métodos permitidos para esa ruta.
- `GET /no-existe`: `404 Not Found`, indica que la ruta solicitada no existe. La respuesta contiene un objeto JSON con la fecha, el estado, el mensaje de error y la ruta solicitada.

**3. Resultado de OPTIONS:**

La petición `OPTIONS /api/hello` devuelve el estado `200` y la cabecera `Allow: GET,HEAD,OPTIONS`. Esto permite conocer los métodos que el servidor anuncia para esa ruta. La respuesta no contiene cuerpo (`Content-Length: 0`).

**4. Información que no debe aparecer en errores:**

Los errores no deberían revelar contraseñas, tokens, cookies de sesión, credenciales, variables de entorno, rutas internas innecesarias ni trazas completas de excepciones que puedan facilitar ataques. Es preferible devolver mensajes genéricos al cliente y guardar los detalles técnicos necesarios en registros protegidos del servidor. En las trazas compartidas deben sustituirse los secretos por `<REDACTED>`.

## 3. Frontera cliente-servidor

**1. Trabajo del navegador:**

El navegador actúa como cliente. Envía peticiones HTTP al servidor, recibe las respuestas y representa el contenido HTML para que el usuario pueda visualizarlo e interactuar con la página. También puede ejecutar JavaScript, validar datos antes de enviarlos y modificar elementos de la interfaz.

**2. Trabajo del servidor:**

El servidor recibe las peticiones HTTP, identifica la ruta y el método solicitados, ejecuta la lógica correspondiente y devuelve una respuesta con un estado HTTP, unas cabeceras y, cuando procede, un cuerpo. En Hello Server, Spring Boot y Tomcat permiten atender las peticiones, mientras que `HelloController` define la lógica de los endpoints. Por ejemplo, `/` devuelve HTML y `/api/hello` devuelve datos JSON.

**3. Modificación local del HTML:**

Las herramientas de desarrollo del navegador permiten modificar temporalmente el HTML mostrado, por ejemplo, cambiar el texto de un encabezado. Esta modificación afecta a la representación local de la página, pero no modifica el código fuente del servidor ni el archivo original. Si se vuelve a cargar la página, el navegador solicita de nuevo el contenido al servidor y la modificación temporal desaparece, salvo que exista algún mecanismo que la guarde.

**4. Validación duplicada:**

La validación en el cliente permite detectar errores rápidamente y mejorar la experiencia del usuario antes de enviar los datos. Sin embargo, no es suficiente como medida de seguridad, porque el cliente puede manipularse o evitarse. Por eso, el servidor debe validar de nuevo los datos recibidos y aplicar las reglas de negocio antes de procesarlos o guardarlos.

**5. Tres ventajas y dos costes de generar contenido dinámico en el servidor:**

Ventajas:
- Permite personalizar las respuestas según los datos, las peticiones y los permisos del usuario.
- Centraliza la lógica de negocio y facilita aplicar reglas y controles de seguridad.
- Puede generar respuestas estructuradas, como HTML o JSON, para distintos tipos de clientes.

Costes:
- Requiere recursos de servidor, como CPU y memoria, para procesar las peticiones y generar respuestas.
- Aumenta la complejidad de la aplicación, ya que hay que mantener, probar y desplegar la lógica del servidor.

## 4. Recorrido y mecanismos

### Recorrido de una petición

Para una petición `GET /health`, el recorrido es el siguiente:

```text
Cliente (cURL o navegador)
        |
        | GET /health
        v
Servidor HTTP integrado (Tomcat)
        |
        v
DispatcherServlet de Spring MVC
        |
        v
HelloController.health()
        |
        v
Spring convierte el Map en JSON
        |
        v
Respuesta HTTP 200 + Content-Type: application/json
        |
        v
Cliente recibe {"status":"UP"}
```

El cliente envía una petición HTTP al servidor. Tomcat recibe la conexión y Spring MVC dirige la petición al método del controlador correspondiente. En este caso, `HelloController.health()` devuelve un `Map` con el estado `UP`. Spring convierte ese objeto en JSON y envía la respuesta con el estado HTTP `200` y el tipo de contenido `application/json`.

### Comparación con CGI y FastCGI

En Hello Server, la aplicación Spring Boot permanece ejecutándose y atiende las peticiones mediante el servidor HTTP integrado Tomcat. No se inicia un nuevo proceso de aplicación para cada petición.

Con **CGI**, el servidor web suele iniciar un proceso externo para ejecutar el programa que genera la respuesta. Crear procesos repetidamente puede añadir costes de tiempo y recursos.

**FastCGI** mantiene procesos de aplicación disponibles para atender varias peticiones, evitando el coste de iniciar un proceso nuevo para cada una. De esta manera, reduce parte de la sobrecarga de CGI.

En resumen, los tres mecanismos permiten generar respuestas dinámicas, pero difieren en cómo se ejecuta y se mantiene disponible la aplicación que las produce.

## 5. Spring Boot y Laravel

| Criterio | Spring Boot | Laravel |
|---|---|---|
| Lenguaje y runtime | | |
| Ejecución | | |
| Tipado y pruebas | | |
| Despliegue | | |
| Papel didáctico | | |

Conclusión contextual:

## 6. QUERY

1. Problema que resuelve:
2. Seguridad e idempotencia:
3. Contenido y `Content-Type`:
4. Verificaciones necesarias para adoptarlo:
