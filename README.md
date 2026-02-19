# 📘 Agente Lógico NL ↔ CPC

Sistema para tradução bidirecional entre **Linguagem Natural (NL)** e **Cálculo Proposicional Clássico (CPC)** utilizando regras formais de lógica.

---

## 🔗 Links do Projeto

🌐 **Interface Web (GitHub Pages):**  
https://marigabbri.github.io/Agente-logico-nl-cpc/

⚙ **API hospedada no Render:**  
*(adicione aqui o link correto da sua API, se necessário)*  
[https://marigabbri.github.io/Agente-logico-nl-cpc/](https://trabalho-m-rcio-ia-oauo.onrender.com)

---

## 🧩 1. Arquitetura do Sistema e Funcionamento

O projeto foi desenvolvido com uma arquitetura simples e modular dividida em:

- Interface Web (Frontend)
- API Backend (Flask)

### 📌 Arquitetura Geral

```
[ Usuário ]
     |
     v
[ Página Web (HTML/CSS/JS — GitHub Pages) ]
     |
     v
[ API Flask — Render ]
     |
     v
[ Módulos de análise e tradução NL <-> CPC ]
```

---

### ✔ Frontend (GitHub Pages)

- Desenvolvido em **HTML, CSS e JavaScript**
- Envia requisições AJAX utilizando `fetch()`
- Possui duas funcionalidades:
  - NL → CPC
  - CPC → NL

---

### ✔ Backend (API Flask no Render)

A API possui dois endpoints principais:

---

#### 🔹 1. `/api/nl-to-cpc`

##### 📥 Entrada

```json
{ 
  "frase": "Se chover, então a grama ficará molhada." 
}
```

##### 📤 Saída

```json
{
  "ok": true,
  "formula_cpc": "(P → Q)",
  "mapeamento": { 
    "P": "chover", 
    "Q": "a grama ficará molhada" 
  }
}
```

---

#### 🔹 2. `/api/cpc-to-nl`

##### 📥 Entrada

```json
{
  "formula": "p^¬q",
  "mapeamento": {
    "p": "Kiki é uma gata",
    "q": "Kiki come de tudo"
  }
}
```

##### 📤 Saída

```json
{
  "ok": true,
  "frase_nl": "Kiki é uma gata e não Kiki come de tudo"
}
```

---

## 🧠 2. Estratégia de Tradução

A solução **não utiliza LLMs**, conforme exigido para um trabalho tradicional de lógica formal.  
A tradução é feita por meio de **regras determinísticas**.

---

### ✔ A) Tradução NL → CPC

#### 🔎 Etapas

##### 1️⃣ Normalização

- Conversão para minúsculas
- Remoção de espaços extras
- Remoção de pontuação final

##### 2️⃣ Detecção do conectivo principal

| Linguagem Natural | Operador Lógico |
|-------------------|-----------------|
| se ... então ...  | → |
| se e somente se   | ↔ |
| mas               | ∧ |
| e                 | ∧ |
| ou                | ∨ |

##### 3️⃣ Identificação de negação

Reconhece padrões como:

- “não X”
- “X não Y”

A negação não gera nova letra proposicional — aplica-se `¬` à variável correspondente.

##### 4️⃣ Identificação das proposições atômicas

Exemplo:

> “Kiki come de tudo, mas Kiki não é uma gata”

Atômicas identificadas:

- "kiki come de tudo"
- "kiki é uma gata"

##### 5️⃣ Mapeamento para letras

As proposições são mapeadas sequencialmente:

```
P, Q, R, S...
```

##### 6️⃣ Construção final da fórmula

**Entrada:**

```
Kiki come de tudo, mas Kiki não é uma gata
```

**Saída:**

```
Fórmula: (P ∧ ¬Q)

Mapeamento:
P = "kiki come de tudo"
Q = "kiki é uma gata"
```

---

### ✔ Análise

- O conectivo “mas” foi corretamente interpretado como ∧
- A proposição positiva foi identificada
- A negação foi aplicada corretamente
- Tradução realizada com sucesso

---

### ❌ Exemplo com pequeno erro

**Entrada:**

```
Se chover então grama molha
```

**Saída:**

```
erro: frase atômica 'grama molha' não mapeada...
```

📌 **Motivo**

O parser espera o padrão:

```
Se X, então Y.
```

✔ **Correção**

```
Se chover, então a grama molha.
```

---

## ⚠ 3. Limitações e Possibilidades de Melhoria

### ❗ Limitações Atuais

- Parser depende de formatação específica
- Frases ambíguas podem não ser corretamente interpretadas
- Não há suporte para:
  - Negações complexas (“é falso que…”)
  - Conectivos múltiplos aninhados
  - Proposições compostas dentro de uma mesma atômica

### 🚀 Melhorias Futuras

- Integração com análise sintática usando SpaCy
- Reconhecimento mais robusto de conectivos
- Suporte a estruturas mais complexas:
  - “ou… ou…”
  - “apesar de que…”
  - “não apenas… mas também…”
- Visualização gráfica da árvore lógica
- Testes automáticos de consistência

---

## 📌 Status do Projeto

✅ Projeto acadêmico funcional  
📚 Desenvolvido para disciplina de Lógica
