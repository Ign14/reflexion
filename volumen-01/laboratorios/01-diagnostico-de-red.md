# Laboratorio 01. Observar la red propia sin invadirla

**Modalidad:** diagnóstico pasivo y pruebas hacia destinos controlados.  
**Permiso:** ejecutar solo en equipo propio y conexiones autorizadas.

## Objetivos
Identificar interfaz, dirección, ruta predeterminada, resolución DNS y síntomas de un fallo de acceso.

## Preparación
Un computador personal, terminal y acceso a una red propia. Anotar sistema operativo, fecha y si se usa Ethernet o Wi-Fi. No publicar direcciones IP públicas, identificadores de dispositivos ni resultados con datos personales.

## Paso 1. Estado de interfaces
- Windows: `ipconfig /all`
- macOS: `networksetup -listallhardwareports` y `ifconfig`
- Linux: `ip -br addr`

Registrar interfaz utilizada, dirección local y estado de conexión. Algunas interfaces virtuales (VPN, contenedores) pueden aparecer junto a la física.

## Paso 2. Ruta
- Windows: `route print`
- macOS: `netstat -rn`
- Linux: `ip route` y `ip -6 route`

Encontrar la ruta predeterminada en la familia IP utilizada. Distinguir puerta de enlace local de destino remoto.

## Paso 3. Resolución DNS
Consultar `example.com` con `nslookup example.com` o `dig example.com` si está instalado. Contrastar el nombre devuelto, el tipo de registro y la respuesta. El resultado puede variar por ubicación y momento.

## Paso 4. Conexión HTTPS
Abrir `https://example.com` en un navegador actualizado. Inspeccionar el candado o detalles de conexión y constatar que la validación TLS sea correcta. No ignorar alertas de certificado.

## Paso 5. Registro de evidencias
| Observación | Hipótesis | Evidencia no sensible | Interpretación |
|---|---|---|---|
| Interfaz activa | Existe enlace | Estado | Confirmar/descartar |
| Dirección local | Existe configuración IP | Prefijo anonimizado | Confirmar/descartar |
| Ruta predeterminada | Hay camino fuera de la LAN | Presencia de ruta | Confirmar/descartar |
| DNS responde | Nombre resoluble | Tipo de respuesta | Confirmar/descartar |
| HTTPS válido | Certificado/TLS aceptado | Nombre y estado | Confirmar/descartar |

## Seguridad y cierre
No realizar barridos de puertos, captura de comunicaciones ajenas, pruebas de credenciales ni manipulación de routers sin permiso. La ausencia de respuesta ICMP no prueba indisponibilidad. Terminar con un diagrama de la red y tres riesgos controlables.
