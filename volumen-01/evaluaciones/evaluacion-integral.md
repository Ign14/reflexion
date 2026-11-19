# Evaluación integral — Los cimientos invisibles

**Edición 0.5 · Modalidad: respuesta razonada y diseño · Total: 100 puntos**

## Parte I. Comprensión de redes (25 puntos)

1. (5) Distingue switch, router, AP, firewall y ONT. Identifica dos funciones que pueden coexistir en un mismo dispositivo.
2. (5) Para `192.168.40.18/26`, indica red, rango convencional de hosts y dirección de difusión. Explica por qué el prefijo no determina permisos.
3. (5) Describe una solicitud HTTP/3 desde una red Wi-Fi, indicando transporte y protección.
4. (5) Diferencia DHCP, DNS, ARP y puerta de enlace.
5. (5) Explica por qué NAT y TLS no sustituyen autorización de aplicación.

## Parte II. Diagnóstico y observabilidad (20 puntos)

6. (10) Una tablet obtiene IP, resuelve DNS, valida TLS y recibe HTTP 403. Formula cuatro hipótesis razonables, evidencia segura y orden de comprobación.
7. (10) Diseña un evento de auditoría para exportación de clientes: campos mínimos, campos prohibidos, acceso y retención.

## Parte III. Arquitectura multiempresa (30 puntos)

8. (10) Dibuja una arquitectura SaaS con dos tenants, identidad, autorización, almacenamiento y un proveedor externo. Marca cinco fronteras de confianza.
9. (10) Construye una matriz de permisos para administrador, operador y usuario externo: crear venta, exportar clientes y cambiar roles. Define controles y pruebas negativas.
10. (10) Una venta se guarda, pero se pierde la respuesta y el cliente reintenta. Explica cómo evitar duplicidad bajo concurrencia y cómo comunicar un estado incierto.

## Parte IV. Incidentes y responsabilidad (15 puntos)

11. (10) Una alerta sugiere lecturas cruzadas entre tenants. Separa hechos, hipótesis, contención, preservación de evidencia y comunicación sin afirmar filtración no confirmada.
12. (5) Explica dos riesgos de registrar demasiada información y dos mecanismos para limitar acceso a los logs.

## Parte V. Reflexión técnica (10 puntos)

13. (10) Escribe 300–500 palabras: «¿Qué significa construir confianza cuando la infraestructura es compartida, los dispositivos son heterogéneos y la información pertenece a personas?». Incluye un ejemplo verificable y una limitación de tu propuesta.

## Rúbrica general

| Criterio | Ponderación |
|---|---:|
| Exactitud técnica | 30 |
| Modelo de amenazas y fronteras de confianza | 20 |
| Controles con evidencia comprobable | 20 |
| Diagnóstico y respuesta proporcional | 15 |
| Privacidad, claridad y reflexión | 15 |

**Dominio inicial sugerido:** 75/100, sin omisiones graves de autorización entre tenants, validación TLS o manejo de secretos. La evaluación no constituye certificación.

## Orientaciones para la revisión

- **Pregunta 2:** `192.168.40.0/26`, hosts convencionales `.1` a `.62`, difusión `.63`.
- **Pregunta 3:** HTTP/3 usa QUIC sobre UDP; QUIC incorpora TLS 1.3 para proteger la conexión.
- **Pregunta 6:** HTTP 403 orienta hacia política de aplicación/intermediarios, no prueba fallo DNS.
- **Pregunta 10:** clave idempotente ligada a tenant y operación, unicidad persistente y control de concurrencia.
- **Pregunta 11:** preservar evidencia y comprobar alcance antes de afirmar exposición.

El resto se evalúa por razonamiento, consistencia del diseño y evidencia, no por memorizar una solución única.
