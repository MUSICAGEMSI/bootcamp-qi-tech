# Prompts prontos para Codex

## A. Reconhecer as duas vertentes (somente leitura)
"Leia AGENTS.md, docs/codex/01-AULAS-E-CONCEITOS.md e docs/codex/02-PORQUINHO-MAPA-TECNICO.md. Compare cada conceito com os arquivos reais do repositório. Monte a matriz aula → conceito → função/código → teste → lacuna. Não altere arquivos nem afirme ter executado testes."

## B. Preparar desafio depois de receber enunciado
"Leia AGENTS.md, docs/codex/ENUNCIADO.md, docs/codex/STATUS.md e o documento original autorizado. Não implemente ainda. Extraia requisitos verificáveis e critérios oficiais; separe fato/proposta; defina MVP, P0/P1/P2, esquema de dados, fluxo de sucesso e falhas, plano de teste, responsabilidades e as 3 ambiguidades mais críticas. Entregue plano enxuto e aguarde aprovação."

## C. Implementar uma feature
"Objetivo: <...>. Aceite: <...>. Arquivos relevantes: <...>. Restrições: não alterar fora do escopo, preservar contratos e testes. Primeiro inspecione o código; escreva teste útil; implemente menor mudança; execute teste afetado e regressão permitida; mostre diff, comandos, resultados, pendências e atualize STATUS apenas se autorizado."

## D. Corrigir falha
"Reproduza o erro a partir desta saída/entrada: <...>. Formule até três hipóteses, verifique cada uma com comandos/inspeção. Escreva teste de regressão. Corrija causa real sem refatoração ampla. Confirme resultado de forma reproduzível; se faltar serviço/permissão, informe bloqueio. Não apague volumes."

## E. Revisar antes de enviar
"Com base no enunciado oficial, execute auditoria de requisitos P0 e faça revisão crítica do diff. Procure falhas em entrada/saída, erro, autorização, dados, concorrência, idempotência, setup e documentação. Faça tabela evidência real por requisito; reporte itens não demonstrados. Não implemente novas features sem aprovação."

## F. Retomar após conversa travada
"Nova tarefa, mesmo projeto. Leia AGENTS.md, docs/codex/ENUNCIADO.md e docs/codex/STATUS.md. Inspecione branch, commit e working tree. Confira o estado real do código e os testes existentes. Resuma o que está de fato concluído e proponha uma única próxima ação P0. Não edite até eu aprovar."

## G. Treino prático agora (sem mudar main)
"Crie uma branch de treino derivada de main e proponha um teste novo pequeno para a API Porquinho Bank. Não mude comportamento de negócio. Execute o teste somente em banco descartável. Mostre comando, diff e resultado; não faça push sem autorização."

## Princípios para escrever prompts
Evitar "construa tudo com maestria" isoladamente. Sempre especificar comportamento visível e aceites; reduzir escopo; reservar momentos de integração. Se faltar requisito, perguntar somente o necessário para escolher arquitetura. Quando Codex concluir, pedir prova com comandos e revisão do diff.
