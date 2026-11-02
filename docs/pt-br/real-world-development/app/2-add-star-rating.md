---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cards de jogos e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e experimentou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a integrará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e conhecer sua estrutura.
- pedir ao agente que faça uma pequena alteração focada no projeto.
- revisar a alteração na visualização de diff do espaço de trabalho.
- executar o aplicativo localmente e confirmar a mudança no navegador.
- abrir e integrar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão tem uma **worktree Git e uma branch dedicadas**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar um recurso e outra para corrigir um bug, por exemplo — sem conflito entre as alterações. As sessões aparecem agrupadas por repositório na barra lateral. Selecione uma sessão para alternar para ela.

Em uma sessão, você verá três coisas: a **conversa** com o agente; a **atividade das ferramentas** enquanto ele explora e edita arquivos; e a lista de **arquivos alterados** com seus diffs.

## Iniciar uma sessão e solicitar a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto pelo repositório do GitHub. Vamos criar uma nova sessão para esse repositório e solicitar a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Selecione **+** na parte superior da barra lateral para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha uma **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando uma nova worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados para adicionar o recurso e cria o código necessário. Você acaba de adicionar um recurso com o aplicativo Copilot!

## Revise o diff

Toda alteração gerada por IA merece uma revisão antes da integração, mesmo as pequenas. Vamos examinar as mudanças no próprio aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

   ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes dos jogos. Ele será semelhante ao exemplo a seguir: um pequeno bloco que mostra a avaliação quando ela existe e usa `No rating yet` quando `starRating` é `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico. O código exato pode variar, mas deve ser relativamente parecido com o exemplo.

## Confira as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme se os testes cobrem um valor numérico de `starRating` e a alternativa `null`. Um pré-requisito ausente ou uma verificação ignorada não significa aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Peça ao Copilot para abrir o site e examine a interface atualizada em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do aplicativo Copilot. Mais adiante, você explorará canvases personalizados e criará o seu. Por enquanto, usaremos o canvas de navegador integrado.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme se os cards dos jogos avaliados exibem o valor de zero a cinco.
4. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão e feche o canvas de navegador:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Abrir e integrar seu primeiro pull request

Você criou o recurso! Agora, abra um pull request (PR) para integrar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes da criação do PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o PR no painel de revisão. Revise-o conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para integrar o PR.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Especificamente, você:

- iniciou uma sessão de agente e conheceu a estrutura das sessões.
- orientou o agente a fazer uma alteração pequena e focada nos cards dos jogos.
- revisou a alteração na visualização de diff do espaço de trabalho.
- executou o aplicativo localmente e confirmou a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez o merge explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#install-and-configure-the-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
