# 05. DNS, HTTP, TLS y recorrido de una petición

## Objetivos
Describir resolución de nombres, establecimiento de una conexión y validación de una solicitud empresarial.

## Recorrido conceptual
1. La app necesita llegar a un servicio identificado por nombre.
2. El sistema consulta resolución DNS usando sus mecanismos configurados y cachés.
3. Se determina la ruta de red al destino.
4. Se establece transporte: TCP en HTTP/1.1 y HTTP/2 típicos; QUIC sobre UDP para HTTP/3.
5. TLS verifica la identidad del servidor mediante certificados y negocia protección criptográfica.
6. HTTP transporta una solicitud que el backend autentica y autoriza.
7. El servidor produce una respuesta y la app actualiza su estado.

**Importante:** este flujo es pedagógico: DNS y establecimiento de transporte pueden variar por caché, proxies, reutilización de conexiones, HTTP/3 y otras condiciones.

## Confianza bien ubicada
HTTPS protege el tráfico en tránsito entre extremos TLS, pero no garantiza que el dispositivo cliente esté libre de malware ni que las reglas de la API estén correctas. El backend debe comprobar cada operación sensible, no confiar en datos editables por el cliente.

## Reflexión
Si un usuario logra modificar un precio en su navegador, ¿qué debe impedir que el precio manipulado se registre como venta?
