# Resumo — Aula 8: VCube, Propriedades & Versão 2

## O Algoritmo VCube

O VCube é um algoritmo distribuído hierárquico. Os N processos são organizados em *clusters*. Cada processo tem logN clusters. Os clusters começam com 2 nodos e vão dobrando a cada rodada de testes. O maior cluster tem metade (N/2) dos processos.

A função **C(i,s)** retorna a lista de processos (em ordem) que devem ser testados pelo testador *i* nos clusters s=1,2,…,logN.

---

## Estratégia de Testes

Processos executam testes periodicamente em intervalos de testes. Cada processo i mantém o vetor local State_i[N]. Em cada intervalo, um processo testa um dos seus logN clusters. Testes são executados sequencialmente em cada cluster, até um processo correto ser testado. Ao testar um processo correto: testador obtém informações sobre todos os demais processos do cluster que não testou neste intervalo.

Uma **rodada de testes** acontece quando todos os processos corretos testaram pelo menos um de seus clusters. O processo mais lento determina a rodada.

---

## Premissas

- Processos falham por parada (*crash*)
- Enlaces não falham: sistema *fully-connected*
- Um processo correto executa testes perfeitos: determina corretamente o estado do processo testado (100% perfeito!)
- Portanto: o sistema é síncrono

---

## Propriedades: Latência

**Lema 1:** Dado um processo i, um cluster qualquer C(i,s) e um instante de tempo qualquer: em no máximo logN rodadas de testes i testa C(i,s). Cada processo tem logN clusters, portanto em logN rodadas de testes todos os processos testaram todos os seus clusters.

**Teorema 1:** O menor caminho entre dois vértices do grafo de testes G=(V,T) contém no máximo logN arestas. *(Provado por indução em t, considerando N = 2ᵗ.)*

**Teorema 2:** Em no máximo **log²N rodadas** de testes todos os processos corretos completam o diagnóstico. Para a execução de cada um dos logN testes podem ser necessárias logN rodadas. Assim: logN × logN = log²N rodadas.

---

## Propriedades: Número de Testes

O número máximo de testes no pior caso ocorre quando todos os processos do maior cluster (com N/2 processos) estão falhos. Os N/2 testadores testam todos os N/2 falhos: **N²/4 = O(N²)**.

---

## Versão 2 do VCube

Esta versão determina quem é o *testador* de cada processo em cada cluster. O testador do nodo j no cluster s é o **primeiro nodo sem-falha em C(j,s)**. Desta forma, apenas 1 testador por processo, ao invés de todos do outro cluster, garantindo **NlogN testes a cada logN rodadas**.

Outra diferença: ao testar um processo correto, o testador obtém qualquer "novidade" que o testado tenha — na versão anterior obtém apenas informações do cluster testado. Esta nova estratégia acelera fortemente o diagnóstico na média — o pior caso continua sendo log²N.

---

> ⚠️ **Adicionado externamente:** A estrutura de seções com títulos (Estratégia de Testes, Premissas, etc.) e os marcadores de destaque em negrito foram organizados pelo resumidor para facilitar a leitura — não aparecem como seções explícitas nos slides originais.
