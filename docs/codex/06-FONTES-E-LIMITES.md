# Fontes, proveniência e limitações

## Vertente aulas (material do bootcamp)
Síntese autoral baseada em materiais anteriormente enviados e acessíveis para análise: resumo rastreável QI_Tech_Plano_Dupla.md (01/10/2026), slides Aula 1 tarde (33), Aula 2 (74), Aula 3 manhã (35) e tarde (28), Aula 4 manhã (39) e tarde (8, revisão), Autoestudo-aula2.pdf (7 páginas), Autoestudo-aula3.pdf (6), autoestudo-aula4.pdf (6), Guia Dia 2, Guia Dia 3, documentação de ferramentas, modelo de RFC PDF e Markdown e RFC exemplo de biblioteca. Não foi incorporado o texto integral dos slides nem os seus arquivos originais. O conteúdo didático não equivale a critérios oficiais do evento final. Material da manhã da Aula 1 não foi integralmente conferido. Demonstrações ao vivo não documentadas não foram reconstruídas como fatos.

Alguns PDFs e slides continham prazos desatualizados/contraditórios: não usar como fonte vigente do novo desafio.

## Vertente projeto — revisão autenticada
Fork `MUSICAGEMSI/bootcamp-qi-tech` ref `main` SHA `10465f6af76f571be9928454f9ddfd71c983c2bd`, também comparado com origem `CodedDrexler/bootcamp-qi-tech`. Arquivos conferidos diretamente: README, controller/repository/resources/model/dto/errors bancários, schemas centrais, app, database, constants, ledger_guards, RFC, operação, testes bancários e de integridade, Compose e CI. Pastas restantes inventariadas pela árvore Git. Se modificar branch, conferir a implementação atual e não assumir que este guia se atualiza sozinho.

NÃO houve nesta geração execução de Docker, PostgreSQL ou pytest. README menciona 70 testes, mas isso não constitui resultado de execução atual. Sem acesso ao enunciado final do hackathon.

## Política de publicação
Este fork é PÚBLICO. Este kit não inclui scans/slides, links privados, credenciais, nomes de terceiros não necessários ou dados pessoais. Se houver material restrito do evento, mantenha-o em repositório PRIVADO autorizado e forneça ao Codex de maneira compatível com as regras de compartilhamento. Revisar direitos de uso/redistribuição antes de publicar novos anexos.

## Links do próprio projeto
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech/blob/main/README.md
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech/blob/main/docs/rfc.md
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech/blob/main/docs/operacao.md
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech/blob/main/tests/integration/test_bank.py
- https://github.com/MUSICAGEMSI/bootcamp-qi-tech/blob/main/tests/integration/test_bank_integrity.py

## Ordem de confiança
(1) enunciado final oficial e regras do evento, (2) código e testes atuais verificáveis, (3) RFC e operação atuais, (4) síntese das aulas, (5) recomendações deste pacote. Em caso de conflito explicitar e pedir decisão.
