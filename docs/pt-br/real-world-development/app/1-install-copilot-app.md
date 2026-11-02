---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e se integra nativamente ao GitHub, portanto seus repositórios, branches e pipelines de CI funcionam sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um no próprio espaço de trabalho isolado — em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo pelo repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e integrar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador deve manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Como você pode imaginar, a primeira etapa para usar o aplicativo GitHub Copilot é instalá-lo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu nome de host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua a configuração inicial.
7. Procure seu repositório em **Sessions** na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se seu repositório aparece na barra lateral. No layout usado aqui, as pastas dos repositórios, como `tailspin-toys`, aparecem agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza os recursos em algumas áreas na barra lateral:

- **New** — permite iniciar uma nova sessão de chat com o Copilot.
- **Pull requests** — mostra seus pull requests por meio da integração nativa com o GitHub. Você pode navegar e filtrar os PRs, conferir o estado da CI e revisá-los sem sair do aplicativo.
- **Issues** — mostra suas issues. Você pode navegar e filtrá-las e iniciar uma sessão a partir de uma issue.
- **Automations** — tarefas de agente salvas para execução agendada ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. A lição de encerramento apresenta esse recurso como próximo passo, não como outro exercício do workshop.
- **Customize** — permite adicionar recursos ao aplicativo Copilot, como servidores MCP, plugins, skills e outros componentes. Você usará essa área para configurar o MCP do Playwright.
- **Chats** — conversas leves para perguntas e brainstorming que não precisam de branch ou espaço de trabalho próprios. Essa seção aparece depois que você inicia seu primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar o primeiro chat.
- **Sessions** — agrupa as pastas dos repositórios e as sessões de agente. Selecione o nome do repositório, como `tailspin-toys`, para abrir a visualização dele ou selecione uma sessão abaixo do repositório para voltar à conversa. Cada sessão é executada em um espaço de trabalho isolado, permitindo executar várias ao mesmo tempo sem conflitos entre as alterações. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, pergunte. Ele ajudará você.

### Localize o backlog criado pelo modelo

Quase todo projeto tem um backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado quando você gerou sua cópia do modelo.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler seus detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma pequena alteração inicial.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de trabalho de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo* — e um **chat rápido** é ideal para isso. Com chats rápidos, você pode fazer uma pergunta ou trocar ideias sem criar uma branch nem uma worktree. São adequados para perguntas rápidas e descartáveis, sem exigir uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como as sessões dele funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em sua própria worktree Git isolada, o que permite executar vários agentes em paralelo sem conflito entre as alterações. Você pode continuar a conversa ou iniciar um novo chat quando quiser.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto pelo repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**.
- usar um chat rápido para fazer uma pergunta simples e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
