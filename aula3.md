# Resumo: Sistemas Distribuídos — Modelos de Enlaces

---

## O Sistema Distribuído

Um sistema distribuído é um conjunto de N processos S = {p1, p2,…, pN}. O mesmo algoritmo é executado por todos os processos localmente, e os processos se comunicam trocando mensagens.

---

## Propriedades de Algoritmos Distribuídos

Para demonstrar que um algoritmo distribuído "funciona de verdade", provamos sua correção através de propriedades. Existem dois tipos principais:

**Safety** garante que "nada de ruim acontece". No exemplo do consenso, garante que jamais 2 processos vão decidir por 2 valores diferentes.

**Liveness** garante que "algo de bom acontece". Uma forma fácil de garantir safety seria deixar os processos parados — claramente insatisfatório. Liveness garante que os processos efetivamente decidem. Pode ser traduzida como "progressão".

**Fairness** é um terceiro tipo: "justiça", todos os processos têm "direitos" iguais. No exemplo da exclusão mútua distribuída: safety (jamais 2 processos acessam o mesmo recurso simultaneamente), progress (efetivamente algum processo acessa o recurso), e fairness (os diferentes processos têm a mesma chance de acessar o recurso).

---

## A Mensagem

A mensagem tem informações "de controle": identificador do processo origem (source) e destino (destination). Também carrega informações específicas da aplicação: o payload.

O identificador da mensagem consiste do endereço da origem e de um contador de mensagens local — zerado antes da execução do algoritmo e incrementado de 1 a cada mensagem transmitida. Assim, cada mensagem carrega seu identificador (id-origem + contador).

---

## Primitivas de Comunicação

As primitivas básicas são **send(msg)**, **receive(msg)** e **deliver(msg)**. Uma mensagem recebida pode ser descartada (por diversos motivos) ou entregue à aplicação via deliver(). Os endereços de origem e destino podem ser explicitados na primitiva: send(source-id, dest-id, msg).

As mensagens são transmitidas por canais de comunicação (também chamados de link ou enlace), e a tecnologia não importa — wireless, fibra ótica, cabo coaxial, Internet, rede local, ou mesmo processos na mesma máquina.

---

## Modelos de Canais de Comunicação

**Toda** vez que se descreve um sistema distribuído ou um algoritmo distribuído, deve ser explicitado o modelo de enlace. Os dois modelos mais importantes são:

---

### Canal Fair-Loss

Os enlaces fair-loss podem perder mensagens, mas existe uma boa probabilidade de que uma mensagem transmitida chegue ao destino. Refletem enlaces reais como IP. Suas propriedades são:

- **No Creation**: se uma mensagem é recebida pelo processo q, então ela foi efetivamente transmitida pelo processo p.
- **Duplicação Finita**: se uma mensagem é transmitida um número finito de vezes, não é recebida infinitas vezes.
- **Fair-Loss**: se uma mensagem é transmitida infinitas vezes pelo processo p, e ambos os processos não falham, a mensagem é recebida infinitas vezes pelo processo q — a probabilidade de recebimento é maior que zero.

---

### Canal Stubborn (Intermediário)

Construído sobre o enlace fair-loss. Suas propriedades são No-Creation e **Stubborn Delivery**: se o processo p transmite uma mensagem para o processo q, e ambos não falham, então q recebe esta mensagem infinitas vezes — as perdas de mensagens não afetam a comunicação.

O algoritmo mantém um conjunto **Sent** e um **TimeDelay**. A cada timeout, retransmite todas as mensagens em Sent. Ao receber uma mensagem, simplesmente faz deliver(msg).

---

### Canal Perfeito (Confiável)

Garante entrega sem duplicação. Suas propriedades são:

- **No Creation**
- **No Duplication**: cada mensagem é entregue 1 única vez.
- **Reliable Delivery**: se p transmite para q, e ambos não falham, q recebe a mensagem *eventually*.

> ⚠️ *[Observação externa ao slide]:* A palavra **eventually** em inglês técnico, diferente de "eventualmente" em português, significa que **com certeza vai acontecer** em algum momento — não se sabe quando, mas é garantido 100%.

O algoritmo acrescenta ao stubborn um conjunto **Delivered**: ao receber uma mensagem, só faz deliver(msg) se ela ainda não estiver em Delivered, depois a adiciona ao conjunto. Isso garante o No Duplication.

Uma versão com **ACKs** é mais eficiente na prática: ao receber um ACK, remove a mensagem confirmada do conjunto Sent, parando as retransmissões. Ao receber uma mensagem, sempre manda um ACK — mesmo que já tenha entregue antes.

---

## Visão Prática

- Canal perfeito → **TCP**
- Canal fair-loss → **UDP / IP**

O TCP pode ser considerado uma implementação de canal de comunicação perfeito.
