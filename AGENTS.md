# AGENTS.md — Preparação Codex | Bootcamp QI Tech 2026

## Missão e escopo
Este branch é uma BASE DE ESTUDO sobre o Porquinho Bank e um kit para preparar um hackathon de aproximadamente 24 horas. NÃO existe enunciado final confirmado neste branch. Não invente tema, critérios, funcionalidades ou exigências. Não trate a API bancária como solução obrigatória do desafio futuro.

## Fontes: ler por necessidade
1. `docs/codex/00-LEIA-PRIMEIRO.md` — índice, uso e fronteiras entre estudo e entrega.
2. `docs/codex/01-AULAS-E-CONCEITOS.md` — conceitos ensinados nas aulas 1–4, aplicação e exercícios.
3. `docs/codex/02-PORQUINHO-MAPA-TECNICO.md` — fatos conferidos na implementação e onde estão.
4. `docs/codex/03-RECEITAS-E-MUDANCAS.md` — como traduzir requisito em arquivos/testes.
5. `docs/codex/04-PLAYBOOK-24H.md` — estratégia, prioridades, divisão de tarefas, integração e contingências.
6. `docs/codex/05-PROMPTS-CODEX.md` — modelos de pedidos para planejamento, implementação e revisão.
7. `docs/codex/STATUS.md` — andamento registrado; verificar também código, branch e resultados reais.
8. `docs/codex/ENUNCIADO.md` — PENDENTE. Só considere regras oficiais depois de preenchido.
9. `docs/codex/06-FONTES-E-LIMITES.md` — proveniência, incertezas e material não incorporado.

Leia somente os documentos necessários para a tarefa. Antes de programar, consulte o enunciado oficial, caso exista.

## Regras de trabalho
- Primeiro: objetivo, entrada, saída, critérios de aceite, erros, dependências, dados e riscos.
- Separe FATOS vistos no repositório de PROPOSTAS e de REGRAS OFICIAIS.
- Prefira menor mudança correta, código legível, camadas com responsabilidades claras, integração precoce.
- Nunca copie código antigo para desafio novo sem conferir pertinência, licença e regras do evento.
- Não force Python/FastAPI/PostgreSQL se o novo enunciado exigir ou favorecer outra stack.
- Para cada alteração: identifique arquivos, implemente, teste e mostre diff e resultado. Não alegue ter executado testes que não rodaram.
- Regra financeira crítica: preserve invariantes, atomicidade, concorrência e idempotência conforme o contrato efetivo. Não execute SQL destrutivo em dados relevantes.
- Não divulgue credenciais, segredos, CPF real, conteúdo privado dos slides ou dados pessoais. Este repositório é PÚBLICO.
- Sem push, merge, deleção irreversível, deploy ou publicação de artefatos sem autorização explícita.
- Não edite a branch `main` do Porquinho Bank por iniciativa própria. Trabalhe em branch própria de tarefa, mantenha alterações pequenas.
- Ao encontrar impedimento técnico: reportar comando, erro relevante, hipótese, próximos testes e o que ficou bloqueado.

## Entrega de cada tarefa
Responder com: (1) feito, (2) arquivos alterados, (3) evidência/testes reais, (4) limites ou pendências, (5) como reproduzir, (6) próximo passo verificável. Quando autorizado a editar arquivos, atualizar `docs/codex/STATUS.md` com commit/branch, progresso, testes e bloqueios; não registrar sucesso hipotético.

## Continuidade
Nova conversa não recupera automaticamente todos os chats anteriores. Ao retomar: ler AGENTS, ENUNCIADO, STATUS; inspecionar Git e o código; confirmar qual branch/commit está ativo; só então continuar. Os documentos são contexto e não substituem a inspeção do estado real.

## Inicialização do novo desafio
Ao receber enunciado, NÃO implementar de imediato. Extrair requisitos e critérios, ordenar P0/P1/P2, levantar dúvidas decisivas, definir MVP e testes, solicitar aprovação curta. Se 24 h estiverem correndo, propor uma decisão provisória explícita quando a ambiguidade não comprometer requisito obrigatório. Durante a implementação, uma funcionalidade completa por ciclo: contrato → código → teste → integração → registro de estado.
