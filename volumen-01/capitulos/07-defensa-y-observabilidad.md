# Capítulo 7 — Observar, detectar y responder sin perder la confianza

**Edición ampliada 0.4 · Tiempo sugerido: 210 minutos**

> Pregunta guía: si no podemos observar todas las amenazas, ¿cómo obtenemos evidencia suficiente para responder sin convertir la vigilancia en un riesgo para la privacidad?

## Objetivos
Distinguir evento, alerta e incidente; definir telemetría y registro de auditoría; diseñar reglas de detección con contexto; priorizar respuesta; proteger evidencia y datos personales; documentar recuperación y aprendizaje.

## 1. Prevenir no es detectar

**Prevención:** reducir oportunidades de daño mediante autenticación robusta, mínimos privilegios, segmentación, actualización y validación. **Detección:** observar señales compatibles con un fallo o abuso. **Respuesta:** analizar, contener, erradicar, recuperar y comunicar según procedimientos. **Mejora:** revisar causas y eficacia de controles.

Un firewall puede bloquear un flujo sin explicar por qué se originó. Un registro puede revelar un patrón sin demostrar que hubo una intrusión. Un sistema de alertas no reemplaza juicio humano ni procedimientos de respuesta.

## 2. Evento, alerta e incidente

- **Evento:** observación registrada, por ejemplo un intento de acceso rechazado.
- **Alerta:** evento o correlación que cumple una condición de atención.
- **Incidente:** situación que, tras evaluación, compromete o amenaza de forma relevante políticas, operaciones o activos según la definición de la organización.

No todos los eventos son incidentes; no todas las alertas son verdaderas. Los falsos positivos consumen tiempo, mientras los falsos negativos ocultan problemas. La detección necesita contexto, revisión y mejora continua.

## 3. Fuentes de telemetría

| Fuente | Qué aporta | Límite |
|---|---|---|
| Router/firewall | Flujos y decisiones de política | No ve toda la lógica de negocio |
| AP/controlador Wi-Fi | Asociaciones y estado inalámbrico | No demuestra identidad humana |
| DNS | Consultas y resolución según configuración | No revela todo el contenido de sesiones |
| API/backend | Resultado, identidad autorizada, operación | Requiere diseño seguro de registros |
| Base de datos | Accesos/cambios según capacidades | No siempre explica intención |
| Proveedor de identidad | Inicios de sesión, MFA y cambios | No sustituye autorización de recursos |
| Endpoint | Estado de equipo y procesos | Visibilidad depende de permisos y herramientas |

En arquitecturas administradas o serverless, parte de la telemetría depende del proveedor. Registrar la fuente, la zona horaria, la precisión del reloj y la retención evita correlaciones erróneas.

## 4. Cómo diseñar un evento de auditoría

Un registro útil puede incluir: identificador de evento, hora UTC, tipo de acción, identificador seudonimizado de actor, identificador de empresa, recurso, decisión permitida/denegada, resultado, correlación y versión del servicio. El contenido exacto depende de finalidad, base jurídica y políticas aplicables.

Ejemplo **sintético**, sin información real:

```json
{
  "timestamp": "2026-11-19T18:00:00Z",
  "event": "sale.create.denied",
  "tenant_id": "tenant-demo",
  "actor_id": "user-demo",
  "request_id": "req-demo-001",
  "reason_code": "INSUFFICIENT_PERMISSION"
}
```

**Nunca registrar por defecto:** contraseñas, tokens de acceso, claves privadas, cookies de sesión, números completos de tarjeta ni cuerpos de solicitudes con datos personales innecesarios. Proteger integridad, acceso y retención de los registros.

## 5. De una señal a una hipótesis

Caso: aumentan los intentos fallidos de acceso. Posibles causas: contraseña olvidada, error de integración, automatización defectuosa, uso indebido de credenciales o ataque de fuerza bruta. Una alerta útil considera volumen, periodo, distribución por cuenta, cambios recientes, origen y resultado de MFA, evitando depender de un único umbral universal.

**Ejemplo de regla conceptual:** avisar cuando una misma cuenta registre numerosos fallos en una ventana corta y exista cambio anómalo de dispositivo o geografía. La configuración debe ajustarse a datos históricos y evitar decisiones automáticas irreversibles por una señal aislada.

## 6. Un proceso de respuesta defendible

1. **Preparar:** responsables, inventario, canales, respaldos y criterios de severidad.
2. **Detectar y analizar:** establecer hechos, cronología, alcance, activos y nivel de confianza.
3. **Contener:** limitar el daño con cambios autorizados y reversibles cuando sea posible.
4. **Erradicar:** corregir causa raíz, credenciales, configuración o software comprometido.
5. **Recuperar:** restaurar servicios, comprobar integridad y monitorear recurrencia.
6. **Aprender:** documentar qué funcionó, qué falló y qué controles requieren inversión.

La secuencia es iterativa: contener puede requerir más análisis y recuperación puede revelar nueva evidencia. La organización debe evaluar obligaciones contractuales y legales de notificación aplicables; no asumir plazos sin revisar jurisdicción y tipo de incidente.

## 7. Matriz de decisión inicial

| Situación | Primera acción razonable | Riesgo de reaccionar mal |
|---|---|---|
| Un login fallido | Registrar y correlacionar | Bloquear usuarios legítimos |
| Muchas denegaciones entre tenants | Priorizar revisión de autorización | Exponer datos al investigar |
| Token expuesto confirmado | Revocar/rotar según procedimiento | Dejar sesiones activas |
| Caída general de API | Diagnóstico de disponibilidad | Atribuir sin pruebas a ataque |
| Dispositivo de caja desconocido | Verificar inventario y propietario | Desconectar equipo crítico sin coordinación |

## 8. Seguridad y privacidad de la observación

La visibilidad debe ser proporcional. Minimizar datos, restringir consultas, establecer retención, separar funciones, auditar acceso a logs y usar seudonimización cuando aporte protección. Un SIEM con permisos excesivos puede concentrar más riesgo que los sistemas que vigila.

## 9. Ejercicio: simulación de mesa

Un sistema multiempresa informa que una cuenta intenta consultar registros de otra empresa. No hay confirmación de exposición.

Redacta: hechos conocidos, hipótesis, evidencia necesaria, medidas temporales proporcionadas, responsable de decisión, comunicación interna y criterio para cerrar el incidente. Evita afirmar «filtración» sin evidencia. Incluye qué registros deben preservarse y cuáles no deberían recopilarse.

## 10. Evaluación

1. ¿Por qué una alerta no es una prueba concluyente?
2. ¿Qué distingue un log técnico de uno de auditoría?
3. ¿Cómo correlacionar eventos sin exponer secretos?
4. ¿Qué harías antes de bloquear una IP compartida por muchos usuarios?
5. ¿Cómo verificarías que una contención no detuvo ventas legítimas?
6. ¿Qué preguntas deben responderse en una revisión posterior al incidente?

## Fuentes base

- NIST SP 800-61 Rev. 3 (Incident Response): https://csrc.nist.gov/pubs/sp/800/61/r3/final
- NIST CSF 2.0: https://www.nist.gov/cyberframework
- NIST SP 800-92 (Log Management): https://csrc.nist.gov/pubs/sp/800/92/final
- OWASP Logging Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html

*Consultar versiones vigentes y obligaciones aplicables antes de convertir estos ejemplos en políticas operativas.*
