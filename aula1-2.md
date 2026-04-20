# Resumos — Sistemas Distribuídos
*Prof. Elias P. Duarte Jr. — UFPR*

---

# Aula 1: Introdução aos Sistemas Distribuídos

## Definição

Um **sistema distribuído** é um conjunto de *processos* que se comunicam e cooperam para resolver uma tarefa específica.

Um **processo** é um programa em execução, composto por: o próprio programa (sequência de instruções), o PC (Program Counter), o estado da memória e registradores, e o estado das conexões TCP.

---

## Sistemas Estáticos vs. Dinâmicos

O sistema distribuído consiste de um conjunto de processos **S = {P1, P2, …, PN}**. Quando N é pré-determinado e constante, o sistema é dito **estático**. Nos sistemas **dinâmicos**, processos podem sair e novos processos podem entrar (exemplo: sistemas P2P). Nesta disciplina são tratados exclusivamente sistemas distribuídos **estáticos**.

---

## Topologias

Existem duas topologias principais:

**Fully Connected** — representável por um grafo completo. Todos os processos têm um canal de comunicação direto com todos os outros, sem necessidade de intermediários. Uma rede local (Ethernet) corresponde a um sistema fully-connected.

**Multi-Hop** — não há canal de comunicação direto entre todos os pares de processos. Em alguns casos é necessário passar por um ou mais intermediários. Envolve roteamento.

---

## Paradigmas de Comunicação

Existem dois grandes paradigmas:

**Troca de Mensagens (Message Passing)** — os processos comunicam transmitindo e recebendo mensagens através de uma rede.

**Memória Compartilhada (Shared Memory)** — os processos compartilham memória através da qual se comunicam, escrevendo e lendo dados.

---

## Primitivas de Comunicação

As três primitivas para troca de mensagens são:

- **send(msg)** — primitiva utilizada para o envio de uma mensagem
- **receive(msg)** — primitiva utilizada no recebimento da mensagem
- **deliver(msg)** — primitiva utilizada pelo destino para a entrega da mensagem recebida à aplicação

O ponto importante sobre o `deliver`: após receber uma mensagem, o processo pode ainda decidir se ela deve ser entregue à aplicação ou descartada. A biblioteca *socket* é praticamente um padrão e oferece `send()` e `receive()`; o `deliver()` é executado interna e localmente pelo processo destinatário.

---

## Modelos

Um modelo é uma forma simplificada de representar um sistema real. Os dois modelos mais importantes de sistemas distribuídos são o **modelo temporal** e o **modelo de falhas**. Toda vez que se fala sobre um sistema distribuído específico é essencial mencionar em qual modelo temporal e em qual modelo de falhas ele se encaixa.

---

## Modelos Temporais

**Sistema Síncrono** — existem limites de tempo *conhecidos* para a transmissão de uma mensagem entre dois processos e para a execução de uma tarefa por um processo. O limite tem que ser respeitado **100% das vezes**.

**Sistema Assíncrono** — não há limites conhecidos nem para o tempo de transmissão de mensagens, nem para o tempo de execução de tarefas. É definido livre de qualquer premissa temporal.

Os modelos síncrono e assíncrono são **extremos de um segmento de possibilidades**. Os sistemas reais (como a Internet) estão em algum lugar no meio do segmento.

**Modelos Parcialmente Síncronos** — modelos intermediários. O mais usado hoje pela comunidade é o **GST (Global Stabilization Time)**: assume-se que o sistema inicia assíncrono, mas a partir de um instante de tempo (o GST) o sistema passa a se comportar como síncrono *para sempre* (isto é, até o final da execução do sistema ou do algoritmo). Se a premissa não for respeitada, o algoritmo pode não funcionar — o modelo é, de certa forma, um "contrato" que diz como deve ser o sistema real para que o algoritmo funcione.

---

*Próxima aula: tolerância a falhas e modelos de falhas.*

