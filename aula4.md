# 📘 Resumo — Aula 11: Consenso, Impossibilidade (FLP) e Detectores de Falhas

---

## 🔹 O Problema do Consenso

Em um sistema distribuído:
- Cada processo **propõe** um valor (ex: `{0,1}`)
- Ao final, todos devem **decidir o mesmo valor**

---

### 🔸 Propriedades do Consenso

- **Acordo (Agreement)**  
  → Todos os processos corretos decidem o mesmo valor  

- **Validade (Validity)**  
  → O valor decidido foi proposto por algum processo  

- **Terminação (Termination)**  
  → Todo processo correto eventualmente decide  

---

📌 Importância:
- Base para:
  - Replicação máquina-de-estado  
  - Difusão atômica  
  - Transações distribuídas  

---

## 🔹 A Impossibilidade FLP

Resultado clássico de 1985 (Fischer, Lynch, Paterson):

❌ **É impossível garantir consenso em sistemas:**
- Assíncronos  
- Com falhas do tipo crash  

---

### 🔸 Intuição
- Não dá para distinguir:
  - Processo **lento**
  - Processo **falho**

➡️ Isso pode levar a decisões inconsistentes

---

📌 Consequência:
- Sistemas reais precisam de **relaxações do modelo**
- Surgem os **modelos parcialmente síncronos**

---

## 🔹 Detectores de Falhas Não Confiáveis

Introduzidos por Chandra e Toueg (1996)

💡 Ideia:
> E se tivermos informação (mesmo imperfeita) sobre falhas?

---

### 🔸 Características

- São **não confiáveis**
- Podem errar de duas formas:
  - Não detectar falha real  
  - Suspeitar de processo correto  

---

### 🔸 Funcionamento

- Cada processo consulta um **módulo local**
- Saída típica:
  - Conjunto de **processos suspeitos**

---

📌 Importante:
- Detectores indicam **suspeita**, não certeza  
- Estados:
  - Correto  
  - Suspeito  

---

📌 Analogia:
- Funcionam como um **oráculo**

---

## 🔹 Completude & Precisão

### 🔸 Completude (Completeness)
- Detecta processos que realmente falharam  

---

### 🔸 Precisão (Accuracy)
- Não acusa processos corretos  

---

📌 Observações:
- Completude → mais fácil  
- Precisão → difícil em sistemas assíncronos  

---

## 🔹 Versões Fortes, Fracas e Eventuais

### 🔸 Completude

- **Forte**  
  → Todos detectam todas as falhas  

- **Fraca**  
  → Pelo menos um processo detecta  

---

### 🔸 Precisão

- **Forte**  
  → Nunca suspeita incorretamente  

- **Fraca**  
  → Pelo menos um nunca erra  

---

### 🔸 Versões Eventuais

- Garantias passam a valer **após algum tempo desconhecido**

---

## 🔹 Classificação dos Detectores

Combinação de completude e precisão:

| Completude ↓ / Precisão → | Forte | Fraca | Forte Eventual | Fraca Eventual |
|--------------------------|------|------|----------------|----------------|
| **Forte**                | P    | S    | ◊P             | ◊S             |
| **Fraca**                | Q    | W    | ◊Q             | ◊W             |

---

## 🔹 Detectores Permitem Consenso?

💡 Resultado surpreendente:

➡️ **◊W (eventual weak) é suficiente para consenso**

⚠️ Mas:
- Não é garantido em toda execução  
- Pode depender do comportamento do sistema  

---

## 🔹 Implementação de Detectores

### 🔸 Modelo Pull
- Processos consultam outros explicitamente  

---

### 🔸 Modelo Push (mais usado)

- Processos enviam **heartbeats** periódicos  

---

### 🔸 Algoritmo Básico

Cada processo mantém:
- Conjunto `Suspeitos`

---

#### Funcionamento:
- Envia heartbeats periodicamente  
- Recebe heartbeat:
  - Remove processo de `Suspeitos`  
- Timeout expira:
  - Adiciona processo a `Suspeitos`  

---

## 🔹 Impacto

Detectores de falhas:
- Revolucionaram sistemas distribuídos  
- Tornaram o consenso mais viável  
- Permitem classificar problemas por requisitos  

---

📌 Exemplo:
- Exclusão mútua distribuída → requer detector perfeito  

---

## 🧠 Insight Final

O consenso é central em sistemas distribuídos, mas **impossível no modelo puro assíncrono**.  
➡️ Detectores de falhas introduzem **informação adicional suficiente** para tornar o problema tratável na prática.
