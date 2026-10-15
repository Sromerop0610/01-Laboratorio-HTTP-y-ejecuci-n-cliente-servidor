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

Incluye solo fragmentos necesarios y sustituye secretos por `<REDACTED>`.

```http
<traza sanitizada>
```

Respuestas:

1. Método, destino, campos y contenido:
2. Estados observados:
3. Resultado de OPTIONS:
4. Información que no debe aparecer en errores:

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
