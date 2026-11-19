# 01. El mapa de un sistema conectado

## Pregunta guía
¿Qué ocurre realmente cuando una persona abre una aplicación de gestión empresarial?

## Objetivos
Identificar dispositivos, redes, servicios, puntos de control y fronteras de confianza; diferenciar elementos físicos de servicios lógicos.

## Conceptos
Un sistema conectado no es únicamente una aplicación. Incluye el dispositivo del usuario, su sistema operativo, la interfaz Wi-Fi o Ethernet, el router y la red local, el proveedor de acceso, la resolución de nombres, los servidores y las dependencias externas.

Modelo simplificado:

```text
Persona → navegador/app → SO → tarjeta de red → Wi-Fi/Ethernet
      → router → ISP → Internet → borde/CDN → API → base de datos
                                           ↘ autenticación
```

La flecha muestra una **dependencia lógica**, no una ruta física única: los paquetes pueden seguir trayectos distintos, atravesar NAT, proxies y balanceadores, y regresar por caminos no idénticos.

## Ejemplo empresarial
Al confirmar una venta, la interfaz envía una operación autenticada al backend. El servidor verifica identidad, autorización, contexto de empresa, integridad de los importes y reglas de negocio antes de registrar la transacción. Que el usuario tenga Wi-Fi cifrado no prueba que esas validaciones ocurran.

## Amenazas y controles
- Dispositivo comprometido: gestión de parches y protección de sesiones.
- Red local no confiable: TLS validado de extremo a extremo.
- Identidad suplantada: autenticación robusta y sesiones seguras.
- Acceso cruzado entre empresas: autorización del lado servidor y aislamiento verificable.
- Indisponibilidad: copias de respaldo, recuperación y monitoreo.

## Reflexión
¿Cuáles son los componentes que controlamos y cuáles dependen de terceros? ¿Cómo sabríamos que falló cada uno?
