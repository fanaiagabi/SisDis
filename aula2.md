# 📘 Resumo — Aula 2: Tolerância a Falhas  
**Prof. Elias P. Duarte Jr. — UFPR**

---

## 🔹 Por que Tolerância a Falhas é Importante?
Organizações modernas são fortemente dependentes de seus sistemas computacionais.  
➡️ Se o sistema falha, a organização pode parar de funcionar.

📌 Fato importante:
- Dado tempo suficiente, **a probabilidade de falha é 100%**
- Sistemas inevitavelmente falham — cedo ou tarde

---

## 🔹 Definição
Um sistema é **tolerante a falhas** se:
- Continua funcionando corretamente  
- Mesmo com alguns componentes falhos  

📌 Exemplos críticos:
- Aviação  
- Sistemas de missão crítica  
- Usinas  
- Monitoramento hospitalar  

📌 Exemplos comerciais:
- Bancos  
- E-commerce (ex: Black Friday)

---

## 🔹 Base: Redundância

Para tolerar falhas, é necessário **redundância**:

### 🔸 Explícita
- Ex: RAID / espelhamento de HD

### 🔸 Implícita
- Ex: roteamento alternativo em redes

📌 Observação:
- Sistemas distribuídos já são naturalmente redundantes (vários processos)

---

### 🔸 Técnica clássica: Triple Modular Redundancy (TMR)
- 3 módulos executam a mesma tarefa  
- Um "votador" decide a saída  
- Tolera falha de 1 módulo  

📌 Para software:
- *Design diversity*
- *N-version programming*

---

## 🔹 Dependability (Confiança no Funcionamento)

Conjunto de atributos que garantem o funcionamento confiável:

### 🔸 1. Reliability (Confiabilidade)
- Probabilidade de funcionamento correto ao longo do tempo  
- Métrica: **MTBF** (Mean Time Between Failures)

---

### 🔸 2. Availability (Disponibilidade)
- Probabilidade de estar funcionando em um instante  
- Inclui falhas e recuperação  
- Métrica: **MTTR** (Mean Time To Repair)

📌 High Availability:
- Indisponibilidade na ordem de minutos por ano

---

### 🔸 3. Safety (Segurança - Safety)
- Evita consequências catastróficas  
- Baseado em modelos formais  
- Essencial em sistemas críticos  

---

### 🔸 4. Security (Segurança)
- Sigilo  
- Integridade  
- Autenticidade  

📌 Observação:
- Segurança e tolerância a falhas são interdependentes

---

📌 Referência clássica (Avizienis et al., 2004):
- Substitui "security" por **integridade**
- Adiciona **maintainability** (facilidade de manutenção)

---

## 🔹 Fault — Error — Failure

Três níveis do mesmo problema:

- **Fault (defeito)**  
  - Problema interno não manifestado  

- **Error (erro)**  
  - Fault se manifesta internamente  

- **Failure (falha)**  
  - Erro afeta a saída do sistema  

---

📌 Possibilidades:
- Fault sem Error  
- Fault + Error sem Failure  
- Fault + Error + Failure  

---

## 🔹 Modelos de Falhas

### 🔸 Crash
- Processo para completamente  
- Perde estado  

---

### 🔸 Crash-Recovery
- Processo pode retornar  
- Mantém estado em memória persistente  

---

### 🔸 Fail-Stop
- Crash + detecção de falha pelos outros nodos  
- Muito usado na prática  

---

### 🔸 Omissão
- Processo não envia/recebe todas mensagens  

---

### 🔸 Bizantina
- Comportamento arbitrário  
- Pode agir de forma maliciosa  

---

### 🔸 Temporização
- Atrasos além do esperado  
- Só faz sentido em sistemas síncronos  

---

📌 Relação de inclusão:

Bizantina ⊃ Omissão ⊃ Crash


---

## 🔹 Resultado Importante: Impossibilidade da Coordenação

### 🔸 Problema
Dois processos devem:
- Tomar a mesma decisão (α ou β)
- Ou não tomar decisão

Mesmo com:
- Canal que pode perder mensagens

---

### 🔸 Resultado
❌ **Impossível garantir coordenação perfeita**

---

### 🔸 Intuição da prova
- Considere solução com menor número de mensagens  
- A última mensagem pode ser perdida  
- Logo, não é essencial  
- Contradição → impossível

---

📌 Aplicação prática:
- Encerramento de conexão TCP

---

## 🧠 Insight Final
Falhas são inevitáveis em sistemas distribuídos.  
➡️ O objetivo não é evitá-las, mas **projetar sistemas que continuem funcionando apesar delas**.
