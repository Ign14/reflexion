# 02. Hardware y medios de transmisión

## Objetivos
Diferenciar NIC, switch, router, punto de acceso, módem/ONT y firewall; reconocer propiedades de cobre, fibra y radio.

## Fundamentos
La **NIC** conecta el equipo a un medio físico o inalámbrico. Un **switch** Ethernet reenvía tramas en una red local usando direcciones MAC aprendidas. Un **router** conecta redes IP y toma decisiones de siguiente salto. Un **punto de acceso** enlaza estaciones Wi-Fi con una red; puede estar integrado en un router doméstico. Una **ONT** termina una conexión de fibra óptica de acceso; otros enlaces utilizan equipos diferentes.

Ethernet sobre cobre emplea señales eléctricas; la fibra transporta luz; Wi-Fi y enlaces satelitales utilizan ondas electromagnéticas en bandas y protocolos específicos. La latencia, capacidad, ruido, interferencia, cobertura y disponibilidad varían por tecnología.

## Cómo observar sin interferir
Identificar los puertos y etiquetas de los equipos propios; documentar modelo, firmware, conexiones y alimentación. Nunca desactivar una interfaz o reiniciar un equipo productivo para comprobar una hipótesis sin ventana de mantenimiento.

## Riesgo típico
Un equipo IoT antiguo conectado al mismo segmento de red que terminales de venta puede ampliar la superficie de ataque. La segmentación y administración de actualizaciones reducen exposición, pero deben verificarse.

## Preguntas
¿Qué dispositivo asigna conectividad local? ¿Cuál decide hacia otra red? ¿Cuál administra la señal óptica o radio?
