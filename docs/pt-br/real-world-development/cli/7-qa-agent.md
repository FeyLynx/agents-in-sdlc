---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem no navegador. Agora, combine esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as conclusões com base na issue de filtragem.

## Cenário

A Tailspin Toys quer revisar requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador de forma consistente antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até agora têm funções distintas. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as conclusões. Ele não substitui os outros recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá tanto as verificações de QA quanto os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites desejados. Isso evita que um agente incompleto ou abrangente demais altere o recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome claro para exibição.
3. Leia as instruções do perfil e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme se o QA apresenta evidências, pergunta antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não contemplar alguma responsabilidade ou limite, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a mesma conversa de filtragem depois de criar o perfil e selecione o agente de QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use este prompt para pedir que o agente de QA revise o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Aguarde o agente de QA trabalhar.
4. Leia o relatório quando ele terminar.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o trabalho do agente. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões com base na issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá esses itens em um PR de recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
