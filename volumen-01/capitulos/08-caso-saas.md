# 08. Caso aplicado: SaaS multiempresa

## Escenario
Una plataforma permite a múltiples negocios administrar productos, ventas, agenda y clientes desde navegador y móvil.

## Activos
Datos de clientes, cuentas de usuarios, catálogos, órdenes, registros de pago y credenciales de integraciones. Los medios de pago requieren tratamiento conforme a responsabilidades de proveedores y normas aplicables; evitar almacenar datos de tarjeta salvo necesidad validada y controles adecuados.

## Fronteras de confianza
Dispositivo → red local → Internet → terminación TLS → frontend/backend → identidad → autorización → almacenamiento → terceros.

## Riesgos y verificaciones
1. **Acceso entre empresas:** cada lectura y escritura debe aplicar aislamiento en backend/reglas verificables; probar con cuentas de dos empresas de laboratorio.
2. **Privilegios excesivos:** diferenciar roles y revisar operaciones efectivas, no solo botones ocultos.
3. **Manipulación de pedidos:** validar precios, descuentos, inventario y totales en origen confiable.
4. **Sesiones expuestas:** usar manejo seguro de tokens, expiración, revocación según arquitectura y protección frente a XSS.
5. **Errores de integración:** exigir autenticación de webhooks, idempotencia y conciliación.
6. **Fallas de conectividad:** establecer modos degradados, política de reintentos y prevención de ventas duplicadas.
7. **Observabilidad:** registrar eventos de auditoría sin tokens ni datos personales excesivos.

## Ejercicio
Para cada operación crítica (crear venta, exportar datos, invitar un usuario) responder: ¿quién puede ejecutarla?, ¿cómo lo verifica el servidor?, ¿qué se registra?, ¿cómo se revoca?, ¿cómo se recupera de un fallo?

## Reflexión
La protección no se mide por cuántas funciones aparentan seguridad, sino por cuáles garantías son demostrables.
