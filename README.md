# Ataque de Denegación de servicio - Cisco Discovery Protocol
Descripción: Este script automatiza la generación e inyección de tramas CDP (Cisco Discovery Protocol) falsas en la red. Utiliza la librería Scapy para construir paquetes de Capa 2 que son aceptados por routers Cisco debido a la falta de mecanismos de autenticación en el protocolo.

--- REQUISITOS PARA USAR EL SCRIPT ---

1. topologia que tenga conexión de punto a punto
2. direccionamiento
3. version actualizada de python3 ( kali Linux )
4. version actualizada de Scapy ( kali Linux )
5. privilegios de root ( kali Linux )

Objetivo:

Demostrar la vulnerabilidad de confianza intrínseca en protocolos de descubrimiento.

Realizar un agotamiento de recursos (RAM) en el dispositivo objetivo (DoS).

Uso:

- sudo python3 cdp_dos.py

Parámetros clave:

Destination MAC: 01:00:0c:cc:cc:cc (Multicast de Cisco).

configuración:

# La IP y la MAC del router (gateway)
target_ip_gateway = "la ip de tu router" 
target_mac_gateway = "la mac de tu router" 

# La IP y la MAC de la víctima
target_ip_victim = "la ip de la victima"
target_mac_victim = "la mac de la victima" 

# El número de paquetes a enviar en cada iteración
packet_count = 2

--- TOPOLOGIA ---

#use un router c3725 
#use un switch para establecer contacto
#use una vpcs para establecer contacto con la maquina de kali
#use una maquina kali para realizar el ataque

--- que no hice ---

no use vlans ya que no pude configurar el switch IOSvl2 ya que no encontré una forma.

--- DIRECCIONAMIENTO ---

mi matricula: 2024-1414

IP DEL ROUTER = 10.24.14.1
IP DE KALI LINUX = 10.24.14.2
IP DEL VPCS = 10.24.14.3

--- PARAMETROS USADOS ---

#CONFIGURACION DE KALI#

sudo ip addr add 10.24.14.2/24 dev eth0
sudo ip link set dev eth0 up
sudo ip route add default via 10.24.14.1

USE ESTOS COMANDOS PARA AGREGAR LA IP A LA ETH0 Y ASI ESTABLECER CONTACTO CON EL ROUTER Y LA VPCS.

#CONFIGURACION DEL VPCS#

ip 10.24.14.2 255.255.255.0 10.24.14.1

#Mitigación para el ataque de CDP#

El problema de CDP es que confía en cualquier trama que recibe. La solución es limitar dónde se escucha este protocolo.

Medidas Técnicas:
Desactivación Global: Si no utilizas herramientas de gestión de red de Cisco, apaga el protocolo por completo.

- R1(config)# no cdp run

Desactivación por Interfaz: La regla de oro es: Desactivar CDP en todos los puertos que conectan a usuarios finales (puertos de acceso). Solo déjalo activo en puertos que conectan a otros routers o switches de confianza.

- R1(config-if)# no cdp enable

Uso de LLDP con Seguridad: Cambiar a LLDP (Link Layer Discovery Protocol), que es el estándar abierto, y configurar filtros específicos si el hardware lo permite.

