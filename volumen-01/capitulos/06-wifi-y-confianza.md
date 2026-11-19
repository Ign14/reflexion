# Capítulo 6 — Wi-Fi, radio y límites de confianza

**Edición ampliada 0.4 · Tiempo sugerido: 180 minutos**

> Pregunta guía: si la información viaja por el aire, ¿qué protege realmente una red inalámbrica y dónde termina esa protección?

## Objetivos
Al terminar, distinguirás radio, enlace, autenticación y cifrado; explicarás SSID, BSSID, bandas, canales y roaming; compararás WPA2 y WPA3; diseñarás una red para invitados separada de las cajas; y reconocerás por qué una conexión Wi-Fi legítima no implica permiso de acceso a una aplicación.

## 1. La radio no es magia

Una estación Wi-Fi transmite información mediante ondas electromagnéticas. El entorno modifica la propagación: paredes, materiales, distancia, interferencias, ocupación de canales y orientación de antenas afectan señal y rendimiento. Las bandas de 2,4 GHz, 5 GHz y, en equipos/regiones compatibles, 6 GHz presentan diferentes características de cobertura y disponibilidad. Las regulaciones nacionales determinan qué canales y potencias pueden utilizarse.

**SSID** es el nombre lógico de la red. **BSSID** suele identificar un conjunto básico de servicio mediante una dirección de enlace. Varios puntos de acceso pueden anunciar el mismo SSID y permitir movilidad; el roaming depende de las capacidades y decisiones de clientes e infraestructura.

No confundas **potencia de señal** con **calidad del enlace**: una señal fuerte puede coexistir con interferencia, congestión o una conexión a Internet saturada.

## 2. Del descubrimiento a la asociación

De forma simplificada, un cliente descubre redes disponibles, elige un punto de acceso, realiza los procedimientos de autenticación/asociación de enlace y establece las claves que correspondan al mecanismo de seguridad configurado. El acceso a la red IP requiere pasos adicionales, como obtener configuración IPv4 o IPv6.

```text
[Dispositivo] --radio--> [Punto de acceso] --Ethernet--> [Switch]
       |                       |                           |
   red elegida             enlace Wi-Fi              políticas LAN
       +-----------------------+---------------------------+
                  NO equivale a permiso de API
```

## 3. WPA2, WPA3 y modalidades empresariales

**WPA2-Personal** utiliza una contraseña compartida; su administración es sencilla, pero conocer la contraseña no distingue roles individuales. **WPA3-Personal** emplea SAE, que mejora el intercambio de autenticación frente a mecanismos anteriores, aunque no elimina todos los riesgos del dispositivo ni de la configuración. **WPA2/WPA3-Enterprise** puede usar autenticación 802.1X/EAP con credenciales individuales y un servidor de autenticación, según diseño.

No asumir que «WPA3» basta para una red segura: importan firmware, configuración, protección de administración, aislamiento, monitoreo y dispositivos compatibles. Los modos de transición necesitan evaluación específica.

### Diferencia crítica entre capas

| Protección | Alcance principal | No resuelve |
|---|---|---|
| WPA2/WPA3 | Enlace inalámbrico bajo su configuración | Permisos de usuario en SaaS |
| Segmentación + firewall | Flujos entre redes | Vulnerabilidades de la API |
| TLS/HTTPS | Comunicación entre extremos TLS | Malware en el cliente |
| Autorización del backend | Acciones y datos permitidos | Disponibilidad física de la red |

## 4. Redes abiertas y redes falsas

Una red abierta no exige el mismo mecanismo de acceso protegido que una red personal/empresarial. En algunos entornos existen mecanismos de cifrado para redes abiertas compatibles, pero nunca se debe inferir seguridad de aplicación por el icono Wi-Fi. Un SSID conocido puede ser imitado: el nombre visible no autentica por sí mismo a la organización.

Controles: verificar redes oficiales, evitar portales sospechosos, mantener TLS validado, actualizar dispositivos y no introducir credenciales corporativas en páginas no verificadas.

## 5. Arquitectura para un comercio

```text
                    [Internet]
                        |
                 [Router / Firewall]
                     /        \
               [Red POS]    [Red invitados]
                  |              |
               [Tablets]     [Clientes]
                  |
            [API SaaS por TLS]
```

Una sola radio puede anunciar varios SSID, pero deben mapearse a segmentos y reglas efectivas. La red de invitados no debería iniciar conexiones hacia POS, administración, impresoras o cámaras. La red POS debe permitir solamente servicios necesarios y actualizaciones autorizadas. Una excepción mal diseñada puede anular el aislamiento.

## 6. Amenazas y señales defensivas

- **Punto de acceso no autorizado:** inventario de equipos, revisión de configuración y procedimiento de retiro.
- **Credencial Wi-Fi compartida filtrada:** rotación controlada, redes separadas y autenticación individual cuando sea viable.
- **Firmware desactualizado:** seguimiento de avisos del fabricante y ventanas de mantenimiento.
- **Clientes invitados alcanzan dispositivos internos:** verificar reglas y aislamiento en laboratorio autorizado.
- **Congestión o interferencia:** medición de calidad y capacidad antes de atribuir la causa a un ataque.

Un problema de rendimiento no es, por sí solo, evidencia de un agente malicioso. Correlacionar registros de AP, router, dispositivo y servicio evita conclusiones precipitadas.

## 7. Procedimiento de endurecimiento

1. Inventariar AP, modelo, soporte y versión de firmware.
2. Definir responsables de administración y credenciales individuales cuando sea posible.
3. Elegir el mecanismo WPA compatible más robusto según requisitos y equipos.
4. Separar invitados, operación y administración mediante política comprobable.
5. Desactivar administración remota innecesaria y funciones no requeridas, según evaluación.
6. Mantener registros mínimos y horarios sincronizados.
7. Probar conectividad permitida y denegada solo en redes propias o autorizadas.
8. Documentar recuperación ante pérdida de configuración o reemplazo del AP.

## 8. Ejercicio

Diseña Wi-Fi para una heladería con POS, terminal de pagos, impresora, tablets administrativas y clientes invitados. Describe SSID, segmentos, reglas de comunicación, responsable de credenciales y cómo responderías si un equipo desconocido aparece en el inventario.

## 9. Preguntas de reflexión

1. ¿Por qué una contraseña Wi-Fi no prueba identidad de una persona?
2. ¿Qué protege WPA3 que no debe confundirse con TLS?
3. ¿Por qué dos SSID no garantizan aislamiento?
4. ¿Qué evidencias separarían interferencia de un incidente de seguridad?
5. ¿Qué cambia cuando un dispositivo deja de recibir actualizaciones?

## Fuentes base

- IEEE 802.11 Working Group: https://www.ieee802.org/11/
- Wi-Fi Alliance (seguridad Wi-Fi): https://www.wi-fi.org/
- NIST SP 800-153, Guidelines for Securing WLANs: https://csrc.nist.gov/pubs/sp/800/153/final
- NIST SP 800-207, Zero Trust: https://csrc.nist.gov/pubs/sp/800/207/final

*Verificar la edición vigente de estándares, compatibilidad de equipos y normativa radioeléctrica local antes de implementar cambios.*
