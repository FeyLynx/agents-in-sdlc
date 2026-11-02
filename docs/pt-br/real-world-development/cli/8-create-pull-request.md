---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você mesmo integrou o pull request (PR) de avaliação por estrelas; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e uma branch e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionar toda a mudança do recurso e as evidências de verificação.
- criar o PR da filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a integração do PR.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle de quem desenvolve.

## Introdução ao Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar um pull request. Quando você o habilita, o Copilot resolve o que estiver impedindo a integração do PR — corrige verificações de integração contínua (CI) que falharam, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, vamos revisar as mudanças, criar o PR e deixar que o Agent Merge cuide do restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se aparece uma mensagem do Copilot semelhante a “PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e, quando o repositório permitir, faça manualmente o merge que você já revisou.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionou toda a mudança do recurso e as evidências de verificação.
- criou o PR da filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou a integração do PR.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar o contexto, os modelos e o compartilhamento e, se quiser, delegar tarefas ao agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
