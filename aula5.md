# 📘 Resumo — VRing: Detector de Falhas em Anel

---

## 🔹 O que é o VRing

O **VRing** é um algoritmo de detecção de falhas baseado em:
- Um **anel virtual (lógico)** de processos  
- Identificadores de `0` até `(N-1)` com aritmética módulo  

📌 Característica principal:
- O anel **se adapta a falhas**
- Cada processo se conecta ao **próximo processo sem-falha**

---

📌 Origem:
- Proposto no contexto de **Adaptive Distributed System-Level Diagnosis (Adaptive-DSD)**  
- Detector do tipo **push**  
- Baseado em **testes entre processos**

---

## 🔹 Modelo de Sistema e Premissas

Sistema:

S = {0, 1, 2, …, (N-1)}


---

### 🔸 Características

- Cada processo pode estar:
  - Correto  
  - Falho (modelo **crash**)  

- Sistema **assíncrono**
  - Resultado de testes:
    - correto  
    - suspeito  

---

### 🔸 Comunicação

- Canais **perfeitos**
- Sistema subjacente:
  - **Fully-connected**
- Anel é apenas **lógico**

---

### 🔸 Modelo Temporal

- Baseado em **GST (Global Stabilization Time)**:
  - Início: assíncrono  
  - Após GST: síncrono  

---

### 🔸 Premissa Importante

- Um novo evento só ocorre após o anterior ser detectado  

📌 Evento:
- Processo falha  
- Processo se recupera  

---

## 🔹 Grafo de Testes

Representado por:

G = (V, T)


- **V** → processos  
- **T** → testes (arestas direcionadas)  

---

### 🔸 Interpretação

- `(a, b) ∈ T` → processo **a testa b**

📌 No VRing:
- Consideramos apenas testes entre **processos corretos**

---

## 🔹 Intervalos vs. Rodadas de Testes

### 🔸 Intervalo de Testes
- Tempo fixo  
- Definido localmente  
- Ex: 10 ms, 30 s  

---

### 🔸 Rodada de Testes
- Todos os processos corretos:
  - Testaram outro processo correto  
  - Ou testaram todos os falhos  

---

📌 Observação:
- Em sistemas reais:
  - Não há relógio global  
  - Rodadas são definidas pelo **processo mais lento**

---

## 🔹 Vetor de Estados `State[N]`

Cada processo mantém:

State[N]


---

### 🔸 Significado dos valores

- `-1` → desconhecido  
- `0` → correto  
- `1` → falho  

---

### 🔸 Atualização

Quando o processo `j` detecta evento em `i`:

Statej[i]++


---

### 🔸 Interpretação

- Valor **par** → processo correto  
- Valor **ímpar** → processo falho  

---

📌 Ideia:
➡️ O contador funciona como um **timestamp de eventos**

---

## 🧠 Insight Final

O VRing combina:
- Estrutura lógica simples (**anel**)  
- Comunicação eficiente (**testes locais**)  
- Representação compacta (**vetor de estados**)  

➡️ Resultando em um detector de falhas adaptativo e robusto em sistemas distribuídos.
