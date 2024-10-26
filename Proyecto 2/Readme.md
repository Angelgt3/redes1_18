# Manual Tecnico 

## Topología de la red
![topologia](img/topologia.png)

## Direcciones IP


## Jutiapa

| VLAN          | ID VLAN | Equipos | Id de red        | Máscara            | Rango de IP                        | Broadcast          |
|---------------|---------|---------|------------------|--------------------|------------------------------------|---------------------|
| RRHH          | 17      | 10      | 192.168.18.0     | 255.255.255.240    | 192.168.18.1 - 192.168.18.14      | 192.168.18.15       |
| Contabilidad  | 27      | 4       | 192.168.18.16    | 255.255.255.248    | 192.168.18.17 - 192.168.18.23      | 192.168.18.23       |
| Ventas        | 37      | 25      | 192.168.18.24    | 255.255.255.224    | 192.168.18.25 - 192.168.18.54      | 192.168.18.55       |
| Informática   | 47      | 12      | 192.168.18.56    | 255.255.255.240    | 192.168.18.57 - 192.168.18.70      | 192.168.18.71       |

## Quiché

| VLAN          | ID VLAN | Equipos | Id de red        | Máscara            | Rango de IP                        | Broadcast          |
|---------------|---------|---------|------------------|--------------------|------------------------------------|---------------------|
| RRHH          | 17      | 12      | 192.178.18.0     | 255.255.255.240    | 192.178.18.1 - 192.178.18.14       | 192.178.18.15       |
| Contabilidad  | 27      | 10      | 192.178.18.16    | 255.255.255.240    | 192.178.18.17 - 192.178.18.30      | 192.178.18.31       |
| Ventas        | 37      | 36      | 192.178.18.32    | 255.255.255.192    | 192.178.18.33 - 192.178.18.94      | 192.178.18.95       |
| Informática   | 47      | 21      | 192.178.18.96    | 255.255.255.224    | 192.178.18.97 - 192.178.18.126     | 192.178.18.127      |


## Escuintla

| VLAN          | ID VLAN | Equipos | Id de red        | Máscara            | Rango de IP                        | Broadcast           |
|---------------|---------|---------|------------------|--------------------|------------------------------------|---------------------|
| ventas        | 37      | 12      | 192.148.18.0     | 255.255.255.192    | 192.148.18.1 - 192.148.18.62       | 192.148.18.63       |
| RRHH          | 17      | 10      | 192.148.18.64    | 255.255.255.224    | 192.148.18.65 - 192.148.18.94      | 	192.148.18.95      |


## Peten

| VLAN          | ID VLAN | Equipos | Id de red        | Máscara            | Rango de IP                        | Broadcast           |
|---------------|---------|---------|------------------|--------------------|------------------------------------|---------------------|
| ventas        | 37      | 30      | 192.158.18.0     | 255.255.255.192    | 192.148.18.1 - 192.148.18.62       |  192.148.18.63      |
| RRHH          | 17      | 14      | 192.158.18.32    | 255.255.255.240    | 192.158.18.33 - 192.158.18.46      | 	192.158.18.47      |
| Informática   | 47      | 14      | 192.158.18.48    | 255.255.255.240    | 192.158.18.49 - 192.158.18.62      |  192.158.18.63      |


## Resumen de Comandos

### Sede de Jutiapa
#### LACP 
##### Switch 2-3
- enable
- configure terminal
- interface range f0/2-3
- channel-group 1 mode active
- exit
- interface port-channel 1
- switchport mode trunk
- end
- wr

#### Configuracion de ip de cada router
#### J1
1. 
- enable
- configure terminal
- inteface f1/0
- ip address 192.167.40.2 255.255.255.0
- no shutdown 
- exit
2. 
- enable
- configure terminal
- interface f0/0
- ip address 11.0.0.2 255.255.255.252
- no shutdown
- exit
- end
- wr

#### J2
1. 
- enable
- configure terminal
- inteface f1/0
- ip address 192.167.40.3 255.255.255.0
- no shutdown 
- exit
2. 
- enable
- configure terminal
- interface f0/0
- ip address 12.0.0.2 255.255.255.252
- no shutdown
- exit
- end
- wr

