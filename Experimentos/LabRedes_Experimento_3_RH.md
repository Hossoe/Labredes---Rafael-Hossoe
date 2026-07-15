Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

# Relatório do Experimento 03 – VLAN e Roteamento Estático

## Identificação

- Nome: Rafael Hossoe Dantas Pinto
- Matrícula: 231036130
- Turma: 1

## Objetivo

Realizar o endereçamento de uma rede IP, configurar e verificar VLANs em switches Ethernet e implementar a interconexão entre VLANs por meio de roteamento estático em um roteador.

## Ambiente experimental

### Lista de Equipamentos

| Andar  | Equipamento | Qtde   | Departamento   |
|:------:| ----------- |:------:| -------------- |
| 1º     | Computador  | 2      | Administrativo |
| 1º     | Servidor    | 1      | Administrativo |
| 1º     | Impressora  | 1      | Administrativo |
| 1º     | Computador  | 5      | Tecnologia     |
| 1º     | Servidor    | 4      | Tecnologia     |
| 1º     | Impressora  | 2      | Tecnologia     |
| **1º** | **Total**   | **15** | —              |
| 2º     | Computador  | 2      | Administrativo |
| 2º     | Servidor    | 1      | Administrativo |
| 2º     | Impressora  | 1      | Administrativo |
| 2º     | Computador  | 2      | Diretoria      |
| 2º     | Impressora  | 1      | Diretoria      |
| 2º     | Computador  | 6      | Marketing      |
| 2º     | Servidor    | 2      | Marketing      |
| 2º     | Impressora  | 2      | Marketing      |
| **2º** | **Total**   | **17** | —              |
| 3º     | Computador  | 1      | Administrativo |
| 3º     | Computador  | 3      | Diretoria      |
| 3º     | Servidor    | 2      | Diretoria      |
| 3º     | Impressora  | 2      | Diretoria      |
| 3º     | Computador  | 5      | Marketing      |
| 3º     | Servidor    | 1      | Marketing      |
| 3º     | Impressora  | 1      | Marketing      |
| 3º     | Computador  | 2      | Tecnologia     |
| 3º     | Impressora  | 1      | Tecnologia     |
| **3º** | **Total**   | **18** | —              |

## Topologia Lógica

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/40bd93750385a425e4f5ac43c2827538a1090625/Imagens%20Lab3/Captura%20de%20tela%202026-07-15%20103855.png)

## Procedimentos

Foi dado os comandos para cada dispositivo a seguir:

### SW1

```
enable
configure terminal

vlan 2
name Administracao
vlan 3
name Diretoria
vlan 4
name Marketing
vlan 5
name Tecnologia
exit

interface FastEthernet0/1
switchport mode access
switchport access vlan 2
exit
interface FastEthernet0/5
switchport mode access
switchport access vlan 5
exit
interface GigabitEthernet0/1
switchport mode trunk
exit
interface FastEthernet0/24
switchport mode trunk

end
write memory
```

### SW2

```
enable
configure terminal

vlan 2
name Administracao
vlan 3
name Diretoria
vlan 4
name Marketing
vlan 5
name Tecnologia
exit

interface FastEthernet0/1
switchport mode access
switchport access vlan 2
exit
interface FastEthernet0/2
switchport mode access
switchport access vlan 3
exit
interface FastEthernet0/3
switchport mode access
switchport access vlan 4
exit
interface range GigabitEthernet0/1 - 2
switchport mode trunk

end
write memory
```

### SW3

```
enable
configure terminal

vlan 2
name Administracao
vlan 3
name Diretoria
vlan 4
name Marketing
vlan 5
name Tecnologia
exit

interface FastEthernet0/1
switchport mode access
switchport access vlan 2
exit
interface FastEthernet0/2
switchport mode access
switchport access vlan 3
exit
interface FastEthernet0/3
switchport mode access
switchport access vlan 4
exit
interface FastEthernet0/4
switchport mode access
switchport access vlan 5
exit
interface GigabitEthernet0/1
switchport mode trunk

end
write memory
```

