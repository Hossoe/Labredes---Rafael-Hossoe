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

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/fc1d00a6e05e419e974f5845fd31b276da15054a/Imagens%20Lab%204/Captura%20de%20tela%202026-07-15%20111508.png)

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

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/fc1d00a6e05e419e974f5845fd31b276da15054a/Imagens%20Lab%204/Captura%20de%20tela%202026-07-15%20111010.png)

#### R1

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/fc1d00a6e05e419e974f5845fd31b276da15054a/Imagens%20Lab%204/Captura%20de%20tela%202026-07-15%20111030.png)

#### R2

![](https://github.com/Hossoe/Labredes---Rafael-Hossoe/blob/fc1d00a6e05e419e974f5845fd31b276da15054a/Imagens%20Lab%204/Captura%20de%20tela%202026-07-15%20111044.png)

## Análise técnica

###### Como as rotas estáticas foram configuradas para que toda a rede se tornasse funcional?
As rotas estáticas foram configuradas manualmente em cada um dos três roteadores utilizando a sintaxe do Cisco IOS: `ip route [rede_destino] [mascara_destino] [ip_do_proximo_salto]`. Para assegurar a comunicação bidirecional de ponta a ponta (requisito essencial da pilha TCP/IP), cada roteador foi munido de rotas apontando os caminhos de **ida** e de **volta** para todas as sub-redes que não estão diretamente conectadas às suas interfaces físicas. 

*   O roteador da esquerda (**R1**, conectado à LAN `192.168.10.0/24`) recebeu rotas estáticas para alcançar as redes `192.168.20.0/24`, `192.168.30.0/24` e o enlace `192.168.50.0/24`, todas apontando para a interface adjacente do roteador central (`192.168.40.2`).
*   O roteador central (**R0**, conectado à LAN `192.168.20.0/24`) recebeu duas rotas direcionando o tráfego da esquerda para o primeiro roteador (`192.168.40.1`) e o tráfego da direita para o terceiro roteador (`192.168.50.2`).
*   O roteador da direita (**R2**, conectado à LAN `192.168.30.0/24`) recebeu rotas para alcançar as redes `192.168.10.0/24`, `192.168.20.0/24` e o enlace `192.168.40.0/24`, todas apontando para a interface adjacente do roteador central (`192.168.50.1`).

###### É necessário configurar rotas nas estações de trabalho das LANs? Por quê?
**Não.** Não é necessário configurar tabelas de roteamento complexas individualmente em cada estação de trabalho. Isso ocorre porque os hosts utilizam o conceito de **Default Gateway (Gateway Padrão)**. Quando uma estação precisa enviar dados para um endereço IP que pertence a uma sub-rede diferente da sua, o protocolo IP do host reconhece que o destino não é local e encaminha o pacote diretamente para o endereço físico (MAC obtido via ARP) do Gateway Padrão (que é a interface do roteador local). A partir desse ponto, a decisão de encaminhamento de pacotes em nível de WAN é de inteira responsabilidade dos roteadores.

###### Quais rotas ficam definidas automaticamente nas estações?
As estações de trabalho criam e mantêm automaticamente uma tabela de roteamento simplificada com as seguintes rotas locais:
1.  **Rota da Sub-rede Local:** Define que todo tráfego destinado à própria faixa IP da LAN (com base no IP e máscara atribuídos à interface do host) deve ser entregue de forma direta através do switch, sem intermediação de um roteador.
2.  **Rota de Loopback (`127.0.0.1 /8` ou `::1`):** Rota interna usada para direcionar o tráfego de teste para a própria pilha de protocolos da máquina local.
3.  **Rota de Link-Local (`169.254.0.0/16`):** Ativada automaticamente caso o host falhe em obter um endereço IP por DHCP (mecanismo APIPA).
4.  **Rota Padrão (Default Route - `0.0.0.0/0`):** Rota que aponta para o endereço do Gateway Padrão configurado na máquina, funcionando como um "encaminhe para cá tudo o que não for local".

###### Caso cada rede utilizasse uma tecnologia de enlace diferente, haveria impacto no funcionamento do roteamento? Justifique.
**Não, não haveria impacto no funcionamento do roteamento.** O roteamento opera estritamente na **Camada 3 (Rede)** do modelo de referência OSI (e da arquitetura TCP/IP) utilizando pacotes IP. Esta camada é agnóstica em relação aos protocolos de enlace e meios físicos das **Camadas 1 e 2** (como Ethernet, IEEE 802.11 Wi-Fi, PPP, Frame Relay, HDLC, etc.). 
Quando um roteador recebe um quadro de dados por uma interface física, ele decapsula o cabeçalho da Camada 2 (ex: descarta o cabeçalho Ethernet), analisa o pacote IP na Camada 3 para tomar a decisão de encaminhamento com base na tabela de rotas, e o re-encapsula em um novo cabeçalho da camada de enlace correspondente à tecnologia física da interface de saída selecionada.

###### O que é retornado pelo comando `ping` entre estações de redes distintas? Explique seu funcionamento.
O comando `ping` é uma ferramenta de diagnóstico que utiliza o protocolo **ICMP (Internet Control Message Protocol)** para testar a conectividade no nível da Camada de Rede. 
*   **Funcionamento:** A estação de origem gera e envia um pacote do tipo *ICMP Echo Request* (Tipo 8) para o IP do destinatário. Este pacote transita pelos roteadores intermediários através das tabelas de rotas até chegar ao destino, que responde enviando de volta um pacote do tipo *ICMP Echo Reply* (Tipo 0).
*   **Retorno em Redes Distintas:** Em uma comunicação bem-sucedida, o comando exibe:
    1.  O tamanho do pacote enviado/recebido em bytes.
    2.  O IP de destino que respondeu à mensagem.
    3.  O **RTT (Round-Trip Time)**, que é o tempo total gasto na viagem de ida e volta, mensurado em milissegundos.
    4.  O valor do campo **TTL (Time to Live)**, que é decrementado por cada roteador que o pacote cruza. 
    Caso ocorra falhas, ele pode retornar erros como *"Request timed out"* (tempo esgotado na espera da resposta) ou *"Destination host unreachable"* (quando um roteador intermediário não possui rota de encaminhamento para a rede de destino).

###### O que é retornado pelo comando `traceroute` entre estações de redes distintas? Explique
*   **Funcionamento:** O `traceroute` (ou `tracert` no console do Windows/Cisco) identifica o caminho físico trilhado pelos pacotes pela rede. Ele funciona enviando sucessivos pacotes de teste (ICMP ou UDP) com o campo **TTL (Time to Live)** configurado incrementalmente a partir de 1. 
    O primeiro roteador intermediário decrementa o TTL de 1 para 0, descarta o pacote e envia uma resposta de volta ao remetente contendo uma mensagem de erro *ICMP Time Exceeded* (Tempo Excedido - Tipo 11)[cite: 1]. O comando registra o IP desse roteador. Em seguida, envia outro pacote com TTL=2 para descobrir o segundo roteador, e assim consecutivamente até alcançar o host final.
*   **Retorno:** O utilitário retorna uma tabela organizada de forma cronológica onde cada linha representa um salto (hop/roteador intermediário). Para cada salto, é exibido o endereço IP da interface do roteador que processou a mensagem e três medições de tempo de resposta em milissegundos correspondentes aos pacotes de teste enviados para aquele trecho.

---

## Conclusão

A realização do Experimento 4 permitiu consolidar de maneira prática os fundamentos teóricos de endereçamento e encaminhamento IP na Camada de Rede, por meio da modelagem e da ativação da topologia proposta no ambiente simulado do Cisco Packet Tracer. Através da configuração manual das rotas estáticas nos roteadores R1, R0 e R2, tornou-se evidente como a tabela de roteamento atua como o núcleo lógico do encaminhamento de pacotes no plano de dados.

A experiência prática evidenciou os desafios inerentes à administração de redes com rotas estáticas: embora apresentem baixíssimo consumo de processamento nos roteadores e sejam ideais para topologias pequenas ou rotas stub, a necessidade de intervenção humana para configurar individualmente o caminho de ida e de volta para cada sub-rede expõe uma clara limitação de escalabilidade. Qualquer alteração estrutural ou falha de link físico em cenários reais maiores exigiria reconfiguração manual imediata, justificando a importância prática do estudo posterior de protocolos de roteamento dinâmico.

Por fim, os testes de validação com os utilitários `ping` e `traceroute` não apenas comprovaram o sucesso do estabelecimento de convergência da rede lógica criada, mas também proporcionaram um entendimento empírico e aprofundado do comportamento prático dos protocolos ICMP e IP e de como os pacotes transitam pelas interfaces físicas adjacentes. Desse modo, os objetivos acadêmicos estipulados para a prática foram plenamente atingidos, fornecendo a base técnica necessária para as próximas etapas da disciplina.