---
---

# Aula 2: Tolerância a Falhas

## Por que Tolerância a Falhas é Importante?

As organizações (incluindo empresas) estão se tornando indistinguíveis de seus sistemas computacionais. Consequência: se a rede (o sistema computacional) não funciona, a própria organização entra em colapso. É fundamental garantir o funcionamento correto do sistema.

Se esperarmos tempo suficiente, a probabilidade de um componente computacional falhar — no sentido de deixar de funcionar como esperado — é **100%**. Sistemas computacionais falham, cedo ou tarde.

---

## Tolerância a Falhas: Definição

Um sistema computacional é **tolerante a falhas** se continua funcionando (produzindo as saídas corretas para as entradas correspondentes) mesmo se alguns de seus componentes estejam falhos.

Alguns sistemas devem ser **obrigatoriamente** tolerantes a falhas: sistemas de aviação, sistemas de Missão Crítica (*Mission Critical*), controle de usinas, monitoramento hospitalar, ferroviários. Nesses casos, a falha do sistema causa desastre, possivelmente com a perda de vidas humanas. Em outros casos é simplesmente desejável que o sistema seja robusto, resiliente, confiável, disponível — como sistemas bancários ou lojas on-line na *Black Friday*.

---

## Base da Tolerância a Falhas: Redundância

Para o sistema continuar funcionando mesmo que um componente falhe, deve haver outro componente que substitui aquele que falhou. A redundância pode ser:

**Explícita** — por exemplo, para tolerar a falha de um HD você pode instalar um segundo HD e fazer espelhamento.

**Implícita** — por exemplo, no roteamento tolerante a falhas, quando falha a rota principal, outra é definida com base na topologia.

Por definição, um sistema distribuído consiste de um grupo (>1) de processos — desta forma é intrinsecamente redundante. "Sopa no mel" para tolerância a falhas.

Uma técnica popular (não SisDis) é a **Triple Modular Redundancy**: um módulo é triplicado, as 3 unidades executam em paralelo com as mesmas entradas, e uma unidade de votação (*voter*) seleciona a resposta da maioria — tolera a falha de 1 unidade. Para bugs de software: *design diversity, N-version programming*.

---

## Dependability: Confiança no Funcionamento

Em português, a CE-TF da SBC (*Comissão Especial de Computação Tolerante a Falhas da Sociedade Brasileira de Computação*) define que o termo *dependability* deve ser traduzido como "confiança no funcionamento" do sistema. Dependability é um conjunto de atributos, todos precisamente definidos.

Os atributos clássicos são:

**1) Reliability (Confiabilidade)** — medida da continuidade de funcionamento correto. É a probabilidade do sistema oferecer serviço correto ao longo de um intervalo de tempo de duração *t*. Mede-se até o sistema falhar. Medida relacionada: **MTBF** (*Mean Time Between Failures*) — o tempo médio entre a ocorrência de falhas; intervalo de tempo em que o sistema fica continuamente correto.

**2) Availability (Disponibilidade)** — medida da esperança de obter serviço ao acessar o sistema. Inclui períodos de falha e recuperação: a probabilidade do sistema estar funcionando corretamente em um instante de tempo. Termo correlato importante: *high-availability* — observando o sistema durante 1 ano, os períodos de indisponibilidade podem ser expressados na ordem de minutos. Medida relacionada: **MTTR** (*Mean Time To Repair*) — intervalo de tempo em que o sistema fica continuamente falho.

**3) Safety (Segurança-Safety)** — medida da probabilidade do sistema evitar consequências catastróficas. Baseada em modelos matemáticos do sistema e seu ambiente, com provas formais. Imprescindível nos sistemas de missão crítica.

**4) Security (Segurança)** — a área clássica de segurança computacional. Consiste de atributos: no mínimo sigilo, autenticidade e integridade. Segurança é parte de Tolerância a Falhas e vice-versa.

