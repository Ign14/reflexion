# Capítulo 2 — Hardware, medios y topologías

**Edición ampliada 0.2 · Tiempo sugerido: 120 minutos**

## Resultados esperados
Distinguir funciones de los equipos, reconocer topologías, comparar medios y documentar inventario sin interrumpir operaciones.

## 1. Qué hace cada equipo

| Equipo | Función principal | Qué no debe asumirse |
|---|---|---|
| NIC | Conectar el dispositivo al enlace | No verifica permisos de aplicación |
| Switch Ethernet | Reenviar tramas dentro de un dominio de enlace según aprendizaje MAC | No es automáticamente un firewall |
| Router | Encaminar paquetes entre redes IP | No garantiza inspección de seguridad |
| AP Wi-Fi | Asociar estaciones y ofrecer acceso inalámbrico | No equivale a acceso autorizado a todos los recursos |
| Módem / ONT | Terminar la tecnología de acceso del operador | No siempre proporciona routing o Wi-Fi |
| Firewall | Aplicar una política de tráfico | No reemplaza validaciones de API |
| UPS | Mantener energía temporalmente | No sustituye continuidad y respaldos |

Un único equipo doméstico puede integrar router, switch, AP y firewall. En instalaciones profesionales pueden ser dispositivos separados o funciones virtualizadas.

## 2. Cómo se representa el tráfico

**Bits** son unidades de información; una **señal** es su representación física. Ethernet usa tramas en el enlace; IP transporta paquetes/datagramas entre redes; TCP presenta un flujo de bytes fiable a la aplicación, mientras UDP entrega datagramas sin garantía propia de recepción u orden. «Paquete» se usa informalmente para distintas unidades; especificar la capa evita confusiones.

En cobre se transmiten señales eléctricas; en fibra, señales ópticas; en radio, señales electromagnéticas. Los tres medios pueden ser seguros o inseguros dependiendo del diseño completo. La fibra no elimina errores de configuración ni fallos de software.

## 3. Capacidad, latencia y pérdida

- **Ancho de banda/capacidad:** tasa máxima nominal bajo condiciones definidas.
- **Throughput:** datos efectivamente transferidos por unidad de tiempo.
- **Latencia:** demora entre puntos o eventos medidos.
- **Jitter:** variación de demora, relevante para voz y tiempo real.
- **Pérdida:** proporción de unidades que no llegan o no son aceptadas.

Ejemplo didáctico: un enlace de 100 Mb/s no garantiza 100 Mb/s de datos útiles de aplicación. Protocolos, retransmisiones, competencia y procesamiento reducen el rendimiento. Una latencia alta puede perjudicar la caja aunque la descarga máxima sea elevada.

## 4. Topologías

**Estrella:** los equipos convergen a un switch o AP; facilita administración, crea dependencias centrales. **Malla:** múltiples rutas potenciales; exige coordinación de encaminamiento. **Punto a punto:** une dos extremos. **Híbrida:** la realidad empresarial suele combinar varias.

```text
          [Internet]
              |
        [Router/Firewall]
              |
          [Switch]
         /    |     \
      [POS] [AP] [Equipo admin]
              |
        [Wi-Fi invitados]
```

La Wi-Fi invitada debe quedar separada por política efectiva; dibujarla debajo del mismo AP no implica aislamiento.

## 5. Direcciones físicas y lógicas

La MAC se utiliza en determinados enlaces y puede ser aleatorizada. Una IP identifica una interfaz dentro de un contexto de encaminamiento. No usar una MAC como identidad permanente de una persona ni como único control de acceso: puede cambiar o suplantarse.

## 6. Riesgos del hardware y firmware

Equipos sin soporte, contraseñas predeterminadas, interfaces administrativas expuestas, cableado sin etiquetar, puntos de acceso no autorizados y alimentación eléctrica inestable. Para cada activo registrar propietario, ubicación general, versión de firmware, soporte, configuración de acceso y procedimiento de actualización.

**Ejemplo:** una impresora de tickets no debería aceptar administración desde la Wi-Fi pública. La política se implementa en el equipo de red adecuado y se comprueba con pruebas autorizadas.

## 7. Laboratorio de inventario visual

Sin alterar conexiones productivas, dibuja el cableado conocido y anota el rol de cada equipo propio. Si no conoces una función, registra «por verificar». Comprueba en documentación oficial si un dispositivo integra router, switch, AP o módem. No asumir que una etiqueta comercial define su comportamiento.

## 8. Cuestionario
1. ¿Qué equipo toma decisiones entre subredes?
2. ¿Por qué una red con alta velocidad nominal puede tener una experiencia lenta?
3. ¿Qué riesgo aparece si un AP permite que invitados lleguen a equipos de caja?
4. ¿Qué cambia cuando el router, el switch y el AP están integrados en una sola caja?
5. ¿Por qué el firmware pertenece al inventario de seguridad?

## Fuentes
- IEEE 802.3 (familia Ethernet): https://www.ieee802.org/3/
- IEEE 802.11 (grupo de trabajo WLAN): https://www.ieee802.org/11/
- NIST CSF 2.0: https://www.nist.gov/cyberframework

*Los estándares completos pueden requerir acceso específico; verificar la edición aplicable antes de citar parámetros concretos.*
