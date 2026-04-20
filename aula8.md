# Resumo — Aula 9: Ordenação de Eventos & Relógios Lógicos

## Relembrando Sistemas Distribuídos

Um Sistema Distribuído é um conjunto de processos (i.e. programas em execução) que cooperam para a realização de alguma tarefa. Nossos sistemas distribuídos são estáticos (constituídos de N processos). Os processos estão *fully-connected*: qualquer processo pode comunicar diretamente com qualquer outro sem passar por intermediários. Os processos se comunicam trocando mensagens: *send*(msg), *receive*(msg), *deliver*(msg).

---

## Eventos e sua Ordem

Muitas vezes é importante dizer se um evento aconteceu antes de outro em um Sistema Distribuído. Para nós na disciplina: **evento = execução de instrução**. Um processo é um programa em execução, portanto executa uma sequência de instruções. Importante: não há um relógio global. Cada processo tem seu relógio local.

Para dizer que um evento *e* aconteceu antes de outro evento *f*, suas ocorrências têm que ser medidas usando o sistema de tempo comum, físico. Isso só é possível usando o mesmo relógio. Mas não há relógio compartilhado!

---

## A Relação "happened-before"

Vamos definir uma relação matemática entre eventos: aconteceu-antes-de ou *happened-before*. Se o evento a aconteceu antes do evento b, representamos assim: **a → b**.

A relação aconteceu-antes-de (→) definida sobre um conjunto de eventos de um sistema distribuído que consiste de múltiplos processos é a menor relação que satisfaz as seguintes condições:

**(1)** se a & b são eventos de um mesmo processo e usando o relógio local foi medido que a aconteceu antes de b, então: a → b

**(2)** Se o evento a corresponde ao envio de uma mensagem, e o evento b corresponde ao recebimento daquela mensagem, então: a → b

**(3)** Se a → b & b → c, então a → c

Há eventos em um sistema distribuído que nem a → b nem b → a, são ditos **concorrentes**.

Uma palavra/definição importante (importantíssima!) que surge aqui é a "**causalidade**": a transmissão de uma mensagem pela origem *causa* o recebimento daquela mensagem no destino.

---

## Ordem Parcial

A relação aconteceu-antes-de define uma **ordem parcial** dos eventos de um sistema distribuído. Por que é "parcial"? Porque deixa eventos concorrentes.

---

## Relógios Lógicos

Relógios Lógicos podem ser pensados como uma forma de enumerar os eventos de um Sistema Distribuído. Não são relógios de tempo físico! Vamos assinalar números sequenciais para eventos: *timestamps*.

O relógio lógico Ci do processo Pi é uma função que assinala um valor Ci(a) para um evento a de Pi. A função C (sem índice) representa o relógio lógico do Sistema Distribuído, para todos os processos.

Esta função C só é correta se for consistente com a relação →. Em outras palavras, deve satisfazer a **condição de relógio** (*clock condition*):

> Para quaisquer eventos a & b, se a → b então C(a) < C(b)

Note que a condição reversa não é garantida!

**Para implementar um relógio lógico:**

1) Cada mensagem msg transmitida carrega o *timestamp* do seu envio send(msg)
2) Um processo Pi deve incrementar Ci entre dois eventos consecutivos
3) Um processo Pj que recebe a mensagem msg com timestamp Tm seta Cj maior que o seu valor presente **e** maior que Tm

---

## Ordenação Total de Eventos

Há eventos que não foram ordenados, estes eventos (de diferentes processos) têm o mesmo *timestamp*. Para ordenar estes eventos com o mesmo *timestamp*: use qualquer estratégia. Por exemplo, ordene pelos identificadores dos processos.

Para dois eventos a & b, dizemos que **a => b** se e somente se:

(i) Ci(a) < Ci(b) ou  
(ii) Ci(a) == Ci(b), mas o evento a é de Pi e o evento b de Pj, com i < j

A relação => é consistente com →: se a → b então a => b.

O que queremos é que todos os processos tenham uma mesma visão comum sobre a ordem total de todos os eventos. É extremamente útil: facilita a coordenação dos processos.

---

> ⚠️ **Adicionado externamente:** A divisão em seções com títulos e a organização visual dos itens numerados foram escolhas do resumidor para facilitar a leitura — não aparecem como seções explícitas nos slides originais.
