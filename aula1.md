# 📘 Resumo — Aula 1: Introdução aos Sistemas Distribuídos  
**Prof. Elias P. Duarte Jr. — UFPR**

---

## 🔹 Definição
Um **sistema distribuído** é um conjunto de processos que se comunicam e cooperam para resolver uma tarefa específica.

Um **processo** é um programa em execução, composto por:
- Programa (sequência de instruções)
- PC (*Program Counter*)
- Estado da memória e registradores
- Estado das conexões TCP

---

## 🔹 Sistemas Estáticos vs. Dinâmicos
Considere um sistema distribuído como:

S = {P1, P2, …, PN}


- **Estático**:  
  - N é fixo e conhecido previamente  
- **Dinâmico**:  
  - Processos podem entrar e sair (ex: sistemas P2P)

📌 *Nesta disciplina: apenas sistemas estáticos*

---

## 🔹 Topologias

### 🔸 Fully Connected
- Representado por um **grafo completo**
- Todos os processos se comunicam diretamente
- Não há intermediários  
- Exemplo: rede local (Ethernet)

### 🔸 Multi-Hop
- Nem todos os processos têm conexão direta
- Comunicação pode exigir intermediários
- Envolve **roteamento**

---

## 🔹 Paradigmas de Comunicação

### 🔸 Message Passing (Troca de Mensagens)
- Processos se comunicam enviando e recebendo mensagens

### 🔸 Shared Memory (Memória Compartilhada)
- Processos compartilham memória para leitura/escrita

---

## 🔹 Primitivas de Comunicação

- `send(msg)` → envio de mensagem  
- `receive(msg)` → recebimento da mensagem  
- `deliver(msg)` → entrega da mensagem à aplicação  

📌 **Importante sobre `deliver`:**
- Após receber, o processo decide:
  - Entregar à aplicação
  - Ou descartar
- `send()` e `receive()` são comuns (ex: sockets)
- `deliver()` é interno ao processo

---

## 🔹 Modelos

Um **modelo** é uma abstração de um sistema real.

Principais tipos:
- **Modelo temporal**
- **Modelo de falhas**

📌 Sempre especificar ambos ao descrever um sistema distribuído

---

## 🔹 Modelos Temporais

### 🔸 Sistema Síncrono
- Limites de tempo **conhecidos e garantidos** para:
  - Comunicação
  - Execução
- Devem ser respeitados **sempre (100%)**

---

### 🔸 Sistema Assíncrono
- **Sem limites de tempo conhecidos**
- Sem garantias temporais
- Modelo mais geral

---

### 🔸 Sistemas Parcialmente Síncronos
- Intermediários entre síncrono e assíncrono

#### 📌 Modelo mais usado: GST (Global Stabilization Time)
- Sistema começa **assíncrono**
- Após um tempo desconhecido (GST), torna-se **síncrono permanentemente**

⚠️ Importante:
- O modelo funciona como um **contrato**
- Se não for respeitado → o algoritmo pode falhar

---

## 🧠 Insight Final
Sistemas reais (como a Internet) **não são totalmente síncronos nem assíncronos**, mas ficam **entre esses extremos**, sendo melhor modelados como **parcialmente síncronos**.
