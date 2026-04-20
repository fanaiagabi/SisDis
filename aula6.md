# 📘 Resumo — VCube: Um Algoritmo Distribuído Hierárquico

---

## 🔹 A Latência do VRing

- Pior caso: **N rodadas de testes**
- Cada rodada: todos os processos corretos testam ao menos 1 processo correto  

📌 Exemplo:
- `t = 30s`, `N = 100`  
- Latência ≈ `30 × 100 = 3000s = 50 minutos`

➡️ **Problema:** latência alta

---

## 🔹 Estratégias Orientadas a Evento

Quando um evento é detectado:
- Informação deve ser **disseminada a todos os processos corretos**

📌 Conceito:
- **Reliable Broadcast (Difusão Confiável)**  
- Custo:  

O(N²)


---

## 🔹 Algoritmo da Força Bruta

### 🔸 Ideia
- Todos testam todos em cada rodada  

---

### 🔸 Características

- Testes por rodada:

O(N²)


- Latência:

1 rodada (ótima)


---

📌 Problema:
- Muito caro em comunicação  

➡️ Exemplo:
- `N = 100` → **10.000 testes por rodada**

---

## 🔹 Comparação Geral

| Algoritmo     | Testes por rodada | Latência |
|--------------|------------------|----------|
| VRing        | O(N)             | O(N)     |
| Força Bruta  | O(N²)            | O(1)     |
| **VCube**    | 🔥 intermediário | 🔥 melhor equilíbrio |

---

## 🔹 O Algoritmo VCube

- Algoritmo **hierárquico**
- **Escalável**
- Combina:
  - Baixa latência  
  - Baixo custo relativo  

---

## 🔹 Estratégia de Testes

Crescimento **exponencial (dobrando)**:

### 🔸 Rodada 1
- Testes em pares:
  - `(0↔1), (2↔3), ...`

---

### 🔸 Rodada 2
- Cada processo já conhece seu par  
- Testa novos processos e **propaga informação**

---

### 🔸 Rodada 3+
- Conjunto dobra novamente  
- Informação se espalha rapidamente  

---

📌 Ideia central:
➡️ **Propagação logarítmica de informação**

---

## 🔹 Topologia: Hipercubo

### 🔸 Definição

- Número de processos:

N = 2^d


- `d` = número de bits do identificador  

---

### 🔸 Conexões

- Dois nodos estão conectados se:
  - Diferem em **1 bit**

---

### 🔸 Exemplos

- `d = 1` → 2 processos  
- `d = 2` → 4 processos  
- `d = 3` → 8 processos  
- `d = 4` → 16 processos  

---

### 🔸 Propriedades

- Grau do nodo:

log N


- Diâmetro:

log N


---

📌 Consequência:
➡️ Crescimento eficiente (escalável)

---

## 🔹 VCube vs Hipercubo

- VCube = **hipercubo virtual**
- Sistema real:
  - Fully-connected  
- Em caso de falhas:
  - Estrutura se **reorganiza dinamicamente**

---

📌 Mantém propriedades:
- Logarítmicas  
- Escaláveis  

---

## 🔹 Clusters no VCube

Processos organizados em **clusters hierárquicos**

---

### 🔸 Características

- Tamanho do cluster:

2^k

- Número de clusters:
log N

---

### 🔸 Função de Teste

C(i, s)

- Retorna lista de processos que:
  - Processo `i` deve testar  
  - No cluster `s`

---

📌 Propriedade:
➡️ Define exatamente **quem testa quem** em cada nível

---

## 🧠 Insight Final

O VCube resolve o trade-off clássico:

- ❌ VRing → pouca mensagem, alta latência  
- ❌ Força Bruta → baixa latência, alto custo  
- ✅ **VCube → equilíbrio ótimo com comportamento logarítmico**

➡️ Resultado: algoritmo **eficiente e escalável para grandes sistemas distribuídos**
