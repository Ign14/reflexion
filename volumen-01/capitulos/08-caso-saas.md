# Capítulo 8 — Arquitectura de confianza en un SaaS multiempresa

**Edición ampliada 0.5 · Tiempo estimado: 210 minutos · Caso integrador**

> Pregunta guía: ¿cómo demostrar que una empresa puede administrar sus operaciones digitales sin acceder a los datos de otra, incluso cuando ambas comparten infraestructura?

## 1. Resultados de aprendizaje

Al finalizar, podrás: identificar activos y fronteras de confianza; distinguir autenticación de autorización; describir alternativas de aislamiento multiempresa; diseñar controles para ventas, reservas y exportaciones; reconocer riesgos de integraciones; establecer evidencias mínimas de seguridad; y plantear un plan de pruebas autorizadas.

## 2. El escenario

Una plataforma SaaS permite a distintos negocios administrar clientes, productos, inventario, ventas, agenda, trabajadores y documentos. Cada negocio es un **tenant** o espacio lógico de organización. Los usuarios acceden mediante navegador o aplicación móvil; los datos residen en servicios administrados; pueden existir integraciones de pago, facturación y mensajería.

Compartir infraestructura no significa compartir autorización. La arquitectura debe preservar límites de información aunque un usuario cambie parámetros en el navegador, un cliente móvil esté desactualizado o una integración externa se comporte inesperadamente.

### Diagrama lógico

```text
[Usuario / app móvil / navegador]
                 |
          [HTTPS / TLS]
                 |
         [Borde / API / BFF]
                 |
      +----------+-----------+
      |                      |
 [Autenticación]       [Autorización]
      |                      |
      +---------[Contexto de tenant]
                             |
                  [Reglas de negocio]
                      /      |       \
                 [Ventas] [Agenda] [Clientes]
                      \      |       /
                       [Persistencia]
                             |
                 [Auditoría / respaldos]
                             |
                   [Servicios externos]
```

Es un modelo conceptual. En sistemas serverless o con acceso directo del cliente a servicios administrados, las reglas de autorización pueden ejecutarse en el propio servicio de datos. **La ubicación física de la validación cambia; su necesidad no.**

## 3. Identidad, sesión y permiso

**Autenticación:** comprobar quién intenta acceder. **Autorización:** decidir qué operaciones puede realizar sobre un recurso. **Contexto de tenant:** determinar para qué organización se ejecuta una operación. Estos tres conceptos no son intercambiables.

Una cuenta puede pertenecer a más de una empresa. Por eso, un identificador de empresa enviado por el cliente nunca debe aceptarse como autoridad por sí mismo. La decisión debe comprobar una membresía o asignación verificable, el rol vigente y el recurso concreto.

```text
Solicitud: editar producto P de empresa B
     |
¿Sesión válida?
     | sí
¿Usuario pertenece a B?
     | sí
¿Rol permite editar productos?
     | sí
¿Producto P pertenece a B?
     | sí
¿Se cumplen reglas de negocio?
     | sí
Ejecutar y auditar
```

Si cualquiera de esas condiciones falla, la operación se rechaza de forma controlada. Ocultar el botón «Editar» mejora experiencia, pero no implementa autorización.

## 4. Aislamiento multiempresa: alternativas

| Diseño | Ventaja | Riesgo que exige control |
|---|---|---|
| Base de datos por tenant | Separación operacional fuerte | Complejidad, migraciones y costos |
| Esquema o partición por tenant | Organización y ciertas barreras lógicas | Errores de configuración y permisos |
| Colecciones/tablas compartidas con tenant_id | Eficiencia y simplicidad inicial | Consultas y reglas que omitan tenant |
| Servicio administrado con reglas por documento | Controles cercanos al dato | Reglas incompletas, consultas incompatibles, privilegios administrativos |

No existe un patrón universalmente superior. El diseño se evalúa por garantías comprobables, escala, costos, recuperación y operaciones. En cualquier alternativa, los procesos privilegiados, exportaciones, trabajos programados y respaldos también necesitan separación.

## 5. Caso técnico: cliente, API y datos

Un frontend es controlado por quien lo ejecuta: sus validaciones y filtros no son una frontera de seguridad. Los datos enviados por el cliente se consideran manipulables. La API o las reglas de un servicio administrado deben evaluar el acceso a cada recurso y operación.

En plataformas con Firebase/Firestore, por ejemplo, las **Security Rules** pueden restringir acceso de SDK cliente; los SDK de servidor con credenciales privilegiadas siguen un modelo de confianza diferente y requieren controles en el backend e IAM. Las reglas no deben confundirse con filtros posteriores a una consulta. El diseño exacto debe verificarse contra documentación vigente y pruebas automatizadas.

### Matriz de permisos ilustrativa

| Acción | Dueño/administrador | Operador | Usuario de otra empresa |
|---|---|---|---|
| Crear venta | Según política | Según política | Denegar |
| Editar roles | Según política | Denegar | Denegar |
| Exportar clientes | Según política explícita | Denegar por defecto | Denegar |
| Ver reportes | Según política | Según rol | Denegar |
| Cambiar configuración de pagos | Privilegio específico | Denegar | Denegar |

«Según política» exige reglas concretas, no es un permiso automático.

