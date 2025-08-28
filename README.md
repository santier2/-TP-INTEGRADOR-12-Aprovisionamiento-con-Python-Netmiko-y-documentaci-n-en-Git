# -TP-INTEGRADOR-12-Aprovisionamiento-con-Python-Netmiko-y-documentaci-n-en-Git

Este repositorio contiene la configuración de red para un escenario con switches Cisco, routers MikroTik y PCs conectadas en distintas VLANs. Incluye:
- Configuración manual inicial de los equipos.
- Script en Python con Netmiko para automatizar la configuración.
- Subnetting aplicado con VLSM.


---


## 🔹 Subredes documentadas


### VLAN Gestión (ID 1299)
- Dirección de red: **10.10.12.0/29**
- Máscara: **255.255.255.248**
- Rango de hosts válidos: **10.10.12.1 – 10.10.12.6**
- Broadcast: **10.10.12.7**
- Gateway: **10.10.12.1 (R1)**


### VLAN Ventas (ID 230)
- Dirección de red: **10.10.12.32/27**
- Máscara: **255.255.255.224**
- Rango de hosts válidos: **10.10.12.33 – 10.10.12.62**
- Broadcast: **10.10.12.63**
- Gateway: **10.10.12.33 (R1)**


### VLAN Técnica (ID 231)
- Dirección de red: **10.10.12.64/28**
- Máscara: **255.255.255.240**
- Rango de hosts válidos: **10.10.12.65 – 10.10.12.78**
- Broadcast: **10.10.12.79**
- Gateway: **10.10.12.65 (R1)**


### VLAN Visitantes (ID 232)
- Dirección de red: **10.10.12.80/29**
- Máscara: **255.255.255.248**
- Rango de hosts válidos: **10.10.12.81 – 10.10.12.86**
- Broadcast: **10.10.12.87**
- Gateway: **10.10.12.81 (R1)**


### VLAN Nativa (ID 239)
- Usada para enlaces trunk nativos (sin direccionamiento IP).


---


## 🔹 Configuración de PCs

### PC Sysadmin (Gestión, Linux)
```bash
sudo ip addr flush dev ens3
sudo ip addr add 10.10.12.5/29 dev ens3
sudo ip link set ens3 up
sudo ip route add default via 10.10.12.1
```


### PC Remota (Gestión, Windows/Linux)
```bash

# Windows (PowerShell)
ip 10.10.12.6 255.255.255.248 10.10.12.1


### PC Visitantes (VLAN 232, Windows/Linux)
```bash
# Windows (PowerShell)
ip 10.10.12.82 255.255.255.248 10.10.12.81

---

## 🔹 Script Netmiko (`Red Vlan Config.py`)


### Objetivo
Automatizar la configuración de:
- VLANs y puertos access en SW1 y SW2.
- Trunks entre SW1–R1, SW2–R2, R1–R2.
- Subinterfaces en R1 con direccionamiento, NAT y DHCP.
- R2 como bridge transparente para todas las VLANs de usuario.


### Ejecución
```bash
cd -TP-INTEGRADOR-12-Aprovisionamiento-con-Python-Netmiko-y-documentaci-n-en-Git
python3 Comandos.py
```


> Requiere tener instalado Netmiko:
```bash
pip install netmiko
```


### Salidas generadas
El script muestra:
- Configuración enviada a cada equipo.
- Verificación de VLANs e interfaces (`show vlan brief`, `show ip interface brief`, `/ip address print`, etc.).


Ejemplo de salida:
```
###### Conectando a SW1 (10.10.12.2) ######
--- Aplicando configuración a SW1 ---
...
-- Verificación en SW1 --
SW1# show vlan brief
VLAN Name Status Ports
---- -------------------------------- --------- -------------------------
230 VENTAS active Et0/1
231 TECNICA active Et0/2
232 VISITANTES active Et0/3
1299 GESTION active Et1/0
```



✍️ Con esto queda documentado el proyecto: subnetting, configuración manual, script de automatización y ejemplos de pruebas.
