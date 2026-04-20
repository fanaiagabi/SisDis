# Resumo: VRing — Detector de Falhas em Anel

---

## O que é o VRing

Os processos formam um **anel virtual (lógico)**. Cada processo tem um identificador sequencial de 0 até (N-1), com operação módulo (0 segue N-1). O anel **resiste a falhas**: um processo forma enlace com o processo sem-falha *seguinte*.

O algoritmo foi originalmente proposto no contexto de **Adaptive Distributed System-Level Diagnosis (Adaptive-DSD)** — um detector de falhas *push* onde processos executam testes entre si. O objetivo é determinar os estados de todos os processos (quais estão falhos). O modelo de falhas é **crash**.

---

## Modelo de Sistema e Premissas

O sistema S consiste de N processos: S = {0, 1, 2, …, (N-1)}. Cada processo pode estar **correto** ou **falho**. O sistema é assíncrono — o resultado de um teste pode ser: **correto** ou **suspeito**.

Os canais de comunicação são perfeitos. Apesar de os processos formarem um anel virtual, o sistema subjacente é **fully-connected** (qualquer processo é capaz de testar qualquer outro).

O algoritmo é apresentado no modelo **GST (Global Stabilization Time)**: o sistema inicia assíncrono e assim permanece até um instante a partir do qual se torna síncrono para sempre.

Uma premissa simplificadora: o próximo evento só ocorre após o evento anterior ter sido detectado. Evento: ou um processo correto falha ou vice-versa.

---

## O Grafo de Testes

O grafo de testes G = (V, T) tem como vértices os processos do sistema em V. O conjunto de arcos (arestas direcionadas) T representa os testes executados: existe um arco (a, b) em T se a testa b.

**No algoritmo VRing:** no anel consideramos apenas arcos correspondentes a testes de processos corretos.

---

## Intervalos vs. Rodadas de Testes

**Intervalo de testes:** intervalo de tempo constante, pré-definido, marcado no relógio local de cada processo (ex: 30 segundos ou 10 milissegundos).

**Rodada de testes:** intervalo de tempo em que todo processo sem-falha testou pelo menos um outro processo sem-falha — ou testou todos os demais processos falhos.

Em um sistema real é difícil identificar as rodadas de testes pois não há relógio global. Quem determina a rodada é o **processo mais lento**.

---

## O Vetor State[N]

Os processos mantêm informações de estado no vetor State[N] — um contador de eventos (*timestamp*):

- **-1** → estado unknown (desconhecido)
- **0** → estado correto
- **1** → processo falho

Quando um evento é detectado pelo processo j no processo i: **Statej[i]++**. A partir daí: State[i] par = correto; State[i] ímpar = falho.

---

## Especificação do Algoritmo

```
// executado pelo processo i, a cada intervalo de testes
Início
  j ← i;
  repita
    j ← (j+1) mod N;
    teste o processo j;
    se ocorreu um evento em j então Statei[j]++;
    se j está correto
      então obtenha Statej[];
        para todo k não testado neste intervalo
          atualize Statei[k] ← Statej[k];
  até (encontrar j correto) ou (testar todos falhos);
Fim.
```

---

## Fluxo de Informações

Os resultados de testes fluem no **sentido oposto** dos testes. Quando um processo correto testa outro processo correto, pode obter as informações de diagnóstico que o processo correto testado possui. Após 1 rodada de testes, todos os processos corretos obtiveram informações de seu processo correto testado.

---

## Prova de Correção

**Teorema 1:** Em uma rodada de testes do VRing, no grafo de testes G=(V,T) há um caminho direcionado entre quaisquer 2 processos corretos. *(Prova por absurdo — ver slides 33 e 34.)*

**Corolário 1:** Em uma rodada de testes, o grafo G=(V,T) consiste de um **ciclo direcionado conectando todos os processos corretos** de S. Um ciclo direcionado é a única estrutura que satisfaz as condições do Teorema 1 mais o fato de cada processo correto executar 1 único teste em outro processo correto por rodada.

**Teorema 2:** Após N rodadas de testes, a entrada Statei[j] é idêntica para todos os processos 0 ≤ i < N. O caminho mais longo no ciclo tem N vértices, portanto em no máximo N rodadas todos os processos mantêm o mesmo valor.

---

## Latência e Número de Testes

- **Latência:** número de rodadas de testes para completar a detecção de 1 evento.
  - Pior caso: **N rodadas de testes**
  - Melhor caso: **1 rodada de testes**

- **Número de testes por rodada:** no máximo **N testes** (cada processo é testado no máximo 1 vez). Isso é **ótimo** matematicamente: impossível um algoritmo que utilize menos de N testes, pois cada processo tem que ser testado pelo menos 1 vez por rodada.

- **Tolerância a falhas:** mesmo que apenas 1 único processo esteja correto, ele detecta todos os demais. Portanto **t = N-1**. Esse resultado é ótimo: não tem como tolerar mais falhas que N-1.

---

## Antes do GST

Até o sistema estabilizar, podem acontecer **falsas suspeitas**. Após uma falsa suspeita, um processo continua testando — aumenta o número de testes. No pior caso, todos os processos estão corretos mas todos suspeitam de todos, e o algoritmo se transforma na **força-bruta**. Porém, **a latência de detecção de falhas não aumenta**.

---

> **Nota:** Este resumo usa exclusivamente os termos e conteúdos presentes nos slides. Nenhuma informação externa foi adicionada.
