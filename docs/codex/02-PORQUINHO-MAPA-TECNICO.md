# Vertente 2 — Porquinho Bank: implementação REAL

Conferido no GitHub em 17/10/2026, repositório MUSICAGEMSI/bootcamp-qi-tech, base main commit `10465f6`. Revalidar branch antes de modificar. Aplicação didática: NÃO liquida dinheiro real nem autentica correntistas.

## Arquitetura de requisição
`src/app.py`: inicialização, registro middleware, resources e erros. `src/constants.py`: env e rotas públicas. `src/database.py`: SQLAlchemy engine/contexto de sessão, timeouts/pool. `src/middlewares/{request_context,metrics,request_logger,internal_token,session_manager}.py`: request id, métricas, logs, token e sessão. Em entrada, middleware é executado em ordem inversa à de registro, conforme o arranjo da aplicação.
`src/resources/bank.py`: valida HTTP, `Idempotency-Key`, paginação e despacha controller. `src/schemas/*.json`: payloads, enums, inteiro, required, additionalProperties false. `src/controllers/bank_controller.py`: regras. `src/repositories/bank_repository.py`: SQLAlchemy, locking, paginação, idempotência. `src/models/bank.py`: sete entidades. `src/dtos/bank_dto.py`: resposta pública. `src/errors/bank_errors.py`: erros e status.
Arquivos `sample_entity*` permanecem como exemplo herdado da base, NÃO são a funcionalidade bancária.

## Entidades do modelo
- Customer: `customer_key` público, CPF/documento e email únicos, nome e data nascimento.
- Account: `account_key` público; `customer_id` FK UNIQUE (no máximo uma conta por cliente); saldo em centavos; ACTIVE/CLOSED.
- PiggyBank: `piggy_bank_key`; FK de conta; nome único por conta, meta informativa, saldo reservado, ACTIVE/CLOSED.
- BankTransaction: evento DEPOSIT/WITHDRAWAL/TRANSFER/SAVE/REDEEM; amount e fee.
- LedgerEntry: contrapartidas assinadas, pocket CHECKING/PIGGY/CLEARING/FEES, `balance_after`.
- StatusEvent: histórico de estados.
- IdempotencyRecord: UNIQUE(scope,key), fingerprint SHA-256 e resposta JSONB.
Models Python em `src/models/bank.py`; SQL em `database/database.sql`, `database/ledger_guards.sql` e migração `database/migrations/001_bank.sql`. Models NÃO migram banco automaticamente.

## Operações e método controller
- `create_customer(payload)`: CPF com dígitos verificadores, data válida, idade 18..120, normaliza email, cria cliente. Formato é separado em post_customer.json; UNIQUE no banco.
- `create_account(payload)`: cliente existente, conta com saldo 0, ACTIVE, evento de abertura; unicidade por cliente.
- `change_balance(entity, delta)`: barra resultado < 0 ou > MAX_MONEY (9_000_000_000_000_000 centavos). Não dispensa locks.
- `transact(key,payload)`: depósito, saque ou transferência. Exige destination_account_key somente em TRANSFER; impede origem=destino. `lock_accounts` bloqueia contas em ordem do ID interno. Tarifa constante `TRANSFER_FEE=100` centavos à origem, além do principal. Transferência cria quatro lançamentos balanceados.
- `create_piggy`: conta ACTIVE, novo porquinho, evento ACTIVE.
- `transact_piggy`: SAVE reduz CHECKING e aumenta PIGGY; REDEEM faz contrário, mantendo soma do total.
- `get_account`: saldo disponível, reservado (soma das reservas), total e eventos. `account_dto` calcula saldo agregado.
- `close_account`: só fecha com CHECKING e reservas vazios, registra CLOSED. `close_piggy`: exige saldo do porquinho 0.
- `statement`: extrato de lançamentos daquela conta ou porquinho, página decrescente.
- `execute(scope,key,payload,operation)`: fingerprint do JSON canônico; `repo.claim` insere/chave ou lê; corpo diferente 409, repetição retorna resposta guardada; operação+resposta+commit confirmados juntos; IntegrityError único vira 409, rollback.

