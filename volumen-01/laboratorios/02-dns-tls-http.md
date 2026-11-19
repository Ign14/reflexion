# Laboratorio 02 — Seguir una solicitud HTTPS sin invadir redes

**Tiempo:** 60–90 minutos · **Alcance:** equipo propio y dominio de documentación `example.com` · **Modalidad:** observación no intrusiva.

## Objetivos
Distinguir resolución DNS, acceso HTTPS, negociación TLS y estado HTTP. Practicar el registro de evidencia sin credenciales ni datos personales.

## Preparación y límites
Utilizar una computadora propia con conexión autorizada. No capturar tráfico de otros usuarios, no realizar escaneos de puertos ni automatizar solicitudes masivas. Los comandos siguientes son de consulta; las respuestas pueden variar.

## Paso 1. Resolución
En Windows, macOS o Linux, si está disponible:

```sh
nslookup example.com
```

Anotar si se obtiene respuesta y qué tipo de dirección aparece. Si no funciona, distinguir fallo de herramienta, configuración del resolutor y conectividad. No interpretar la IP de respuesta como identidad de un usuario.

## Paso 2. Cabeceras HTTPS

```sh
curl -I https://example.com
```

Observar el código de estado y encabezados. `-I` solicita encabezados mediante HEAD; el comportamiento de HEAD puede diferir del GET y algunos servidores no lo admiten. No añadir `-k` o `--insecure`.

## Paso 3. Información de conexión

```sh
curl -v -o /dev/null https://example.com
```

En Windows PowerShell, usar `curl.exe` si `curl` es un alias. En Windows, reemplazar `/dev/null` por `NUL` cuando corresponda. La salida detallada puede incluir metadatos de red: revisarla antes de compartirla. Nunca ejecutar el comando con credenciales o cookies reales.

## Paso 4. Comparar observaciones

| Etapa | Evidencia | Qué confirma | Qué no confirma |
|---|---|---|---|
| DNS | Respuesta del resolutor | Nombre resoluble en ese momento | Identidad del servidor |
| Transporte | Conexión establecida | Alcance de un extremo | Autorización de negocio |
| TLS | Validación aceptada | Identidad y canal según configuración | Seguridad del dispositivo |
| HTTP | Código de estado | Resultado de una solicitud | Integridad de una venta externa |

## Paso 5. Diagnóstico guiado

Si DNS funciona pero TLS falla, enumerar tres causas plausibles: hora incorrecta, nombre/certificado no coincidente o cadena de confianza inválida. Si TLS funciona pero HTTP devuelve 403, investigar políticas de aplicación o de intermediarios, no asumir un problema de cifrado.

## Entregable
Un informe de una página: fecha, sistema operativo, red propia, comandos, resultados **anonimizados**, interpretación y dos limitaciones de lo observado.

## Cierre seguro
No modificar configuración productiva, no desactivar validaciones de certificados y no publicar tokens, direcciones internas completas, cookies ni trazas sensibles.
