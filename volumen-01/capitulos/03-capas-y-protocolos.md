# Capítulo 3 — Capas, encapsulación y protocolos

**Edición ampliada 0.2 · Tiempo sugerido: 120 minutos**

## Objetivos
Explicar modelos de capas, encapsulación, TCP/UDP, puertos, HTTP/TLS y diagnóstico por hipótesis.

## 1. Por qué existen las capas

Separar responsabilidades permite cambiar el medio físico sin reescribir toda la aplicación. Un navegador puede utilizar Ethernet o Wi-Fi y seguir realizando peticiones HTTP. Las capas son modelos de razonamiento: las implementaciones reales incorporan optimizaciones, túneles, intermediarios y excepciones.

| OSI (modelo pedagógico) | TCP/IP aproximado | Ejemplos |
|---|---|---|
| Física y enlace | Acceso a red | Ethernet, Wi-Fi |
| Red | Internet | IPv4, IPv6, ICMP |
| Transporte | Transporte | TCP, UDP |
| Sesión/presentación/aplicación | Aplicación | DNS, HTTP, TLS en su contexto |

No hay correspondencia uno a uno universal entre OSI y protocolos modernos.

## 2. Encapsulación

```text
Mensaje HTTP
  dentro de registros TLS
    sobre flujo TCP
      dentro de paquetes IP
        dentro de tramas Ethernet o Wi-Fi
```

Este es un ejemplo de HTTP/1.1 o HTTP/2 sobre TCP. Para HTTP/3, HTTP viaja sobre QUIC, que utiliza UDP y proporciona seguridad mediante TLS 1.3 integrado en QUIC. La información de cada capa permite que distintos equipos realicen funciones distintas.

## 3. Direcciones y puertos

Una dirección IP sirve para encaminar; un puerto permite identificar puntos de comunicación de transporte en un host. Un servicio HTTPS suele escuchar en TCP 443, pero también puede ofrecer HTTP/3 sobre UDP 443. Los puertos son convenciones/configuración, no pruebas de qué aplicación es realmente segura.

La IP de origen observada por un servidor puede corresponder a un proxy, NAT o gateway, no necesariamente al dispositivo final. Los encabezados que declaran IP de cliente solo son confiables cuando una cadena de proxies está correctamente configurada.

## 4. TCP y UDP

**TCP:** conexión lógica, secuenciación, retransmisión, control de flujo y congestión. La fiabilidad de TCP no significa que una operación de negocio se ejecute exactamente una vez: un cliente puede repetir una solicitud tras perder la respuesta.

**UDP:** datagramas sin garantía intrínseca de entrega, orden ni retransmisión. Protocolos superiores pueden añadirlas; QUIC es un ejemplo.

## 5. DNS y TLS

DNS resuelve nombres y otros tipos de registros; una respuesta DNS no demuestra identidad criptográfica del servicio. TLS autentica el extremo según el certificado y la validación configurada, y protege confidencialidad e integridad del tráfico entre extremos TLS. Si un proxy termina TLS, hay otra frontera que debe evaluarse.

## 6. Caso diagnóstico: «La web no carga»

| Síntoma | Hipótesis inicial | Verificación autorizada |
|---|---|---|
| Sin enlace de red | Medio/interfaz | Estado de NIC y AP |
| IP local ausente | DHCP/configuración | Configuración del equipo |
| Sin ruta | Gateway/routing | Tabla de rutas |
| Nombre no resuelve | DNS | Consulta de nombre |
| Certificado rechazado | TLS/fecha/nombre | Detalles del error y certificado |
| HTTP 403 | Autorización | Registros del backend y rol |
| HTTP 500 | Error del servidor | Correlación y logs de aplicación |

No convertir el síntoma en conclusión: un HTTP 403 también puede originarse en un proxy o WAF; el contexto determina la causa.

## 7. Ejercicio de razonamiento

Una persona registra una venta; la app no recibe respuesta y vuelve a intentarlo. La segunda solicitud llega correctamente. ¿Por qué TCP no evita necesariamente una venta duplicada? Diseña una clave de idempotencia y una regla de persistencia del lado servidor para evitar dos registros de la misma operación lógica.

## 8. Preguntas
- ¿Qué ve un switch y qué necesita ver un router?
- ¿Por qué no debe desactivarse la validación de certificados para solucionar un error?
- ¿Qué metadatos pueden seguir visibles pese a TLS?
- ¿En qué capa se identifica un HTTP 403 y dónde puede estar su causa?

## Fuentes
- RFC 9293 TCP: https://www.rfc-editor.org/rfc/rfc9293
- RFC 768 UDP: https://www.rfc-editor.org/rfc/rfc768
- RFC 8200 IPv6: https://www.rfc-editor.org/rfc/rfc8200
- RFC 8446 TLS 1.3: https://www.rfc-editor.org/rfc/rfc8446
- RFC 9000 QUIC: https://www.rfc-editor.org/rfc/rfc9000
- RFC 9110 HTTP: https://www.rfc-editor.org/rfc/rfc9110
- RFC 9114 HTTP/3: https://www.rfc-editor.org/rfc/rfc9114