A referência clássica da área (Avizienis, Laprie, Randell, Landwehr, 2004) altera levemente estes atributos: ao invés de segurança usa *integridade*, e inclui *maintainability* — facilidade de alteração e manutenção do sistema.

---

## Fault — Error — Failure

Três níveis diferentes do mesmo fenômeno:

**Fault (falha/falta/defeito)** — defeito do sistema, decorrente de sua fabricação, projeto ou implementação. Está lá no sistema, mas não se manifestou.

**Error (erro)** — ocorre quando a falha (*fault*) é ativada e se manifesta, por exemplo na saída de uma operação.

**Failure (falha-colapso)** — a falha se propaga para a saída do sistema.

Um exemplo ilustrativo dos slides: considere dois somadores com falhas do tipo *stuck-at* (a saída não varia). É possível ter *Fault* sem *Error* e sem *Failure* (quando as entradas coincidem por acaso com o valor travado), *Fault* e *Error* sem *Failure* (quando os erros se cancelam no resultado final), e o ciclo completo *Fault + Error + Failure* (quando as falhas se manifestam em erros que se propagam para a saída final do sistema).

---

## Modelos de Falhas

Exatamente como um processo (também chamado de **nodo**) falha? Há vários modelos, alguns clássicos:

**Crash (falha por parada)** — o componente simplesmente para de funcionar. Não produz qualquer saída para nenhuma entrada e perde seu estado interno completamente.

**Crash-Recovery** — o componente falho pode recuperar e retornar ao sistema. No modelo *crash* simples, retorna sem qualquer informação sobre seu estado antes da falha. No modelo *crash-recovery*, o componente mantém informações chave em memória não-volátil (memória secundária).

**Fail-Stop** — muito usado na construção de sistemas distribuídos tolerantes a falhas práticos. As falhas dos nodos são *crash* e todos os nodos sem-falha "sabem" quais nodos estão falhos — inclui o sistema de monitoramento de falhas. Pode ser considerado um *baseline*: com este modelo podemos focar no algoritmo.

**Omissão** — um nodo que falha por omissão não produz todas as mensagens que deveria (recebimento e transmissão). Um subconjunto das mensagens é omitido.

**Bizantina** — falhas arbitrárias. Correspondem a qualquer comportamento fora da especificação. Um sistema invadido tem comportamento arbitrário, correspondendo à falha bizantina. Relacionado a Sistemas Tolerantes a Intrusão.

**Temporização** — só faz sentido em sistemas síncronos. A transmissão de uma mensagem ou a execução de uma tarefa demora mais do que deveria.

A relação entre os três principais modelos é de inclusão: **Bizantina ⊃ Omissão ⊃ Crash**. As falhas bizantinas incluem as por omissão (processos não transmitem todas as mensagens que deveriam), que por sua vez incluem crash (caso em que o conjunto de mensagens omitidas corresponde a *todas* as mensagens que deveria transmitir).

---

## Um Resultado Surpreendente: A Impossibilidade da Coordenação

O **Problema da Coordenação**: dois processos A e B, conectados por canal de comunicação que perde mensagens ocasionalmente, devem executar um algoritmo distribuído que garanta que, ao término da execução, dadas duas ações α e ß: ou ambos os processos executam a mesma ação (α ou ß), ou nenhum dos dois executa nenhuma ação.

A prova de impossibilidade é por absurdo: supondo que existem soluções, escolhe-se a que precisa do menor número de mensagens. A última mensagem (sem perda de generalidade, de A para B) não pode ser indispensável — pois o canal pode perdê-la. Logo a solução funciona sem ela, o que contradiz ter escolhido a solução com o menor número de mensagens. **ABSURDO** — impossível portanto resolver o problema.

Este não é um problema puramente teórico: o encerramento da conexão TCP envolve justamente este problema. Absolutamente prático, vivemos este problema no nosso dia-a-dia.
