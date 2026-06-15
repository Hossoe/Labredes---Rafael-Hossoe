Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

# Relatório do Experimento 9 : BGP

## Identificação

- Nome: Rafael Hossoe Dantas Pinto
- Matrícula: 231036130
- Turma: 1

## Objetivo

Esse experimento tem como objetivo aprender mais sobre o BGP, sua função e como utilizar na prática. Será entendido seu comportamento em uma redes de acordo com a rede proposta para o relatório.

## Ambiente experimental

### Equipamentos

- 8 Cisco IOL Routers - L3-ADVENTERPRISEK9-M-15.4-2T.bin

- 3 Cisci IOL Switches - L2-ADVENTERPRISEK9-M-15.2-20150703.bin

- 3 VPCs

### Topologia Lógica

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/86d35034ec570bacb5e95bb5eb55250ee2e1d009/Imagens%20Lab9/Captura%20de%20tela%202026-06-12%20110139.png)

## Procedimentos

Foi dado os comandos para cada dispositovo a seguir:

### R1

```
en
conf t
int e0/0
ip add 172.16.10.1 255.255.255.0
no shut
int e0/1
ip add 172.16.20.1 255.255.255.0
no shut

router ospf 1
network 172.16.10.0 255.255.255.0 area 10
network 172.16.20.0 255.255.255.0 area 10
```

### R2

```
en
conf t
int e0/0
ip add 172.16.20.2 255.255.255.0
no shut
int e0/1
ip add 172.16.30.1 255.255.255.0
no shut

router ospf 1
network 172.16.20.0 255.255.255.0 area 10
network 172.16.30.0 255.255.255.0 area 0
```

### R3

```
en
conf t

int loopback 0
ip add 172.16.0.1 255.255.255.0
int e0/0
ip add 172.16.30.2 255.255.255.0
no shut
int e0/1
ip add 172.31.20.1 255.255.255.0
no shut
int e0/2
ip add 172.31.10.1 255.255.255.0
no shut

router ospf 1
redistribute bgp 3200 subnets
network 172.16.0.0 255.255.0.0 area 0

router bgp 3200
bgp router-id 172.16.0.1
network 172.16.0.0 mask 255.255.0.0
neighbor 172.31.10.2 remote-as 4100
neighbor 172.31.20.2 remote-as 1900

ip route 172.16.0.0 255.255.0.0 Null0
```

### R4

```
en
conf t

int loopback 0
ip add 192.168.0.1 255.255.255.0
int e0/0
ip add 172.31.20.2 255.255.255.0
no shut
int e0/1
ip add 172.31.30.2 255.255.255.0
no shut
int e0/2
ip add 192.168.30.1 255.255.255.0
no shut

router ospf 1
redistribute bgp 1900 subnets
network 192.168.0.0 255.255.0.0 area 0

router bgp 1900
bgp router-id 192.168.0.1
network 192.168.0.0 mask 255.255.0.0
neighbor 172.31.20.1 remote-as 3200
neighbor 172.31.30.1 remote-as 4100

ip route 192.168.0.0 255.255.0.0 Null0
```

### R5

```
en
conf t

int e0/0
ip add 192.168.30.2 255.255.255.0
no shut
int e0/1
ip add 192.168.20.2 255.255.255.0
no shut

router ospf 1
network 192.168.20.0 255.255.255.0 area 20
network 192.168.30.0 255.255.255.0 area 0
```

### R6

```
en
conf t

int e0/0
ip add 192.168.20.1 255.255.255.0
no shut
int e0/1
ip add 192.168.10.1 255.255.255.0
no shut

router ospf 1
network 192.168.10.0 255.255.255.0 area 20
network 192.168.20.0 255.255.255.0 area 20
```

### R7

