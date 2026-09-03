# UNIVERSIDAD DE SAN CARLOS DE GUATEMALA

## Facultad de Ingeniería
## Ingeniería en Ciencias y Sistemas

<br>

# PROYECTO 1
# CHAPIN RED

### Redes de Computadoras 2

<br>

**Estudiante:** Keitlyn Valentina Tunchez Castañeda  
**Carné:** 202201139  
**Curso:** Redes de Computadoras 2  
**Proyecto:** Proyecto 1 - Chapin Red  
**Fecha:** 04 Septiembre 2026  

---

# Índice

1. Introducción
2. Objetivos
3. Topología de red
4. Direccionamiento IP y subneteo
5. Configuración de VLAN
6. Configuración VTP
7. EtherChannel
   - 7.1 LACP - Edificio izquierdo
   - 7.2 PAgP - Edificio derecho
8. Tolerancia a fallos
   - 8.1 Tolerancia a fallos LACP
   - 8.2 Tolerancia a fallos PAgP
9. Spanning Tree Protocol - STP
10. Enrutamiento dinámico EIGRP
11. DHCP y DHCP Relay
12. Listas de control de acceso - ACL
13. Pruebas de conectividad
14. Verificación final
15. Conclusiones

---

# 1. Introducción

En el presente proyecto se realizó el diseño, configuración y comprobación de una red empresarial utilizando Cisco Packet Tracer. La topología se encuentra distribuida principalmente en dos edificios y está compuesta por switches multicapa, switches de acceso y diferentes equipos finales.

Para el funcionamiento de la red se implementaron diferentes tecnologías y protocolos estudiados durante el curso, entre ellos VLAN, VTP, EtherChannel, STP, EIGRP, DHCP y listas de control de acceso (ACL).

También se configuraron mecanismos de redundancia mediante EtherChannel, utilizando LACP en el edificio izquierdo y PAgP en el edificio derecho. Esto permite aprovechar múltiples enlaces físicos como un solo enlace lógico y mantener la comunicación incluso ante la pérdida de uno de sus enlaces.

Finalmente, se realizaron diferentes pruebas de conectividad, asignación dinámica de direcciones IP, funcionamiento de ACL y tolerancia a fallos para comprobar que las configuraciones realizadas funcionaran correctamente.

---

# 2. Objetivos

## 2.1 Objetivo general

Diseñar e implementar una topología de red funcional en Cisco Packet Tracer aplicando los conceptos y protocolos estudiados durante el curso de Redes de Computadoras 2.

## 2.2 Objetivos específicos

- Segmentar la red mediante VLAN según los diferentes grupos de usuarios.
- Utilizar VTP para facilitar la distribución de las VLAN.
- Implementar EtherChannel mediante LACP y PAgP.
- Comprobar la tolerancia a fallos de los enlaces EtherChannel.
- Utilizar STP para prevenir bucles de capa 2.
- Configurar EIGRP con el sistema autónomo 39 para el enrutamiento dinámico.
- Implementar DHCP para la asignación automática de parámetros de red.
- Utilizar DHCP Relay mediante `ip helper-address`.
- Aplicar ACL para controlar la comunicación entre los diferentes segmentos.
- Verificar mediante pruebas de conectividad el funcionamiento de la red.

---

# 3. Topología de red

La topología implementada está formada por dos áreas principales: el edificio izquierdo y el edificio derecho.

Dentro de cada edificio se utilizaron switches multicapa y switches de acceso para conectar los diferentes dispositivos finales. Los switches multicapa también permiten realizar el enrutamiento necesario entre las distintas redes.

Los enlaces redundantes entre switches fueron agrupados mediante EtherChannel y los diferentes segmentos de usuarios fueron separados utilizando VLAN.

### Evidencia: Topología completa

