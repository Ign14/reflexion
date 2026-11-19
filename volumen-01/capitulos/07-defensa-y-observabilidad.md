# 07. Fundamentos defensivos y observabilidad

## Objetivos
Definir inventario, registros, alertas, mínimos privilegios, segmentación, actualización y respuesta inicial.

## Defensa en profundidad
No existe un único interruptor de seguridad. Las medidas se complementan: inventario actualizado, segmentación, MFA, mínimos privilegios, validaciones del backend, cifrado, registros, respaldo, controles de recuperación y pruebas.

**Prevención** reduce probabilidad; **detección** identifica señales; **respuesta** contiene y recupera; **aprendizaje** elimina causas y mejora el sistema.

## Señales útiles
- Múltiples intentos fallidos de acceso, observados con contexto.
- Cambios de permisos y creación inesperada de credenciales.
- Solicitudes con denegaciones repetidas o patrones anómalos de uso de API.
- Incremento de errores, latencia o fallas de servicios.
- Acceso entre segmentos no esperado.

La observación requiere políticas de retención, acceso restringido y minimización de datos sensibles en logs. Ningún indicador aislado demuestra una intrusión.

## Mini-procedimiento
Registrar hora y alcance del evento; preservar evidencia sin divulgar secretos; clasificar impacto; aislar según procedimiento autorizado; comunicar; corregir; verificar restauración y documentar el aprendizaje.

## Reflexión
¿Podríamos explicar, después de un incidente, quién hizo qué, cuándo y con qué autorización, sin registrar información innecesaria?