## 6. Integridad de una venta

Una venta involucra identidad, precios, descuentos, inventario, impuestos, comprobantes y estado de pago. No basta con recibir un JSON válido.

1. Verificar sesión, tenant y permiso de operación.
2. Leer precios y reglas desde fuente autorizada.
3. Validar cantidades, límites y estado de inventario.
4. Calcular totales del lado confiable.
5. Ejecutar una escritura coherente o transacción adecuada.
6. Asociar una clave de idempotencia a la intención de venta.
7. Registrar evento mínimo de auditoría y resultado.
8. Distinguir venta creada de pago confirmado o conciliado.

**Caso límite:** el servidor guarda la venta pero la respuesta se pierde. El cliente reintenta. La idempotencia debe operar en la persistencia confiable y contemplar concurrencia; deshabilitar el botón no evita duplicados en todos los casos.

## 7. Integraciones y terceros

Una plataforma puede depender de pasarelas de pago, facturación, correo y mensajería. Cada integración añade credenciales, disponibilidad y fronteras contractuales.

Para un webhook: verificar autenticidad mediante el mecanismo que documenta el proveedor (por ejemplo, firma validada), prevenir repeticiones indebidas, procesar de forma idempotente, controlar orden y retrasos de eventos y conciliar estados con la fuente autorizada. No inventar formatos de firma ni asumir que un identificador público autentica un evento.

No registrar secretos ni datos de tarjeta en logs. Definir claramente quién es responsable del almacenamiento y del tratamiento de cada categoría de datos.

## 8. Seguridad de sesión y aplicaciones cliente

Los riesgos incluyen robo de sesión, XSS, abuso de credenciales, dispositivos extraviados y permisos excesivos. Los controles se eligen según arquitectura: gestión segura de tokens, protección frente a inyección, expiración/revocación cuando corresponda, MFA en operaciones sensibles y revisión de permisos.

Un dispositivo conectado a Wi-Fi legítimo no adquiere permisos adicionales. La red es una capa de transporte; el backend sigue evaluando identidad y autorización.

## 9. Disponibilidad y recuperación

Un negocio necesita operar incluso cuando un servicio externo falla. La arquitectura debe distinguir errores temporales de definitivos, aplicar reintentos con límites, evitar ventas duplicadas y comunicar estados ambiguos. Los respaldos deben acompañarse de **pruebas de restauración**; una copia no probada no demuestra capacidad de recuperación.

Definir objetivos RPO (pérdida de datos tolerable) y RTO (tiempo objetivo de recuperación) según impacto del negocio. No prometer operación offline si no existe un diseño validado de sincronización y resolución de conflictos.

## 10. Observabilidad y protección de datos

Registrar cambios de rol, denegaciones de acceso, exportaciones, operaciones críticas y eventos de integraciones con hora, identificador de correlación, tenant y resultado. Minimizar datos personales, restringir lectura de logs y definir retención.

La auditoría debe permitir investigar una anomalía sin crear un repositorio paralelo de información sensible.

## 11. Modelo de amenazas de ejemplo

| Activo | Amenaza | Control propuesto | Evidencia |
|---|---|---|---|
| Datos de clientes | Lectura entre tenants | Autorización por recurso | Pruebas cruzadas negativas |
| Ventas | Total manipulado | Cálculo confiable | Prueba de importe alterado |
| Roles | Escalada de privilegios | Permisos de servidor | Prueba con rol operador |
| Webhooks | Evento falsificado | Validación de autenticidad | Evento sintético inválido rechazado |
| Disponibilidad | Reintentos duplicados | Idempotencia | Prueba de solicitud repetida |
| Logs | Exposición de secretos | Minimización y controles de acceso | Revisión de campos registrados |

## 12. Laboratorio conceptual

Crear dos tenants ficticios, A y B, con cuentas y datos sintéticos. En un entorno de pruebas propio, documentar para cada operación: resultado autorizado, resultado denegado entre tenants, registro generado y comportamiento al repetir la solicitud. No usar datos reales ni realizar pruebas en producción sin autorización y plan.

## 13. Preguntas de cierre

1. ¿Por qué un identificador de tenant en la URL no basta para autorizar?
2. ¿Qué diferencia hay entre reglas de datos para clientes y privilegios de servicios backend?
3. ¿Cómo se demuestra aislamiento en lecturas, escrituras y exportaciones?
4. ¿Por qué una venta creada no siempre es un pago conciliado?
5. ¿Cómo se preserva evidencia sin almacenar tokens o datos excesivos?
6. ¿Qué garantías cambiarían si una empresa necesitara una base de datos exclusiva?

## Referencias

- OWASP ASVS: https://owasp.org/www-project-application-security-verification-standard/
- OWASP API Security Top 10: https://owasp.org/API-Security/
- NIST SP 800-207: https://csrc.nist.gov/pubs/sp/800/207/final
- Firebase Security Rules: https://firebase.google.com/docs/rules
- Firestore Security Rules: https://firebase.google.com/docs/firestore/security/get-started
- Firebase Admin SDK: https://firebase.google.com/docs/admin/setup
- NIST CSF 2.0: https://www.nist.gov/cyberframework

*Este caso es un modelo didáctico; no constituye auditoría ni validación de una implementación real.*
