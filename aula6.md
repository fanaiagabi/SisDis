i# Resumo: VCube — Um Algoritmo Distribuído Hierárquico

---

## A Latência do Algoritmo VRing

A latência do algoritmo VRing é no pior caso de **N rodadas de testes**, sendo N o número total de processos. Um evento corresponde a um processo correto se tornar falho ou vice-versa. Uma rodada de testes é o intervalo de tempo no qual todos os processos corretos testam pelo menos 1 outro processo correto.

Para dimensionar o problema: se um intervalo de testes seja de t=30s e N=100, a latência pode chegar a 30\*100=3000s=50min. Como diminuir esta latência?

---

## Estratégias Orientadas a Evento

Ao ser detectado um evento: dispara a disseminação de informação que deve chegar a todos os processos corretos. Transmitir 1 mensagem para ser entregue por todos os processos corretos, mesmo que haja falhas, é **Reliable Broadcast (Difusão Confiável)**. Custo: **O(N²) mensagens**.

---

## O Algoritmo da Força Bruta

No VRing os processos estão organizados em anel. No algoritmo distribuído da força bruta, todos os processos testam todos os demais em cada intervalo de testes. O número de testes executados é **O(N²) testes por rodada**, mas a latência é de **apenas 1 rodada de testes no pior caso** — latência ótima (a melhor possível!). Porém o número de testes é muito caro: se N=100, o número de mensagens (testes) é 10.000 (dez mil!). **Super caro!**

---

## O Algoritmo VCube

Uma alternativa é usar um algoritmo distribuído que seja escalável. O VCube fica "entre" o VRing e a Força-Bruta:

- VRing: N testes por rodada, latência N rodadas
- Força-Bruta: N² testes por rodada, latência 1 rodada

O **VCube é um algoritmo hierárquico, escalável por definição**.

---

## VCube: Estratégia de Testes

A ideia central do VCube é hierárquica e cresce dobrando os nodos a cada rodada:

- Na **primeira rodada**: pares testam entre si (0↔1, 2↔3, …)
- Na **segunda rodada**: todos os processos corretos têm informações completas de seus pares — por exemplo, 0 testa 2 e obtém info 3; 2 testa 0 e obtém info 1; e assim por diante
- Na **terceira rodada**: podemos dobrar os nodos para 8 e completam o diagnóstico

---

## Topologia Hipercubo

O VCube é baseado na topologia hipercubo. Em um hipercubo de d dimensões há N=2d processos. d é o número de bits necessário para identificar os N processos. Existe uma aresta entre dois vértices (i,j) se os identificadores de i e j **diferem em exatamente 1 bit**.

Exemplos:
- Hipercubo de 1 dimensão: N=2¹=2 processos
- Hipercubo de 2 dimensões: N=2²=4 processos
- Hipercubo de 3 dimensões: N=2³=8 processos
- Hipercubo de 4 dimensões: dois hipercubos de 8 nodos

O hipercubo tem **diversas propriedades logarítmicas** — logN (logaritmos base 2, sempre). Grau de cada nodo e diâmetro (distância máxima) são logarítmicos. Quando dobra o número de nodos, o incremento é de 1 unidade. Propriedades logarítmicas correspondem a **escalabilidade: funciona bem quando N cresce**.

---

## VCube: Muito Mais que Hipercubo

O hipercubo foi muito usado para a construção de arquiteturas paralelas, entre outras aplicações computacionais. O VCube corresponde a um **hipercubo virtual quando todos os processos estão corretos**. A topologia subjacente é *fully-connected*. Quando processos falham, a topologia se reorganiza, mantendo diversas propriedades logarítmicas.

---

## VCube: Clusters de Processos

No VCube os processos são organizados em **clusters** sob o ponto de vista do testador. O tamanho do cluster é sempre uma potência de 2 — há **logN clusters**. A função **C(i,s)** retorna a lista de processos (em ordem) que devem ser testados pelo testador i nos clusters s=1,2,…,logN.

Exemplo completo para N=8 processos:

| s | C(0,s) | C(1,s) | C(2,s) | C(3,s) | C(4,s) | C(5,s) | C(6,s) | C(7,s) |
|---|--------|--------|--------|--------|--------|--------|--------|--------|
| 1 | 1 | 0 | 3 | 2 | 5 | 4 | 7 | 6 |
| 2 | 2,3 | 3,2 | 0,1 | 1,0 | 6,7 | 7,6 | 4,5 | 5,4 |
| 3 | 4,5,6,7 | 5,6,7,4 | 6,7,4,5 | 7,4,5,6 | 0,1,2,3 | 1,2,3,0 | 2,3,0,1 | 3,0,1,2 |

---

## Especificação do Algoritmo VCube

```
Algoritmo VCube executado pelo processo i:
  repita
    para s de 1 até logN e voltando a 1 faça
      repita
        execute um teste no próximo processo de C(i,s);
        se o processo foi testado correto
          então obtenha info cluster s e atualize Statei[N];
      até ter testado um processo correto ou todos falhos;
      durma até o próximo intervalo de testes;
    fim-para;
  para-sempre;
```

Cada processo i mantém o vetor local Statei[N]. Em cada intervalo, um processo testa um dos seus logN clusters. Testes são executados sequencialmente em cada cluster, até um processo correto ser testado. Ao testar um processo correto: testador obtém informações sobre todos os demais processos do cluster que não testou neste intervalo.

---

## Propriedades do VCube: Latência e Número de Testes

**Latência no pior caso:** não é logN, mas **log²N rodadas de testes** — detalhes na próxima aula.

**Número máximo de testes:** considere que todos os processos do maior cluster (com N/2 processos) estão falhos. Os N/2 testadores testam todos os N/2 falhos: **N²/4 = O(N²)**. Caso especial, mas este é o número de testes da força-bruta — por isso outra estratégia foi definida: próxima aula.

---

## Referência Original

"A Hierarchical Adaptive Distributed System-Level Diagnosis Algorithm," *IEEE Transactions on Computers*, Vol. 47, No. 1, 1998.

---

> **Nota:** Este resumo usa exclusivamente os termos e conteúdos presentes nos slides. As únicas informações adicionadas externamente foram: a explicação sobre o que é "dobrar nodos" na estratégia de testes e a observação de que a tabela C(i,s) parcial do slide 35 foi substituída pela tabela completa do slide 36 — ambas presentes nos slides originais.
