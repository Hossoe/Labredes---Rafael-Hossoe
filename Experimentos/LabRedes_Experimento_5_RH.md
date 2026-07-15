Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

# Relatório do Experimento 05 – Protocolo RIP (Routing Information Protocol)

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

![](C:/Users/rafah/AppData/Roaming/marktext/images/2026-07-15-12-00-01-image.png)

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

![Captura de tela 2026-07-15 115730.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20115730.png)

#### R1

![Captura de tela 2026-07-15 115751.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20115751.png)

#### R2

![Captura de tela 2026-07-15 115810.png](C:\Users\rafah\OneDrive\Pictures\Screenshots\Captura%20de%20tela%202026-07-15%20115810.png)

## Análise técnica

###### Como o RIP propagou as informações de roteamento na topologia?

O RIP propagou as informações utilizando mensagens de atualização periódica enviadas a cada 30 segundos via multicast para o endereço `224.0.0.9` (padrão do RIPv2). Cada roteador anunciou as redes diretamente associadas às suas interfaces de rede ativas (configuradas sob o comando `network`). Ao receber um anúncio, o roteador vizinho incrementou a métrica de distância em 1 (métrica baseada em número de saltos ou *hop count*) e incorporou os novos destinos à sua tabela de rotas, identificando-os com o caractere **R**.

###### Quais diferenças foram observadas em relação ao roteamento estático?

A principal diferença foi o dinamismo e a automação do plano de controle:

1. **Configuração simplificada:** Em vez de mapear todos os destinos remotos manualmente, o administrador declara apenas as redes vizinhas locais.
2. **Reação a falhas:** Diferente do roteamento estático (onde um link inativo mantém a rota estática estagnada na tabela, gerando perda de pacotes), o RIP detectou a queda de interfaces, propagou essa falha dinamicamente e atualizou as tabelas de rotas dos vizinhos sem intervenção manual.
3. **Métrica dinâmica:** O RIP decide o melhor caminho puramente pelo menor número de roteadores no trajeto, enquanto o roteamento estático é absoluto e rígido.

###### Qual o impacto da convergência lenta em redes maiores?

Em redes de grande escala, a convergência lenta do RIP gera períodos prolongados de inconsistência nas tabelas de roteamento das pontas. Isso acarreta:

* **Loops de Roteamento:** Dois roteadores podem ficar repassando pacotes entre si repetidamente por possuírem tabelas desatualizadas (problema de contagem ao infinito).
* **Indisponibilidade Temporária (Black Holes):** Pacotes enviados a redes que caíram continuam sendo transmitidos desnecessariamente até que as tabelas converjam.
* **Sobrecarga de banda e processamento:** O tráfego de pacotes perdidos ou em loop consome recursos valiosos da infraestrutura.

###### Como os mecanismos de mitigação ajudam a reduzir loops e instabilidade?

* **Maximum Hop Count:** Limita o diâmetro da rede a 15 saltos. O 16º salto é considerado infinito (inalcançável), interrompendo loops de contagem ao infinito.

* **Split Horizon:** Impede que um roteador anuncie uma rota de volta pela mesma interface física pela qual ele a aprendeu originalmente, evitando loops simples de dois nós.
* **Poison Reverse Update:** Quando um link falha, o roteador anuncia imediatamente essa rede para seus vizinhos com métrica 16 (infinita), forçando-os a invalidar a rota imediatamente.
* **Triggered Updates:** Envia mensagens de atualização imediatamente após uma mudança na topologia, sem esperar o timer padrão de 30 segundos, acelerando a convergência.
* **Hold-down Timers:** Período de tempo (geralmente 180s) em que o roteador ignora novas informações sobre uma rota que acabou de cair, dando tempo para que a notícia da falha se propague por toda a rede de forma consistente antes de aceitar um caminho alternativo que possa ser falso.

###### Em quais cenários o RIP deixa de ser uma solução adequada?

O RIP deixa de ser adequado em:

1. **Redes de Grande Porte:** Infraestruturas que excedam 15 saltos entre quaisquer duas pontas.
2. **Redes com Links de Velocidades Muito Distintas:** Como sua métrica avalia apenas a quantidade de roteadores (*hops*), o RIP prefere uma rota lenta de 1 salto (ex: link de 64 kbps) em detrimento de uma rota rápida de 2 saltos (ex: links de Fibra Óptica Gigabit), desperdiçando largura de banda.
3. **Ambientes Críticos de Alta Convergência:** Onde indisponibilidades de até minutos causadas pelos tempos de espera (*timers*) do RIP não sejam toleradas (cenário ideal para protocolos de estado de enlace como OSPF).

---

## Conclusão - Experimento 5 (RIP)

A execução do Experimento 05 permitiu compreender na prática as dinâmicas de funcionamento e as limitações dos protocolos de roteamento vetor de distância. Ao substituir as diretivas de roteamento estático do laboratório anterior pelo RIPv2, observou-se uma considerável redução no esforço administrativo de configuração, evidenciando as vantagens que o aprendizado dinâmico de topologia introduz à gerência de redes de computadores.

Por meio dos testes de desconexão de interfaces da Rede 30 no roteador R2, foi possível monitorar a reação dinâmica do RIP. No entanto, esse ensaio prático expôs de forma clara o principal gargalo do protocolo: a convergência lenta. Os temporizadores internos do RIP (timers de atualização de 30 segundos e timeouts de 180 segundos) demonstram que, embora a automatização seja eficiente para pequenos cenários, ela cria períodos críticos de instabilidade e potencial perda de pacotes enquanto os roteadores vizinhos não reestabelecem a convergência mútua.

Os estudos realizados acerca dos mecanismos de mitigação de loops (Split Horizon, Poison Reverse e Hold-down Timers) consolidaram a compreensão das contramedidas matemáticas e lógicas projetadas para contornar as deficiências de consistência do algoritmo Bellman-Ford. Conclui-se que o RIP cumpre de maneira excelente seu papel didático e sua aplicação em redes corporativas de pequeno porte, contudo, para cenários escaláveis de alta performance, a transição para protocolos baseados em estado de enlace (como OSPF) ou vetores de caminho (BGP) torna-se tecnicamente indispensável.
