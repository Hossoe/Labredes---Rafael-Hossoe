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

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20221213.png)

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

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222321.png)

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222411.png)

##### R2

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222439.png)

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222455.png)

##### R3

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222514.png)

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222533.png)

##### VPC4

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222813.png)

##### VPC5

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222902.png)

##### VPC6

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/033aa65b677e0e2859cd398b596fa0b45ddf6e10/Imagens%20Lab%206/Captura%20de%20tela%202026-07-14%20222907.png)

## Análise técnica

###### Como o OSPF organiza a topologia em áreas?

O OSPF utiliza uma organização hierárquica de duas camadas para segmentar uma rede em subconjuntos menores e mais gerenciáveis, otimizando o processamento de roteamento. Essa organização funciona da seguinte forma:
* **Área 0 (Backbone):** É a área central de trânsito pela qual todo o tráfego inter-área deve passar obrigatoriamente. No nosso experimento, ela foi estabelecida pelos links que interligam os roteadores R1, R2 e R3.
* **Áreas Não-Backbone (Áreas periféricas):** São áreas destinadas a agrupar redes locais de usuários para limitar a propagação de atualizações de estado de enlace (LSAs)[cite: 1]. No cenário do laboratório, temos:
  * **Área 1:** Rede local do VPC4 ($172.16.0.0/26$) conectada ao R1.
  * **Área 2:** Rede local do VPC5 ($10.0.0.0/29$) conectada ao R2.
  * **Área 3:** Rede local do VPC6 ($192.168.1.0/28$) conectada ao R3.
* **Roteadores de Borda de Área (ABRs):** Roteadores como R1, R2 e R3 que possuem interfaces em múltiplas áreas (Área 0 e suas respectivas áreas locais), sendo responsáveis por resumir e encaminhar rotas entre elas.

###### Qual o papel da Área 0 no funcionamento do protocolo?

A Área 0 (backbone) desempenha funções fundamentais para o funcionamento estrutural do OSPF:
* **Prevenção de Loops:** Ela age como um hub central de distribuição de informações topológicas. Como todas as áreas devem se conectar a ela, o tráfego inter-área sempre segue um caminho livre de loops através da hierarquia lógica.
* **Trânsito Inter-área:** Qualquer pacote que viaje de uma área não-backbone para outra deve necessariamente transitar pela Área 0. Os roteadores de borda (ABRs) condensam as tabelas locais e anunciam essas redes apenas dentro do backbone.

###### O que acontece se uma área não estiver conectada ao backbone?

Se uma área não-backbone não estiver conectada diretamente (física ou logicamente) à Área 0:
* **Isolamento de Roteamento:** Os roteadores dessa área conseguirão trocar informações de rotas internamente (rotas intra-área), mas não conseguirão se comunicar com redes localizadas em outras áreas.
* **Bloqueio de Anúncios pelos ABRs:** Pelas regras de design do OSPF, um ABR só propaga informações de roteamento inter-área se possuir uma conexão ativa com a Área 0.
* **Soluções:** Para contornar esse problema em situações reais onde o enlace físico direto é inviável, utiliza-se a configuração de um túnel lógico chamado *Virtual Link* através de uma área de trânsito intermediária.

###### Como o OSPF calcula o melhor caminho?

O cálculo do melhor caminho no OSPF é baseado em estado de enlace utilizando o algoritmo de Dijkstra:
* **Construção do Banco de Dados (LSDB):** Cada roteador constrói um mapa idêntico da topologia de sua própria área a partir de anúncios de estado de enlace (LSAs).
* **Algoritmo SPF (Shortest Path First):** Utilizando esse mapa, o algoritmo calcula uma árvore de caminhos mais curtos tendo o próprio roteador como raiz.
* **Métrica de Custo:** A decisão do caminho baseia-se no "Custo" (*Cost*), que é inversamente proporcional à largura de banda acumulada das interfaces de saída no trajeto. A fórmula padrão para o custo de uma interface é:
  $$\text{Custo} = \frac{10^8}{\text{Largura de Banda (em bps)}}$$

###### Quais vantagens o OSPF apresenta em relação ao RIP?

O OSPF apresenta vantagens técnicas significativas comparado a protocolos de vetor de distância como o RIP:
* **Métrica Inteligente:** O OSPF baseia-se em largura de banda (custo), enquanto o RIP utiliza apenas a contagem de saltos, podendo escolher caminhos mais lentos se tiverem menos roteadores no trajeto.
* **Escalabilidade:** A divisão em áreas do OSPF permite segmentar redes gigantescas, ao passo que o RIP possui um limite rígido de diâmetro de rede de no máximo 15 saltos.
* **Velocidade de Convergência:** O OSPF converge quase instantaneamente ao enviar atualizações disparadas por eventos (Triggered Updates) quando ocorrem mudanças físicas, enquanto o RIP sofre com convergência lenta dependente de temporizadores periódicos de 30 segundos.
* **Eficiência de Banda:** O OSPF consome menos banda pois envia atualizações completas apenas no início e depois apenas mudanças incrementais, enquanto o RIP repassa toda a sua tabela de roteamento periodicamente.

---

## Conclusão

A realização deste laboratório permitiu consolidar na prática os conceitos avançados de roteamento dinâmico utilizando o protocolo OSPF de múltiplas áreas. Através do emulador PNETLab e de imagens de roteadores Cisco IOL, foi possível observar detalhadamente as etapas essenciais para o planejamento e a ativação de uma infraestrutura de rede corporativa estruturada e escalável.

A correta parametrização do protocolo — incluindo a definição de IDs de roteadores exclusivos, o cálculo das máscaras curinga (*wildcards*) e a distribuição das interfaces nas respectivas áreas de influência — permitiu observar o processo de estabelecimento de adjacências e convergência de tabelas. Os resultados obtidos nos consoles dos roteadores R1, R2 e R3 confirmaram a formação correta das rotas do tipo *Inter-Area* (O IA), garantindo que as áreas periféricas 1, 2 e 3 pudessem trocar pacotes de dados por intermédio da Área 0 (backbone).

Por fim, os testes de comunicação direta realizados a partir dos computadores virtuais (VPC4, VPC5 e VPC6) validaram a total alcançabilidade de ponta a ponta da topologia proposta[cite: 1, 2]. Essa experiência prática evidenciou a importância de arquiteturas hierárquicas para a garantia de desempenho, convergência rápida e resiliência em sistemas autônomos complexos, pilares cruciais na formação em Engenharia de Redes de Comunicação.
