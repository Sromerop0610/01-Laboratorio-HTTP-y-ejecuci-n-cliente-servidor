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

1. Trabajo del navegador:
2. Trabajo del servidor:
3. Modificación local del HTML:
4. Validación duplicada:
5. Tres ventajas y dos costes:

## 4. Recorrido y mecanismos

```text
cliente -> ... -> respuesta
```

Comparación con CGI y FastCGI:

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
