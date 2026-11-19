# 04. Direcciones y redes locales

## Objetivos
Distinguir MAC, IP, máscara/prefijo, gateway, DHCP, DNS y NAT.

## Idea central
Una dirección **MAC** identifica una interfaz en un dominio de enlace y puede ser aleatoria o modificable. Una dirección **IP** permite dirigir paquetes entre redes según su alcance y políticas. El **prefijo** define la porción de red; por ejemplo, `192.168.10.25/24` pertenece a `192.168.10.0/24` en el caso habitual de una subred IPv4.

**DHCP** puede entregar dirección, puerta de enlace, DNS y otros parámetros. La **puerta de enlace predeterminada** recibe tráfico con destino fuera de las rutas más específicas. **NAT** traduce direcciones y/o puertos en un dispositivo intermedio; no debe tratarse como sustituto completo de firewall. IPv6 no requiere NAT para operar en Internet.

## Ejemplo
Separar terminales de caja, dispositivos invitados y servidores administrativos en VLAN/subredes diferentes permite aplicar políticas entre segmentos; **una VLAN por sí sola no es un control suficiente** si el equipo de capa 3 permite todo el tráfico.

## Actividad de papel
Dibujar una red con `192.168.10.0/24` para administración y `192.168.20.0/24` para invitados. Marcar router, DHCP y reglas que impedirían acceso desde invitados a administración.

## Reflexión
¿Quién puede comunicarse con quién, y dónde se hace cumplir esa política?
