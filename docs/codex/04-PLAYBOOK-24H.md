# Playbook de hackathon — 24 horas

**Importante:** cronograma sugerido; o formato do desafio, regras de IA, tamanho da equipe, rede e critérios ainda exigem confirmação oficial. Não assumir tema bancário.

## Primeiros 40 minutos: leitura e decisão
- Ler enunciado original e critérios; extrair requisitos P0 com aceite mensurável.
- Confirmar o que é permitido (Codex, outras IAs, código anterior, internet, bibliotecas, dados).
- Marcar ambiguidades decisivas; escolher abordagem mínima e reversível.
- Definir uma demo com caminho completo em menos de 3 minutos.
- Criar repo/branch, STATUS, quem integra, quem revisa e onde ficam os outputs.

## Agenda sugerida
| Janela | Meta | Critério para avançar |
|---|---|---|
| 0–2 h | entender + plano P0/P1/P2 | ambas pessoas explicam MVP |
| 2–4 h | ambiente e vertical slice | sistema inicializa e uma feature executa ponta a ponta |
| 4–15 h | implementar P0 em incrementos | cada feature possui teste executado e commit |
| 15–20 h | integração/falhas e P1 se couber | demo estável + casos negativos |
| 20–22 h | README, demo, apresentação | instalação limpa e roteiro ensaiado |
| 22–24 h | contingência + entrega | commit/URL, arquivos e requisitos confirmados |

## Divisão de equipe sem perder visibilidade
Pessoa A: produto, contrato, dados e integração/revisão. Pessoa B: implementação/testes/execução; TROCAR revisão para não criar silo. Dividir por features completas, não "um só faz docs, outro só escreve código". Definir proprietário temporário de arquivos compartilhados. Codex faz tarefa delimitada em branch/worktree isolado; uma pessoa valida os diffs e os testes; não ter dois agentes editando o mesmo arquivo simultaneamente.

## Ritual a cada 60–90 minutos
1. Atualizar STATUS: feito, evidência, próximo, bloqueios, hash de commit.
2. Conferir requisitos P0 ainda abertos e tempo restante.
3. Integrar branch curta e executar smoke test.
4. Escolher um único próximo trabalho crítico para cada frente.
5. Ao detectar bloqueio >20 min: reproduzir erro, reduzir caso, pedir diagnóstico Codex ou ajuda da dupla.

## Estratégia de prompts
1. Para planejar: "não implemente; leia enunciado; liste P0/P1/P2, ambiguidades e caminho mínimo".
2. Para editar: "altere apenas <arquivos> para cumprir <aceite>; escreva teste; execute; reporte diff".
3. Para bug: "reproduza antes de corrigir; não faça mudanças especulativas".
4. Para revisão: "tente quebrar as invariantes e a demo; faça análise de lacunas com evidências".
Sempre inspecionar resultados de comandos reais e não confiar apenas na narrativa do agente.

## Definição do MVP
O menor fluxo REAL, reproduzível, que prova o objetivo central e os P0. Exemplo bancário didático: cadastrar cliente → criar conta → depositar → transferir com tarifa → consultar extrato → testar saldo insuficiente. NÃO é template obrigatório de outro domínio.

## Falhas previstas
- Ambiente não sobe: ver portas, env, logs, Docker, versão, network, DB; evitar reestruturar tudo.
- Testes inconsistentes: isolar banco/fixtures e chamadas simultâneas.
- Contexto longo: nova tarefa lê AGENTS + ENUNCIADO + STATUS + Git; não precisa recapitular todo o chat.
- Modelo lento: esforço médio para tarefas normais; alto para arquitetura/bugs complexos; instrução específica e escopo curto valem mais que esforço extremo.
- Sem internet: usar documentação local, dependências já instaladas e reduzir escopo.
- Pouco tempo: congelar escopo, terminar P0 e demo, descartar extras não essenciais.

## Saída final
Repositório no commit esperado, instrução curta de execução, suite/report de testes efetivamente executados, demonstração, limitações, decisões e contrato explícito. Não inventar métricas, testes ou benchmarks.
