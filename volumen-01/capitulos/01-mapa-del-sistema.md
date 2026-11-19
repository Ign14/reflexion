# Capítulo 1 — El mapa de un sistema conectado

**Edición ampliada 0.2 · Tiempo sugerido: 90 minutos · Nivel: fundamentos**

> Pregunta central: cuando una persona confirma una venta desde su teléfono, ¿qué componentes deben funcionar y cuáles deben ser confiables?

## 1. Objetivos verificables

Al finalizar podrás: (a) distinguir dispositivos, medios, redes, servicios y datos; (b) dibujar el trayecto lógico de una transacción; (c) reconocer cinco fronteras de confianza; (d) explicar por qué una conexión cifrada no equivale a una operación autorizada; (e) proponer evidencia para investigar un fallo.

## 2. Sistema, red y servicio: tres cosas diferentes

Un **sistema de información** combina personas, procedimientos, software, equipos y datos. Una **red** permite intercambiar información entre nodos. Un **servicio** expone una capacidad (resolver nombres, autenticar, almacenar o procesar una venta). Un servicio puede residir en varios servidores; un dispositivo puede ejecutar muchos servicios.

En un negocio pequeño, un teléfono, una impresora de comprobantes y un router parecen suficientes para describir la red. Pero una venta digital depende también de la energía, del proveedor de internet, del DNS, de la plataforma de identidad, de la API y de la persistencia de datos. Las dependencias invisibles explican por qué un fallo aparentemente local puede tener origen remoto.

### Diagrama 1. Trayecto lógico, no mapa físico

```text
 [Persona]
     | interacción
 [App / navegador] ---- [SO + almacenamiento local]
     | solicitud HTTPS
 [Interfaz Wi-Fi/Ethernet] -- [AP/switch] -- [router/firewall]
                                             |
                                           [ISP]
                                             |
                                       [Internet]
                                             |
                                      [CDN / borde]
                                             |
                                          [API]
                                        /   |    \
                               [Identidad] [Reglas] [Auditoría]
                                             |
                                       [Base de datos]
                                             |
                                     [Copia de respaldo]
```

Este dibujo omite deliberadamente saltos de tránsito, balanceadores, caches y servicios externos. No supone que una conexión HTTP use siempre TCP: HTTP/3 emplea QUIC sobre UDP. Tampoco supone que una copia de respaldo se actualice de forma sincrónica.

## 3. Las fronteras de confianza

Una frontera de confianza es el punto donde cambia quién controla un componente o qué garantías pueden asumirse.

| Frontera | Qué cambia | Pregunta defensiva |
|---|---|---|
| Persona → dispositivo | Intención humana frente a software manipulable | ¿Cómo se autentica la acción? |
| Dispositivo → red local | Medio compartido y equipos vecinos | ¿Qué tráfico está permitido? |
| LAN → proveedor | Administración y rutas fuera del negocio | ¿Se protege el contenido en tránsito? |
| Internet → borde/API | Entrada de solicitudes no confiables | ¿Se limitan y validan las peticiones? |
| API → datos de empresas | Acceso a información persistente | ¿Se comprueba tenant y permiso en cada operación? |
| Plataforma → tercero | Otra organización y contrato | ¿Qué datos se transfieren y cómo se audita? |

**Regla:** toda entrada externa es potencialmente manipulable, incluso si procede de una app oficial o de una red corporativa. Las decisiones de autorización pertenecen al servidor o a un componente confiable equivalente.

## 4. Una venta paso a paso

1. La persona selecciona productos y pulsa «Confirmar».
2. El cliente prepara una solicitud. Su representación visual y sus importes son **datos no confiables** para el backend.
3. La red transporta la solicitud hacia el servicio. TLS protege la comunicación hasta el extremo donde termina TLS.
4. El backend verifica identidad, vigencia de sesión y permisos para el negocio correcto.
5. El backend recupera precios y reglas vigentes de una fuente autorizada, comprueba inventario y calcula el total.
6. La transacción se persiste de manera coherente; se considera idempotencia para evitar duplicados en reintentos.
7. Se genera un resultado y un evento de auditoría minimizado; el cliente muestra confirmación solo cuando la operación queda confirmada.

Una respuesta HTTP exitosa no prueba por sí sola que un pago externo haya sido liquidado. Los estados «pedido creado», «pago autorizado» y «pago conciliado» deben diferenciarse.

## 5. Cinco propiedades de seguridad

- **Confidencialidad:** datos accesibles solo para quien corresponde.
- **Integridad:** datos y operaciones no alterados sin autorización.
- **Disponibilidad:** servicios recuperables dentro de objetivos acordados.
- **Autenticidad:** posibilidad de verificar la identidad/origen pertinente.
- **Trazabilidad:** evidencias para reconstruir operaciones y decisiones.

Estas propiedades entran en tensión. Registrar cuerpos completos de solicitudes puede mejorar depuración pero exponer información personal. Se prefieren identificadores de correlación, eventos mínimos y retención limitada.

## 6. Ejemplo de fallo: «No se guardó la venta»

Hipótesis alternativas: señal Wi-Fi débil; DNS fallando; TLS rechazado; sesión caducada; regla de autorización incorrecta; base de datos indisponible; conflicto de inventario. No se resuelve todo reiniciando el router.

**Método de diagnóstico:** definir síntoma y hora; comprobar conectividad local; verificar resolución y acceso al servicio; revisar códigos de respuesta y correlación; contrastar registros de backend y base de datos; documentar causa y acción correctiva. Nunca publicar tokens o datos reales en capturas.

## 7. Actividad de aprendizaje

Dibuja el trayecto de una reserva online y de una venta en caja. Señala qué elementos administra el negocio, cuáles administra el proveedor SaaS y cuáles terceros. Identifica un control preventivo, uno de detección y uno de recuperación para cada operación.

### Criterios de revisión

- Al menos 8 componentes correctamente identificados.
- Al menos 5 fronteras de confianza explicadas.
- Diferencia explícita entre cifrado, autenticación y autorización.
- Tres fallos posibles con una evidencia concreta para distinguirlos.

## 8. Errores frecuentes

- «HTTPS significa que el servidor no puede ser comprometido». Falso.
- «Una contraseña Wi-Fi fuerte protege la base de datos». Insuficiente.
- «Ocultar un botón equivale a negar una operación». Falso.
- «Si la app recibió una respuesta, el pago se completó». No necesariamente.
- «NAT sustituye un firewall». No.

## 9. Preguntas de reflexión

¿En qué punto se decide quién puede modificar una venta? ¿Quién responde cuando un proveedor externo falla? ¿Cómo se prueba que dos empresas no pueden leer datos entre sí?

## Referencias base

- NIST CSF 2.0: https://www.nist.gov/cyberframework
- NIST SP 800-207: https://csrc.nist.gov/pubs/sp/800/207/final
- RFC 9110 (HTTP): https://www.rfc-editor.org/rfc/rfc9110
- RFC 8446 (TLS 1.3): https://www.rfc-editor.org/rfc/rfc8446

*Las referencias orientan la profundización; el capítulo no pretende reproducir íntegramente sus requisitos.*