![Topología completa](https://i.ibb.co/V092xjYT/Topolog-a-completa-P1.png)

---

# 4. Direccionamiento IP y subneteo

Para las redes LAN se utilizó el bloque `192.188.39.0/24`, el cual fue dividido de acuerdo con la cantidad de hosts necesaria para cada segmento.

## 4.1 Redes LAN - VLSM

| VLAN | Segmento | Red | Máscara | Gateway | Hosts utilizables | Broadcast |
|---:|---|---|---|---|---|---|
| 10 | Naranja - Edificio izquierdo | `192.188.39.0/27` | `255.255.255.224` | `192.188.39.1` | `192.188.39.1 - 192.188.39.30` | `192.188.39.31` |
| 20 | Verde - Edificio izquierdo | `192.188.39.32/27` | `255.255.255.224` | `192.188.39.33` | `192.188.39.33 - 192.188.39.62` | `192.188.39.63` |
| 30 | Naranja - Edificio derecho | `192.188.39.64/28` | `255.255.255.240` | `192.188.39.65` | `192.188.39.65 - 192.188.39.78` | `192.188.39.79` |
| 40 | Verde - Edificio derecho | `192.188.39.80/28` | `255.255.255.240` | `192.188.39.81` | `192.188.39.81 - 192.188.39.94` | `192.188.39.95` |
| 99 | Administración | `192.188.39.96/29` | `255.255.255.248` | `192.188.39.97` | `192.188.39.97 - 192.188.39.102` | `192.188.39.103` |

## 4.2 Enlaces de enrutamiento - FLSM /30

Para los enlaces punto a punto entre los dispositivos multicapa se utilizaron subredes `/30`, todas con máscara `255.255.255.252`.

| No. | Red | Máscara | Hosts utilizables | Broadcast |
|---:|---|---|---|---|
| 1 | `10.4.39.0/30` | `255.255.255.252` | `10.4.39.1 - 10.4.39.2` | `10.4.39.3` |
| 2 | `10.4.39.4/30` | `255.255.255.252` | `10.4.39.5 - 10.4.39.6` | `10.4.39.7` |
| 3 | `10.4.39.8/30` | `255.255.255.252` | `10.4.39.9 - 10.4.39.10` | `10.4.39.11` |
| 4 | `10.4.39.12/30` | `255.255.255.252` | `10.4.39.13 - 10.4.39.14` | `10.4.39.15` |
| 5 | `10.4.39.16/30` | `255.255.255.252` | `10.4.39.17 - 10.4.39.18` | `10.4.39.19` |
| 6 | `10.4.39.20/30` | `255.255.255.252` | `10.4.39.21 - 10.4.39.22` | `10.4.39.23` |
| 7 | `10.4.39.24/30` | `255.255.255.252` | `10.4.39.25 - 10.4.39.26` | `10.4.39.27` |
| 8 | `10.4.39.28/30` | `255.255.255.252` | `10.4.39.29 - 10.4.39.30` | `10.4.39.31` |

## 4.3 Configuración de enlaces de capa 3

Los enlaces punto a punto se configuraron como interfaces enrutadas utilizando `no switchport`.

Ejemplo en MS1:

```text
enable
configure terminal

interface gigabitEthernet1/1/3
 no switchport
 ip address 10.4.39.1 255.255.255.252
 no shutdown
exit

interface gigabitEthernet1/1/4
 no switchport
 ip address 10.4.39.5 255.255.255.252
 no shutdown
exit

end
write memory
```

Ejemplo en MS6:

```text
configure terminal

interface gigabitEthernet1/1/3
 no switchport
 ip address 10.4.39.14 255.255.255.252
 no shutdown
exit

interface gigabitEthernet1/1/4
 no switchport
 ip address 10.4.39.17 255.255.255.252
 no shutdown
exit

end
write memory
```

Verificación:

```text
show ip interface brief
```

---

# 5. Configuración de VLAN

Las VLAN permiten separar lógicamente los diferentes grupos de usuarios aunque se encuentren conectados dentro de una misma infraestructura física.

En el edificio izquierdo se utilizaron principalmente:

- VLAN 10: `VLAN_Naranja_EdificioIZQ_202201139`
- VLAN 20: `VLAN_Verde_EdificioIZQ_202201139`

En el edificio derecho se utilizaron:

- VLAN 30: `VLAN_Naranja_EdificioDER_202201139`
- VLAN 40: `VLAN_Verde_EdificioDER_202201139`

Además se utilizó:

- VLAN 99: `VLAN_ADMIN_202201139`

Los puertos correspondientes a los equipos finales fueron configurados en modo acceso y asignados a su respectiva VLAN.

## Comandos utilizados para VLAN

```text
enable
configure terminal

vlan 10
 name VLAN_Naranja_EdificioIZQ_202201139
exit

vlan 20
 name VLAN_Verde_EdificioIZQ_202201139
exit

interface fa0/1
 switchport mode access
 switchport access vlan 10
exit

interface fa0/2
 switchport mode access
 switchport access vlan 20
exit

end
write memory
```

Verificación:

```text
show vlan brief
show interfaces fa0/1 switchport
show interfaces fa0/2 switchport
```

### Evidencia: VLAN configuradas en SW1

![VLANs](https://i.ibb.co/wNN4RxHN/VLANs-SW1.png)

---

# 6. Configuración VTP

Para facilitar la administración y propagación de las VLAN se configuró VTP.

El dominio utilizado fue:

`CHAPINRED`

Dentro de la topología se utilizaron dispositivos trabajando como servidores VTP y otros como clientes. Esto permitió distribuir la información correspondiente a las VLAN entre los switches conectados mediante enlaces troncales.

Durante la comprobación se verificaron parámetros como:

- Dominio VTP.
- Modo de operación.
- Número de VLAN existentes.
- Número de revisión.
- Estado de VTP.

## Comandos utilizados para VTP

Servidor:

```text
enable
configure terminal
vtp domain CHAPINRED
vtp mode server
end
write memory
```

Clientes:

```text
enable
configure terminal
vtp domain CHAPINRED
vtp mode client
end
write memory
```

Verificación:

```text
show vtp status
```

### Evidencia: VTP

![VTP](https://i.ibb.co/G3FJVgGm/VTP-MS9.png)

---

# 7. EtherChannel

EtherChannel permite agrupar varios enlaces físicos para que funcionen como un único enlace lógico.

Esta configuración proporciona mayor disponibilidad y redundancia, ya que la pérdida de uno de los enlaces físicos no necesariamente provoca la caída completa de la comunicación.

En la topología se utilizaron:

- **LACP:** edificio izquierdo.
- **PAgP:** edificio derecho.

## 7.1 LACP - Edificio izquierdo

En el edificio izquierdo los enlaces EtherChannel fueron configurados mediante LACP.

La verificación se realizó utilizando:

```text
show etherchannel summary
```

Los Port-Channel correctamente establecidos presentan el estado `SU`.

- `S`: EtherChannel de capa 2.
- `U`: Port-Channel actualmente en uso.
- `(P)`: interfaz física correctamente agrupada dentro del Port-Channel.

### Comandos utilizados para LACP

```text
enable
configure terminal

interface range fa0/1-3
 channel-group 1 mode active
exit

interface range fa0/4-6
 channel-group 2 mode active
exit

interface range fa0/7-8
 channel-group 4 mode active
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20
exit

interface port-channel 2
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20
exit

interface port-channel 4
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20
exit

end
write memory
```

Verificación:

```text
show etherchannel summary
show interfaces trunk
```

### Evidencia: LACP

![LACP](https://i.ibb.co/twqs30hb/LACP-izquierdo.png)

### Evidencia adicional de EtherChannel

![EtherChannel LACP y PAgP](https://i.ibb.co/NRZnQGn/Ether-Channel-LACP-PAg-P.png)

Esta segunda evidencia permite observar simultáneamente diferentes EtherChannel configurados dentro de la topología.

## 7.2 PAgP - Edificio derecho

En el edificio derecho se utilizó PAgP para formar los enlaces EtherChannel.

De la misma manera, los Port-Channel activos presentan el estado `SU` y sus interfaces integrantes aparecen identificadas mediante `(P)`.

### Comandos utilizados para PAgP

```text
enable
configure terminal

interface range fa0/1-4
 channel-group 7 mode desirable
exit

interface range fa0/5-6
 channel-group 8 mode desirable
exit

interface port-channel 7
 switchport mode trunk
exit

interface port-channel 8
 switchport mode trunk
exit

end
write memory
```

Verificación:

```text
show etherchannel summary
```

### Evidencia: PAgP

![PAgP](https://i.ibb.co/rG2ft7jW/PAg-P-Derecho.png)

---

# 8. Tolerancia a fallos

Una característica importante de EtherChannel es la redundancia proporcionada por la utilización de múltiples enlaces físicos.

Para comprobarla se realizaron pruebas deshabilitando temporalmente una interfaz perteneciente a un Port-Channel.

## 8.1 Tolerancia a fallos LACP

Se deshabilitó uno de los enlaces físicos pertenecientes a un EtherChannel LACP.

```text
enable
configure terminal

interface fa0/7
 shutdown
end

show etherchannel summary
```

Después de comprobar la tolerancia se restauró el enlace:

```text
configure terminal

interface fa0/7
 no shutdown
end

write memory
```

El canal pudo continuar funcionando mediante los enlaces restantes, comprobando que la pérdida de un único enlace físico no provoca necesariamente la pérdida completa del Port-Channel.

### Evidencia

![Tolerancia a fallos LACP](https://i.ibb.co/ycJk9KVv/Tolerancia-fallos-LACP.png)

## 8.2 Tolerancia a fallos PAgP

La misma comprobación fue realizada sobre uno de los EtherChannel PAgP.

```text
enable
configure terminal

interface fa0/5
 shutdown
end

show etherchannel summary
```

Después de la prueba se restauró el enlace:

```text
configure terminal

interface fa0/5
 no shutdown
end

write memory
```

### Evidencia

![Tolerancia a fallos PAgP](https://i.ibb.co/60q9Zpjt/Tolerancia-fallos-PAg-P.png)

---

# 9. Spanning Tree Protocol - STP

STP se utiliza para evitar bucles de capa 2 cuando existen caminos redundantes dentro de una red.

La utilización de EtherChannel y múltiples conexiones entre los switches hace especialmente importante el funcionamiento de STP.

Durante las verificaciones se observaron los estados y roles de los diferentes Port-Channel.

El estado `FWD` indica que una interfaz se encuentra en **Forwarding**, por lo que puede transportar tráfico normalmente.

## Comandos utilizados para verificar STP

```text
show spanning-tree
show spanning-tree vlan 10
show spanning-tree vlan 20
show running-config | include spanning-tree
```

La configuración observada fue:

```text
spanning-tree mode pvst
```

### Evidencia: STP VLAN 10

![STP VLAN 10](https://i.ibb.co/YFGjx9cP/STP-VLAN10.png)

La evidencia permite comprobar el comportamiento de STP sobre la VLAN 10 y los Port-Channel involucrados.

---

# 10. Enrutamiento dinámico EIGRP

Para comunicar las diferentes redes de la topología se utilizó EIGRP.

El sistema autónomo configurado fue:

`AS 39`

Los switches multicapa anuncian mediante EIGRP las redes que les corresponden, incluyendo los enlaces punto a punto `/30` y las redes LAN necesarias.

## Comandos utilizados para EIGRP

MS7:

```text
enable
configure terminal

router eigrp 39
 network 10.4.39.0 0.0.0.3
 network 10.4.39.8 0.0.0.3
 network 10.4.39.12 0.0.0.3
 network 192.188.39.0 0.0.0.31
 network 192.188.39.32 0.0.0.31

end
```

MS1:

```text
router eigrp 39
 network 10.4.39.0 0.0.0.3
 network 10.4.39.4 0.0.0.3
 network 10.4.39.24 0.0.0.3
 network 10.4.39.28 0.0.0.3
```

MS2:

```text
router eigrp 39
 network 10.4.39.4 0.0.0.3
 network 10.4.39.8 0.0.0.3
 network 10.4.39.16 0.0.0.3
 network 10.4.39.20 0.0.0.3
```

MS6:

```text
router eigrp 39
 network 10.4.39.12 0.0.0.3
 network 10.4.39.16 0.0.0.3
 network 192.188.39.96 0.0.0.7
```

Verificación:

```text
show ip eigrp neighbors
show ip route
show running-config | section router eigrp
```

### Evidencia: Configuración EIGRP AS 39

![Configuración EIGRP AS 39](https://i.ibb.co/XZd83vf8/Configuracion-EIGRP-AS39.png)

Esta evidencia permite observar las sentencias `network` configuradas en distintos switches multicapa.

## 10.1 Vecinos EIGRP

Para verificar la formación de las adyacencias se utilizó:

```text
show ip eigrp neighbors
```

La aparición de los vecinos correspondientes confirma que existe comunicación entre los dispositivos y que EIGRP puede intercambiar información de enrutamiento.

### Evidencia

![Vecinos EIGRP](https://i.ibb.co/Zp61N5FK/EIGRP-MS7.png)

## 10.2 Tabla de enrutamiento

También se verificó la tabla de enrutamiento para comprobar que las redes remotas fueran aprendidas correctamente.

### Evidencia

![Tabla de enrutamiento](https://i.ibb.co/0yXkFz88/Tabla-de-enrutamiento-MS1.png)

La presencia de rutas hacia redes remotas demuestra que la información de enrutamiento se está propagando dentro de la topología.

---

# 11. DHCP y DHCP Relay

DHCP fue utilizado para asignar automáticamente a los equipos finales parámetros como:

- Dirección IPv4.
- Máscara de subred.
- Gateway predeterminado.
- Servidor DHCP.

Debido a que los servidores DHCP no se encuentran necesariamente dentro del mismo dominio de broadcast que los clientes, se utilizó DHCP Relay mediante `ip helper-address`.

Las interfaces VLAN correspondientes actúan como gateway de cada segmento y reenvían las solicitudes DHCP hacia el servidor configurado.

En el edificio izquierdo se utilizaron:

- VLAN 10 → `192.188.39.1/27`
- VLAN 20 → `192.188.39.33/27`

En el edificio derecho:

- VLAN 30 → `192.188.39.65/28`
- VLAN 40 → `192.188.39.81/28`

## Comandos utilizados para DHCP Relay

MS7:

```text
enable
configure terminal

interface vlan 10
 ip address 192.188.39.1 255.255.255.224
 ip helper-address 10.4.39.26
 no shutdown
exit

interface vlan 20
 ip address 192.188.39.33 255.255.255.224
 ip helper-address 10.4.39.26
 no shutdown
exit

end
```

MS5:

```text
configure terminal

interface vlan 30
 ip address 192.188.39.65 255.255.255.240
 ip helper-address 10.4.39.30
 no shutdown
exit

interface vlan 40
 ip address 192.188.39.81 255.255.255.240
 ip helper-address 10.4.39.30
 no shutdown
exit

end
```

Verificación:

```text
show running-config | section interface Vlan
show ip interface brief
```

En los equipos finales:

```text
ipconfig /all
```

### Evidencia: DHCP Relay e IP Helper

![DHCP Relay e IP Helper](https://i.ibb.co/PG02MsVZ/DHCP-Relay-IP-Helper.png)

### Evidencia: DHCP en ambos edificios

![DHCP en ambos edificios](https://i.ibb.co/G4MNqqVb/DHCP-Ambos-Edificios.png)

Estas pruebas permiten comprobar tanto la configuración del relay como la asignación dinámica de direcciones a los clientes.

---

# 12. Listas de control de acceso - ACL

Las listas de control de acceso extendidas se utilizaron para controlar qué segmentos pueden iniciar comunicación hacia otros segmentos.

Se configuraron ACL específicas para los edificios izquierdo y derecho.

Entre las ACL implementadas se encuentran:

- `BLOQUEO_NARANJA_IZQ`
- `BLOQUEO_VERDE_IZQ`
- `BLOQUEO_NARANJA_DER`
- `BLOQUEO_VERDE_DER`

Las reglas `deny` restringen únicamente el tráfico que no debe ser permitido, mientras que al final se utiliza `permit ip any any` para evitar bloquear tráfico adicional que sí debe circular.

## Comandos utilizados para las ACL

Edificio izquierdo:

```text
enable
configure terminal

ip access-list extended BLOQUEO_NARANJA_IZQ
 10 deny ip 192.188.39.0 0.0.0.31 192.188.39.32 0.0.0.31
 15 permit icmp 192.188.39.0 0.0.0.31 192.188.39.96 0.0.0.7 echo-reply
 17 deny icmp 192.188.39.0 0.0.0.31 192.188.39.96 0.0.0.7 echo
 20 permit ip any any
exit

ip access-list extended BLOQUEO_VERDE_IZQ
 10 deny ip 192.188.39.32 0.0.0.31 192.188.39.0 0.0.0.31
 15 permit icmp 192.188.39.32 0.0.0.31 192.188.39.96 0.0.0.7 echo-reply
 17 deny icmp 192.188.39.32 0.0.0.31 192.188.39.96 0.0.0.7 echo
 20 permit ip any any
exit
```

Edificio derecho:

```text
ip access-list extended BLOQUEO_NARANJA_DER
 10 deny ip 192.188.39.64 0.0.0.15 192.188.39.80 0.0.0.15
 15 permit icmp 192.188.39.64 0.0.0.15 192.188.39.96 0.0.0.7 echo-reply
 17 deny icmp 192.188.39.64 0.0.0.15 192.188.39.96 0.0.0.7 echo
 20 permit ip any any
exit

ip access-list extended BLOQUEO_VERDE_DER
 10 deny ip 192.188.39.80 0.0.0.15 192.188.39.64 0.0.0.15
 15 permit icmp 192.188.39.80 0.0.0.15 192.188.39.96 0.0.0.7 echo-reply
 17 deny icmp 192.188.39.80 0.0.0.15 192.188.39.96 0.0.0.7 echo
 20 permit ip any any
exit

end
write memory
```

Verificación:

```text
show access-lists
```

## 12.1 Restricción Naranja - Verde

Las ACL impiden la comunicación no autorizada entre los segmentos Naranja y Verde.

### Evidencia

![ACL Naranja Verde DER](https://i.ibb.co/r2X2V4KD/ACL-Naranja-Verde-DER.png)

## 12.2 Acceso hacia Administración

También se configuraron reglas para controlar la comunicación iniciada desde las VLAN de usuarios hacia la red administrativa.

La red ADMIN puede establecer comunicación hacia los segmentos permitidos, mientras que los segmentos restringidos no pueden iniciar libremente comunicación hacia ADMIN.

### Evidencia

![ACL VLAN hacia ADMIN](https://i.ibb.co/YBb9nY9F/ACL-VLAN-hacia-ADMIN.png)

### Evidencia adicional

![ACL ADMIN hacia todas las VLAN](https://i.ibb.co/Q7vzHDWK/ACL-ADMIN-Todas-VLAN.png)

## 12.3 Pruebas de ACL

Las restricciones se comprobaron utilizando `ping`.

Cuando una comunicación está permitida se reciben respuestas del destino. Cuando la ACL impide la comunicación, la prueba falla, demostrando que la política configurada se está aplicando.

### Evidencia: Pruebas ACL PC1

![Pruebas ACL PC1](https://i.ibb.co/Z6H9KxHM/Pruebas-ACL-PC1.png)

### Evidencia: Edificio derecho

![Pruebas ACL Edificio DER](https://i.ibb.co/PvBvv4ZG/Pruebas-ACL-Edificio-DER.png)

Con estas pruebas se comprueba que las ACL permiten el tráfico autorizado y bloquean el tráfico definido por las restricciones.

---

# 13. Pruebas de conectividad

## Comandos utilizados para las pruebas

```text
ping 192.188.39.3
ping 192.188.39.34
ping 192.188.39.68
ping 192.188.39.82
ping 192.188.39.98
ipconfig /all
```

Después de implementar las configuraciones se realizaron pruebas mediante `ping` para comprobar el funcionamiento del enrutamiento.

Entre las direcciones utilizadas durante las pruebas se encuentran:

- `192.188.39.3`
- `192.188.39.34`
- `192.188.39.68`
- `192.188.39.82`

En las comunicaciones permitidas se obtuvo respuesta con `0%` de pérdida de paquetes.

Esto permite comprobar que existe comunicación entre redes remotas y que EIGRP está proporcionando las rutas necesarias.

Las pruebas bloqueadas intencionalmente por ACL no representan una falla de conectividad, sino el funcionamiento esperado de las políticas de seguridad configuradas.

---

# 14. Verificación final

Al finalizar el proyecto se comprobó el funcionamiento de los principales elementos implementados:

| Elemento | Estado |
|---|---|
| VLAN | Configuradas |
| VTP | Funcionando |
| Troncales | Funcionando |
| LACP | Funcionando |
| PAgP | Funcionando |
| Tolerancia a fallos LACP | Comprobada |
| Tolerancia a fallos PAgP | Comprobada |
| STP | Funcionando |
| EIGRP AS 39 | Funcionando |
| Adyacencias EIGRP | Establecidas |
| Rutas remotas | Aprendidas |
| DHCP | Funcionando |
| DHCP Relay | Configurado |
| ACL | Aplicadas |
| Conectividad permitida | Comprobada |
| Tráfico restringido | Comprobado |

---

# 15. Conclusiones

La realización del proyecto permitió integrar dentro de una misma topología varios de los conceptos estudiados durante el curso de Redes de Computadoras 2.

La segmentación mediante VLAN permitió separar los diferentes grupos de usuarios, mientras que VTP facilitó la administración y distribución de estas VLAN entre los switches correspondientes.

La implementación de EtherChannel mediante LACP y PAgP permitió utilizar múltiples enlaces físicos como enlaces lógicos y proporcionar redundancia. Las pruebas de tolerancia a fallos demostraron que la comunicación puede continuar aunque uno de los enlaces pertenecientes al canal deje de funcionar.

STP permitió mantener una topología con enlaces redundantes evitando la formación de bucles de capa 2.

Por otra parte, EIGRP con AS 39 permitió intercambiar dinámicamente las rutas entre los switches multicapa, haciendo posible la comunicación entre las diferentes redes sin configurar manualmente cada ruta.

La implementación de DHCP y DHCP Relay permitió automatizar la asignación de los parámetros de red a los equipos finales incluso cuando estos se encontraban en segmentos diferentes al servidor DHCP.

Finalmente, las ACL permitieron controlar la comunicación entre las distintas VLAN y restringir el tráfico de acuerdo con las reglas establecidas. Las pruebas de conectividad realizadas permitieron comprobar tanto el funcionamiento del enrutamiento como las restricciones aplicadas.

En conjunto, la topología implementada cumple con los mecanismos de segmentación, redundancia, enrutamiento, asignación dinámica y control de tráfico necesarios para el funcionamiento de la red.
