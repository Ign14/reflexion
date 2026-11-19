# Laboratorio 03 — Segmentación y respuesta: simulación segura

**Duración:** 90–120 minutos · **Tipo:** ejercicio de diseño y mesa, sin escaneo ni cambios productivos.

## Escenario
Una heladería utiliza tablets de caja, una impresora, un computador administrativo y Wi-Fi para clientes. Un cliente informa que puede ver el nombre de una impresora de la tienda desde su dispositivo. No se ha demostrado acceso a información ni compromiso.

## Parte A. Modelo de red
Dibuja una arquitectura con redes separadas para POS, administración e invitados. Identifica router/firewall, AP, switch y servicios externos. Especifica dónde se hacen cumplir las reglas.

## Parte B. Matriz de flujos
Completa la tabla:

| Origen | Destino | ¿Necesario? | Regla propuesta | Evidencia de prueba autorizada |
|---|---|---|---|---|
| Invitados | POS | No | Denegar | Prueba en laboratorio |
| Invitados | Internet | Sí | Permitir con restricciones | Acceso a servicio de prueba |
| POS | API de ventas | Sí | Permitir TLS | Operación sintética |
| Administración | Gestión de red | Según rol | Restringir | Acceso con cuenta de prueba |

## Parte C. Mesa de incidente
1. Registrar hora, descripción y quién reporta.
2. Distinguir «dispositivo visible» de «dispositivo accesible».
3. Formular hipótesis: descubrimiento multicast, aislamiento incompleto, nombre publicado o información en caché.
4. Elegir verificaciones en equipos propios, sin inspeccionar tráfico ajeno.
5. Proponer contención proporcional y plan de reversión.
6. Documentar responsable, decisión, evidencia y criterio de cierre.

## Entregable
Una página de arquitectura, una matriz de flujos, una cronología y un informe de máximo dos páginas. No incluir direcciones reales, credenciales, capturas con datos de clientes ni secretos.

## Rúbrica (10 puntos)
- 2: fronteras de confianza identificadas.
- 2: reglas coherentes con necesidades del negocio.
- 2: hipótesis separadas de hechos.
- 2: evidencias y pruebas autorizadas.
- 2: contención, reversión y comunicación proporcionadas.

**Aprobación orientativa:** 8/10 con ninguna práctica que invada sistemas de terceros.
