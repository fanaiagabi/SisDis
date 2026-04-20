# 📘 Resumo — Aula 8: VCube, Propriedades & Versão 2

---

## 🔹 O Algoritmo VCube

- Algoritmo **distribuído hierárquico**
- Processos organizados em **clusters**
- Cada processo possui:

log N

clusters

---

### 🔸 Estrutura dos Clusters
- Começam com **2 nodos**
- Dobram a cada rodada  
- Maior cluster:

N / 2


---

### 🔸 Função de Teste

C(i, s)


- Retorna a lista de processos que:
  - Processo `i` deve testar  
  - No cluster `s`

---

## 🔹 Estratégia de Testes

- Testes ocorrem em **intervalos periódicos**
- Cada processo `i` mantém:

State\_i[N]


---

### 🔸 Execução

- Em cada intervalo:
  - Testa **um cluster**
- Testes dentro do cluster:
  - Executados **sequencialmente**
  - Até encontrar um processo correto  

---

### 🔸 Otimização

Ao encontrar um processo correto:
- Obtém informações dos **demais processos do cluster**

---

### 🔸 Rodada de Testes

- Ocorre quando:
  - Todos os processos corretos testaram **um cluster**

📌 Definida pelo processo mais lento

---

## 🔹 Premissas

- Falhas do tipo **crash**
- Sistema **fully-connected**
- Testes são **perfeitos (100% corretos)**
- Sistema é:
  - **Síncrono**

---

## 🔹 Propriedades: Latência

---

### 🔸 Lema 1

➡️ Em no máximo:

log N

rodadas:

- Processo testa qualquer cluster  
- Todos os clusters são testados

---

### 🔸 Teorema 1

- Menor caminho no grafo de testes:

≤ log N


---

### 🔸 Teorema 2

- Diagnóstico completo em:
log² N

rodadas

---

📌 Intuição:

log N × log N = log² N


---

## 🔹 Propriedades: Número de Testes

Pior caso:
- Maior cluster (`N/2`) com todos falhos  

---

### 🔸 Custo

N² / 4 = O(N²)


---

## 🔹 Versão 2 do VCube

### 🔸 Principal Ideia

- Define **um único testador por processo**

---

### 🔸 Regra

- Testador de `j` no cluster `s`:
  - Primeiro processo **sem-falha** em:

C(j, s)


---

### 🔸 Resultado

- Redução de testes para:
N log N

a cada:

log N

---

## 🔹 Melhoria na Disseminação

Na versão 2:

- Ao testar um processo correto:
  - Obtém **todas as novidades**  

📌 Antes:
- Apenas informações do cluster  

---

### 🔸 Impacto

- Diagnóstico muito mais rápido **na média**
- Pior caso permanece:
log² N


---

## 🧠 Insight Final

O VCube combina:
- Estrutura **hierárquica**
- Crescimento **logarítmico**
- Disseminação eficiente de informação  

➡️ A versão 2 melhora significativamente a eficiência prática, mantendo garantias teóricas fortes.


