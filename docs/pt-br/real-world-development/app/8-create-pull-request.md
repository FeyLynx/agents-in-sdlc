---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você integrou o pull request (PR) de avaliação por estrelas; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e uma branch e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionar todo o PR do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar a integração do PR.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle de quem desenvolve.

## Introdução ao Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando a integração — corrige verificações de integração contínua (CI) que falharam, responde aos comentários de revisão e faz rebase quando necessário — e integra o PR assim que o GitHub permitir. O processo é executado em segundo plano, continua após reinicializações do aplicativo e é desativado quando o pull request é integrado.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas sua capacidade de editar código e fazer merge ainda exige autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão de merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Escolher a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge começa. Ele vai:

- criar o pull request com título e descrição.
- se a sessão tiver começado a partir de uma issue, incluir uma referência à issue no corpo da descrição.
- fazer rebase ou resolver possíveis conflitos de merge com a branch de destino.
- acompanhar a CI para confirmar se todas as verificações foram aprovadas.
- monitorar o PR em busca de feedback de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer alterações para resolver os comentários.
- opcionalmente, integrar o PR automaticamente depois que tudo for aprovado.

Vamos permitir que o Agent Merge também integre o PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão de merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Acompanhe o progresso e resolva os bloqueios relatados. Antes de continuar, confirme se o PR está com o estado **Merged**. A permissão de merge habilitada ou um agente em execução não confirma que a integração foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de integração.
- inspecionou todo o PR do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou a integração do PR.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
