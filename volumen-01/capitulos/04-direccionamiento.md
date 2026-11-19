# Capítulo 4 — Direccionamiento, rutas y segmentación

**Edición ampliada 0.3 · Tiempo sugerido: 150 minutos · Nivel: fundamentos aplicados**

> Pregunta guía: ¿cómo sabe un equipo a dónde enviar un dato y por qué estar conectado a la misma infraestructura no debería conceder acceso a toda la empresa?

## 1. Resultados de aprendizaje

Al terminar podrás: (a) interpretar direcciones IPv4 y prefijos CIDR; (b) explicar la relación entre ARP, IP y puerta de enlace; (c) distinguir DHCP, DNS, NAT y firewall; (d) reconocer aspectos esenciales de IPv6; (e) diseñar una separación entre invitados, cajas y administración; (f) justificar una regla de acceso con evidencia.

## 2. Tres identificadores que suelen confundirse

**MAC** identifica una interfaz en un enlace compatible con direcciones MAC. Puede cambiar, aleatorizarse o suplantarse. No equivale a identidad humana ni prueba autorización.

**IP** permite que dispositivos y routers encaminen datagramas entre redes. La dirección puede ser pública, privada, local al enlace o tener otro alcance según versión y configuración. Una IP observada en el backend no identifica de manera fiable a una persona.

**Puerto de transporte** identifica un extremo de comunicación dentro de TCP o UDP. Una misma dirección IP puede ofrecer muchos servicios; un puerto abierto no implica que su contenido sea seguro.

```text
[Equipo 192.168.10.25/24]
      |
      | Destino 192.168.10.80: mismo prefijo
      +--> resolución ARP en IPv4 --> trama al equipo local
      |
      | Destino 203.0.113.10: fuera del prefijo
      +--> siguiente salto 192.168.10.1 --> router --> ruta externa
```

La decisión real usa la tabla de rutas y la coincidencia de prefijo más largo; el ejemplo supone una red simple, sin rutas adicionales. Las direcciones 203.0.113.0/24 se reservan para documentación y no representan servidores reales.

## 3. Entender CIDR sin memorizar tablas

Una dirección IPv4 tiene 32 bits. En `192.168.10.25/24`, los primeros 24 bits describen el prefijo de red y los 8 restantes la parte de host en el caso convencional. La red es `192.168.10.0/24`. Una subred `/26` tiene 64 direcciones IPv4 totales; en una LAN IPv4 convencional quedan normalmente 62 direcciones asignables a hosts, porque se reservan la dirección de red y la de difusión.

| Prefijo | Direcciones IPv4 totales | Uso didáctico |
|---|---:|---|
| /24 | 256 | Segmento de una sede |
| /25 | 128 | Dos segmentos grandes dentro de un /24 |
| /26 | 64 | Cuatro segmentos dentro de un /24 |
| /30 | 4 | Enlaces IPv4 clásicos punto a punto |
| /32 | 1 | Ruta a una dirección específica |

La cantidad de direcciones no debe confundirse con la capacidad de usuarios simultáneos ni con la capacidad de la red.

## 4. ¿Qué ocurre al conectarse?

En IPv4, DHCP suele entregar dirección, máscara, router y resolutores DNS. No autentica por sí mismo la legitimidad de todos los equipos de la LAN. En IPv6, la configuración puede emplear anuncios de router, SLAAC y/o DHCPv6; no se debe suponer que funciona igual que IPv4.

**ARP** ayuda a conocer la dirección de enlace del siguiente salto IPv4 en redes compatibles. IPv6 utiliza Neighbor Discovery basado en ICMPv6, no ARP. Ambos mecanismos necesitan consideración de seguridad en entornos empresariales.

**DNS** traduce nombres a registros, entre otras funciones; no asigna direcciones IP a las interfaces. **DHCP** configura parámetros; no comprueba que una API permita realizar una venta.

## 5. NAT: utilidad y límites

