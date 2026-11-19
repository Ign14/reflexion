# Capítulo 5 — El recorrido de una petición: DNS, TLS y HTTP

**Edición ampliada 0.3 · Tiempo sugerido: 180 minutos · Nivel: fundamentos aplicados**

> Pregunta guía: ¿qué garantías se construyen —y cuáles no— cuando un navegador envía una operación empresarial por HTTPS?

## 1. Resultados de aprendizaje

Explicar resolución de nombres, rutas, transporte, TLS y HTTP; identificar el límite del cifrado; diferenciar errores de DNS, conexión, certificados, autorización y aplicación; proponer medidas contra duplicidad de operaciones y fuga de datos.

## 2. Un nombre no es una conexión

Cuando una persona abre `https://example.com`, el navegador y el sistema pueden consultar cachés, mecanismos de resolución y resolutores configurados. DNS devuelve registros como A o AAAA cuando existen. También puede entregar CNAME y otros registros relevantes; no se debe asumir que siempre hay una sola IP.

El resolutor puede comunicarse por DNS clásico o mediante mecanismos cifrados, según configuración. DNSSEC puede aportar autenticidad e integridad a datos DNS validados, pero **no cifra** por sí mismo las consultas y no reemplaza TLS para proteger la aplicación.

```text
Nombre solicitado
   |
Caché / resolutor
   |
Registros DNS (A/AAAA, etc.)
   |
Selección de dirección y ruta
   |
Conexión al servicio
```

Una respuesta DNS correcta no demuestra que el servidor tenga permiso para procesar información de clientes.

## 3. De la ruta al transporte

El sistema consulta su tabla de rutas. El tráfico atraviesa uno o varios saltos. Para HTTP/1.1 y HTTP/2 suele utilizarse TCP, mientras HTTP/3 usa QUIC sobre UDP. La selección concreta depende de navegador, servidor, red y negociación.

TCP ofrece entrega fiable de un flujo de bytes, pero no garantiza que una operación de negocio se ejecute exactamente una vez. Si la respuesta se pierde después de que el servidor guardó una venta, el cliente puede repetir la solicitud y producir una duplicidad si la API no contempla idempotencia.

## 4. TLS: qué protege realmente

En TLS, el cliente verifica el certificado y la identidad del servidor según las reglas aplicables, y ambos extremos negocian claves para proteger la comunicación. En conexiones modernas, TLS 1.3 es una referencia importante. No conviene enseñar a ignorar alertas de certificado: hacerlo elimina una protección esencial contra suplantación.

**Alcance:** TLS protege confidencialidad e integridad del tráfico entre sus extremos. Un CDN, balanceador o proxy puede terminar TLS y establecer una conexión posterior hacia la aplicación; esa segunda conexión también necesita protección y controles adecuados.

**No garantiza:** que el dispositivo del cliente esté libre de malware, que la API valide el precio, que el usuario tenga permiso para ver datos de otra empresa o que el servidor no registre información excesiva.

Algunos metadatos de conexión pueden permanecer observables dependiendo del protocolo y configuración. No debe afirmarse que HTTPS oculta toda la actividad de red.

## 5. HTTP es una conversación con significado

Una solicitud contiene método, destino, encabezados y, en algunos casos, cuerpo. La respuesta contiene estado, encabezados y contenido. El significado del método importa: GET está definido como seguro respecto a efectos solicitados; POST suele utilizarse para operaciones que cambian estado. Esto no impide efectos secundarios incidentales como logs.

Ejemplo **ilustrativo**, no endpoint real:

```http
POST /api/ventas HTTP/1.1
Host: negocio.example
Content-Type: application/json
Idempotency-Key: venta-demo-0001

{"items":[{"sku":"A1","cantidad":2}]}
```

El backend no debe aceptar un total calculado por el cliente como autoridad. Debe obtener precios, descuentos y permisos de fuentes confiables y registrar la operación bajo controles transaccionales.

## 6. Estados HTTP y diagnóstico

| Señal | Interpretación inicial | Advertencia |
|---|---|---|
| Error DNS | Nombre no resuelto o fallo del resolutor | Puede haber caché o configuración específica |
| Error TLS | Validación o negociación fallida | No desactivar verificación |
| 401 | Autenticación requerida o inválida | Consultar esquema concreto |
| 403 | Acceso denegado | Puede originarse en API o intermediario |
| 404 | Recurso no encontrado o no revelado | No implica necesariamente ausencia física |
| 429 | Limitación de solicitudes | Respetar política de reintentos |
| 500 | Error interno del servidor | Investigar con correlación y logs |
| 503 | Servicio no disponible | Puede existir mantenimiento o saturación |

No registrar tokens, contraseñas, cookies de sesión ni datos sensibles en reportes de errores.

## 7. La operación «exactamente una vez» no aparece por arte de magia

Escenario: el servidor crea una venta, pero la respuesta se pierde. El usuario pulsa otra vez. Para reducir duplicados, el cliente asigna un identificador estable a la **misma intención de venta**, y el servidor aplica una restricción única y devuelve el resultado previamente registrado cuando corresponde.

Una clave de idempotencia debe estar acotada al contexto adecuado (por ejemplo, empresa y operación), con política de expiración y control de concurrencia. No basta con comparar el cuerpo JSON ni con deshabilitar el botón en la interfaz.

## 8. Seguridad de datos en tránsito y en reposo

Cifrar tránsito no sustituye cifrado en almacenamiento, permisos mínimos, copias de respaldo, gestión de claves, auditoría y retención limitada. Tampoco todo dato merece la misma conservación. Una organización responsable debe definir qué recoge, por qué, durante cuánto tiempo y quién puede acceder.

## 9. Actividad de razonamiento

Una tablet de caja resuelve DNS y abre HTTPS, pero recibe HTTP 403 al crear una venta. Formula al menos cuatro hipótesis y una evidencia segura para cada una. Explica por qué reiniciar el router probablemente no es el primer paso y por qué no debes copiar el token de sesión a una herramienta pública.

## 10. Preguntas

1. ¿Por qué DNS y TLS resuelven problemas distintos?
2. ¿En qué se diferencia HTTP/3 de HTTP/2 en su transporte habitual?
3. ¿Qué implica que un CDN termine TLS?
4. ¿Por qué un POST puede duplicar una venta aunque TCP sea fiable?
5. ¿Qué datos deberían omitirse de un log de error?
6. ¿Qué verifica el backend antes de persistir una operación de otra empresa?

## Fuentes base

- RFC 1034/1035 (DNS): https://www.rfc-editor.org/rfc/rfc1034 y https://www.rfc-editor.org/rfc/rfc1035
- RFC 4033 (DNSSEC): https://www.rfc-editor.org/rfc/rfc4033
- RFC 8446 (TLS 1.3): https://www.rfc-editor.org/rfc/rfc8446
- RFC 9110 (HTTP): https://www.rfc-editor.org/rfc/rfc9110
- RFC 9114 (HTTP/3): https://www.rfc-editor.org/rfc/rfc9114
- RFC 9000 (QUIC): https://www.rfc-editor.org/rfc/rfc9000
- OWASP ASVS: https://owasp.org/www-project-application-security-verification-standard/