### R1

```
enable
configure terminal

interface FastEthernet0/0
no shutdown
exit
interface FastEthernet0/0.1
description VLAN1 - GERENCIA
encapsulation dot1Q 1
ip address 10.0.10.254 255.255.255.0
exit
interface FastEthernet0/0.2
description VLAN2 - ADMINISTRACAO
encapsulation dot1Q 2
ip address 10.0.20.254 255.255.255.0
exit
interface FastEthernet0/0.3
description VLAN3 - DIRETORIA
encapsulation dot1Q 3
ip address 10.0.30.254 255.255.255.0
exit
interface FastEthernet0/0.4
description VLAN4 - MARKETING
encapsulation dot1Q 4
ip address 10.0.40.254 255.255.255.0
exit
interface FastEthernet0/0.5
description VLAN5 - TECNOLOGIA
encapsulation dot1Q 5
ip address 10.0.50.254 255.255.255.0

end
write memory
```

## Análise técnica

**A) Razões para falhas de conectividade nas etapas iniciais:** Sem enlaces do tipo *Trunk*, os switches não propagam tags de VLANs criadas de forma personalizada. O tráfego de Camada 2 das demais VLANs fica restrito individualmente àquele switch de origem, impedindo a comunicação até mesmo de computadores na mesma VLAN que estejam em andares (switches) diferentes.

**B) Configurações necessárias nos switches para comunicação intra-VLAN:** Criação das VLANs idênticas em todos os switches e parametrização dos canais de interligação como enlaces de *Trunking* através do comando `switchport mode trunk`.

**C) Configurações necessárias para comunicação inter-VLAN:** Criação de subinterfaces lógicas em uma porta física de um roteador configurada para aceitar e rotular pacotes utilizando o protocolo de encapsulamento dot1Q para cada ID de VLAN. A interface física do switch conectada a este roteador precisa operar obrigatoriamente como porta *Trunk*.

**D) Finalidade do protocolo IEEE 802.1Q e vantagens de sua utilização:** O protocolo IEEE 802.1Q é o padrão de mercado para multiplexar o tráfego de várias VLANs sobre um único link físico (enlace trunking), injetando uma etiqueta (*tag*) de 4 bytes no cabeçalho do frame Ethernet para identificar a qual rede lógica ele pertence. A sua principal vantagem é a **economia de infraestrutura física**, eliminando a necessidade de passar uma interface cabeada física de switch a roteador para cada VLAN presente na rede corporativa.

## Conclusão

A realização deste experimento permitiu compreender, de forma prática e integrada, a importância da segmentação de redes locais através de VLANs e a necessidade de roteamento para a comunicação entre diferentes domínios de broadcast.

Durante as etapas iniciais, observou-se que a simples divisão lógica dos departamentos não é suficiente para garantir a comunicação em uma topologia distribuída por múltiplos switches. A falha de conectividade intra-VLAN evidenciou que, sem a parametrização de enlaces *Trunk* (sob o protocolo IEEE 802.1Q) nos uplinks de interconexão, os switches são incapazes de propagar as tags de identificação de tráfego personalizado entre si.

Ademais, o teste de ping entre setores distintos confirmou o isolamento completo de Camada 2 imposto pelas sub-redes. A resolução deste problema por meio da abordagem *Router-on-a-Stick* — utilizando subinterfaces associadas a cada VLAN ID no roteador — demonstrou uma solução altamente escalável e de excelente custo-benefício para redes corporativas de pequeno e médio porte. Essa técnica viabilizou a comunicação inter-VLAN segura e controlada utilizando apenas uma única interface física e um cabo físico conectado ao switch principal.

Em suma, as atividades consolidaram os conceitos teóricos de engenharia de redes sobre o fluxo de pacotes, marcação de frames e gerência de ativos. O projeto e a implementação executados simularam com precisão os desafios reais de design de infraestrutura, evidenciando como otimizar a performance, a segurança e o uso de hardware em redes locais segmentadas.
