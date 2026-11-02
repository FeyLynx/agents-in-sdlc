---
title: "Lição 10 - Revisão e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e localizou a issue de filtragem existente.
- adicionou avaliações por estrelas, revisou o resultado no navegador encaminhado e integrou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a mudança de filtragem e autorizou o Agent Merge no PR de filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas:** exibir o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e a mesma branch. Reunimos o trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que não são necessários testes novos também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Boas práticas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para pensar na abordagem antes de começar, **Interactive** para acompanhar de perto mudanças focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e outro mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* influencia significativamente o resultado.

## Explore mais

Você concluiu o fluxo principal. Veja outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar o uso de qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe seus aprendizados com a equipe e aprenda com os demais. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
