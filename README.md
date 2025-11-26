🧠 Agente de Tradução NL ↔ CPC
Tradução automática entre Linguagem Natural e Cálculo Proposicional Clássico (CPC)

Este projeto implementa um agente de IA simbólica capaz de converter:

Frases simples em português (NL – Natural Language) → fórmulas do Cálculo Proposicional (CPC)

Fórmulas proposicionais → frases compreensíveis em português

A aplicação é composta por uma API em Flask, hospedada no Render, e uma interface web estática, hospedada no GitHub Pages.

📌 1. Arquitetura do Sistema e Funcionamento
📐 Arquitetura
+-------------------+           +--------------------------+
|   Interface Web   |  HTTP     |          API Flask       |
| (GitHub Pages)    +---------> |   /api/nl-to-cpc         |
|   index.html      |  JSON     |   /api/cpc-to-nl         |
+-------------------+           +--------------------------+
                                   |
                                   | Lógica Simbólica
                                   v
                          +------------------------+
                          |  Motor de Tradução    |
                          |  - Regras linguísticas|
                          |  - Parsing proposicional|
                          |  - SymPy (CPC)        |
                          +------------------------+

✔ Front-end (GitHub Pages)

Código HTML/CSS/JS.

Envia requisições fetch para a API.

Exibe CPC, mapeamentos e texto reconstruído.

✔ Back-end (Render – Flask)

Recebe requisições HTTP POST.

Normaliza e analisa frases.

Identifica conectivos ("e", "ou", "mas", "se... então...").

Quebra em proposições atômicas.

Gera fórmulas em CPC.

Usa SymPy para interpretar fórmulas e reconstruir frases.

📌 2. Estratégia de Tradução

A tradução foi feita sem LLMs, apenas com:

Regras linguísticas simples

Manipulação de strings

SymPy para álgebra proposicional

🧱 2.1 Normalização da frase

transformar em minúsculas

remover espaços repetidos

lidar com variações de "não"

Exemplo:

"Kiki  NÃO   é uma gata." 
→ "kiki não é uma gata"

🔍 2.2 Identificação do conectivo principal

Regras implementadas:

"se X então Y"          → X → Y
"X se e somente se Y"   → X ↔ Y
"X mas Y"               → X ∧ Y
"X e Y"                 → X ∧ Y
"X ou Y"                → X ∨ Y


Exemplo:

"Sofia é uma gata, mas Sofia não come peixe."
→ (P ∧ ¬Q)

🔤 2.3 Quebra em proposições atômicas

Exemplo:

"Kiki é uma gata, e Kiki não come peixe."
→ atomicas = ["kiki é uma gata", "kiki não come peixe"]

🔡 2.4 Mapeamento automático NL → letras proposicionais

Exemplo:

P: "kiki é uma gata"
Q: "kiki come peixe"


Se a atômica tem "não", guardamos a versão POSITIVA no dicionário.

📘 2.5 Construção da fórmula em CPC

Exemplo:
Frase:

"Kiki é uma gata e Kiki não come peixe"


Saída:

(P ∧ ¬Q)

📗 2.6 Tradução reversa CPC → NL (via SymPy)

SymPy decodifica a estrutura:

And(P, Not(Q)) → "P e não Q"


Resultado final:

"Kiki é uma gata e não Kiki come peixe"

📌 3. Exemplos de Input/Output com análise
✅ Exemplo 1

Entrada (NL):

Se chover, então a grama fica molhada.


Saída:

formula_cpc: (P → Q)
mapeamento:
P: "chover"
Q: "a grama fica molhada"


✔ CORRETO: conectivo bem identificado.

⚠ Exemplo 2 — Erro conhecido

Entrada:

Maria é alta ou João é baixo e Pedro corre.


Problema: a sentença é ambígua sem parênteses.

O sistema assume interpretação mais simples:

→ ((P ∨ Q) ∧ R)


✔ Limitação conhecida — sem análise sintática completa.

📌 4. Limitações e Possibilidades de Melhoria
❌ Limitações atuais
1. Não entende frases complexas

orações subordinadas

ambiguidade sintática

pronomes ("ela", "ele")

2. Não utiliza modelos estatísticos (spaCy removido)

Por limitações do Render.

3. Sem reconhecimento semântico

O sistema não sabe que:

"Kiki" = "ela" = "a gata"

4. Fórmulas grandes ficam difíceis de verbalizar
🚀 Possibilidades de melhoria
1. Reintroduzir o spaCy (modelo pt-core)

✔ análise sintática
✔ lematização
✔ dependências

2. Adicionar parênteses implícitos

Melhor resolução da ordem dos conectivos.

3. Adicionar cache de mapeamentos para texto mais natural
4. Interface com LLM (opcional)

Para:

reescrita natural do texto

desambiguação

geração de mapeamentos mais inteligentes

📌 5. Vídeo de Demonstração 🎥

👉 Link do vídeo demonstrativo:
(COLOQUE O LINK AQUI)
Pode ser YouTube, Google Drive ou Streamable.

📌 6. Como executar localmente
pip install flask flask-cors sympy
python app.py


Abrir o index.html no navegador e usar.

📌 7. Hospedagem

API: Render

Front-end: GitHub Pages

Ambos já configurados para funcionar juntos.