#### Configuracion de HSRP J1-J2
##### J1
- enable
- configure terminal
- interface fa1/0
- standby version 2
- standby 21 192.167.40.1
- standby 21 priority 109
- standby 21 preempt
- end
- wr

##### J2
- enable
- configure terminal
- interface fa1/0
- standby version 2
- standby 21 192.167.40.1
- standby priority 100
- end
- wr

#### Configuracion del VTP Y RSTP ESW1 SW2 SW3
Falta por terminar
##### ESW1
- enable
- configure terminal
- vtp mode server
- vtp domain P18
- vtp version 2
- exit
- wr

##### SW2 - SW3
- enable
- configure terminal
- vtp mode client
- vtp domain P18
- exit
- wr

##### Creacion de VLANs ESW1
- enable
- configure terminal
- vlan 17
- name RRHH
- vlan 27
- name contabilidad
- vlan 37
- name ventas
- vlan 47
- name informatica
- end 
- wr

#### Truncar los switches
##### ESW1
- enable
- configure terminal
- interface range f0/1-2
- switchport trunk encapsulation dot1q
- switchport mode trunk
- switchport trunk allowed vlan all
- end
- wr

##### SW2 - SW3
- enable
- configure terminal
- interface range f0/1-3
- switchport mode trunk
- switchport trunk allowed vlan all
- end
- wr

#### Configuracion del modo acceso 
##### SW2
- enable
- configure terminal
- interface f0/11
- switchport mode access 
- switchport access vlan 17
- exit
- interface f0/12
- switchport mode access 
- swithcport access vlan 27
- exit
- interface f0/13
- switchport mode access
- switchport access vlan 27
- exit
- end
- wr

##### SW3
- enable
- configure terminal
- interface f0/11
- switchport mode access 
- switchport access vlan 37
- exit
- interface f0/12
- switchport mode access 
- swithcport access vlan 17
- exit
- interface f0/13
- switchport mode access
- switchport access vlan 47
- exit
- end
- wr

##### RSTP
##### Configuramos el root ESW1
- enable
- configure terminal
- spanning-tree vlan 1 root primary 
- exit
- wr

##### Configuramos el modo rstp en ESW1, SW2, SW3
- enable
- configue terminal
- spanning-tree mode rapid-pvst
- exit
- write



### Sede Quiche

#### Configuracion de ip del router QUICHE
1.
- enable
- configure terminal
- interface f8/0
- ip address 192.178.18.128 255.255.255.0
- no shutdown
- exit
2.
- enable
- configure terminal
- interface f9/0
- ip address 192.178.18.129 255.255.255.0
- no shutdown
- exit
- end
- wr

#### Configuracion del VTP
##### SW1
- enable
- configure terminal
- vtp mode server
- vtp domain P18
- vtp version 2
- exit
- wr

##### Switch3
- enable
- configure terminal
- vtp mode transparent
- vtp domain P18
- vtp version 2
- exit
- wr

#### Creacion de las VLANs SW1 y Switch3
- enable
- configure terminal
- vlan 17
- name RRHH
- vlan 27
- name contabilidad
- vlan 37
- name ventas
- vlan 47
- name informatica
- end 
- wr

#### Configuracion del modo acceso
##### SW1
- enable
- configure terminal
- interface f0/11 
- switchport mode access
- switchport access vlan 17
- exit
- interface f0/12
- switchport mode access
- switchport access vlan 47
- exit 
- interface f0/13
- switchport mode access
- switchport access vlan 47
- exit 
- interface f0/14 
- switchport mode access
- switchport access vlan 17
- end
- wr


#### Switch3
- enable
- configure terminal
- interface f0/11 
- switchport mode access
- switchport access vlan 24
- exit
- interface f0/12
- switchport mode access
- switchport access vlan 24
- exit 
- interface f0/13
- switchport mode access
- switchport access vlan 34
- end
- wr



### Comandos para verificar la configuracion
- show vtp status
- show running-config
- show standby
- show startup-config
- show ip route
- show spanning-tree