```
en
conf t

int loopback 0
ip add 10.10.0.1 255.255.255.0
int e0/0
ip add 172.31.10.2 255.255.255.0
no shut
int e0/1
ip add 172.31.30.1 255.255.255.0
no shut
int e0/2
ip add 10.10.10.1 255.255.255.0
no shut

router ospf 1
redistribute bgp 4100 subnets
network 10.10.0.0 255.255.0.0 area 0

router bgp 4100
bgp router-id 10.10.0.1
network 10.10.0.0 mask 255.255.0.0
neighbor 172.31.10.1 remote-as 3200
neighbor 172.31.30.2 remote-as 1900

ip route 10.10.0.0 255.255.0.0 Null0
```

### R8

```
en
conf t

int e0/0
ip add 10.10.10.2 255.255.255.0
no shut
int e0/1
ip add 10.10.20.1 255.255.255.0
no shut

router ospf 1
network 10.10.10.0 255.255.255.0 area 0
network 10.10.20.0 255.255.255.0 area 30
```

### VPC1

```
ip 172.16.10.2/24 172.16.10.1
```

### VPC2

```
ip 192.168.10.2/24 192.168.10.1
```

### VPC3

```
ip 10.10.20.2/24 10.10.10.1
```

## Resultados e evidências

Tendo configurado os dispositivos dessa maneira a rede fica dessa forma no final, a partir das tabelas de roteamento e os pings dos VPCs.

### R1

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222311.png)

Legenda: "show running-config" do R1

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222403.png)

### R2

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222620.png)

Legenda: "show running-config" do R2

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222720.png)

### R3

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222745.png)

Legenda: "show running-config" do R3

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222812.png)

### R4

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222850.png)

Legenda: "show running-config" do R4

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222916.png)

### R5

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20222940.png)

Legenda: "show running-config" do R5

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223007.png)

### R6

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223038.png)

Legenda: "show running-config" do R6

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223104.png)

### R7

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223125.png)

Legenda: "show running-config" do R7

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223149.png)

### R8

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223217.png)

Legenda: "show running-config" do R8

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223244.png)

### VPC1

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223337.png)

Legenda: ping do VPC da AS 3200 para as AS 1900 e 4100

### VPC2

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223528.png)

Legenda: ping do VPC da AS 1900 para as AS 3200 e 4100

### VPC3

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/d6e3f47a6c34097eae5491e5a73df095d3bbea2a/Imagens%20Lab9/Captura%20de%20tela%202026-06-14%20223556.png)

Legenda: ping do VPC da AS 4100 para as AS 1900 e 3200

## Conclusão

Com base nesse experimento, podemos concluir que os Hosts de cada AS conseguem conectar entre si por meio do uso do OSPF para criar as subredes de cada AS e utilizar o BGP para identificar os vizinhos de cada roteador e assim poder se comunicarem. Também foi utilizado o uso do Loopback nos roteadores que são as pontas de cada AS, que tem como papel fazer elas se comunirem, para ser utilizada como o router id para as redes.

## Perguntas do Experimento

* Qual a diferença se comparado com RIP e OSPF?

O OSPF acaba sendo a melhor opção quando tiver também o BGP, sendo que esse protocolo atua no próprio IP, sem precisar utilisar o TCP ou UDP, facilitando uma melhor comunicação entre o OSPF e o BGP. Pelo fato do RIP atuar no UDP, acaba sendo mais dificil sua comunicação com o BGP sendo que possuem protocolos de transporte diferentes.

* Aonde estão as políticas de roteamento e a hierarquia?

As políticas de roteamento estão concentradas nos roteadores da ponta de cada AS, com o propósito de fiscalizar o tráfego de uma rede para a outra. Além disso, no OSPF deles, foram organizados por hierarquias qual área é de qual rede, seja área 10 para a AS 3200, área 20 para a AS 1900 e a área 30 para a AS 4100, além de possuir a área 0 (backbone)  nos roteadores de ponta sendo que as outras áreas devem se comunicar com o backbone. Com isso, permitindo que a área 0 possa filtrar o que cada uma as outras áreas vão receber entre elas.




























