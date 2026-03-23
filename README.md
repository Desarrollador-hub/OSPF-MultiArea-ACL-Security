# OSPF-MultiArea-ACL-Security
# Red de Infraestructura Escalable: OSPF Multi-Área y Seguridad mediante ACLs
Infraestructura de Red de Alta Disponibilidad: OSPF Multi-Área y Seguridad Perimetral

## 1. Introducción
Este proyecto documenta la implementación de una topología de red compleja utilizando **Cisco Packet Tracer**, enfocada en el enrutamiento dinámico y la restricción de acceso mediante **Listas de Control de Acceso (ACL)**.

## 2. Topología y Protocolos
* **Enrutamiento Dinámico**: Implementación de **OSPF (Open Shortest Path First)** con proceso ID 10.
* **Segmentación por Áreas**:
    * **Área 0 (Backbone)**: Enlace troncal entre Router2 y Router3.
    * **Área 10**: Interconexión de Router1 con redes LAN `192.168.1.0/24` y `192.168.2.0/24`.
    * **Área 20**: Interconexión de Router4 con redes LAN `192.168.3.0/24` y `192.168.4.0/24`.

## 3. Implementación de Seguridad (ACLs)
### A. Gestión Remota (Telnet)
Se ha restringido el acceso administrativo únicamente a los vecinos directos para proteger el plano de gestión:
* **Router1**: Solo permite acceso desde la IP `10.0.0.6` (Router2).
* **Router4**: Solo permite acceso desde la IP `10.0.0.9` (Router3).

### B. Filtrado de Tráfico (ACL Estándar)
En el **Router4**, se aplica la ACL `RESTRICCION` para filtrar el tráfico de datos hacia las redes locales:
* **Bloqueo de Host**: Deniega el tráfico del host `192.168.1.200`.
* **Bloqueo de Subred**: Deniega el acceso a la red `192.168.2.0/24`.

## 4. Pruebas de Verificación
### Evidencia de Bloqueo Telnet (PC a Router)
```text
C:\> telnet 10.0.0.10
Trying 10.0.0.10 ...
% Connection refused by remote host
```
##  5. Arquitectura Lógica de la Red
La topología se fundamenta en un diseño de tres niveles, optimizando la convergencia mediante **OSPF (ID 10)**.
* **Área 0 (Backbone)**: Núcleo de la red que interconecta los Border Routers (R2 y R3).
* **Área 10 (Acceso Oeste)**: Gestionada por el **Router1**, sirve a las subredes `192.168.1.0/24` y `192.168.2.0/24`.
* **Área 20 (Acceso Este)**: Gestionada por el **Router4**, sirve a las subredes `192.168.3.0/24` y `192.168.4.0/24`.

A continuación se presenta el esquema lógico de la infraestructura implementada:

![Diagrama de Red](DIAGRAM-NETWORK.png)
## Tabla de direccionaminto
En la siguiente tabla se muestra el direccionamiento de interfaces por dispositivo usado en nuestra topologia
| Dispositivo | Interfaz | Dirección IP | Máscara | Propósito / Área |
| :--- | :--- | :--- | :--- | :--- |
| **Router1** | Gig0/0 | 10.0.0.5 | /30 | Enlace a Router2 (Área 10) |
| | Gig0/1 | 192.168.2.1 | /24 | Gateway LAN 192.168.2.0 |
| | Gig0/2 | 192.168.1.1 | /24 | Gateway LAN 192.168.1.0 |
| **Router2** | Gig0/0 | 10.0.0.1 | /30 | Enlace a Router3 (Backbone Área 0) |
| | Gig0/1 | 10.0.0.6 | /30 | Enlace a Router1 (Área 10) |
| **Router3** | Gig0/0 | 10.0.0.2 | /30 | Enlace a Router2 (Backbone Área 0) |
| | Gig0/1 | 10.0.0.9 | /30 | Enlace a Router4 (Área 20) |
| **Router4** | Gig0/0 | 10.0.0.10 | /30 | Enlace a Router3 (Área 20) |
| | Gig0/1 | 192.168.3.1 | /24 | Gateway LAN 192.168.3.0 |
| | Gig0/2 | 192.168.4.1 | /24 | Gateway LAN 192.168.4.0 |

