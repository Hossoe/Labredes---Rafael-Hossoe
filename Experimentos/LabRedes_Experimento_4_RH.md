Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

# Relatório do Experimento 4 - Roteamento Estático

## Identificação

- Nome: Rafael Hossoe Dantas Pinto
- Matrícula: 231036130
- Turma: 1

## Objetivo

Este laboratório tem como objetivo introduzir e consolidar os **conceitos fundamentais de roteamento estático em redes IP**, permitindo ao estudante compreender como rotas configuradas manualmente possibilitam a comunicação entre redes distintas.

A atividade enfatiza o papel do **endereçamento IP**, da **tabela de rotas** e da **decisão de encaminhamento**, servindo como base conceitual para o estudo posterior de protocolos de roteamento dinâmico.

## Ambiente experimental

- **3 Roteadores**: Modelo **2911**

- **3 Switches**: Modelo **2960** 

- 6 PCs (Hosts)

## Topologia Lógica

![](C:/Users/rafah/AppData/Roaming/marktext/images/2026-07-15-11-15-11-image.png)

## Procedimentos

Foi dado os comandos para cada dispositivo a seguir:

### R0

```
enable
configure terminal

interface GigabitEthernet 0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet 0/1
ip address 192.168.40.2 255.255.255.0
no shutdown
exit
interface GigabitEthernet 0/2
ip address 192.168.50.1 255.255.255.0
no shutdown
exit

ip route 192.168.10.0 255.255.255.0 192.168.40.1
ip route 192.168.30.0 255.255.255.0 192.168.50.2

end
write memory
```

### R1

```
enable
configure terminal

interface GigabitEthernet 0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet 0/1
ip address 192.168.40.2 255.255.255.0
no shutdown
exit
interface GigabitEthernet 0/2
ip address 192.168.50.1 255.255.255.0
no shutdown
exit

ip route 192.168.10.0 255.255.255.0 192.168.40.1
ip route 192.168.30.0 255.255.255.0 192.168.50.2

end
write memory
```

### R2

```
enable
configure terminal

interface GigabitEthernet 0/0
ip address 192.168.30.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet 0/2
ip address 192.168.50.2 255.255.255.0
no shutdown
exit

ip route 192.168.20.0 255.255.255.0 192.168.50.1
ip route 192.168.40.0 255.255.255.0 192.168.50.1
ip route 192.168.10.0 255.255.255.0 192.168.50.1

end
write memory
```

### PCs

| **Dispositivo** | **IP Address** | **Subnet Mask** | **Default Gateway** | **Rede**             |
| --------------- | -------------- | --------------- | ------------------- | -------------------- |
| **PC0**         | `192.168.10.2` | `255.255.255.0` | `192.168.10.1`      | **Rede 10** (LAN R1) |
| **PC1**         | `192.168.10.3` | `255.255.255.0` | `192.168.10.1`      | **Rede 10** (LAN R1) |
| **PC2**         | `192.168.20.2` | `255.255.255.0` | `192.168.20.1`      | **Rede 20** (LAN R2) |
| **PC3**         | `192.168.20.3` | `255.255.255.0` | `192.168.20.1`      | **Rede 20** (LAN R2) |
| **PC4**         | `192.168.30.2` | `255.255.255.0` | `192.168.30.1`      | **Rede 30** (LAN R3) |
| **PC5**         | `192.168.30.3` | `255.255.255.0` | `192.168.30.1`      | **Rede 30** (LAN R3) |

## Resultados e evidências

#### R0

![Captura de tela 2026-07-15 111010.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20111010.png)

#### R1

![Captura de tela 2026-07-15 111030.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20111030.png)

#### R2

![Captura de tela 2026-07-15 111044.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20111044.png)

## Análise técnica

###### Como as rotas estáticas foram configuradas para que toda a rede se tornasse funcional?

###### É necessário configurar rotas nas estações de trabalho das LANs? Por quê?

###### Quais rotas ficam definidas automaticamente nas estações?

###### Caso cada rede utilizasse uma tecnologia de enlace diferente, haveria impacto no funcionamento do roteamento? Justifique.

###### O que é retornado pelo comando `ping` entre estações de redes distintas? Explique seu funcionamento.

###### O que é retornado pelo comando `traceroute` entre estações de redes distintas? Explique

## Conclusão