NAT puede traducir direcciones o puertos, por ejemplo al permitir que varios equipos privados compartan una dirección pública IPv4. La traducción suele conservar estado de flujos. Aunque ciertos esquemas de NAT impiden conexiones entrantes no solicitadas, **NAT no sustituye una política de firewall**, no protege por sí mismo el tráfico de salida y no valida solicitudes HTTP.

IPv6 fue diseñado para funcionar sin NAT como requisito de conectividad. Esto no significa que deba dejarse una red IPv6 sin filtrado. Una política defensiva debe contemplar ambas familias de direcciones.

## 6. Segmentación empresarial

Ejemplo de red para un comercio:

```text
                        [Internet]
                            |
                      [Firewall/router]
                         /    |     \
                  VLAN 10  VLAN 20  VLAN 30
                  Admin    Caja     Invitados
                  /24      /24      /24
              10.10.10.0 10.10.20.0 10.10.30.0
```

El diagrama describe separación lógica, no garantías automáticas. El router o firewall debe aplicar reglas entre segmentos. En un equipo que no soporte VLAN, pueden necesitarse AP y routers separados o una arquitectura diferente.

### Política mínima ilustrativa

| Origen | Destino | Decisión | Motivo |
|---|---|---|---|
| Invitados | Administración | Denegar | Evitar acceso lateral |
| Invitados | Caja | Denegar | Aislar equipos de operación |
| Caja | API externa necesaria | Permitir según servicio | Procesar ventas |
| Administración | Consola de red | Permitir solo roles autorizados | Mantenimiento |
| Cualquier segmento | Servicios no requeridos | Denegar por defecto, con excepciones justificadas | Reducir superficie |

Las reglas reales deben contemplar DNS, DHCP, actualizaciones, sincronización de tiempo y dependencias documentadas. «Denegar por defecto» sin inventario de flujos puede detener el negocio.

## 7. Amenazas, controles y límites

- **Equipo invitado comprometido:** aislar invitados; probar que no alcanzan redes internas.
- **Dispositivo IoT sin soporte:** segmentar y limitar destinos; gestionar sustitución.
- **Configuración DHCP no autorizada:** proteger infraestructura de acceso y vigilar anomalías donde el equipo lo soporte.
- **Exposición de administración:** limitar interfaz de gestión, MFA cuando exista y actualización de firmware.
- **Reglas inconsistentes IPv4/IPv6:** revisar ambas familias; una política solo IPv4 deja brechas.

## 8. Ejercicio aplicado

Una empresa tiene tablets POS, notebooks administrativos, cámaras IP y Wi-Fi de clientes. Diseña cuatro segmentos y una matriz de comunicación permitida. Justifica qué flujos son indispensables y cómo comprobarías las denegaciones en un laboratorio autorizado. No escanear redes ajenas.

## 9. Comprobación

1. ¿Por qué una IP pública no identifica a una persona?
2. ¿Qué diferencia existe entre ARP y DNS?
3. ¿Qué función cumple la ruta por defecto?
4. ¿Por qué una VLAN no garantiza aislamiento?
5. ¿Qué error aparece si las reglas IPv6 son más permisivas que las IPv4?
6. ¿Por qué la conexión al Wi-Fi corporativo no debería dar acceso a toda la base de datos?

## Fuentes base

- RFC 1918 (IPv4 privado): https://www.rfc-editor.org/rfc/rfc1918
- RFC 4632 (CIDR): https://www.rfc-editor.org/rfc/rfc4632
- RFC 826 (ARP): https://www.rfc-editor.org/rfc/rfc826
- RFC 4861 (IPv6 Neighbor Discovery): https://www.rfc-editor.org/rfc/rfc4861
- RFC 8200 (IPv6): https://www.rfc-editor.org/rfc/rfc8200
- RFC 2131 (DHCPv4): https://www.rfc-editor.org/rfc/rfc2131
- NIST SP 800-207: https://csrc.nist.gov/pubs/sp/800/207/final
