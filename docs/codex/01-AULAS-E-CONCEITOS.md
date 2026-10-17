# Vertente 1 — Aulas e conceitos do Bootcamp QI Tech 2026

**Natureza:** síntese didática original dos materiais da formação, não transcrição literal nem regra oficial de avaliação final. As aulas ocorreram em 12/09, 19/09, 26/09 e 03/10. Referências: slides das aulas 1/tarde (33), 2 (74), 3/manhã (35), 3/tarde (28), 4/manhã (39), 4/tarde (8, revisão), autoestudos 2/3/4, guias, modelo de RFC e exemplo da biblioteca. Não alegar ter acesso à apresentação integral da manhã da aula 1. Algumas orientações antigas de prazo se contradizem; a etapa final usa somente convocação oficial.

## Aula 1 — Desafio, Python/FastAPI e camadas
**O que construir foi proposto:** cadastrar cliente, vincular conta, registrar transações e saldo, cobrar tarifa de transferência, extrato paginado; disponibilizar código executável e RFC. Produto bancário real, identidade de consumidores e métricas avançadas não eram automaticamente obrigatórios.

**Python essencial:** função (def, parâmetros, retorno), tipos e dicionário JSON, lista, laços, if/else, classes, instâncias, self, herança, imports, tratamento de exceções, decorators, contextos e lambdas. Saiba rastrear qual função chama qual e o valor devolvido. Exemplo:
```python
def aplicar_variacao(saldo_centavos: int, delta: int) -> int:
    novo = saldo_centavos + delta
    if novo < 0:
        raise ValueError("saldo insuficiente")
    return novo
```
Exemplo didático, não substitui controle concorrente nem persistência.

**FastAPI e HTTP:** uma rota registra verbo+caminho+função. Uma requisição passa por middleware, validação de schema, resource, controller, repository, model/banco; DTO seleciona a resposta. Middleware envolve a chamada e processa também a saída.

| Camada | Decide | NÃO decide |
|---|---|---|
| middleware | token, logging, contexto, sessão | regra financeira |
| schema | forma/tipo/campos da entrada | saldo suficiente |
| resource | contrato HTTP/roteamento | SQL ou regra financeira complexa |
| controller | negócio, fluxo, limites, transação | detalhe HTTP arbitrário |
| repository | seleção/inserção/locks/paginação | código HTTP 404 |
| model/SQL | entidade, constraints, FK, UNIQUE | experiência do consumidor |
| DTO | representação JSON pública | atualizar saldo |
| errors/handlers | erro de domínio → status/corpo | apagar erro real sem rastreio |

**Exercício:** trace POST /customers e diga o que é validado antes do controller, como duplicate é impedido e quem constrói o JSON.

## Aula 2 — Modelagem, contratos, banco e RFC
**Bloco 1 — contratos:** cliente/servidor, ciclo requisição/resposta, GET/POST/PUT/PATCH/DELETE, status 2xx/4xx/5xx, JSON, REST como convenção de recursos, path/query/body/header, schema entrada ≠ DTO saída, paginação, UUID público diferente do ID relacional; IDOR/BOLA = autorização por recurso (UUID não concede permissão).
Exemplo: POST /accounts/{key}/transactions declara entradas e falhas, não só um endereço. Valores inválidos devem falhar antes de criar efeitos.

**Bloco 2 — dados:** tabela, PK/FK, 1:N/1:1, NOT NULL/UNIQUE/CHECK, índice, transação e isolamento. ACID: atomicidade = tudo ou nada; consistência = invariantes mantidas; isolamento = concorrentes não podem violar invariantes; durabilidade = commit persistido conforme garantias do SGBD. COMMIT ≠ FLUSH. Falha antes do COMMIT costuma permitir rollback, depois do COMMIT precisa de uma operação compensatória ou idempotência (não apagamento do histórico).
Dinheiro: usar centavos inteiros ou Decimal rigoroso, nunca float binário como valor financeiro canônico. Definir moeda, limites, tarifa, sinal e destino contábil.
Concorrência: dois saques de 700 sobre saldo 1000 não podem ambos vencer. Alternativas: SELECT FOR UPDATE; atualização condicional atômica; versão otimista; níveis de isolamento. Lock ordenado diminui deadlock; não é prova de ausência absoluta. Identificador UUID não substitui autorização.
Histórico: event sourcing/append-only e estados são alternativas de desenho; separar evento, saldo e transação. Erros também são ramos do fluxo.

**Bloco 3 — RFC:** documentar contexto, público/problema, invariantes, alternativas escolhidas/descartadas e seus custos, ER, contratos (entrada/saída/erro), fluxo feliz + falhas, testes e limites. Modelo oficial preenchível indicava duas seções e alvo de 2–4 páginas, enquanto o exemplo da biblioteca era mais longo e aulas permitiam outra organização: não confundir exemplo com norma absoluta.

