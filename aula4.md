# Resumo: Aula 11 — O Consenso, sua Impossibilidade e os Detectores de Falhas

---

## O Consenso

Considere um sistema distribuído que consiste de um conjunto de processos. Quando iniciam a execução do consenso, os processos "propõem" valores iniciais — cada processo propõe um valor de um conjunto de valores já definido, o clássico é {0,1}. Ao final do consenso: todos os processos "decidem" pelo mesmo valor, que é um daqueles que foram propostos inicialmente.

As três propriedades que definem o consenso são:

**Acordo**: todos os processos corretos decidem o mesmo valor. **Validade**: se um processo correto decide um determinado valor, então este valor foi proposto por um processo do sistema. **Terminação**: todo processo correto eventualmente decide um valor.

Muitos enxergam o consenso como o problema mais importante de sistemas distribuídos. Diversas aplicações dependem dele, em particular na replicação distribuída ("replicação máquina-de-estado"). Diversos algoritmos distribuídos são baseados no consenso: difusão atômica, transações distribuídas, etc.

---

## A Impossibilidade FLP

Em 1985, Fisher, Lynch e Paterson provaram que é impossível garantir a correta execução do consenso em sistemas distribuídos assíncronos, sujeitos a falhas crash — a chamada "Impossibilidade FLP".

A prova explora justamente o fato de que em um sistema assíncrono é impossível distinguir um processo lento de um processo falho. O processo lento pode ser confundido como falho e acaba tomando uma decisão diferente dos demais.

A impossibilidade FLP é um resultado decisivo em Sistemas Distribuídos. O consenso é muito importante, e os sistemas reais não são síncronos — o que torna este um resultado prático. Diversos trabalhos propõem as mais diversas formas de contornar a impossibilidade FLP; justamente neste contexto foram inicialmente investigados os modelos parcialmente síncronos.

---

## Os Detectores de Falhas Não Confiáveis

Um dos trabalhos mais importantes que surgiram na onda de tentar "contornar" a Impossibilidade FLP é o que definiu os Detectores de Falhas Não Confiáveis. Em 1996, Chandra e Toueg pensaram no seguinte: se os processos executando o consenso tiverem acesso a informação sobre a falha de processos, o consenso se torna possível? — quem falhou? quem está correto?

Os detectores de falhas são — por definição — não confiáveis. Podem cometer erros de 2 tipos: um processo que na realidade está falho é informado como correto pelo detector, ou um processo que na realidade está correto é informado como falho pelo detector.

Os detectores são implementados como módulos aos quais os processos do sistema têm acesso localmente. A saída (clássica) de um detector de falhas é a lista de processos suspeitos de terem falhado. Como em sistemas assíncronos é impossível distinguir um processo falho de um processo lento, o detector informa a "suspeita de falha" de um processo, não a "falha". Os estados são: **correto** e **suspeito**.

> ⚠️ *[Observação externa ao slide]:* A visão dos detectores como "oráculos" — entidades que os processos consultam para saber o estado dos demais — foi introduzida por pesquisadores franceses, com inspiração no Oráculo de Delfos da Grécia antiga.

---

## Completude & Precisão

Chandra & Toueg propuseram 2 propriedades básicas dos Detectores de Falhas:

**Completeness (Completude)**: o detector de falhas suspeita de processos que efetivamente falharam. **Accuracy (Precisão)**: o detector de falhas não suspeita de processos que efetivamente estão corretos.

A completude é fácil de conseguir — um processo que falha no modelo crash não comunica de forma alguma. Porém, observe que mesmo assim há um tempo entre a falha e a detecção da mesma; neste intervalo de tempo o detector vai dizer que o processo falho está correto. A precisão é difícil de garantir — pode ser que eu jamais suspeite que um processo correto está falho, mas não tem como garantir isso.

---

## Propriedades Fracas, Fortes e Eventuais

As propriedades de completude e precisão são reclassificadas como fortes e fracas:

**Completude Forte**: eventually, todo processo que falha é suspeitado por todo processo correto. **Completude Fraca**: eventually, todo processo que falha é suspeitado por pelo menos 1 processo correto. Note que a completude fraca pode ser transformada em forte: o processo que suspeita informa os demais corretos.

**Precisão Forte**: nenhum processo correto é suspeitado antes de falhar. **Precisão Fraca**: pelo menos um processo correto jamais é suspeitado antes de falhar. Há versões eventually: **Precisão Forte Eventual** — existe um tempo após o qual nenhum processo é suspeitado antes de falhar; **Precisão Fraca Eventual** — existe um tempo após o qual pelo menos 1 processo correto não é suspeitado.

---

## Classificação dos Detectores

A combinação de completude (forte/fraca) com precisão (forte/fraca/eventual) gera 8 classes de detectores:

| Completude ↓ / Precisão → | Forte | Fraca | Forte Eventual | Fraca Eventual |
|---|---|---|---|---|
| **Forte** | Perfeito **P** | Strong **S** | Evnt. Perfect **◊P** | **◊S** |
| **Fraca** | **Q** | Weak **W** | **◊Q** | Event. Weak **◊W** |

---

## Afinal: Os Detectores Permitem o Consenso?

A resposta pode parecer surpreendente: basta que o detector de falhas seja **◊W** para garantir o consenso! Mas não há como garantir, pode "acontecer" na execução.

---

## Implementação de Detectores

Os detectores de falhas podem ser implementados em dois modelos: **Pull** — parecido com os testes do diagnóstico, processos perguntam explicitamente aos outros os seus estados; e **Push** — este é o modelo mais usado, cada processo manda periodicamente mensagens chamadas "heartbeats" para todos os demais.

No algoritmo básico (Push), cada processo i mantém um conjunto Suspeitos. Ao iniciar, computa um timeout para cada processo j e envia heartbeats periodicamente. Ao receber um heartbeat de j, remove j dos Suspeitos e atualiza o timeout. Ao expirar o timeout de j, adiciona j aos Suspeitos. Quando invocado, retorna o conjunto Suspeitos.

---

## Impacto

Os detectores de falhas revolucionaram os sistemas distribuídos. Não apenas o consenso foi revisto para incorporar detecção de falhas, mas **virtualmente todos** os problemas de sistemas distribuídos foram revisitados e classificados de acordo com o detector que precisam. Por exemplo: a exclusão mútua distribuída precisa de detector perfeito.
