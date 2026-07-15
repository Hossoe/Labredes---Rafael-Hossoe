Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

# Relatório do Experimento 6 - OSPF (Open Shortest Path First)

## Identificação

- Nome: Rafael Hossoe Dantas Pinto
- Matrícula: 231036130
- Turma: 1

## Objetivo

Ao final deste laboratório, o estudante deverá ser capaz de:

- Configurar corretamente o protocolo OSPF em múltiplos roteadores
- Compreender o papel da **Área 0 (backbone)**
- Implementar e identificar **múltiplas áreas OSPF**
- Verificar a formação de adjacências OSPF
- Analisar a tabela de rotas resultante da convergência
- Relacionar topologia lógica e organização por áreas

## Ambiente experimental

- 3 Cisco IOL - L3-ADVENTERPRISEK9-M-15.4-2T.bin
- 3 VPCs

## Topologia Lógica

![](C:/Users/rafah/AppData/Roaming/marktext/images/2026-07-14-22-12-18-image.png)

## Procedimentos

Foi dado os comandos para cada dispositivo a seguir:

### R1

```
enable
configure terminal

interface Ethernet0/0
ip address 172.16.0.1 255.255.255.192
no shutdown
interface Ethernet0/1
ip address 200.10.1.2 255.255.255.252
no shutdown
interface Ethernet0/2
ip address 200.10.1.9 255.255.255.252
no shutdown

router ospf 1
router-id 1.1.1.1
network 172.16.0.0 0.0.0.63 area 1
network 200.10.1.0 0.0.0.3 area 0
network 200.10.1.8 0.0.0.3 area 0

end
write memory
```

### R2

```
enable
configure terminal

interface Ethernet0/0
ip address 10.0.0.1 255.255.255.248
no shutdown
interface Ethernet0/1
ip address 200.10.1.1 255.255.255.252
no shutdown
interface Ethernet0/2
ip address 200.10.1.6 255.255.255.252
no shutdown

router ospf 1
router-id 2.2.2.2
network 10.0.0.0 0.0.0.7 area 2
network 200.10.1.0 0.0.0.3 area 0
network 200.10.1.4 0.0.0.3 area 0

end
write memory
```

### R3

```
enable
configure terminal

interface Ethernet0/0
ip address 192.168.1.1 255.255.255.240
no shutdown
interface Ethernet0/1
ip address 200.10.1.10 255.255.255.252
no shutdown
interface Ethernet0/2
ip address 200.10.1.5 255.255.255.252
no shutdown

router ospf 1
router-id 3.3.3.3
network 192.168.1.0 0.0.0.15 area 3
network 200.10.1.4 0.0.0.3 area 0
network 200.10.1.8 0.0.0.3 area 0

end
write memory
```

### VPC4

```
ip 172.16.0.2/26 172.16.0.1
save
```

### VPC5

```
ip 10.0.0.2/29 10.0.0.1
save
```

### VPC6

```
ip 192.168.1.2/28 192.168.1.1
save
```

## Resultados e evidências

##### R1

![Captura de tela 2026-07-14 222321.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222321.png)

![Captura de tela 2026-07-14 222411.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222411.png)

##### R2

![Captura de tela 2026-07-14 222439.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222439.png)

![Captura de tela 2026-07-14 222455.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222455.png)

##### R3

![Captura de tela 2026-07-14 222514.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222514.png)

![Captura de tela 2026-07-14 222533.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222533.png)

##### VPC4

![Captura de tela 2026-07-14 222813.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222813.png)

##### VPC5

![Captura de tela 2026-07-14 222902.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222902.png)

##### VPC6

![Captura de tela 2026-07-14 222907.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-14%20222907.png)

## Análise técnica

###### Como o OSPF organiza a topologia em áreas?

###### Qual o papel da Área 0 no funcionamento do protocolo?

###### O que acontece se uma área não estiver conectada ao backbone?

###### Como o OSPF calcula o melhor caminho?

###### Quais vantagens o OSPF apresenta em relação ao RIP?

## Conclusão