**Exercício:** desenhe transferência de 2500 centavos + tarifa 100; origem deve ter 2600; transação deve registrar efeito total e desfazer tudo em erro.

## Aula 3 — Desenvolvimento, testes, Docker e integração
**TDD:** lista de comportamentos → teste falha pelo motivo correto (vermelho) → implementação mínima (verde) → refatorar e reexecutar. AAA: Arrange/Act/Assert. Pytest: função test_, assert, fixtures, parametrize, leitura do relatório de falha. Teste unitário verifica função isolada; integração black-box verifica API por HTTP sem importar src, preservando liberdade para refatorar camadas. Cobertura percentual não demonstra invariantes não exercitadas.
**Mockserver:** imita SOMENTE parceiro externo nos testes; não transforma a própria API ou banco em fake. Simule sucesso, timeout, erro e verifique que requisição ao parceiro foi feita quando aplicável. Não adicionar mock sem parceiro externo.
**Docker:** Dockerfile = receita; imagem = artefato; container = instância; registry distribui imagens; volume persiste dados; Docker Compose descreve serviços, portas, redes, ambiente e healthchecks. Schema definido em database.sql pode rodar só na primeira inicialização do volume; mudanças posteriores requerem migrações. Não usar down -v com dados importantes. Reproduzir em máquina limpa.
**Exercício:** teste via requests GET e POST, confirme status e JSON, depois GET novamente para comprovar ausência de efeito em erro. Rode com banco exclusivo de testes.

## Aula 4 — Produção, segurança e confiabilidade
**Observabilidade:** logs respondem "o que aconteceu com esta requisição?", request-id correlaciona; métricas mostram taxa/erros/duração (RED), latência/tráfego/erros/saturação (golden signals); tracing mostra spans entre serviços. Percentil p95: 95% das observações são menores/iguais ao valor (conforme método de cálculo). Liveness verifica processo vivo; readiness verifica aptidão a receber tráfego; não confundir.
**Segurança:** autenticação (quem chama), autorização (o que pode fazer com QUAL recurso), segredo externo ao código, least privilege, TLS, OWASP API Top 10/IDOR. INTERNAL-TOKEN apenas protege acesso ao serviço no cenário interno, não autentica cliente final.
**Resiliência:** timeout limita espera; retry pode duplicar efeito; backoff+jitter reduzem rajadas; circuit breaker limita chamadas ao serviço falho; idempotency key + UNIQUE + transação impedem efeito duplicado segundo contrato de escopo/payload/resultado; tratar mesma chave com payload diferente. Em integrações reais, sucesso pode ocorrer mesmo com resposta perdida.
**Escala:** app sem estado persistido na memória do processo, load balancer, health check, autoscaling, limites do banco/pool, métrica agregada por réplica. Conhecer conceitos não significa implementar Kubernetes.
**Exercício:** cliente retransmite POST após timeout. Mostre cenários antes e depois do commit e verifique repetição sem segunda cobrança.

## O raciocínio que une as quatro aulas
1. Identifique o problema e escreva invariantes de domínio.
2. Defina dados, contratos e falhas antes do código.
3. Implemente separando responsabilidades.
4. Prove o caminho feliz e os erros com testes executáveis.
5. Verifique retry, concorrência, segurança e reprodução.
6. Documente decisões e trade-offs sem superestimar garantias.

## Fontes externas úteis
- Python 3.11: https://docs.python.org/3.11/tutorial/
- FastAPI: https://fastapi.tiangolo.com/
- MDN HTTP: https://developer.mozilla.org/pt-BR/docs/Web/HTTP
- PostgreSQL transações: https://www.postgresql.org/docs/current/tutorial-transactions.html
- PostgreSQL locking: https://www.postgresql.org/docs/current/explicit-locking.html
- OWASP API Security: https://owasp.org/API-Security/
- Fowler TDD: https://martinfowler.com/bliki/TestDrivenDevelopment.html
- pytest: https://docs.pytest.org/
- Docker: https://docs.docker.com/get-started/
- SRE monitoramento: https://sre.google/sre-book/monitoring-distributed-systems/
- Idempotência AWS: https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/

## Arguição: perguntas para treino
Por que não deixar todas as regras na rota? Diferença schema/DTO? Onde fica o commit? O que é flush? Como two concurrent withdrawals não overdraft? Como a tarifa aparece no extrato? O que acontece com chave idempotente após timeout? Qual a diferença entre liveness e readiness? O que Docker não resolve? O que os testes provam e o que ainda não foi testado?
