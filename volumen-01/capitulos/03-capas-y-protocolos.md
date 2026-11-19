# 03. Capas y protocolos

## Objetivos
Relacionar modelos OSI y TCP/IP con protocolos reales; interpretar encapsulación y errores por capa.

## Modelo práctico
- Enlace: tramas Ethernet o Wi-Fi y entrega local.
- Red: IPv4/IPv6 y encaminamiento entre redes.
- Transporte: TCP y UDP, puertos, entrega y control de congestión según protocolo.
- Aplicación: DNS, HTTP y otros protocolos que usan los anteriores.
- Seguridad en tránsito: TLS autentica extremos según configuración y protege el contenido de la sesión; no sustituye autorización de negocio.

El modelo OSI de siete capas es útil para enseñar y diagnosticar; no cada protocolo encaja perfectamente en una sola casilla. En TCP/IP se agrupan funciones de manera distinta.

## Ejemplo
En una llamada HTTP sobre TLS sobre TCP sobre IP sobre Ethernet, cada capa agrega información necesaria para su función. Los intermediarios no siempre pueden leer la carga útil protegida, pero suelen ver metadatos como direcciones de red y características de tráfico.

## Diagnóstico por hipótesis
Si la tarjeta no enlaza, revisar medio e interfaz. Si hay dirección IP pero no ruta, revisar puerta de enlace. Si la ruta funciona pero no el nombre, investigar DNS. Si TLS falla, inspeccionar fecha, certificado, nombre de host y cadena de confianza; no desactivar validación para 'resolver' el problema.

## Reflexión
¿En qué capa se observa el síntoma y en cuál está la causa real?
