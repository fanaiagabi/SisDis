# 📘 Resumo — Sistemas Distribuídos: Modelos de Enlaces

---

## 🔹 O Sistema Distribuído
Um sistema distribuído é composto por:

S = {p1, p2, …, pN}


- Conjunto de processos  
- Todos executam o **mesmo algoritmo localmente**  
- Comunicação ocorre via **troca de mensagens**

---

## 🔹 Propriedades de Algoritmos Distribuídos

Para provar que um algoritmo funciona:

### 🔸 Safety (Segurança)
- “Nada de ruim acontece”  
- Ex: dois processos **nunca** decidem valores diferentes

---

### 🔸 Liveness (Progresso)
- “Algo de bom acontece”  
- Garante que o sistema **avança**  
- Ex: processos eventualmente decidem

---

### 🔸 Fairness (Justiça)
- Todos os processos têm chances iguais  

📌 Exemplo: Exclusão Mútua Distribuída
- Safety → nunca 2 processos acessam simultaneamente  
- Liveness → algum processo acessa  
- Fairness → acesso é justo  

---

## 🔹 A Mensagem

Uma mensagem contém:

- **Controle:**
  - origem (*source*)
  - destino (*destination*)

- **Payload:**
  - dados da aplicação  

---

### 🔸 Identificador da Mensagem
- Formado por:
  - ID do processo origem  
  - Contador local de mensagens  

📌 O contador:
- Começa em 0  
- Incrementa a cada envio  

➡️ Garante **unicidade das mensagens**

---

## 🔹 Primitivas de Comunicação

- `send(msg)` → envia  
- `receive(msg)` → recebe  
- `deliver(msg)` → entrega à aplicação  

📌 Observações:
- Mensagens podem ser descartadas  
- Pode-se usar:
  - `send(source, dest, msg)`

---

## 🔹 Canais de Comunicação

Mensagens trafegam por enlaces (*links*):
- Internet  
- Rede local  
- Fibra  
- Wireless  
- Mesmo máquina  

📌 O importante é o **modelo do canal**, não a tecnologia

---

## 🔹 Modelos de Enlaces

---

### 🔸 Canal Fair-Loss
Modelo mais próximo da realidade (ex: IP)

#### Propriedades:
- **No Creation**
  - Mensagem recebida foi enviada  

- **Duplicação Finita**
  - Não há infinitas duplicações  

- **Fair-Loss**
  - Se enviar infinitas vezes → será recebida infinitas vezes  

📌 Pode perder mensagens

---

### 🔸 Canal Stubborn (Intermediário)

Construído sobre o fair-loss

#### Propriedades:
- **No Creation**
- **Stubborn Delivery**
  - Se enviado → recebido infinitas vezes  

---

#### 🔧 Funcionamento:
- Mantém conjunto `Sent`  
- Usa timeout para retransmissão  
- Ao receber:
  - `deliver(msg)` diretamente  

📌 Ideia:
➡️ Reenvio contínuo até garantir entrega

---

### 🔸 Canal Perfeito (Confiável)

#### Propriedades:
- **No Creation**
- **No Duplication**
  - Mensagem entregue uma única vez  
- **Reliable Delivery**
  - Mensagem será entregue **com certeza**

---

📌 ⚠️ *Eventually*:
- Significa que **vai acontecer com certeza**
- Tempo é desconhecido

---

#### 🔧 Implementação:
- Adiciona conjunto `Delivered`  
- Só entrega mensagens novas  

---

### 🔸 Otimização com ACK
- Ao receber → envia ACK  
- Ao receber ACK → remove de `Sent`  
- Evita retransmissões desnecessárias  

---

## 🔹 Visão Prática

- Canal perfeito → **TCP**  
- Canal fair-loss → **UDP / IP**

📌 O TCP pode ser visto como:
➡️ Uma implementação de canal confiável

---

## 🧠 Insight Final
A construção de sistemas distribuídos confiáveis depende fortemente da **abstração do canal de comunicação** — algoritmos são projetados assumindo garantias específicas desses enlaces.
