# Kit Codex — leia primeiro

## O que é este pacote
Duas vertentes intencionalmente SEPARADAS:
- **AULAS** (`01-AULAS-E-CONCEITOS.md`): conceitos e raciocínio ensinados no Bootcamp QI Tech 2026; não presume que tudo esteja no código.
- **PROJETO** (`02-PORQUINHO-MAPA-TECNICO.md`): o que existe de fato na branch principal do Porquinho Bank, verificado no commit `10465f6` do fork em 17/10/2026. Revalidar caso o código mude.

Depois vêm as **receitas** para mudanças (`03`), o **playbook** das 24 horas (`04`), **prompts** (`05`), o **STATUS** e um espaço para o **ENUNCIADO**. `06-FONTES-E-LIMITES.md` detalha a proveniência e lacunas.

## Onde está e como usar
Este pacote está em `codex/preparacao-hackathon-2026`, no fork público `MUSICAGEMSI/bootcamp-qi-tech`. A branch `main` foi preservada. Para o Codex Cloud, selecione o fork e esta branch ou traga estas alterações para a branch de trabalho apropriada. Apenas criar arquivos no GitHub NÃO injeta automaticamente contexto em tarefas já abertas; confira o commit da tarefa.

Para estudar o projeto: leia `AGENTS.md` + `01` + `02` e peça uma explicação rastreável por arquivo. Para implementar mudança real no banco: `03`, implementação, tests e RFC. Para desafio novo: transfira o AGENTS e os arquivos gerais para o **repositório do novo desafio**, adapte regras e preencha o enunciado. Não transfira automaticamente toda a solução bancária.

## Princípio de engenharia
**Enunciado → invariantes de negócio → contrato HTTP/interface → dados/estados → fluxos e erros → implementação → testes → entrega reproduzível.** Simples não significa sem testes; robusto não significa infraestrutura excessiva.

## O que NÃO fazer
- Não publicar os PDFs originais das aulas, notas internas, dados de participantes ou documentos restritos.
- Não presumir que IA, reutilização de código, internet ou bibliotecas externas são permitidas na etapa presencial; conferir regras oficiais.
- Não executar `docker compose down -v` ou testes que recriam o schema em banco com dados importantes.
- Não declarar que monitoração Prometheus equivale a alertas externos, ou que INTERNAL-TOKEN autentica correntistas.
- Não deixar o único estado da equipe preso a uma conversa: registrar decisões e testes no `STATUS.md` ou em commits.

## Sequência para retomar depois de erro no chat
1. Abrir outra tarefa Codex no repositório/branch correta.
2. Pedir: "Leia `AGENTS.md`, `docs/codex/ENUNCIADO.md`, `docs/codex/STATUS.md` e mostre o Git status/commit. Explique estado real e próximo P0; não edite."
3. Validar o resultado contra o código e testes.
4. Executar uma tarefa pequena e registrar progresso.

## Pontos de partida
- Código bancário: `src/resources/bank.py`, `src/controllers/bank_controller.py`, `src/repositories/bank_repository.py`.
- Banco: `src/models/bank.py`, `database/database.sql`, `database/ledger_guards.sql`, `database/migrations/001_bank.sql`.
- Evidências: `tests/integration/test_bank.py` e `test_bank_integrity.py`.
- Fontes existentes: `README.md`, `docs/rfc.md`, `docs/operacao.md`, `docs/entrega.md`.
