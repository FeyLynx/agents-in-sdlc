---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, também é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você pode ver todas as tarefas do agente com pull requests abertos. Também pode explorar as operações executadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois que o agente recebeu a tarefa de criar um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar os estilos do site e adicionar alternadores, pareceu natural incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente o modo claro e o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento e revise as operações executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull request (PR) à direita. Ele mostra o PR e os arquivos alterados para que você acompanhe o trabalho.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione um alternador para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado atual do trabalho.

   ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que o Copilot termine a configuração e comece o trabalho. Você saberá que isso aconteceu quando começarem a aparecer mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o recebimento e incluir a solicitação no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

O processo levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o centro de controle para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Você pode monitorar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento para ajustar o escopo ou fornecer orientações.

Você explorou:

- como usar o Mission Control e a página de agentes para acompanhar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para pedir funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