## Lançamentos reais em centavos
| Tipo | Efeitos no ledger | Soma |
|---|---|---|
| DEPOSIT 10000 | +10000 CHECKING, -10000 CLEARING | 0 |
| WITHDRAWAL 500 | -500 CHECKING, +500 CLEARING | 0 |
| TRANSFER 2500 + fee 100 | -2500 CHECKING origem; -100 CHECKING origem (FEE); +2500 CHECKING destino; +100 FEES | 0 |
| SAVE 4000 | -4000 CHECKING; +4000 PIGGY | 0 |
| REDEEM 4000 | +4000 CHECKING; -4000 PIGGY | 0 |

Se A deposita 10000, reserva 4000 e transfere 2500 com fee 100, CHECKING de A termina em 3400; PIGGY em 4000; B recebe 2500; tarifa fica em FEES. Não há criação de dinheiro pela reserva.
`database/ledger_guards.sql`: history immutable triggers contra update/delete; deferred constraint triggers verificam transações com >=2 lançamentos, soma zero, owner do porquinho, saldo CHECKING/PIGGY igual aos lançamentos e `balance_after` reconstituível. Checks ocorrem no COMMIT; custo cresce com histórico.

## Repositório, concorrência, paginação
`BankRepository.add`: add + flush, NÃO commit. `find`: select; `page`: ORDER BY id DESC, LIMIT limit+1, OFFSET limit*page. `statement`: JOIN LedgerEntry e BankTransaction, filtra account_id e piggy_bank_id. DTO `page_dto`: `is_last_page` sem contagem total. `lock_accounts`: SELECT FOR UPDATE em ordem crescente do id; `lock_piggy`: lock do porquinho; `claim`: INSERT ... ON CONFLICT DO NOTHING + SELECT FOR UPDATE de registro de idempotência.
Concorrência protegida para contas e idem por constraint PostgreSQL; paginação offset não fornece snapshot estável se dados forem inseridos durante navegação.

## Contratos públicos
POST negócios exige INTERNAL-TOKEN, Content-Type JSON, Idempotency-Key ASCII 1..128. Repetição mesmo corpo → mesmo 201/corpo; chave igual/corpo distinto → 409. Erros padronizados `title, description, translation, code`. GET e PATCH não requerem chave idempotente (PATCH close é idempotente pelo estado). Valores de dinheiro inteiros, em centavos BRL; não usar float.
Rotas em `README.md` e `docs/rfc.md`: POST/GET /customers, /accounts; POST/GET /accounts/{key}/transactions; POST/GET /accounts/{key}/piggy_banks; POST/GET reservas e transações; PATCH close; /health_check e /metrics. Consulte a versão em `src/resources/bank.py` antes de adicionar endpoints.

## Testes e execução
`docker compose up --build --wait` inicializa API e PostgreSQL no localhost; `pytest -v` com Python 3.11 e requirements-dev; `python scripts/demo.py` mostra cenário; `docker compose --profile observability up --build` inicia Prometheus. `test_bank.py`: HTTP black-box, validação, tarifa, idempotência concorrente, duas retiradas simultâneas, encerramento. `test_bank_integrity.py`: introspecção SQL de imutabilidade, reconciliação e timeout. `.github/workflows/ci.yml`: Compose, pytest, check Prometheus. README indica 70 testes; NÃO executados por esta documentação. Testes herdados recriam `public` schema: banco EXCLUSIVO de teste.
`docs/operacao.md`: /health_check é liveness e NÃO acessa banco; request ID nos logs, métricas protegidas, regras de alertas mas SEM Alertmanager para notificações externas. INTERNAL-TOKEN não autentica titular de conta. Sem Pix real, gateway, autenticação de usuário final, tracing distribuído ou backup/restore.
`docs/rfc.md`: decisões e trade-offs. Reconciliação SQL custosa em grande escala; opção consciente de integridade demonstrável para protótipo.

## Pontos de defesa
Explique por que: centavos, soma zero dos lançamentos, ordenação de locks, idempotência com escopo/hash, event history append-only, schema SQL+model, DTO, teste por HTTP, Compose reproduzível e limites da segurança.
**Não confundir:** `src/connectors/bankslip_connector.py` herdado existe mas não participa do fluxo bancário; a ferramenta de monitoramento opcional não significa alertas enviados; formatação da RFC não é funcionalidade financeira.
