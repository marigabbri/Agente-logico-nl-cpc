📘 README – Agente Lógico NL ↔ CPC
🔗 Links do Projeto

🌐 Interface Web (GitHub Pages):
https://marigabbri.github.io/Agente-logico-nl-cpc/

⚙ API hospedada no Render:
(adicione aqui o seu link, ex.:)
https://sua-api-no-render.onrender.com

🧩 1. Arquitetura do Sistema e Funcionamento

O projeto foi desenvolvido com uma arquitetura simples e modular dividida em Interface Web + API Backend.

📌 Arquitetura Geral
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

✔ Frontend (GitHub Pages)

Desenvolvido em HTML, CSS e JavaScript.

Envia requisições AJAX usando fetch().

Possui duas funcionalidades:

NL → CPC

CPC → NL

✔ Backend (API Flask no Render)

A API possui dois endpoints:

1. /api/nl-to-cpc

Entrada:

{ "frase": "Se chover, então a grama ficará molhada." }


Saída:

{
  "ok": true,
  "formula_cpc": "(P → Q)",
  "mapeamento": { "P": "chover", "Q": "a grama ficará molhada" }
}

2. /api/cpc-to-nl

Entrada:

{
  "formula": "p^¬q",
  "mapeamento": {
    "p": "Kiki é uma gata",
    "q": "Kiki come de tudo"
  }
}


Saída:

{
  "ok": true,
  "frase_nl": "Kiki é uma gata e não Kiki come de tudo"
}

🧠 2. Estratégia de Tradução (Regras, Mapeamento, LLMs) + Exemplos e Análise

A solução não usa LLMs, conforme solicitado para um trabalho tradicional de lógica — a tradução é feita por regras formais.

✔ A) Tradução NL → CPC
Etapas

Normalização

Texto é transformado para minúsculas

Espaços extras removidos

Pontuação final removida

Detecção do conectivo principal

“se … então …” → →

“se e somente se” → ↔

“mas” → ∧ (tratado como “e” lógico)

“e” → ∧

“ou” → ∨

Negação é identificada como:

“não X”

“X não Y”

Identificação das proposições atômicas

Exemplos:
“Kiki come de tudo, mas Kiki não é uma gata”
→ atomicas:

“kiki come de tudo”

“kiki é uma gata”

Mapeamento para letras

P, Q, R, S…

Negativa não vira nova letra, usa mesma letra + ¬

Construção final da fórmula

✔ Exemplo e Análise
Entrada:
Kiki come de tudo, mas Kiki não é uma gata

Saída:
Fórmula: (P ∧ ¬Q)
Mapeamento:
P = "kiki come de tudo"
Q = "kiki é uma gata"

✔ Análise

O sistema identificou corretamente o conectivo “mas” → ∧

Detectou a atômica positiva “kiki é uma gata” e aplicou negação

Funcionamento perfeito

❌ Exemplo com leve erro:

Entrada:

Se chover então grama molha


Sem vírgula.

Saída:

erro: frase atômica 'grama molha' não mapeada...


📌 Por quê?
A frase não segue o padrão “Se X, então Y” com vírgula.
O parser espera “se X então Y”.

✔ Correção:

Se chover, então a grama molha.

⚠ 3. Limitações e Possibilidades de Melhoria
❗ Limitações atuais:

Parser depende de formatações específicas.

Algumas frases ambíguas não são tratadas.

Não há suporte para:

Negações complexas (“é falso que…”)

Conectivos múltiplos aninhados

Proposições compostas dentro de uma mesma atômica

🚀 Melhorias futuras:

Adicionar análise sintática com SpaCy.

Criar reconhecimento mais robusto de conectivos.

Suporte a frases mais complexas:

“ou… ou…”

“apesar de que…”

“não apenas… mas também…”

Criar uma visualização gráfica da árvore lógica.

Adicionar testes automáticos de consistência.

