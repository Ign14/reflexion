# Laboratorio 04 — Verificar aislamiento multiempresa

**Tipo:** ejercicio defensivo con datos sintéticos · **Duración:** 120 minutos

## Requisitos y autorización
Usar exclusivamente un entorno propio de pruebas, dos tenants ficticios y usuarios de laboratorio. No ejecutar sobre clientes reales ni producción. Preparar respaldo y procedimiento de limpieza.

## Objetivo
Comprobar que un usuario del tenant A no puede leer, modificar ni exportar recursos del tenant B, aun si altera parámetros de la interfaz.

## Preparación
Crear empresa ficticia A y empresa ficticia B, cada una con un producto y un cliente inventados. Crear roles administrador y operador. Registrar matriz esperada de permisos antes de las pruebas.

## Casos de prueba

| Caso | Actor | Operación | Resultado esperado |
|---|---|---|---|
| 1 | Admin A | Leer producto A | Permitido |
| 2 | Admin A | Leer producto B | Denegado |
| 3 | Operador A | Cambiar rol de A | Denegado si política no lo autoriza |
| 4 | Operador A | Crear venta A | Permitido según política |
| 5 | Operador A | Crear venta B | Denegado |
| 6 | Admin A | Exportar clientes B | Denegado |
| 7 | Admin A | Repetir misma venta con misma clave | Una sola operación lógica |

## Registro de resultados
Anotar identificador sintético del caso, fecha, versión de reglas/backend, resultado esperado, resultado observado, evidencia redactada y conclusión. No guardar credenciales ni tokens en el informe.

## Criterios de cierre
Todas las operaciones cruzadas deben denegarse, las operaciones legítimas deben funcionar y el sistema debe producir registros de auditoría mínimos. Si una prueba falla, suspender la promoción de esa versión a producción y documentar corrección y nueva verificación.

## Reflexión
¿Qué otras rutas de acceso podrían eludir la prueba: exportaciones, tareas programadas, SDK privilegiados, respaldos o integraciones?
