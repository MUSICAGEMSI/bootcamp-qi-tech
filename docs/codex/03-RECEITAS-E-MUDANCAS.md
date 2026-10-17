# Receitas reutilizáveis — requisito → arquivos → teste

## Método universal (dois iniciantes + Codex)
Para cada alteração: (1) ler regra e aceite, (2) localizar o fluxo atual e testes, (3) escolher alteração mínima, (4) definir entradas/saídas/erros/dados, (5) escrever caso feliz + falha, (6) implementar, (7) executar testes afetados e suíte, (8) revisar diff, (9) atualizar documentação/STATUS, (10) commit isolado. Não começar três funcionalidades em paralelo antes de ter uma integração verificável.

## Mapa de alterações do Porquinho Bank (caminhos efetivos)
| Solicitação | Alterar | Verificar |
|---|---|---|
| Nova rota | src/resources/bank.py e registro da rota | schema, controller, status, integração |
| Campo de entrada | src/schemas/post_*.json | controller, model/SQL se persistido, teste validação, DTO |
| Mudar a tarifa | TRANSFER_FEE em src/controllers/bank_controller.py | regra de débito, entries FEE/FEES, testes, README e RFC |
| Nova transação | schema, transact ou transact_piggy, BankTransaction.type | CHECK no SQL/model, ledger, idempotência, teste de balanceamento |
| Saldo ou limite | change_balance, MAX_MONEY, JSON schema | CHECK SQL, invariantes e overflow |
| Consulta/paginação | resource.pagination, repository.page/statement | dto.page_dto, teste de ordenação/limite |
| Resposta JSON | src/dtos/bank_dto.py | consumidores e asserts |
| Novo erro | src/errors/bank_errors.py | exception mapping, status + code, tests |
| Nova tabela | models/bank.py + database/migrations/*.sql | FK, index, constraints, repository, CI |
| Métricas | src/middlewares/metrics.py | monitoring/prometheus.yml/alerts.yml, script check |
| Integração externa | src/connectors/*.py | timeout, segurança, mockserver e erros |
| Log/segurança | middlewares, constants | erros HTTP, headers, secrets, testes |
| Infraestrutura | Dockerfile, docker-compose.yml, .env.example | clone limpo, health, teste em banco isolado |

## Modelo de tarefa para o Codex
**Objetivo:** <verbo e resultado visível>.
**Critérios:** <status JSON/estado e falhas>.
**Escopo permitido:** <arquivos>.
**Não alterar:** <outras áreas>.
**Evidência:** <testes/outputs>.
**Entrega:** lista de arquivos, diff e limitações.

## Exemplo 1 — Tarifa de R$1 para R$2,50 (treino; NÃO aplicar automaticamente)
Inspecionar `TRANSFER_FEE = 100` no controller. Alterar para 250, verificar transferência de 2500 exige 2750 disponíveis; quatro lançamentos de -2500, -250, +2500, +250; testes `test_transfer_fee_statement_and_rollback` e demais referências; docs/rfc.md, README e demo. Rodar pytest em banco descartável. Não mudar SQL sem razão (tipo/limite não foi alterado). A mudança só fica pronta se novos saldos e histórico refletirem a tarifa, inclusive recusa por saldo insuficiente.

## Exemplo 2 — Novo filtro no extrato
Descrever semântica de query parameter (tipo, intervalo, ordenação); validar no resource; repository.statement usa SQL com filtros sobre LedgerEntry/BankTransaction sem devolver records fora da conta; DTO continua determinístico; testes com múltiplas transações e paginação. Se mudar API pública, atualizar RFC/README.

## Exemplo 3 — Nova entidade de domínio
Antes de código: definir PK, FK, UNIQUE e estados; escrever contrato de criação/leitura/erro; SQL migration e model espelhados; criar repository + controller + resource/schema/DTO; registrar rota; teste HTTP de sucesso, duplicidade, inexistência e autorização. Só adicionar transação distribuída ou fila se enunciado justificar.

## Exemplo 4 — Diagnóstico de bug sob tempo
Reproduzir falha com comando e payload; ler log e request-id; localizar no caminho resource→controller→repository→SQL; escrever teste que reproduza; corrigir apenas causa; rodar testes; revisar side effects. Se falha de ambiente, conferir docker compose ps/logs, portas, variáveis, saúde do banco, volume e URL antes de alterar lógica. Não corrigir infraestrutura apagando dados.

## Exemplo 5 — Idempotência
Contrato inclui: onde vem a chave, formato/escopo, comparação de payload canônico, armazenamento de sucesso, falhas, concorrência entre chamadas e resposta repetida. Apenas consultar antes de inserir sem constraint não é prova; ver unique+transação. Não copiar política de idempotência para todo método HTTP indiscriminadamente.

## Critérios de pronto
Código revisado; teste feliz+erro realizado; efeitos colaterais verificados; docs coerentes; demo mínima; commit rastreável; STATUS atualizado. Se não pôde testar, registrar "não testado" e por quê.
